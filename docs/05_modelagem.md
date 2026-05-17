# 05 — Modelagem

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Diagrama de Casos de Uso

```
                        ┌─────────────────────────────────────┐
                        │           Sistema Mob Burger         │
                        │                                      │
   ┌──────────┐         │  ┌─────────────────────────────┐    │
   │  Cliente │─────────┼─▶│ Visualizar cardápio          │    │
   └──────────┘         │  └─────────────────────────────┘    │
         │              │  ┌─────────────────────────────┐    │
         ├──────────────┼─▶│ Montar pedido (carrinho)     │    │
         │              │  └─────────────────────────────┘    │
         ├──────────────┼─▶│ Finalizar pedido (delivery/  │    │
         │              │  │ retirada)                    │    │
         │              │  └─────────────────────────────┘    │
         ├──────────────┼─▶│ Pagar (Pix / Cartão /        │    │
         │              │  │ Na entrega)                  │    │
         │              │  └─────────────────────────────┘    │
         └──────────────┼─▶│ Acompanhar status do pedido  │    │
                        │  └─────────────────────────────┘    │
                        │                                      │
   ┌──────────────┐     │  ┌─────────────────────────────┐    │
   │  Atendente / │─────┼─▶│ Ver novos pedidos (painel)   │    │
   │    Caixa     │     │  └─────────────────────────────┘    │
   └──────────────┘     │  ┌─────────────────────────────┐    │
         │              ├─▶│ Atualizar status do pedido   │    │
         │              │  └─────────────────────────────┘    │
         └──────────────┼─▶│ Imprimir cupom (térmica)     │    │
                        │  └─────────────────────────────┘    │
                        │                                      │
   ┌───────────┐        │  ┌─────────────────────────────┐    │
   │   Admin   │────────┼─▶│ Gerenciar cardápio           │    │
   │  (Murilo) │        │  └─────────────────────────────┘    │
   └───────────┘        │  ┌─────────────────────────────┐    │
         │              ├─▶│ Configurar taxa de entrega   │    │
         │              │  └─────────────────────────────┘    │
         │              │  ┌─────────────────────────────┐    │
         ├──────────────┼─▶│ Configurar horário           │    │
         │              │  └─────────────────────────────┘    │
         └──────────────┼─▶│ Ver relatório do dia          │    │
                        │  └─────────────────────────────┘    │
                        └─────────────────────────────────────┘

   ┌─────────┐
   │ Stripe  │──── Webhook → Confirmar pagamento Pix/Cartão
   └─────────┘

   ┌─────────┐
   │  Z-API  │──── Enviar notificações WhatsApp ao cliente
   └─────────┘
```

---

## Modelo de Dados

### Entidades principais

| Entidade | Descrição | Campos principais |
|---|---|---|
| `User` | Admin/atendente do sistema | id, email, passwordHash, role, createdAt |
| `Category` | Categoria do cardápio | id, name, slug, position, active |
| `Product` | Item do cardápio | id, categoryId, name, description, price, imageUrl, active |
| `ProductOption` | Opções de personalização | id, productId, label, type (checkbox/radio), required |
| `OptionItem` | Itens dentro de uma opção | id, optionId, name, additionalPrice |
| `Order` | Pedido realizado | id, orderNumber, type (delivery/pickup), status, totalPrice, paymentMethod, paymentStatus, createdAt |
| `OrderItem` | Item dentro de um pedido | id, orderId, productId, quantity, unitPrice, observations |
| `OrderItemOption` | Personalização escolhida | id, orderItemId, optionItemId |
| `Customer` | Dados do cliente | id, name, phone, createdAt |
| `DeliveryInfo` | Endereço para delivery | id, orderId, customerId, street, number, neighborhood, complement |
| `DeliveryZone` | Bairros com taxa de entrega | id, name, fee |
| `StoreConfig` | Configurações da loja | id, openingHours, isOpen, whatsappNumber |

---

### Schema Prisma

```prisma
model User {
  id           String   @id @default(cuid())
  email        String   @unique
  passwordHash String
  role         Role     @default(ATTENDANT)
  createdAt    DateTime @default(now())
}

enum Role {
  ADMIN
  ATTENDANT
}

model Category {
  id       String    @id @default(cuid())
  name     String
  slug     String    @unique
  position Int
  active   Boolean   @default(true)
  products Product[]
}

model Product {
  id          String          @id @default(cuid())
  categoryId  String
  category    Category        @relation(fields: [categoryId], references: [id])
  name        String
  description String?
  price       Float
  imageUrl    String?
  active      Boolean         @default(true)
  options     ProductOption[]
  orderItems  OrderItem[]
}

model ProductOption {
  id        String       @id @default(cuid())
  productId String
  product   Product      @relation(fields: [productId], references: [id])
  label     String
  type      OptionType
  required  Boolean      @default(false)
  items     OptionItem[]
}

enum OptionType {
  CHECKBOX
  RADIO
}

model OptionItem {
  id              String              @id @default(cuid())
  optionId        String
  option          ProductOption       @relation(fields: [optionId], references: [id])
  name            String
  additionalPrice Float               @default(0)
  selections      OrderItemOption[]
}

model Customer {
  id        String         @id @default(cuid())
  name      String
  phone     String
  createdAt DateTime       @default(now())
  orders    Order[]
  deliveries DeliveryInfo[]
}

model Order {
  id            String        @id @default(cuid())
  orderNumber   Int
  customerId    String
  customer      Customer      @relation(fields: [customerId], references: [id])
  type          OrderType
  status        OrderStatus   @default(AWAITING_PAYMENT)
  totalPrice    Float
  paymentMethod PaymentMethod
  paymentStatus PaymentStatus @default(PENDING)
  stripePaymentId String?
  items         OrderItem[]
  delivery      DeliveryInfo?
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt
}

enum OrderType {
  DELIVERY
  PICKUP
}

enum OrderStatus {
  AWAITING_PAYMENT
  CONFIRMED
  PREPARING
  READY
  OUT_FOR_DELIVERY
  DELIVERED
  PICKED_UP
  CANCELLED
}

enum PaymentMethod {
  PIX
  CARD
  CASH_ON_DELIVERY
  CARD_ON_DELIVERY
}

enum PaymentStatus {
  PENDING
  PAID
  FAILED
  REFUNDED
}

model OrderItem {
  id           String            @id @default(cuid())
  orderId      String
  order        Order             @relation(fields: [orderId], references: [id])
  productId    String
  product      Product           @relation(fields: [productId], references: [id])
  quantity     Int
  unitPrice    Float
  observations String?
  options      OrderItemOption[]
}

model OrderItemOption {
  id           String     @id @default(cuid())
  orderItemId  String
  orderItem    OrderItem  @relation(fields: [orderItemId], references: [id])
  optionItemId String
  optionItem   OptionItem @relation(fields: [optionItemId], references: [id])
}

model DeliveryInfo {
  id           String   @id @default(cuid())
  orderId      String   @unique
  order        Order    @relation(fields: [orderId], references: [id])
  customerId   String
  customer     Customer @relation(fields: [customerId], references: [id])
  street       String
  number       String
  neighborhood String
  complement   String?
  zoneId       String?
  zone         DeliveryZone? @relation(fields: [zoneId], references: [id])
}

model DeliveryZone {
  id        String         @id @default(cuid())
  name      String
  fee       Float
  deliveries DeliveryInfo[]
}

model StoreConfig {
  id           String  @id @default(cuid())
  isOpen       Boolean @default(true)
  openingHours Json
  whatsappNumber String?
}
```

---

## Diagrama de Componentes

```
┌──────────────────────────────────────────────────────────────┐
│                        CLIENTE (Browser)                      │
│  ┌─────────────────┐  ┌──────────────────┐  ┌─────────────┐ │
│  │  Site Cardápio  │  │  Painel Tracking  │  │  Admin Panel│ │
│  │  (Next.js SSR)  │  │  (Next.js SSR)   │  │  (Next.js)  │ │
│  └────────┬────────┘  └────────┬─────────┘  └──────┬──────┘ │
└───────────┼────────────────────┼───────────────────┼─────────┘
            │                    │                   │
            ▼                    ▼                   ▼
┌──────────────────────────────────────────────────────────────┐
│                      API REST (Node.js + Express)             │
│                                                               │
│  /menu    /orders    /payments    /admin    /webhooks         │
│                                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────────┐  │
│  │  Auth Module │  │ Order Module │  │  Payment Module   │  │
│  └──────────────┘  └──────────────┘  └───────────────────┘  │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────────┐  │
│  │  Menu Module │  │  Admin Module│  │  Notify Module    │  │
│  └──────────────┘  └──────────────┘  └───────────────────┘  │
└──────────────────────────────┬───────────────────────────────┘
                               │
            ┌──────────────────┼──────────────────┐
            ▼                  ▼                  ▼
      ┌──────────┐      ┌──────────┐      ┌──────────────┐
      │PostgreSQL│      │  Stripe  │      │    Z-API     │
      │(Railway) │      │(Payments)│      │  (WhatsApp)  │
      └──────────┘      └──────────┘      └──────────────┘
                                                  
      ┌──────────────────────────────────┐
      │   SSE / WebSocket (Painel        │
      │   Operador — atualizações real   │
      │   time via Server-Sent Events)   │
      └──────────────────────────────────┘
```

---

## Diagrama de Sequência — Fluxo Principal (Pedido com Pix)

```
Cliente        Next.js         API            Stripe         Z-API        Operador
   │               │             │               │               │             │
   │ Acessa site   │             │               │               │             │
   │──────────────▶│             │               │               │             │
   │               │ GET /menu   │               │               │             │
   │               │────────────▶│               │               │             │
   │               │◀────────────│               │               │             │
   │◀──────────────│             │               │               │             │
   │               │             │               │               │             │
   │ Monta pedido  │             │               │               │             │
   │ Clica "Pagar" │             │               │               │             │
   │──────────────▶│             │               │               │             │
   │               │ POST /orders│               │               │             │
   │               │────────────▶│               │               │             │
   │               │             │ Cria ordem    │               │             │
   │               │             │ CreatePayIntent│               │             │
   │               │             │──────────────▶│               │             │
   │               │             │◀──────────────│               │             │
   │               │             │ (QR Code Pix) │               │             │
   │               │◀────────────│               │               │             │
   │◀──────────────│             │               │               │             │
   │ Exibe QR Code │             │               │               │             │
   │               │             │               │               │             │
   │ Paga Pix      │             │               │               │             │
   │──────────────────────────────────────────────▶│             │             │
   │               │             │               │               │             │
   │               │             │◀──── Webhook ─│               │             │
   │               │             │ (payment_intent.succeeded)    │             │
   │               │             │ Confirma pedido│              │             │
   │               │             │ Envia WhatsApp │              │             │
   │               │             │───────────────────────────────▶│            │
   │               │             │               │               │             │
   │               │             │ SSE: novo pedido              │             │
   │               │             │────────────────────────────────────────────▶│
   │               │             │               │               │             │
   │ Redireciona   │             │               │               │             │
   │ para tracking │             │               │               │             │
```

---

## Fluxo de Usuário — Jornada Principal do Cliente

```
1. Acessa mobburger.com.br
        ↓
2. Vê o cardápio (categorias: Burgers, Combos, Bebidas)
        ↓
3. Clica em um burger → Vê detalhes + personalizações
        ↓
4. Escolhe: sem cebola / ponto da carne / adiciona queijo extra
        ↓
5. Adiciona ao carrinho → Continua comprando ou vai ao carrinho
        ↓
6. No carrinho: vê resumo + total
        ↓
7. Escolhe: Delivery ou Retirada no balcão
   [Delivery] → Informa endereço → Vê taxa de entrega calculada
        ↓
8. Informa nome e WhatsApp
        ↓
9. Escolhe forma de pagamento: Pix / Cartão / Pagar na entrega
   [Pix]    → QR Code exibido → Paga no app do banco
   [Cartão] → Formulário Stripe → Confirma
   [Entrega]→ Confirma sem pagamento online
        ↓
10. Pedido confirmado → Número do pedido exibido
        ↓
11. Página de rastreamento: acompanha status em tempo real
        ↓
12. Recebe WhatsApp: "Seu pedido saiu para entrega! 🍔"
        ↓
13. Pedido entregue ✓
```
