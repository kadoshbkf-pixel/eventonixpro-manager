# AgroSimples — MVP e Roadmap

## 1. MVP recomendado (Fase 1) — “ver o lucro hoje”

Objetivo: entregar valor em minutos, validar o produto com baixo custo.

**Inclui:**
- Login (ou uso local, no MVP single-file atual)
- Onboarding em 1 tela (propriedade + produtor)
- Cadastro de propriedade, áreas, produtos e clientes
- **Registro de vendas** (baixa estoque + gera “a receber” quando fiado)
- **Registro de gastos**
- **Contas a pagar e a receber** (marcar pago/recebido, alerta de atraso)
- **Agenda de tarefas** rurais
- **Dashboard** (lucro do mês, a pagar/receber, avisos, tarefas de hoje)
- **Relatório financeiro simples** + ranking de produtos e gastos
- **Exportação PDF (impressão) e Excel/CSV**
- **IA básica** (análise por regras + chat de perguntas) ✅ já implementada
- **Offline** via PWA (Service Worker + armazenamento local) ✅ já implementado
- **Backup** export/import JSON

> **Status neste repositório:** o MVP acima está **funcional** em `agrosimples/index.html` (React single-file, `localStorage`, PWA). Pronto para testar com produtores reais sem infraestrutura.

**Critérios de pronto (DoD) do MVP:**
- Produtor registra venda e gasto em < 15s cada.
- Dashboard mostra lucro correto do mês.
- Funciona offline e não perde dados ao fechar o app.
- Exporta um relatório em PDF.

---

## 2. Fase 2 — Produção
- Controle de **plantio** (custos, custo/ha, lucro por cultura, status)
- Controle de **colheita** (atualiza estoque, registra perdas)
- Controle de **estoque** (produção) e **insumos**
- Relatórios por **cultura** e por **talhão**
- Contas a pagar/receber avançadas e exportação Excel
- Backend real: **Supabase** (Auth + Postgres + RLS), migração da lógica do MVP

## 3. Fase 3 — Escala
- Controle de **animais** (rebanho, sanitário, produtivo, alertas de vacina)
- **Modo offline completo** com fila de sincronização (IndexedDB)
- **IA avançada** (LLM): análise financeira profunda, perguntas livres, sugestões
- **App mobile nativo** (React Native/Expo)
- **Relatórios avançados** (produtividade, comparativos)
- **Plano Cooperativa** (multi-propriedade, relatórios consolidados, painel admin)
- Integrações: **WhatsApp** (lembretes/cobranças), **pagamentos** (assinatura)

---

## 4. Roadmap (linha do tempo sugerida)

| Trimestre | Entregas | Marco |
|-----------|----------|-------|
| **T1** | MVP em campo (PWA) + 10 produtores piloto | validação |
| **T2** | Supabase + login + Fase 2 (plantio/estoque) | primeiros pagantes |
| **T3** | IA avançada + WhatsApp + assinaturas | monetização |
| **T4** | Animais + app nativo + Plano Cooperativa | escala/B2B |

---

## 5. Métricas de sucesso
- **Ativação:** % de produtores que registram ≥ 5 vendas na 1ª semana.
- **Retenção:** uso semanal recorrente após 30 dias.
- **Valor percebido:** % que abre Relatórios/IA ao menos 1×/mês.
- **Conversão:** upgrade Básico → pago.
- **NPS** simples (carinha 😀/😐/🙁 no app).
