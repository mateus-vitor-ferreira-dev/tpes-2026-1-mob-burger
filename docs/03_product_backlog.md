# 03 — Product Backlog

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Épicos

| ID | Épico | Descrição |
|---|---|---|
| EP01 | Cardápio | Exibição de produtos, categorias, fotos e preços |
| EP02 | Pedidos | Carrinho, personalização, finalização de pedido |
| EP03 | Pagamentos | Pix, cartão (Stripe), pagamento na entrega/retirada |
| EP04 | Painel Operador | Gestão de pedidos em tempo real pela cozinha/caixa |
| EP05 | Rastreamento | Status do pedido visível ao cliente |
| EP06 | Notificações | WhatsApp automático para cliente e operador |
| EP07 | Admin | Gestão de cardápio, preços e configurações pelo dono |
| EP08 | Integrações Delivery | iFood, Rappi, Aiqfome |
| EP09 | Impressão | Cupom automático na impressora térmica da cozinha |

---

## Histórias de Usuário

### EP01 — Cardápio

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US01 | Como **cliente**, quero ver o cardápio dividido por categorias (Burgers, Bebidas, Combos) para encontrar o que quero rapidamente | Alta | M |
| US02 | Como **cliente**, quero ver foto, nome, descrição e preço de cada item para decidir o que pedir | Alta | M |
| US03 | Como **cliente**, quero ver quais itens estão disponíveis ou esgotados para não pedir algo indisponível | Alta | P |
| US04 | Como **admin (Murilo)**, quero criar, editar e remover itens do cardápio sem precisar de suporte técnico | Alta | G |
| US05 | Como **admin**, quero ativar/desativar um item temporariamente (sem excluir) quando acabar o estoque | Alta | P |

### EP02 — Pedidos

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US06 | Como **cliente**, quero adicionar itens ao carrinho e ver o total atualizado em tempo real | Alta | M |
| US07 | Como **cliente**, quero personalizar meu burger (ex: sem cebola, ponto da carne) antes de adicionar ao carrinho | Alta | M |
| US08 | Como **cliente**, quero escolher entre **delivery** ou **retirada no balcão** ao finalizar o pedido | Alta | M |
| US09 | Como **cliente** que escolheu delivery, quero informar meu endereço e ver o cálculo do frete | Alta | G |
| US10 | Como **cliente**, quero receber um número de pedido ao confirmar, para referência | Alta | P |

### EP03 — Pagamentos

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US11 | Como **cliente**, quero pagar via Pix e receber o QR Code na tela para copiar e pagar | Alta | G |
| US12 | Como **cliente**, quero pagar com cartão de crédito/débito direto no site | Alta | G |
| US13 | Como **cliente**, quero escolher "pagar na entrega" (dinheiro ou maquininha) ao finalizar o pedido | Alta | P |
| US14 | Como **cliente**, quero receber confirmação automática quando meu pagamento Pix for identificado | Alta | M |

### EP04 — Painel Operador

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US15 | Como **atendente**, quero ver todos os pedidos novos em tempo real, separados por status | Alta | G |
| US16 | Como **atendente**, quero atualizar o status de um pedido (Recebido → Em preparo → Saiu → Entregue) com um clique | Alta | M |
| US17 | Como **atendente**, quero ouvir um alerta sonoro quando um novo pedido chegar | Alta | P |
| US18 | Como **atendente**, quero ver os detalhes completos de cada pedido (itens, personalizações, endereço, forma de pagamento) | Alta | M |

### EP05 — Rastreamento

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US19 | Como **cliente**, quero acompanhar o status do meu pedido em uma página dedicada após finalizar | Alta | M |
| US20 | Como **cliente**, quero receber notificação no WhatsApp quando o status do meu pedido mudar | Alta | M |

### EP06 — Notificações (WhatsApp)

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US21 | Como **cliente**, quero receber mensagem no WhatsApp confirmando que meu pedido foi recebido | Alta | M |
| US22 | Como **cliente**, quero receber mensagem quando meu pedido sair para entrega | Alta | P |
| US23 | Como **Murilo (dono)**, quero receber alerta no WhatsApp quando um novo pedido chegar, caso esteja fora do painel | Média | P |

### EP07 — Admin

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US24 | Como **admin**, quero fazer login com e-mail e senha para acessar o painel administrativo | Alta | P |
| US25 | Como **admin**, quero ver um relatório básico de pedidos do dia (total, faturamento, pedidos por status) | Média | M |
| US26 | Como **admin**, quero configurar horário de funcionamento do delivery | Média | P |
| US27 | Como **admin**, quero definir taxa de entrega por bairro ou raio de distância | Alta | G |

### EP08 — Integrações Delivery (pós-MVP)

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US28 | Como **Murilo**, quero que pedidos do iFood apareçam no painel unificado | Baixa | G |
| US29 | Como **Murilo**, quero que pedidos do Aiqfome apareçam no painel unificado | Baixa | G |

### EP09 — Impressão Térmica

| ID | História | Prioridade | Esforço |
|---|---|---|---|
| US30 | Como **atendente**, quero que o pedido seja impresso automaticamente na impressora da cozinha quando for confirmado | Alta | G |

---

## MVP vs. Futuro

### ✅ MVP (Sprint 1 ao 4)

- US01 a US23 (Cardápio, Pedidos, Pagamentos, Painel, Rastreamento, Notificações WhatsApp)
- US24 a US27 (Admin básico)
- US30 (Impressora térmica)

### 🔜 V2 (pós-lançamento)

- US28, US29 — Integrações iFood e Aiqfome
- Programa de fidelidade (pontos por pedido)
- App mobile nativo (React Native + Expo)
- Relatórios avançados (semana, mês, produto mais vendido)
- Cupons de desconto
