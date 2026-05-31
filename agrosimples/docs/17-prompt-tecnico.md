# 17 — Prompt Técnico de Desenvolvimento

Este documento entrega um **prompt pronto** para passar a um programador (ou a
uma IA programadora) começar a desenvolver o AgroSimples, além de um resumo
técnico consolidado. Use junto com os demais documentos desta pasta.

---

## 1. Prompt para a IA programadora (copie e cole)

```
Você é um engenheiro de software sênior. Construa o sistema "AgroSimples —
Gestão Inteligente para Pequenos Produtores", um SaaS multi-tenant simples,
mobile-first e offline-first, voltado a pequenos produtores rurais com pouca
familiaridade com tecnologia.

OBJETIVO DO PRODUTO
Mostrar ao produtor, de forma simples, se a propriedade está dando lucro.
Em poucos toques ele registra vendas e gastos e vê o lucro do mês.

STACK
- Frontend: Next.js + React + Tailwind, como PWA instalável e responsivo.
- Backend/DB/Auth/Storage: Supabase (PostgreSQL com Row Level Security).
- IA: API da Anthropic (Claude), chamada SEMPRE no servidor (Edge Function);
  use o modelo Claude mais recente e econômico adequado a resumos/perguntas.
- Pagamentos: gateway com Pix/cartão (assinaturas).
- Mobile nativo (React Native/Expo) e modo offline completo: fase posterior.

PRINCÍPIOS DE UX
- Linguagem simples ("Registrar gasto", não "lançamento financeiro").
- Poucos botões por tela, ícones grandes, cores suaves, alto contraste.
- Botão de ajuda (?) e exemplo em estado vazio em cada tela.
- Verde = lucro/positivo, vermelho = prejuízo/dívida.

ARQUITETURA DE DADOS (multi-tenant)
- Toda tabela tem id UUID, tenant_id, created_at, updated_at, deleted_at.
- RLS isola dados por tenant_id (produtor só vê sua propriedade; cooperativa vê
  as propriedades do seu tenant).
- Tabelas: tenants, users, properties, areas, machines, employees, plantings,
  harvests, products, stock_items, stock_movements, customers, sales, sale_items,
  finance_entries, animals, animal_health_records, animal_production_records,
  tasks, alerts, ai_interactions, reports, sync_queue, audit_logs.
  (Schema completo no doc 02-banco-de-dados.md.)

REGRA DE OURO DOS CÁLCULOS
O backend é a fonte da verdade dos números (lucro, custos, saldos). A IA apenas
explica em linguagem simples; nunca recalcula. Fórmulas no doc 11-regras-de-
negocio.md (lucro_mes = entradas_pagas − saidas_pagas; custo_por_ha; etc.).

MÓDULOS (entregar por fase)
- Fase 1 (MVP): auth, propriedade, produtos, clientes, vendas, despesas, contas
  a pagar/receber, agenda de tarefas, dashboard de lucro, relatório financeiro +
  PDF, IA básica (resumo financeiro). PWA + cobrança Básico/Intermediário.
- Fase 2: plantio, colheita, estoque, insumos, relatórios por cultura/talhão,
  Excel/CSV, automações de estoque. Plano Premium parcial.
- Fase 3: animais, offline completo, app mobile, IA avançada, dashboard
  completo, alertas inteligentes, backup nuvem, WhatsApp API, plano Cooperativa.

PERMISSÕES
Papéis: administrador, produtor, funcionário (granular), contador (leitura +
exportar), consultor (leitura + sugestões). Matriz no doc 03.

OFFLINE
Registrar venda/gasto/plantio/colheita sem internet: gravar local
(IndexedDB/SQLite) + sync_queue; UUID no cliente; "última escrita vence";
mostrar badge "X dados pendentes de sincronização".

AUTOMAÇÕES (jobs agendados)
Conta vencendo, cliente atrasado, estoque baixo, produto vencendo, vacina
próxima, colheita prevista, tarefa atrasada, resumo semanal, relatório mensal.
Canais: push no app, WhatsApp (wa.me no MVP), e-mail.

SEGURANÇA
Senhas com hash (Supabase Auth), RLS, soft delete, audit_logs, HTTPS, segredos
em variáveis de ambiente, LGPD (termos, privacidade, exportar/excluir dados).

ENTREGUE
1) Migrations SQL (tabelas + RLS).
2) Estrutura de pastas (monorepo: apps/web, packages/core|db|ui|ai, supabase).
3) Telas do MVP (lista no doc 05): login, dashboard, propriedade, produtos,
   clientes, venda, gasto, contas a pagar, contas a receber, agenda, relatórios,
   IA. Responsivas e PWA.
4) Funções de cálculo (lucro, custo/ha, saldos) em packages/core, com testes.
5) Edge Function da IA (recebe resumo agregado, retorna texto simples).
6) Seed de dados de exemplo (1 produtor de hortaliças com vendas e gastos).

Comece pela Fase 1 (MVP). Código limpo, comentários em português, commits
pequenos e descritivos.
```

## 2. Resumo técnico consolidado (one-pager)

| Tema | Decisão |
|------|---------|
| Front | Next.js + React + Tailwind (PWA) |
| Back/DB | Supabase (PostgreSQL + Auth + Storage + Edge Functions) |
| Multi-tenant | `tenant_id` + RLS em todas as tabelas |
| IA | Claude via Edge Function; resumos agregados; cache; planos Premium+ |
| Offline | IndexedDB/SQLite + `sync_queue` (UUID cliente, última escrita vence) |
| WhatsApp | `wa.me` (MVP) → API oficial (fase 3) |
| Pagamentos | Gateway com Pix/cartão (assinaturas) |
| Mobile | React Native/Expo (fase 3) |
| Exportação | PDF (jsPDF) → Excel/CSV (SheetJS) |
| Segurança | Hash de senha, RLS, soft delete, audit_logs, LGPD |
| Hospedagem | Vercel/Netlify (front) + Supabase (back) |

## 3. APIs/endpoints essenciais (esboço)

> Com Supabase, grande parte é PostgREST automático sobre as tabelas + RLS.
> Edge Functions cobrem o que precisa de lógica server-side:

```
POST  /functions/ai-summary        → resumo/pergunta da IA (recebe período)
POST  /functions/generate-report   → gera PDF/Excel e salva no Storage
POST  /functions/run-daily-jobs    → automações diárias (alertas)
POST  /functions/sync              → recebe fila offline e concilia
POST  /functions/whatsapp-send     → envia lembrete/cobrança (fase 3)
POST  /functions/subscribe         → cria/atualiza assinatura (webhook gateway)
```

Operações CRUD (vendas, despesas, plantios, etc.) via PostgREST/SDK do Supabase
com RLS, mais **triggers/funções no banco** para efeitos colaterais (baixa de
estoque, criação de conta a receber, atualização de `actual_yield`).

## 4. Prompts complementares (para detalhar depois)

**Para telas:**
```
Crie o design detalhado de cada tela do AgroSimples (campos, botões, menus,
fluxo, mensagens de erro e sucesso), mobile-first e para produtores com pouca
experiência em tecnologia. Base: docs/05-telas.md e docs/14-identidade-visual.md.
```

**Para o plano técnico aprofundado:**
```
Transforme este projeto em plano técnico: estrutura de pastas, tabelas e
relacionamentos, APIs, autenticação, permissões por usuário, modo offline,
dashboard, relatórios e integração com IA. Base: docs 02, 03, 07, 11 e 17.
```
