# 01 — Problema e Visão do Produto

**Projeto:** Mob Burger  
**Cliente:** Murilo (proprietário)  
**Desenvolvido por:** Codexa  
**Data:** 2026-05-17  
**Autor:** Mateus Vitor Ferreira

---

## O Problema

Hamburguerias locais em cidades como Lavras/MG dependem quase exclusivamente de plataformas de delivery como iFood, Rappi e Aiqfome para receber pedidos online. Esse modelo traz três dores reais:

1. **Alta comissão:** as plataformas cobram entre 12% e 30% por pedido — o que corrói diretamente a margem do negócio.
2. **Sem relacionamento com o cliente:** o dono não tem acesso aos dados dos clientes (nome, histórico, preferências). O cliente é da plataforma, não da hamburgueria.
3. **Dependência total:** se o negócio sair da plataforma ou for penalizado por avaliações ruins, perde toda a operação de delivery.

Como resolvem hoje — e por que é ruim:
- Dependem do iFood/Aiqfome → pagam comissão alta em cada pedido, sem canal próprio.
- Recebem pedidos por WhatsApp manual → sem controle, sem histórico, confuso na operação.
- Não têm painel para a cozinha → anotações em papel, erro humano frequente.

---

## A Solução

A Mob Burger terá seu próprio sistema de pedidos online — um canal direto com o cliente, sem intermediários e sem comissão por pedido.

O cliente acessa o site da Mob Burger, monta o pedido, paga online (Pix ou cartão via Stripe) ou escolhe pagar na entrega, e acompanha o status em tempo real. A cozinha recebe o pedido automaticamente, imprime o cupom e atualiza o status com um clique.

---

## Visão do Produto

> *"Para moradores de Lavras/MG que querem pedir burgers artesanais de qualidade sem depender de apps com taxas abusivas, a Mob Burger é um sistema de pedidos próprio que oferece uma experiência direta, rápida e sem comissão — diferente do iFood e Aiqfome, que ficam com até 30% de cada pedido e não permitem que o dono conheça seus clientes."*

---

## Público-Alvo

| Persona | Descrição | Dor principal |
|---|---|---|
| **Cliente final** | Morador de Lavras/MG, 18–40 anos, pede delivery pelo celular | Quer praticidade e pagar pelo que vale, sem taxas extras |
| **Atendente / Caixa** | Funcionário que gerencia os pedidos no dia a dia | Quer clareza sobre o que foi pedido e em que status está |
| **Murilo (dono)** | Proprietário, gerencia cardápio, preços e relatórios | Quer controle total do negócio sem depender de plataformas |

---

## Objetivos do Produto

1. **Reduzir dependência do iFood/Aiqfome** — ter ao menos 30% dos pedidos pelo canal próprio em 3 meses após o lançamento
2. **Zerar comissão nos pedidos online diretos** — economia direta na margem por pedido
3. **Ter controle operacional real** — painel que substitui anotações manuais na cozinha
4. **Construir base de clientes própria** — histórico de pedidos, dados do cliente, fidelização
5. **Lançar MVP funcional em até 2 meses** — hamburgueria abre e já tem canal digital próprio

---

## O que NÃO é o produto (MVP)

- Não é um app mobile nativo (iOS/Android) — o site será responsivo e funciona como PWA
- Não é um sistema de fidelidade com pontos — fica para uma v2
- Não é um ERP completo — sem controle de estoque, fluxo de caixa ou folha de pagamento
- Não faz gestão de redes sociais ou tráfego pago

---

## Critérios de Sucesso

| Critério | Métrica | Prazo |
|---|---|---|
| Canal próprio ativo | Sistema no ar antes da abertura da hamburgueria | Mês 2 |
| Pedidos diretos | 30% dos pedidos via canal próprio | Mês 3 pós-lançamento |
| Operação sem papel | 100% dos pedidos gerenciados pelo painel | Dia 1 |
| Economia em comissão | Redução de pelo menos R$ 500/mês em comissões | Mês 3 |
| Satisfação do dono | Murilo consegue operar o sistema sem suporte técnico | Mês 1 |
