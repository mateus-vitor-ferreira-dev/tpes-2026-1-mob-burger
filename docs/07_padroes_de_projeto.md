# 07 — Padrões de Projeto

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Padrões Identificados

| Padrão | Onde é usado | Problema que resolve | Justificativa |
|---|---|---|---|
| **Repository** | `orders.service.ts`, `menu.service.ts` | Isola o acesso ao banco (Prisma) da lógica de negócio | Se trocar o ORM, muda só o repositório, não o service |
| **Singleton** | `database.ts`, `redis.ts`, `stripe.ts` | Garante instância única de conexões pesadas | Evita múltiplas conexões ao banco em ambiente serverless/Railway |
| **Strategy** | `payments.service.ts` | Diferentes formas de pagamento (Pix, Cartão, Na entrega) com interface única | Adicionar novo método de pagamento sem alterar o controller |
| **Observer** | SSE no `orders.sse.ts` | Notificar o painel do operador quando o status de um pedido muda | Desacopla a atualização de status da notificação em tempo real |
| **Chain of Responsibility** | Middlewares do Express | Autenticação → Validação → Rate Limit → Controller | Cada middleware faz uma coisa só e passa adiante |
| **Factory** | `order-number.ts` | Criação de número de pedido sequencial com lógica isolada | Regra de negócio específica encapsulada fora do service |
| **DTO (Data Transfer Object)** | Schemas Zod nas rotas | Definir exatamente o que entra e sai de cada endpoint | Validação automática + type-safety em todo o fluxo |

---

## Exemplos de Código

### Singleton — Prisma Client

```typescript
// src/config/database.ts
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient }

export const prisma =
  globalForPrisma.prisma ?? new PrismaClient({ log: ['error'] })

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

---

### Strategy — Pagamentos

```typescript
// src/modules/payments/payment.strategy.ts
interface PaymentStrategy {
  process(order: Order): Promise<PaymentResult>
}

class PixStrategy implements PaymentStrategy {
  async process(order: Order) {
    const intent = await stripe.paymentIntents.create({
      amount: Math.round(order.totalPrice * 100),
      currency: 'brl',
      payment_method_types: ['pix'],
    })
    return { qrCode: intent.next_action?.pix_display_qr_code?.data }
  }
}

class CardStrategy implements PaymentStrategy {
  async process(order: Order) {
    const intent = await stripe.paymentIntents.create({
      amount: Math.round(order.totalPrice * 100),
      currency: 'brl',
      payment_method_types: ['card'],
    })
    return { clientSecret: intent.client_secret }
  }
}

class CashOnDeliveryStrategy implements PaymentStrategy {
  async process(order: Order) {
    return { message: 'Pagamento na entrega — sem processamento online' }
  }
}

// Uso no service:
const strategies: Record<PaymentMethod, PaymentStrategy> = {
  PIX: new PixStrategy(),
  CARD: new CardStrategy(),
  CASH_ON_DELIVERY: new CashOnDeliveryStrategy(),
  CARD_ON_DELIVERY: new CashOnDeliveryStrategy(),
}

export function getPaymentStrategy(method: PaymentMethod) {
  return strategies[method]
}
```

---

### Chain of Responsibility — Middlewares

```typescript
// src/app.ts
app.use('/api/admin', rateLimitMiddleware, authMiddleware, adminRoutes)
app.use('/api/orders', rateLimitMiddleware, validateMiddleware(orderSchema), orderRoutes)
app.use('/api/payments/webhook', express.raw({ type: 'application/json' }), webhookRoutes)
```

---

### Observer — SSE para Painel do Operador

```typescript
// src/modules/orders/orders.sse.ts
const clients = new Set<Response>()

export function addSSEClient(res: Response) {
  res.setHeader('Content-Type', 'text/event-stream')
  res.setHeader('Cache-Control', 'no-cache')
  res.setHeader('Connection', 'keep-alive')
  clients.add(res)
  res.on('close', () => clients.delete(res))
}

export function broadcastOrderUpdate(order: Order) {
  const data = JSON.stringify(order)
  clients.forEach(client => client.write(`data: ${data}\n\n`))
}

// Chamado no service ao atualizar status:
// broadcastOrderUpdate(updatedOrder)
```

---

### Factory — Número do Pedido

```typescript
// src/utils/order-number.ts
export async function generateDailyOrderNumber(): Promise<number> {
  const today = new Date()
  today.setHours(0, 0, 0, 0)

  const count = await prisma.order.count({
    where: { createdAt: { gte: today } },
  })

  return count + 1 // #001, #002, #003...
}
```

---

## Antipadrões Evitados

| Antipadrão | Como foi evitado |
|---|---|
| **God Service** — um service faz tudo | Cada módulo tem seu próprio service com responsabilidade única |
| **Lógica no Controller** — regras de negócio no controller | Controllers apenas recebem req e chamam o service |
| **Magic Strings** — strings soltas pelo código | Enums Prisma + constantes TypeScript para status e tipos |
| **Callback Hell** — callbacks aninhados | async/await em toda a base de código |
| **Over-engineering** — abstrações prematuras | Sem repositórios abstratos desnecessários — Prisma é o repositório |
| **Fat client** — toda lógica no frontend | Regras de negócio e validações no backend; frontend é interface |
