# 02 — Processo e Organização

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Metodologia Adotada

**Scrum adaptado (solo dev)** com sprints de 2 semanas.

Justificativa: o projeto tem prazo de 1–2 meses, escopo bem definido e um único desenvolvedor (Codexa). O Scrum adaptado mantém o ritmo de entrega sem a burocracia de cerimônias completas para times grandes.

---

## Papéis

| Papel | Responsável |
|---|---|
| Product Owner | Murilo (define prioridades e valida entregas) |
| Dev Team | Mateus (Codexa) — desenvolvimento full-stack |
| Scrum Master | Mateus (Codexa) — facilitação e remoção de impedimentos |

---

## Cerimônias

| Cerimônia | Frequência | Duração | Formato |
|---|---|---|---|
| Sprint Planning | Início de cada sprint | 1h | Reunião online (Meet/WhatsApp) |
| Sprint Review | Final de cada sprint | 30min | Demo ao vivo para Murilo |
| Retrospectiva | Final de cada sprint | 15min | Async (mensagem com pontos) |
| Check-in semanal | Toda quarta-feira | 15min | WhatsApp — status rápido |

---

## Ferramentas

| Finalidade | Ferramenta |
|---|---|
| Código | GitHub (repositório privado) |
| Gerência de tarefas | Notion (board Kanban) |
| Comunicação | WhatsApp (cliente) + Discord (dev) |
| Design / Protótipo | Figma (telas principais antes de codar) |
| Deploy | Railway (API + DB) + Vercel (Frontend) |
| Pagamentos | Stripe (Pix + Cartão) |
| WhatsApp automático | Z-API |

---

## Convenções de Código

**Branch naming:**
```
main          → produção (sempre estável)
develop       → integração contínua
feat/nome     → nova funcionalidade
fix/nome      → correção de bug
chore/nome    → configuração, deps, infra
```

**Conventional Commits:**
```
feat: adicionar cardápio com categorias
fix: corrigir cálculo de frete
chore: configurar variáveis de ambiente Railway
docs: atualizar README com instruções de setup
```

**PR Policy:**
- Todo código vai para `develop` via Pull Request
- PR requer ao menos 1 revisão própria (checklist de código) antes do merge
- Merge para `main` apenas após testes manuais em staging

---

## Definição de Pronto (DoD)

Uma tarefa está **concluída** quando:

- [ ] Código implementado e funcionando localmente
- [ ] Sem erros de TypeScript (`tsc --noEmit`)
- [ ] Testado manualmente no fluxo principal (happy path)
- [ ] Deploy realizado em staging (Railway/Vercel preview)
- [ ] Validado por Murilo (quando se tratar de funcionalidade visível)
- [ ] Documentação da API atualizada (se rota nova)
