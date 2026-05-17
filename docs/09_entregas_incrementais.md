# 09 — Entregas Incrementais

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17  
**Prazo total:** 8 semanas (2 meses)

---

## Cronograma de Sprints

| Sprint | Período | Objetivo | Status |
|---|---|---|---|
| Sprint 0 | Semana 1 | Setup e infraestrutura | Planejado |
| Sprint 1 | Semanas 2–3 | Cardápio + Admin básico | Planejado |
| Sprint 2 | Semanas 4–5 | Pedidos + Pagamentos | Planejado |
| Sprint 3 | Semanas 6–7 | Painel do Operador + Rastreamento | Planejado |
| Sprint 4 | Semana 8 | Notificações + Impressora + QA final | Planejado |

---

## Sprint 0 — Setup e Infraestrutura
**Período:** Semana 1  
**Meta:** Ambiente de desenvolvimento e produção prontos para codar

### Tarefas
- [ ] Criar repositórios GitHub (`mob-burger-api`, `mob-burger-web`)
- [ ] Configurar projeto Railway (API + PostgreSQL)
- [ ] Configurar projeto Vercel (Frontend)
- [ ] Configurar Cloudflare DNS + R2 (storage de imagens)
- [ ] Setup inicial: `mob-burger-api` com Express + Prisma + TypeScript
- [ ] Setup inicial: `mob-burger-web` com Next.js 15 + Tailwind + shadcn/ui
- [ ] Configurar variáveis de ambiente (`.env.example` documentado)
- [ ] Rodar primeira migration do Prisma com schema inicial
- [ ] Configurar conta Stripe + Z-API + endpoint de webhook local (Stripe CLI)
- [ ] Deploy em staging funcionando (mesmo que vazio)

### Definição de Pronto
- API retorna `200` em `GET /health`
- Frontend sobe em Vercel sem erros
- Banco migrado com schema completo

---

## Sprint 1 — Cardápio + Admin Básico
**Período:** Semanas 2–3  
**Meta:** Murilo consegue cadastrar o cardápio e o cliente consegue visualizá-lo

### Histórias planejadas
| ID | História | Esforço |
|---|---|---|
| US01 | Cardápio com categorias | M |
| US02 | Foto, nome, descrição e preço | M |
| US03 | Itens indisponíveis | P |
| US04 | Admin — CRUD de produtos | G |
| US05 | Admin — ativar/desativar item | P |
| US24 | Admin — login com e-mail e senha | P |

### Artefatos entregues
- Página do cardápio ao vivo (`mobburger.com.br`)
- Painel admin com login e gestão de produtos
- Upload de imagens funcionando (Cloudflare R2)
- Seed com cardápio inicial da Mob Burger

### Validação com Murilo
- Demo: Murilo faz login, cadastra um burger com foto, vê no site

---

## Sprint 2 — Pedidos + Pagamentos
**Período:** Semanas 4–5  
**Meta:** Cliente consegue montar um pedido e pagar (Pix ou cartão)

### Histórias planejadas
| ID | História | Esforço |
|---|---|---|
| US06 | Carrinho com total | M |
| US07 | Personalização de itens | M |
| US08 | Delivery ou retirada | M |
| US09 | Endereço + cálculo de frete | G |
| US10 | Número de pedido | P |
| US11 | Pagamento Pix + QR Code | G |
| US12 | Pagamento Cartão (Stripe) | G |
| US13 | Pagar na entrega | P |
| US14 | Confirmação automática Pix (webhook) | M |
| US27 | Admin — configurar taxa de entrega por bairro | G |

### Artefatos entregues
- Fluxo completo: carrinho → checkout → pagamento → confirmação
- Webhook Stripe funcionando (Pix + Cartão)
- Página de confirmação com número do pedido

### Validação com Murilo
- Demo: pedido de teste com Pix real + pedido com cartão de teste

---

## Sprint 3 — Painel do Operador + Rastreamento
**Período:** Semanas 6–7  
**Meta:** Atendente gerencia pedidos em tempo real; cliente acompanha o status

### Histórias planejadas
| ID | História | Esforço |
|---|---|---|
| US15 | Painel — ver pedidos em tempo real | G |
| US16 | Painel — atualizar status | M |
| US17 | Painel — alerta sonoro | P |
| US18 | Painel — detalhes do pedido | M |
| US19 | Rastreamento pelo cliente | M |
| US25 | Admin — relatório do dia | M |
| US26 | Admin — horário de funcionamento | P |

### Artefatos entregues
- Painel do operador com SSE (tempo real)
- Página de rastreamento do cliente
- Dashboard básico do admin

### Validação com Murilo
- Demo: pedido criado → painel atualiza instantaneamente → status muda → cliente vê no tracking

---

## Sprint 4 — Notificações + Impressora + QA Final
**Período:** Semana 8  
**Meta:** Sistema pronto para produção com todas as integrações e sem bugs críticos

### Histórias planejadas
| ID | História | Esforço |
|---|---|---|
| US21 | WhatsApp — confirmação de pedido | M |
| US22 | WhatsApp — saiu para entrega | P |
| US23 | WhatsApp — alerta para Murilo | P |
| US30 | Impressora térmica | G |

### Tarefas adicionais
- [ ] Testes de integração (CT01 a CT10)
- [ ] Checklist de segurança (JWT, HTTPS, webhook signature)
- [ ] Teste de carga básico (k6 — 50 pedidos simultâneos)
- [ ] Revisão de responsividade mobile
- [ ] Documentação de variáveis de ambiente para Murilo
- [ ] Treinamento de 1h com Murilo e atendente no sistema

### Definição de MVP Pronto
- [ ] Fluxo completo funciona em produção (não só em staging)
- [ ] Murilo consegue operar sem suporte técnico
- [ ] Impressora térmica imprime pedidos automaticamente
- [ ] WhatsApp enviando mensagens corretamente
- [ ] Nenhum bug crítico em aberto

---

## Riscos e Mitigações

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| Impressora térmica com driver incompatível | Média | Alto | Testar protocolo ESC/POS na semana 1 com o modelo que Murilo tem |
| Webhook Stripe com delay (Pix pode ser lento) | Baixa | Médio | Implementar polling de backup + expiração de 30min |
| Z-API instável ou número banido | Média | Médio | Sistema funciona sem WhatsApp — é complemento, não bloqueio |
| Aiqfome sem API pública disponível | Alta | Baixo | Integração Aiqfome vai para V2; iFood tem API mais documentada |
| Murilo não conseguir operar o admin | Baixa | Alto | Treinamento de 1h + vídeo gravado + guia em PDF |
| Prazo estourar na Sprint 2 (pagamentos são complexos) | Média | Alto | Começar pelo Stripe no início da sprint, não deixar para o final |

---

## Critérios de MVP

O sistema pode ser lançado quando:
- [ ] Cardápio visível e atualizado pelo admin
- [ ] Pedidos online funcionando (Pix + Cartão + Pagar na entrega)
- [ ] Painel do operador atualizando em tempo real
- [ ] Rastreamento do pedido funcionando
- [ ] WhatsApp enviando confirmação
- [ ] Impressora térmica imprimindo pedidos
- [ ] Sistema no ar no domínio da Mob Burger com HTTPS
