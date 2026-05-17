# 08 — Testes

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Estratégia de Testes

| Tipo | Ferramenta | O que cobre | % estimada |
|---|---|---|---|
| **Unitário** | Vitest | Funções utilitárias, validações Zod, lógica de cálculo | 40% da cobertura |
| **Integração** | Vitest + Supertest | Rotas da API com banco de teste real (PostgreSQL) | 50% da cobertura |
| **Manual** | Checklist antes de deploy | Fluxos completos (cardápio → pedido → pagamento → painel) | Complementar |
| **E2E** | [A DEFINIR — Playwright] | Jornada completa do usuário no browser | Pós-MVP |

**Cobertura alvo no MVP:** 70% nas rotas críticas (pedidos, pagamentos, webhook Stripe)

---

## Casos de Teste

### CT01 — Geração de número de pedido

| Campo | Valor |
|---|---|
| ID | CT01 |
| Funcionalidade | Geração de número sequencial diário |
| Entrada | 3 pedidos já criados hoje |
| Saída esperada | Novo pedido recebe número #004 |
| Resultado | [ ] Pendente |

---

### CT02 — Criação de pedido (happy path)

| Campo | Valor |
|---|---|
| ID | CT02 |
| Funcionalidade | POST /api/orders |
| Entrada | Carrinho válido, endereço, tipo delivery, pagamento Pix |
| Saída esperada | HTTP 201, pedido criado com status `AWAITING_PAYMENT`, QR Code Pix retornado |
| Resultado | [ ] Pendente |

---

### CT03 — Webhook Stripe — Pix confirmado

| Campo | Valor |
|---|---|
| ID | CT03 |
| Funcionalidade | POST /api/payments/webhook (payment_intent.succeeded) |
| Entrada | Evento Stripe com assinatura válida, orderId no metadata |
| Saída esperada | Pedido muda para status `CONFIRMED`, SSE notifica painel, WhatsApp enviado ao cliente |
| Resultado | [ ] Pendente |

---

### CT04 — Webhook Stripe — Assinatura inválida

| Campo | Valor |
|---|---|
| ID | CT04 |
| Funcionalidade | POST /api/payments/webhook |
| Entrada | Evento Stripe com assinatura forjada |
| Saída esperada | HTTP 400, pedido não é alterado |
| Resultado | [ ] Pendente |

---

### CT05 — Atualização de status pelo operador

| Campo | Valor |
|---|---|
| ID | CT05 |
| Funcionalidade | PATCH /api/orders/:id/status |
| Entrada | Token de atendente válido, status `PREPARING` |
| Saída esperada | HTTP 200, status atualizado, SSE broadcast disparado |
| Resultado | [ ] Pendente |

---

### CT06 — Rota admin sem autenticação

| Campo | Valor |
|---|---|
| ID | CT06 |
| Funcionalidade | Segurança — rotas protegidas |
| Entrada | GET /api/admin/products sem header Authorization |
| Saída esperada | HTTP 401 |
| Resultado | [ ] Pendente |

---

### CT07 — Item desativado não aparece no cardápio

| Campo | Valor |
|---|---|
| ID | CT07 |
| Funcionalidade | GET /api/menu |
| Entrada | Produto com `active: false` no banco |
| Saída esperada | Produto não retorna na lista do cardápio público |
| Resultado | [ ] Pendente |

---

### CT08 — Cálculo de frete por bairro

| Campo | Valor |
|---|---|
| ID | CT08 |
| Funcionalidade | Cálculo de taxa de entrega |
| Entrada | Endereço no bairro "Centro" (taxa = R$ 5,00) |
| Saída esperada | Total do pedido inclui R$ 5,00 de frete |
| Resultado | [ ] Pendente |

---

### CT09 — Pedido fora do horário de funcionamento

| Campo | Valor |
|---|---|
| ID | CT09 |
| Funcionalidade | POST /api/orders fora do horário |
| Entrada | Pedido enviado às 03:00 (loja fechada) |
| Saída esperada | HTTP 422 com mensagem "Loja fechada no momento" |
| Resultado | [ ] Pendente |

---

### CT10 — QR Code Pix expirado

| Campo | Valor |
|---|---|
| ID | CT10 |
| Funcionalidade | Expiração de pagamento Pix |
| Entrada | Pedido com `AWAITING_PAYMENT` após 30 minutos |
| Saída esperada | Pedido marcado como `CANCELLED`, cliente pode refazer o pedido |
| Resultado | [ ] Pendente |

---

## Critérios de Aceite Técnicos (Pré-deploy)

- [ ] `tsc --noEmit` sem erros (TypeScript)
- [ ] Todos os testes de integração passando (`vitest run`)
- [ ] Fluxo manual: cardápio → carrinho → checkout Pix → webhook → painel atualizado
- [ ] Fluxo manual: cartão → pagamento confirmado → WhatsApp enviado
- [ ] Painel do operador atualizando em tempo real (SSE)
- [ ] Impressora térmica imprimindo pedido de teste
- [ ] Rotas admin retornando 401 sem token
- [ ] Site responsivo em celular (Chrome DevTools — iPhone 14)

---

## Registro de Bugs

| ID | Descrição | Severidade | Status |
|---|---|---|---|
| — | Nenhum bug registrado ainda | — | — |

> Atualizar durante o desenvolvimento com bugs encontrados nos testes manuais.
