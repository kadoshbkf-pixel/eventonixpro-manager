# AgroSimples — Prompt Técnico de Desenvolvimento e Identidade Visual

## 1. Prompt técnico (para entregar a um dev ou IA programadora)

> **Contexto:** Construir o **AgroSimples**, um SaaS mobile-first para pequenos produtores rurais (agricultura familiar). Lema: “O sistema que mostra se sua propriedade está dando lucro de verdade.” Público com pouca familiaridade tecnológica → simplicidade extrema, linguagem do produtor, offline.
>
> **Stack:** Next.js (App Router) + React + Tailwind; Supabase (Auth, PostgreSQL com RLS, Storage, Edge Functions); PWA com Service Worker + IndexedDB para offline; LLM via Edge Function para o Assistente; WhatsApp Cloud API e Stripe/Asaas (fases pagas).
>
> **Multi-tenant:** `organizations` → `farms` → `memberships(role)`. RLS isola por `farm_id`. Papéis: admin, produtor, funcionário, contador, consultor. Planos: básico, intermediário, premium, cooperativa (gating por `organizations.plan`).
>
> **Modelo de dados:** ver `docs/02-banco-de-dados.md` (tabelas, FKs, triggers: venda fiada→a receber; venda→baixa estoque; colheita→estoque; views de resumo do mês/rankings; soft delete + audit_log).
>
> **Módulos MVP:** propriedade, áreas, produtos, clientes, vendas, gastos, contas a pagar/receber, tarefas, dashboard, relatórios (PDF/CSV), assistente IA (regras + LLM), backup. Fase 2: plantio, colheita, estoque, insumos. Fase 3: animais, offline completo, IA avançada, app nativo, plano cooperativa.
>
> **Regras de negócio:** ver `docs/05-perfis-planos-regras.md` (lucro = vendas−gastos; margem; ponto de equilíbrio; % de perda; custo/ha; alertas).
>
> **UX:** ver `docs/04-telas-ux-fluxos.md` — poucos botões, ícones grandes, linguagem simples, exemplos, botão de ajuda, navegação inferior, estados vazio/offline/sucesso/erro.
>
> **Offline:** escrever primeiro no local (`synced=false`), badge de pendências, fila de sync ao reconectar, last-write-wins por `updated_at`.
>
> **Domínio testável:** isolar cálculos financeiros em `lib/domain` (funções puras) com testes unitários.
>
> **Referência funcional:** o MVP single-file em `agrosimples/index.html` já implementa toda a lógica de domínio (cálculos, regras de venda/estoque/recebível, IA por regras) — use-o como especificação executável ao migrar para Next.js + Supabase.
>
> **Entregue por etapas** seguindo `docs/06-mvp-roadmap.md`, com migrations SQL versionadas e seed de demonstração.

### Prompt complementar — telas
> Crie o design das telas listadas em `docs/04-telas-ux-fluxos.md`. Para cada tela: campos, botões, menus, fluxo, mensagens de erro/sucesso e layout responsivo (celular e PC). Priorize simplicidade para produtores com pouca experiência. Use a identidade visual da seção 2 deste documento.

---

## 2. Identidade visual

### Conceito
Confiança do campo + simplicidade. Verde (terra/folha) como cor principal, branco para limpeza, dourado para destaques (sol/colheita).

### Paleta
| Uso | Cor | Hex |
|-----|-----|-----|
| Verde principal | folha | `#2E7D32` |
| Verde escuro | texto/realce | `#1B5E20` |
| Verde claro | fundos suaves | `#E8F5E9` |
| Fundo geral | off-white | `#F4F7F2` |
| Dourado | destaques/sol | `#F59E0B` |
| Vermelho | gasto/alerta | `#E53935` |
| Azul | informação | `#1976D2` |
| Texto | quase-preto verde | `#1F2A22` |

### Tipografia
**Nunito** (arredondada, amigável, ótima legibilidade) — pesos 700/800/900 para títulos, 600 para texto. Fontes grandes (≥16px).

### Logo e ícone
Símbolo 🌱 (broto) sobre quadrado verde com cantos arredondados. Wordmark “**AgroSimples**” em Nunito ExtraBold; abaixo, o lema em verde-escuro.

### Tom de voz
Próximo, acolhedor, prático. Fala como um vizinho que entende de roça e de números — nunca corporativo.

### Componentes-chave
Cards arredondados (16px) com sombra leve; botões grandes e cheios; pills de status coloridas; ícones emoji para reconhecimento imediato; barra de navegação inferior. (Tudo já materializado em `agrosimples/index.html`.)
