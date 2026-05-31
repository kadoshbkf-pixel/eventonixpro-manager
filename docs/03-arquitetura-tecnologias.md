# AgroSimples — Arquitetura e Tecnologias

## 1. Recomendação resumida (baixo custo → escalável)

| Camada | Tecnologia | Por quê |
|--------|-----------|---------|
| Web/PWA | **Next.js (React)** | SSR/SEO para landing, PWA instalável, um só código web+mobile-web |
| Mobile (Fase 3) | **React Native (Expo)** ou **PWA** | Reaproveita conhecimento React; PWA já cobre 90% das necessidades no início |
| UI | **Tailwind CSS** + componentes próprios | telas limpas, responsivas, rápidas |
| Backend/DB | **Supabase** (PostgreSQL + Auth + Storage + Realtime) | barato, escalável, RLS multi-tenant, pronto p/ produção |
| API | **Supabase auto-API** + **Edge Functions** (Deno) | regras de negócio, integrações, webhooks |
| Offline | **IndexedDB** (app) / `localStorage` (MVP) + fila de sync | funciona sem internet na roça |
| IA | **API de LLM (Claude/OpenAI)** via Edge Function | análises e perguntas; cache em `ai_insights` |
| WhatsApp | **WhatsApp Cloud API** (Meta) ou provedor (Z-API/Twilio) | lembretes e cobranças |
| Pagamentos | **Stripe** ou **Asaas/Pagar.me** (Pix/boleto BR) | assinaturas |
| Backup/Storage | **Supabase Storage** + export JSON/CSV | recibos, comprovantes, backups |
| Hospedagem | **Vercel** (web) + Supabase (infra) | deploy simples, custo inicial baixo |

> **Estado atual deste repositório (MVP funcional):** o app em `agrosimples/index.html` é uma SPA React **single-file** (React via CDN, sem build), com persistência em `localStorage` e **Service Worker** (`sw.js`) + `manifest.webmanifest` para virar PWA instalável e offline. Isso valida o produto sem custo de infraestrutura. A migração para Next.js + Supabase reaproveita 100% da lógica de domínio.

---

## 2. Diagrama de arquitetura (alvo)

```
        ┌───────────────────────────┐
        │  Cliente (PWA / RN app)   │
        │  React + IndexedDB cache  │
        │  Service Worker (offline) │
        └─────────────┬─────────────┘
                      │ HTTPS / Realtime
        ┌─────────────▼─────────────┐
        │         Supabase          │
        │  Auth · PostgreSQL (RLS)  │
        │  Storage · Realtime       │
        │  Edge Functions (Deno)    │
        └───┬───────────┬───────────┘
            │           │
   ┌────────▼──┐   ┌────▼─────────┐
   │ LLM (IA)  │   │ WhatsApp API │
   └───────────┘   └──────────────┘
            │
     ┌──────▼──────┐
     │  Stripe/Asaas (assinaturas)  │
```

---

## 3. Estrutura de pastas (alvo Next.js)

```
agrosimples/
├─ app/                    # rotas (Next.js App Router)
│  ├─ (auth)/login/
│  ├─ (app)/dashboard/
│  ├─ (app)/vendas/
│  ├─ (app)/gastos/
│  ├─ (app)/contas/
│  ├─ (app)/tarefas/
│  ├─ (app)/relatorios/
│  ├─ (app)/assistente/
│  ├─ (app)/plantio/        # Fase 2
│  ├─ (app)/estoque/        # Fase 2
│  ├─ (app)/animais/        # Fase 3
│  └─ (app)/config/
├─ components/             # UI reutilizável (Card, Btn, Field, Modal…)
├─ lib/
│  ├─ supabase.ts          # client
│  ├─ db/                  # queries por módulo
│  ├─ offline/             # fila de sync + IndexedDB
│  ├─ ai/                  # prompts + chamadas LLM
│  └─ domain/              # cálculos (lucro, margem, custo/ha) — puros e testáveis
├─ supabase/
│  ├─ migrations/          # SQL (tabelas, RLS, triggers, views)
│  └─ functions/           # Edge Functions (ai-analyze, wa-reminders, billing-webhook)
├─ public/                 # manifest, ícones, sw.js
└─ tests/
```

---

## 4. Autenticação e segurança
- **Login** por e-mail/senha + **OTP por SMS/WhatsApp** (produtor sem e-mail).
- Senhas com hash (gerenciado pelo Supabase Auth). Recuperação de senha por link/OTP.
- **RLS** isola dados por propriedade; papéis com políticas específicas.
- **LGPD:** consentimento no cadastro, termos de uso e política de privacidade, exportação e exclusão de dados a pedido, dados em região compatível.
- **Backup automático** diário (Supabase) + export manual JSON/CSV pelo usuário.
- **Soft delete** + `audit_log` (histórico de alterações) contra exclusão acidental.

---

## 5. Modo offline (detalhe)
1. App carrega do cache (Service Worker) e do IndexedDB → abre instantâneo, mesmo sem rede.
2. Toda escrita vai primeiro ao armazenamento local com `synced=false`.
3. Indicador “**dados pendentes de sincronização**” aparece no topo.
4. Ao voltar a rede, a **fila de sync** envia as operações em ordem; sucesso marca `synced=true`.
5. Conflitos → last-write-wins por `updated_at`, com aviso quando houver divergência relevante.

---

## 6. Integração com IA (resumo)
- **Edge Function `ai-analyze`** recebe um snapshot agregado do mês (somente números, sem PII desnecessária), monta um prompt de “consultor rural amigável” e devolve insights curtos em português simples.
- Resultados são **cacheados** em `ai_insights` para reduzir custo/latência e funcionar parcialmente offline.
- **Fallback local** (regras): o MVP já implementa análises por regras (`analyze()` em `index.html`), garantindo valor mesmo sem LLM. Detalhes e prompts em [`07-ia-automacoes.md`](07-ia-automacoes.md).

---

## 7. Escalabilidade multi-cliente
- Multi-tenant lógico por `org_id`/`farm_id` (mesma base, isolado por RLS) — barato e simples.
- Plano **Cooperativa** consolida várias `farms` sob uma `organization`, com relatórios comparativos via views materializadas.
- Crescimento: read replicas, particionamento por `farm_id` e cache (CDN/Vercel) quando necessário — sem reescrever o modelo.
