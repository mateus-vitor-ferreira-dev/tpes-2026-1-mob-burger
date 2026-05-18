# Mob Burger — Sistema de Pedidos Online

Repositório de documentação do projeto **Mob Burger**, desenvolvido pela **[Codexa](https://github.com/mateus-vitor-ferreira-dev)**.

---

## Sobre o projeto

O Mob Burger é um sistema de pedidos online próprio para a hamburgueria **Mob Burger**, localizada em **Lavras/MG**.

**Problema que resolve:** hamburguerias locais dependem 100% do iFood e Aiqfome para receber pedidos online, pagando até 30% de comissão por pedido e sem acesso aos dados dos próprios clientes.

**Solução:** o cliente acessa o site, monta o pedido, paga pelo site (Pix ou cartão via Stripe) e acompanha o status em tempo real. A cozinha recebe os pedidos em um painel e imprime automaticamente na impressora térmica — sem depender de plataformas externas.

---

## Repositórios

| Repositório | Descrição |
|-------------|-----------|
| [mob-burger-api](https://github.com/mateus-vitor-ferreira-dev/mob-burger-api) | API REST + WebSocket |
| [mob-burger-web](https://github.com/mateus-vitor-ferreira-dev/mob-burger-web) | Aplicação web (cliente + painel) |

---

## Stack

| Camada | Tecnologia |
|--------|------------|
| Backend | Node.js + Express v5 + Prisma v6 + PostgreSQL + TypeScript |
| Frontend | Next.js 16 (App Router) + React 19 + TypeScript + Tailwind CSS v4 + shadcn/ui |
| Estado / Cache | Zustand + TanStack Query |
| Formulários | React Hook Form + Zod |
| Pagamentos | Stripe (Pix + Cartão) |
| Real-time | Server-Sent Events (SSE) |
| Notificações | Z-API (WhatsApp) |
| Deploy | Railway (API + BD) + Vercel (Frontend) |

---

## Organização do repositório

```text
.
├── README.md
├── docs/
│   ├── 01_problema_e_visao_do_produto.md
│   ├── 02_organizacao_do_projeto.md
│   ├── 03_product_backlog.md
│   ├── 04_requisitos.md
│   ├── 05_modelagem.md
│   ├── 06_arquitetura_e_projeto.md
│   ├── 07_padroes_de_projeto.md
│   ├── 08_testes.md
│   └── 09_entregas_incrementais.md
└── sprints/
    ├── sprint_00.md
    ├── sprint_01.md
    ├── sprint_02.md
    ├── sprint_03.md
    └── sprint_04.md
```

---

## Roadmap de sprints

| Sprint | Título | Status |
|--------|--------|--------|
| [Sprint 0](sprints/sprint_00.md) | Setup e Infraestrutura | ✅ Concluída |
| [Sprint 1](sprints/sprint_01.md) | Cardápio + Admin Básico | 🔄 Em andamento |
| Sprint 2 | Pedidos + Pagamentos | 📅 Planejada |
| Sprint 3 | Painel do Operador + Rastreamento | 📅 Planejada |
| Sprint 4 | Notificações + Impressora + QA Final | 📅 Planejada |

---

## Desenvolvido por

**Codexa** — Mateus Vitor Ferreira  
[github.com/mateus-vitor-ferreira-dev](https://github.com/mateus-vitor-ferreira-dev)
