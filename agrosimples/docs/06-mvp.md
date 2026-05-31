# 06 — MVP (Primeira Versão)

O objetivo do MVP é **entregar valor rápido**: em poucos toques o produtor
registra venda e gasto e **vê seu lucro do mês**. Nada além do essencial.

## 1. Escopo do MVP (Fase 1)

| Incluído no MVP | Tela |
|-----------------|------|
| Login + recuperação de senha | 1 |
| Cadastro da propriedade | 3 |
| Cadastro de produtos | 5 |
| Cadastro de clientes | 6 |
| Registro de vendas | 7 |
| Registro de despesas | 8 |
| Contas a pagar | 9 |
| Contas a receber | 10 |
| Agenda de tarefas | 16 |
| Relatório financeiro simples | 17 |
| Dashboard básico | 2 |
| Exportação PDF | 17 |
| **IA básica** (resumo financeiro) | 18 |

### Fora do MVP (vem nas fases seguintes)
- Fase 2: plantio, colheita, estoque, insumos.
- Fase 3: animais, modo offline completo, IA avançada, app mobile nativo,
  relatórios avançados, plano cooperativa.

> Roadmap detalhado em [13 — Roadmap](13-roadmap.md).

## 2. Por que esse escopo

- **Vendas + Despesas + Dashboard** já respondem a pergunta central
  ("estou tendo lucro?"). É o gancho comercial.
- **Clientes + Contas a receber** resolvem a dor do "fiado" (muito comum).
- **Agenda** cria hábito de abrir o app todo dia.
- **IA básica** entrega o "uau" com baixo custo (um resumo por semana).

## 3. IA do MVP (mínima e barata)

Apenas **resumo financeiro** e perguntas simples sobre dados já calculados:
> "Esta semana você vendeu R$ 2.450, gastou R$ 1.300 e teve lucro estimado de
> R$ 1.150. Há 3 contas a pagar e 2 clientes em atraso."

Implementação: enviar à LLM um **resumo agregado** (totais já calculados no
backend), não a base inteira — barato e seguro. Detalhes em
[10 — IA](10-inteligencia-artificial.md).

## 4. Critérios de "pronto" (Definition of Done) do MVP

- [ ] Produtor cria conta e cadastra propriedade em < 3 minutos.
- [ ] Registrar uma venda leva ≤ 4 toques.
- [ ] Registrar um gasto leva ≤ 3 toques.
- [ ] Dashboard mostra **Lucro do mês** correto (vendas − gastos do período).
- [ ] Conta a receber é criada automaticamente em venda "fiado".
- [ ] Relatório financeiro mensal exporta em PDF.
- [ ] Resumo da IA gerado a partir de dados reais.
- [ ] App responsivo e usável em celular simples (tela pequena, 3G).
- [ ] Login seguro, senha com hash, recuperação por e-mail.
- [ ] Funciona como **PWA** (instala na tela inicial do celular).

## 5. Métricas de sucesso do MVP

- **Ativação:** % de contas que registram a 1ª venda em 24h.
- **Hábito:** % que registra algo em ≥ 4 dias na 1ª semana.
- **Aha-moment:** % que abre o dashboard de lucro pelo menos 3x.
- **Conversão:** % que assina ao fim do trial.

## 6. Stack mínima do MVP

- **Frontend:** Next.js (PWA) — web responsivo que instala no celular.
- **Backend/DB/Auth:** Supabase (PostgreSQL + Auth + Storage).
- **IA:** API da Anthropic (Claude) para resumos — usar o modelo mais econômico
  adequado (ex.: Haiku) no MVP.
- **PDF:** geração no cliente (ex.: jsPDF) ou serverless.
- **WhatsApp:** no MVP, **link `wa.me`** (envio manual em 1 toque). Integração via
  API oficial fica para fase posterior.

> Stack completa em [07 — Arquitetura](07-arquitetura-tecnologias.md).
