# Mob Burger — Trabalho Final de Engenharia de Software (TPES 2026/1)

Repositório de documentação do projeto **Mob Burger**, desenvolvido com a metodologia da disciplina de **Tópicos em Engenharia de Software**, com uso de **Scrum** e **entregas incrementais**.

---

## 1. Identificação do projeto

**Nome do projeto:** Mob Burger  
**Problema escolhido:** Hamburguerias locais dependem 100% do iFood e Aiqfome para receber pedidos online, pagando até 30% de comissão por pedido e sem acesso aos dados dos próprios clientes.  
**Turma/Semestre:** 2026/1  
**Docente:** [A DEFINIR]

### Integrantes do grupo

| Nome | Papel no Scrum |
|------|----------------|
| Mateus Vitor Ferreira | Product Owner / Dev Team |

---

## 2. Sobre o Mob Burger

O Mob Burger é um sistema de pedidos online próprio para a hamburgueria **Mob Burger**, localizada em **Lavras/MG**. O cliente acessa o site, monta o pedido, paga pelo site (Pix ou cartão via Stripe) e acompanha o status em tempo real. A cozinha recebe os pedidos em um painel e imprime automaticamente na impressora térmica — sem depender de plataformas externas.

**Repositórios:**
- API: [mob-burger-api](https://github.com/mateus-vitor-ferreira-dev/mob-burger-api)
- Web: [mob-burger-web](https://github.com/mateus-vitor-ferreira-dev/mob-burger-web)

**Stack:**
- Backend: Node.js + Express + Prisma ORM + PostgreSQL
- Frontend: Next.js 15 (App Router) + Tailwind CSS + shadcn/ui
- Pagamentos: Stripe (Pix + Cartão)
- Real-time: Server-Sent Events (SSE)
- Notificações: Z-API (WhatsApp)
- Deploy: Railway (API + BD) + Vercel (Frontend)

---

## 3. Organização do repositório

```text
.
├── README.md
├── docs/
│   ├── 01_problema_e_visao_do_produto.md
│   ├── 02_scrum_e_organizacao_do_grupo.md
│   ├── 03_product_backlog.md
│   ├── 04_requisitos.md
│   ├── 05_modelagem.md
│   ├── 06_arquitetura_e_projeto.md
│   ├── 07_padroes_de_projeto.md
│   ├── 08_testes.md
│   ├── 09_entregas_incrementais.md
│   └── 10_apresentacao_final.md
└── sprints/
    ├── sprint_00.md
    ├── sprint_01.md
    ├── sprint_02.md
    ├── sprint_03.md
    └── sprint_04.md
```

---

## 4. Sprints

| Sprint | Título | Período | Status |
|--------|--------|---------|--------|
| [Sprint 0](sprints/sprint_00.md) | Setup e Infraestrutura | Semana 1 | ✅ Concluída |
| [Sprint 1](sprints/sprint_01.md) | Cardápio + Admin Básico | Semanas 2–3 | 🔄 Em andamento |
| Sprint 2 | Pedidos + Pagamentos | Semanas 4–5 | 📅 Planejada |
| Sprint 3 | Painel do Operador + Rastreamento | Semanas 6–7 | 📅 Planejada |
| Sprint 4 | Notificações + Impressora + QA Final | Semana 8 | 📅 Planejada |

---

## 5. Entregas previstas

| Entrega | Foco principal | Arquivo |
|---------|----------------|---------|
| Entrega 1 | Problema, visão do produto e organização | [`01`](docs/01_problema_e_visao_do_produto.md) · [`02`](docs/02_scrum_e_organizacao_do_grupo.md) |
| Entrega 2 | Backlog e requisitos | [`03`](docs/03_product_backlog.md) · [`04`](docs/04_requisitos.md) |
| Entrega 3 | Modelagem e decisões de projeto | [`05`](docs/05_modelagem.md) |
| Entrega 4 | Arquitetura e padrões | [`06`](docs/06_arquitetura_e_projeto.md) · [`07`](docs/07_padroes_de_projeto.md) |
| Entrega 5 | Estratégia de testes e evidências | [`08`](docs/08_testes.md) |
| Entrega final | Consolidação e apresentação | [`09`](docs/09_entregas_incrementais.md) · [`10`](docs/10_apresentacao_final.md) |

---

## 6. Convenções do repositório

- Branch principal: `main`
- Branches de trabalho: `feat/nome-curto`, `fix/nome-curto`
- Commits: Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:` etc.)
- PR com descrição objetiva antes do merge
