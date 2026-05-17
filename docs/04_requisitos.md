# 04 — Requisitos

**Projeto:** Mob Burger  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17

---

## Como os Requisitos foram Identificados

- Análise do domínio de delivery de food (iFood, Aiqfome como referência de UX)
- Entrevista com o proprietário (Murilo) sobre operação esperada
- Estudo das dores de hamburguerias locais com canal próprio
- Requisitos técnicos de integração: Stripe, Z-API, ESC/POS

---

## Requisitos Funcionais (RF)

| ID | Descrição | Prioridade | História relacionada |
|---|---|---|---|
| RF01 | O sistema deve exibir o cardápio organizado por categorias com foto, nome, descrição e preço | Alta | US01, US02 |
| RF02 | O sistema deve indicar visualmente itens indisponíveis (esgotados/desativados) | Alta | US03 |
| RF03 | O cliente deve poder adicionar itens ao carrinho e personalizar cada item (observações/remoções) | Alta | US06, US07 |
| RF04 | O cliente deve poder escolher entre delivery ou retirada no balcão | Alta | US08 |
| RF05 | Para delivery, o sistema deve capturar o endereço e calcular a taxa de entrega | Alta | US09 |
| RF06 | O sistema deve gerar um número único de pedido ao confirmar | Alta | US10 |
| RF07 | O sistema deve gerar QR Code Pix via Stripe e confirmar pagamento automaticamente via webhook | Alta | US11, US14 |
| RF08 | O sistema deve processar pagamento via cartão (crédito/débito) usando Stripe | Alta | US12 |
| RF09 | O cliente deve poder escolher "pagar na entrega" sem processamento online | Alta | US13 |
| RF10 | O painel do operador deve exibir pedidos em tempo real, separados por status | Alta | US15 |
| RF11 | O operador deve poder atualizar o status de um pedido com um clique | Alta | US16 |
| RF12 | O sistema deve emitir alerta sonoro no painel quando um novo pedido chegar | Alta | US17 |
| RF13 | O cliente deve poder acompanhar o status do pedido em uma página de rastreamento | Alta | US19 |
| RF14 | O sistema deve enviar mensagem WhatsApp automática ao cliente em cada mudança de status | Alta | US20, US21, US22 |
| RF15 | O admin deve poder fazer login com e-mail e senha no painel administrativo | Alta | US24 |
| RF16 | O admin deve poder criar, editar, remover e ativar/desativar itens do cardápio | Alta | US04, US05 |
| RF17 | O admin deve poder configurar taxa de entrega por bairro ou distância | Alta | US27 |
| RF18 | O admin deve poder configurar horário de funcionamento | Média | US26 |
| RF19 | O sistema deve enviar o pedido confirmado para a impressora térmica da cozinha | Alta | US30 |
| RF20 | O admin deve ver resumo do dia (pedidos, faturamento, status) no painel | Média | US25 |

---

## Requisitos Não Funcionais (RNF)

| ID | Categoria | Descrição | Critério de Medição |
|---|---|---|---|
| RNF01 | Performance | O cardápio deve carregar em menos de 2 segundos em conexão 4G | Lighthouse Performance Score ≥ 85 |
| RNF02 | Performance | O painel do operador deve atualizar novos pedidos em tempo real com latência ≤ 3 segundos | Medição via SSE/WebSocket |
| RNF03 | Disponibilidade | O sistema deve estar disponível 99,5% do tempo (≤ 3,6h de downtime/mês) | Uptime Monitor (UptimeRobot) |
| RNF04 | Segurança | Senhas devem ser armazenadas com hash bcrypt (custo ≥ 12) | Code review |
| RNF05 | Segurança | Todas as rotas do admin devem exigir autenticação JWT válida | Testes de endpoint sem token |
| RNF06 | Segurança | Comunicação entre cliente e servidor deve usar HTTPS | Verificação de certificado SSL |
| RNF07 | Segurança | Dados de cartão nunca devem passar pelo servidor — processados direto no Stripe | Conformidade PCI: uso de Stripe.js |
| RNF08 | Usabilidade | O site deve ser 100% responsivo e funcionar em celulares (mobile-first) | Teste manual em Android/iOS |
| RNF09 | Usabilidade | O painel do operador deve funcionar em tablet (tela do caixa/cozinha) | Teste manual em tablet 10" |
| RNF10 | Manutenibilidade | O código deve seguir estrutura modular (routes → controller → service → repository) | Code review |
| RNF11 | Escalabilidade | A API deve suportar ao menos 50 pedidos simultâneos sem degradação | Load test básico com k6 |

---

## Regras de Negócio (RN)

| ID | Regra |
|---|---|
| RN01 | Um pedido só é enviado à cozinha após o pagamento ser confirmado (Pix/cartão) ou após o cliente selecionar "pagar na entrega" |
| RN02 | O status do pedido segue a sequência: `Aguardando pagamento → Confirmado → Em preparo → Pronto → Saiu para entrega → Entregue` (para delivery) ou `Confirmado → Em preparo → Pronto → Retirado` (para balcão) |
| RN03 | Um item desativado pelo admin não aparece no cardápio para o cliente, mas pedidos já realizados com esse item são mantidos |
| RN04 | O delivery só está disponível nos horários configurados pelo admin — fora do horário, o site exibe "fechado" |
| RN05 | A taxa de entrega é calculada com base no bairro informado pelo cliente; se o bairro não estiver cadastrado, o sistema solicita verificação manual |
| RN06 | Pagamentos Pix têm validade de 30 minutos — após esse prazo, o QR Code expira e o cliente deve reiniciar o pedido |
| RN07 | O webhook do Stripe deve validar a assinatura antes de confirmar qualquer pagamento |
| RN08 | O número do pedido é gerado sequencialmente por dia (ex: #001, #002) — reinicia a cada dia |

---

## Rastreabilidade RF → História de Usuário

| Requisito | Histórias |
|---|---|
| RF01 | US01, US02 |
| RF02 | US03 |
| RF03 | US06, US07 |
| RF04 | US08 |
| RF05 | US09 |
| RF06 | US10 |
| RF07 | US11, US14 |
| RF08 | US12 |
| RF09 | US13 |
| RF10 | US15 |
| RF11 | US16 |
| RF12 | US17 |
| RF13 | US19 |
| RF14 | US20, US21, US22 |
| RF15 | US24 |
| RF16 | US04, US05 |
| RF17 | US27 |
| RF18 | US26 |
| RF19 | US30 |
| RF20 | US25 |
