# Sprint 0 — Setup e Infraestrutura

## Objetivo da Sprint

Ter os dois repositórios criados, ambiente de desenvolvimento funcionando e infraestrutura de produção configurada, antes de escrever qualquer funcionalidade.

## Período

Semana 1 — 2026-05-17 a 2026-05-24

---

## Sprint Backlog

| ID | Tarefa | Responsável | Status |
|----|--------|-------------|--------|
| T01 | Criar repositórios GitHub (`mob-burger-api`, `mob-burger-web`) | Mateus | ✅ Concluído |
| T02 | Setup `mob-burger-api`: Express + Prisma + TypeScript | Mateus | ✅ Concluído |
| T03 | Setup `mob-burger-web`: Next.js 15 + Tailwind + shadcn/ui | Mateus | ✅ Concluído |
| T04 | Schema Prisma completo com todas as entidades | Mateus | ✅ Concluído |
| T05 | Configurar `.env.example` documentado | Mateus | ✅ Concluído |
| T06 | Seed inicial com cardápio, bairros e config da loja | Mateus | ✅ Concluído |
| T07 | Estrutura de módulos (auth, menu, orders, payments, admin) | Mateus | ✅ Concluído |
| T08 | SSE para painel do operador em tempo real | Mateus | ✅ Concluído |
| T09 | Cliente HTTP (`src/lib/api.ts`) no frontend | Mateus | ✅ Concluído |
| T10 | Criar repositório `tpes-2026-1-mob-burger` com 10 docs | Mateus | ✅ Concluído |

---

## O que foi entregue

- `mob-burger-api` com Express + TypeScript + Prisma — estrutura modular completa
- Schema Prisma com 13 modelos: User, Category, Product, ProductOption, OptionItem, Customer, Order, OrderItem, OrderItemOption, DeliveryInfo, DeliveryZone, StoreConfig
- Módulos implementados: auth (JWT), menu, orders (+ SSE), payments (Stripe + webhook), admin
- `mob-burger-web` com Next.js 15 App Router + Tailwind + shadcn/ui + Zustand + TanStack Query
- Seed com cardápio inicial (Mob Classic, Mob Smash, Combo, Bebidas) e bairros de entrega de Lavras/MG

**Repositórios:**
- API: [mob-burger-api](https://github.com/mateus-vitor-ferreira-dev/mob-burger-api)
- Web: [mob-burger-web](https://github.com/mateus-vitor-ferreira-dev/mob-burger-web)

---

## Decisões técnicas

| Decisão | Escolha | Motivo |
|---------|---------|--------|
| Real-time | SSE (Server-Sent Events) | Painel do operador só precisa receber dados — SSE é suficiente sem WebSocket |
| Webhook Stripe | Body raw antes do json parser | Stripe exige o body cru para validar a assinatura |
| Seed | Dados reais de Lavras/MG | Bairros e cardápio baseados no contexto real da hamburgueria |

---

## Impedimentos / Observações

- Railway e Vercel precisam ser configurados manualmente pelo Murilo (requer conta e cartão)
- Z-API (WhatsApp) será configurada na Sprint 4
- iFood e Aiqfome ficaram fora do MVP — sem API pública documentada do Aiqfome
