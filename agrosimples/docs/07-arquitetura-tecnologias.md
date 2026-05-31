# 07 — Arquitetura e Tecnologias

## 1. Princípios de arquitetura

- **Comece barato, cresça depois.** Usar serviços gerenciados (Supabase) para
  não manter servidores no início.
- **Um só código para web e celular** no começo (PWA), evoluindo para app
  nativo depois.
- **Offline-first** no que importa: registrar venda, gasto, plantio e colheita.
- **Multi-tenant** desde o dia 1 (preparado para cooperativas).

## 2. Stack recomendada

| Camada | Tecnologia recomendada | Por quê |
|--------|------------------------|---------|
| **Frontend Web/PWA** | **Next.js + React** | Web responsivo, instalável, SSR, ecossistema enorme |
| **UI** | Tailwind CSS + componentes simples | Telas limpas e rápidas |
| **Backend / API** | **Supabase** (PostgREST + Edge Functions) e/ou **Node.js** | Pouca infra, escala bem |
| **Banco de dados** | **PostgreSQL** (no Supabase) | Robusto, relacional, RLS para multi-tenant |
| **Autenticação** | Supabase Auth | Login, recuperação de senha, JWT |
| **Storage** | Supabase Storage | Recibos, comprovantes, exportações |
| **IA** | API da Anthropic (Claude) | Assistente AgroSimples |
| **App mobile (fase 3)** | **React Native (Expo)** | Reaproveita conhecimento React; offline robusto |
| **Offline** | IndexedDB (PWA) / SQLite (RN) + fila de sync | Não perder dados sem internet |
| **WhatsApp** | `wa.me` (MVP) → API oficial / provedor (fase 2+) | Lembretes e cobranças |
| **Notificações** | Web Push (PWA) / FCM (mobile) | Alertas e tarefas |
| **PDF/Excel** | jsPDF / SheetJS ou serverless | Exportar relatórios |
| **Hospedagem** | Vercel/Netlify (front) + Supabase (back) | Deploy simples |
| **Pagamentos (assinatura)** | Stripe ou gateway nacional (Pix) | Cobrança dos planos |

> **Decisão Supabase × Firebase:** recomendado **Supabase/PostgreSQL** por ser
> relacional (ideal para finanças, relatórios e relacionamentos fortes deste
> domínio) e por suportar RLS multi-tenant. Firebase seria alternativa, mas o
> modelo relacional encaixa melhor aqui.

## 3. Visão de arquitetura

```
┌──────────────────────────────────────────────────────────┐
│                    CLIENTES                                │
│  PWA (Next.js)  ·  App RN (fase 3)  ·  Web desktop         │
│  └ Cache offline local (IndexedDB/SQLite) + fila de sync   │
└───────────────┬───────────────────────────────┬───────────┘
                │ HTTPS / REST / Realtime         │ Web Push
        ┌───────▼────────────┐         ┌──────────▼─────────┐
        │   SUPABASE         │         │  NOTIFICAÇÕES      │
        │  Auth · PostgREST  │         │  Web Push / FCM    │
        │  Edge Functions    │         └────────────────────┘
        │  PostgreSQL (RLS)  │
        │  Storage           │
        └───────┬────────────┘
                │ chamadas seguras (server-side)
     ┌──────────┼───────────────┬──────────────────┐
┌────▼─────┐ ┌──▼───────────┐ ┌─▼──────────────┐ ┌─▼────────────┐
│ IA Claude│ │ WhatsApp API │ │ Gateway Pagto  │ │ Backups nuvem│
│ (Anthropic)│ (provedor)  │ │ (Stripe/Pix)   │ │ (automáticos)│
└──────────┘ └─────────────┘ └────────────────┘ └──────────────┘
```

## 4. Modo offline

**Requisitos:** registrar venda, despesa, plantio e colheita **sem internet** e
sincronizar quando voltar, sem perder dados.

**Estratégia:**
1. **Banco local** no dispositivo (IndexedDB no PWA; SQLite no RN).
2. Toda operação offline grava local + entra numa **fila de sincronização**
   (`sync_queue`): `{ entity, operation, payload, client_created_at, device_id }`.
3. Ao voltar a conexão, a fila é enviada em ordem ao backend.
4. **Resolução de conflitos:** "última escrita vence" por padrão; em campos
   sensíveis (financeiro), usar `client_created_at` + `updated_at` do servidor.
5. **IDs gerados no cliente** (UUID) para evitar colisão ao sincronizar.
6. **Indicador visível:** badge "X dados pendentes de sincronização" e ✅ quando
   tudo sincroniza. Avisar o produtor; nunca apagar dado não sincronizado.

## 5. Integração com IA

- Chamadas à API da Anthropic feitas **no servidor** (Edge Function), nunca
  expondo a chave no cliente.
- Para reduzir custo e proteger dados: enviar **resumos agregados** (totais já
  calculados), não a base bruta.
- Cache de respostas para perguntas repetidas (ex.: "lucro do mês").
- Ver [10 — Inteligência Artificial](10-inteligencia-artificial.md).

## 6. Backup e segurança

- **Backup automático** diário do PostgreSQL (Supabase) + exportação semanal.
- **RLS** isola dados por `tenant_id`.
- Senhas com **hash** (gerenciado pelo Auth).
- **Soft delete** + `audit_logs` (histórico de alterações).
- **LGPD básica:** termos de uso, política de privacidade, exportação e
  exclusão de dados a pedido do titular.
- HTTPS em tudo; chaves e segredos em variáveis de ambiente.

## 7. Escalabilidade para muitos clientes

- Multi-tenant com RLS → milhares de produtores no mesmo banco com isolamento.
- Índices nas colunas `tenant_id`, `property_id`, `due_date`, `sold_at`.
- Relatórios pesados via **views materializadas** ou jobs agendados.
- Funções serverless escalam automaticamente para picos (ex.: fim do mês).

## 8. Estrutura de pastas sugerida (monorepo)

```
agrosimples/
├─ apps/
│  ├─ web/                 # Next.js (PWA) — web + mobile-first
│  └─ mobile/              # React Native (Expo) — fase 3
├─ packages/
│  ├─ core/                # regras de negócio, cálculos (lucro, custos)
│  ├─ db/                  # schema, migrations, tipos
│  ├─ ui/                  # componentes compartilhados
│  └─ ai/                  # prompts e cliente da IA
├─ supabase/
│  ├─ migrations/          # SQL das tabelas e RLS
│  └─ functions/           # Edge Functions (IA, WhatsApp, relatórios)
└─ docs/                   # esta documentação
```
