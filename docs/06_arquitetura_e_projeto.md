# 06 — Arquitetura e Projeto

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Estilo Arquitetural

**Monolito modular + Frontend desacoplado**

Justificativa: O projeto tem escopo claro e equipe de um desenvolvedor. Microsserviços adicionariam complexidade operacional desnecessária. O monolito modular (API REST organizada por domínio) permite crescer e extrair serviços no futuro sem reescrever tudo.

---

## Stack Técnica

### Frontend

| Camada | Tecnologia | Motivo | Alternativas descartadas |
|---|---|---|---|
| Framework | Next.js 15 (App Router) | SSR para SEO do cardápio, RSC para performance, ecossistema React | Remix (menor comunidade), Vite SPA (perde SEO) |
| Estilização | Tailwind CSS + shadcn/ui | Velocidade de desenvolvimento, componentes acessíveis | Styled-components (runtime CSS), Chakra UI (mais pesado) |
| State global | Zustand | Simples, sem boilerplate, ideal para carrinho | Redux (verboso), Context API (re-renders) |
| Data fetching | TanStack Query | Cache, revalidação, loading states prontos | SWR (menos features) |
| Formulários | React Hook Form + Zod | Validação type-safe, performance | Formik (mais lento) |
| Real-time | SSE (Server-Sent Events) | Atualização do painel do operador em tempo real, sem a complexidade de WebSocket full-duplex | WebSocket (mais complexo para esse caso unidirecional) |

### Backend

| Camada | Tecnologia | Motivo | Alternativas descartadas |
|---|---|---|---|
| Runtime | Node.js 20 LTS | Familiaridade, ecossistema npm, TypeScript nativo | Bun (menos maduro para produção) |
| Framework | Express.js | Simples, extensível, comunidade enorme | Fastify (mais rápido, mas Express é suficiente), NestJS (pesado demais para esse escopo) |
| Linguagem | TypeScript | Type-safety, DX, menos bugs em runtime | JavaScript puro (sem type-safety) |
| ORM | Prisma | Migrations automáticas, type-safety total, DX excelente | TypeORM (verboso), Drizzle (menos maduro) |
| Validação | Zod | Schemas compartilháveis com frontend, integração com TypeScript | Joi (sem inferência de tipos) |
| Autenticação | JWT (access + refresh tokens) | Controle total, sem vendor lock-in | Clerk (custo para escalar), NextAuth (acoplado ao Next.js) |

### Banco de Dados e Infraestrutura

| Componente | Tecnologia | Motivo |
|---|---|---|
| Banco principal | PostgreSQL 16 | Relacional, transações robustas, suporte nativo no Railway |
| Cache | Redis (Upstash) | Rate limiting, sessões, invalidação de cache do cardápio |
| Deploy API | Railway | Deploy em 1 comando, PostgreSQL e Redis integrados, $5/mês |
| Deploy Frontend | Vercel | CI/CD automático via GitHub, CDN global, gratuito para esse volume |
| DNS + CDN | Cloudflare | DDoS protection gratuita, cache de assets |
| Storage (imagens) | Cloudflare R2 | Custo zero egress, S3-compatible, ideal para imagens de produtos |
| Pagamentos | Stripe | Pix nativo (Brasil), cartão, webhooks confiáveis, PCI compliant |
| WhatsApp | Z-API | API de WhatsApp para notificações, integração com Node.js |
| Impressão | escpos (npm) | Protocolo ESC/POS para impressoras térmicas via rede/USB |

---

## Estrutura de Pastas

### API (mob-burger-api)

```
mob-burger-api/
├── src/
│   ├── config/
│   │   ├── database.ts          # Prisma client singleton
│   │   ├── redis.ts             # Upstash Redis client
│   │   ├── stripe.ts            # Stripe client
│   │   └── env.ts               # Zod env validation
│   ├── constants/
│   │   ├── http-status.ts       # HTTP status codes
│   │   └── order-status.ts      # Status enum helpers
│   ├── middlewares/
│   │   ├── auth.middleware.ts   # JWT validation
│   │   ├── validate.middleware.ts # Zod schema validation
│   │   └── rate-limit.middleware.ts
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.routes.ts
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   └── auth.schema.ts
│   │   ├── menu/
│   │   │   ├── menu.routes.ts
│   │   │   ├── menu.controller.ts
│   │   │   ├── menu.service.ts
│   │   │   └── menu.schema.ts
│   │   ├── orders/
│   │   │   ├── orders.routes.ts
│   │   │   ├── orders.controller.ts
│   │   │   ├── orders.service.ts
│   │   │   ├── orders.schema.ts
│   │   │   └── orders.sse.ts    # Server-Sent Events para painel
│   │   ├── payments/
│   │   │   ├── payments.routes.ts
│   │   │   ├── payments.controller.ts
│   │   │   ├── payments.service.ts
│   │   │   └── webhooks.controller.ts
│   │   ├── notifications/
│   │   │   ├── whatsapp.service.ts  # Z-API integration
│   │   │   └── printer.service.ts   # ESC/POS thermal printer
│   │   └── admin/
│   │       ├── admin.routes.ts
│   │       ├── admin.controller.ts
│   │       └── admin.service.ts
│   ├── utils/
│   │   ├── order-number.ts      # Geração sequencial diária
│   │   ├── format-currency.ts
│   │   └── logger.ts
│   └── app.ts                   # Express setup + rotas
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts                  # Dados iniciais (cardápio base)
├── tests/
│   ├── integration/
│   │   ├── orders.test.ts
│   │   └── payments.test.ts
│   └── unit/
│       ├── order-number.test.ts
│       └── menu.service.test.ts
├── .env.example
├── tsconfig.json
└── package.json
```

### Frontend (mob-burger-web)

```
mob-burger-web/
├── app/
│   ├── (menu)/
│   │   ├── page.tsx             # Cardápio principal (SSR)
│   │   └── [productId]/
│   │       └── page.tsx         # Detalhe do produto
│   ├── pedido/
│   │   ├── carrinho/
│   │   │   └── page.tsx         # Carrinho
│   │   ├── checkout/
│   │   │   └── page.tsx         # Checkout + pagamento
│   │   └── [orderId]/
│   │       └── page.tsx         # Rastreamento do pedido
│   ├── operador/
│   │   └── page.tsx             # Painel do operador (protegido)
│   ├── admin/
│   │   ├── cardapio/
│   │   │   └── page.tsx
│   │   ├── pedidos/
│   │   │   └── page.tsx
│   │   └── configuracoes/
│   │       └── page.tsx
│   ├── login/
│   │   └── page.tsx
│   ├── layout.tsx
│   └── globals.css
├── components/
│   ├── menu/
│   │   ├── CategoryTabs.tsx
│   │   ├── ProductCard.tsx
│   │   └── ProductModal.tsx
│   ├── cart/
│   │   ├── CartDrawer.tsx
│   │   └── CartItem.tsx
│   ├── checkout/
│   │   ├── PaymentForm.tsx
│   │   └── PixQrCode.tsx
│   ├── operator/
│   │   ├── OrderCard.tsx
│   │   └── StatusBadge.tsx
│   └── ui/                      # shadcn/ui components
├── lib/
│   ├── api.ts                   # API client (fetch wrapper)
│   ├── stripe.ts                # Stripe.js setup
│   └── sse.ts                   # SSE client hook
├── store/
│   └── cart.store.ts            # Zustand cart store
├── hooks/
│   ├── useCart.ts
│   └── useOrderStatus.ts
└── package.json
```

---

## Camadas da Aplicação (API)

```
Request HTTP
    │
    ▼
[ Middleware ] → auth, validate, rate-limit
    │
    ▼
[ Controller ] → recebe req, chama service, retorna res
    │
    ▼
[ Service ] → regras de negócio, orquestração
    │
    ├──▶ [ Repository (Prisma) ] → acesso ao banco
    ├──▶ [ Stripe Service ] → pagamentos
    ├──▶ [ WhatsApp Service ] → notificações
    └──▶ [ Printer Service ] → impressão
```

---

## Decisões de Projeto

| Decisão | Escolha | Motivo |
|---|---|---|
| Monolito vs Microsserviços | Monolito modular | Escopo pequeno, 1 dev, evita complexidade operacional desnecessária |
| REST vs GraphQL | REST | Mais simples de implementar e consumir no frontend; GraphQL seria overhead |
| WebSocket vs SSE | SSE | O painel do operador só precisa receber dados (unidirecional) — SSE é suficiente e mais simples |
| Auth própria vs Clerk | JWT próprio | Custo zero, controle total; Clerk ficaria caro com volume |
| Mercado Pago vs Stripe | Stripe | Pix nativo melhor implementado, documentação superior, webhooks mais confiáveis no Brasil |
| Monorepo vs Polirepo | Polirepo | Dois repositórios separados (API + Web) — mais simples para 1 dev |
| R2 vs S3 | Cloudflare R2 | Egress gratuito para imagens de cardápio (muitas requisições de leitura) |

---

## Infraestrutura

```
Internet
    │
    ▼
Cloudflare (DNS + DDoS + Cache de assets)
    │
    ├── Vercel (Frontend Next.js)
    │       └── CDN global → mobile-first, carregamento rápido
    │
    └── Railway (Backend)
            ├── Node.js API (Express)
            ├── PostgreSQL 16
            └── Redis (Upstash)
                        │
                        └── Stripe (Webhooks)
                        └── Z-API (WhatsApp)
                        └── Cloudflare R2 (Imagens)
```

**Estimativa de custo mensal:**
| Serviço | Custo |
|---|---|
| Railway Hobby (API + PostgreSQL) | ~$5–10/mês |
| Vercel Free | $0 |
| Cloudflare Free | $0 |
| Upstash Redis Free tier | $0 |
| Cloudflare R2 (até 10GB) | $0 |
| Z-API (plano básico) | ~R$ 80/mês |
| Stripe (por transação) | 2,9% + R$ 0,30 por pedido pago online |
| **Total fixo estimado** | **~R$ 130/mês** |

> Repassar ao Murilo como parte do plano de manutenção mensal da Codexa.
