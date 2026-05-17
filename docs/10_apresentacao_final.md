# 10 — Pitch do Projeto

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Contexto e Problema (2 min)

**O cenário:**
Hamburguerias locais em Lavras/MG dependem do iFood e Aiqfome para receber pedidos online. Mas esse modelo tem um custo alto: até 30% de comissão por pedido, sem acesso aos dados dos clientes e sem controle da operação.

**A dor real:**
- Murilo quer abrir a Mob Burger com identidade própria, sem ser mais um restaurante no meio de dezenas no iFood.
- Sem um canal direto, cada pedido online representa margem perdida.
- A operação na cozinha hoje é manual: papel e caneta, chance de erro, sem histórico.

---

## Solução e Visão do Produto (2 min)

**O que entregamos:**
Um sistema de pedidos próprio para a Mob Burger — o cliente acessa `mobburger.com.br`, monta o pedido, paga pelo site e acompanha em tempo real.

**O diferencial:**
- Sem comissão por pedido (apenas custo fixo de ~R$ 130/mês de infraestrutura)
- Painel para a cozinha: pedidos chegam em tempo real, atualizados com 1 clique
- Impressora térmica: o pedido é impresso automaticamente ao chegar
- WhatsApp: cliente recebe atualizações sem depender de app

**Stack:**
- Frontend: Next.js 15 + Tailwind + shadcn/ui (Vercel)
- Backend: Node.js + Express + Prisma + PostgreSQL (Railway)
- Pagamentos: Stripe (Pix + Cartão)
- Notificações: Z-API (WhatsApp)
- Real-time: Server-Sent Events

---

## Demo / Protótipo (3 min)

**Roteiro da demo:**
1. Abrir `mobburger.com.br` no celular
2. Navegar pelas categorias (Burgers, Combos, Bebidas)
3. Montar um pedido com personalização (sem cebola, queijo extra)
4. Selecionar delivery + endereço → ver frete calculado
5. Pagar com Pix → mostrar QR Code
6. Abrir painel do operador → novo pedido aparece em tempo real
7. Imprimir cupom na térmica
8. Atualizar status → cliente vê no tracking
9. WhatsApp chegando no celular do cliente

---

## Decisões Técnicas Principais (2 min)

| Decisão | Escolha | Por quê |
|---|---|---|
| Pagamentos | Stripe (não Mercado Pago) | Webhooks mais confiáveis, Pix nativo melhor implementado |
| Real-time | SSE (não WebSocket) | Painel só precisa receber dados — SSE é suficiente e mais simples |
| Sem app mobile | Site responsivo (PWA) | Mesmo usuário final, zero custo de App Store, entrega em 2 meses |
| Integrações delivery | iFood/Aiqfome fora do MVP | API do Aiqfome sem documentação pública; deixar para V2 |

---

## Resultados e Próximos Passos (1 min)

**Resultado esperado no lançamento:**
- Sistema no ar no dia de abertura da Mob Burger
- Murilo opera sem suporte técnico
- Primeiros pedidos pelo canal próprio nas primeiras semanas

**Próximos passos pós-MVP:**
- Integração Aiqfome e iFood (painel unificado)
- Programa de fidelidade (pontos por pedido)
- App mobile (React Native + Expo)
- Relatórios avançados (semana, mês, produtos mais vendidos)

---

## Checklist de Apresentação

- [ ] Demo ao vivo funciona em produção (não em localhost)
- [ ] Celular com pedido de teste feito antes da apresentação
- [ ] Painel do operador aberto em um segundo dispositivo (tablet/notebook)
- [ ] Impressora térmica ligada e conectada
- [ ] WhatsApp chegando no número de teste
- [ ] Slides com diagrama de arquitetura preparados
- [ ] Contrato de manutenção mensal pronto para assinar
