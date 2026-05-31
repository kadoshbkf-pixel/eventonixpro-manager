# 09 — Automações

Automações são **rotinas que rodam sozinhas** para avisar o produtor na hora
certa e poupar trabalho. Executadas por **jobs agendados** (ex.: diário) e por
**gatilhos** (ao salvar um registro).

## 1. Catálogo de automações

| Automação | Gatilho / quando roda | O que faz |
|-----------|------------------------|-----------|
| **Conta vencendo** | Diário (manhã) | Avisa contas a pagar que vencem hoje / em 3 dias |
| **Cliente atrasado** | Diário | Avisa quem está com pagamento atrasado e oferece cobrança no WhatsApp |
| **Estoque baixo** | Ao movimentar estoque / diário | Alerta quando item fica abaixo do mínimo |
| **Produto vencendo** | Diário | Avisa produção/insumo perto da validade |
| **Vacina próxima** | Diário | Alerta animais com vacina/medicamento na data ou véspera |
| **Colheita prevista** | Diário | Avisa plantios com colheita prevista para a semana |
| **Tarefa atrasada** | Diário | Marca tarefas vencidas como "Atrasada" e notifica |
| **Resumo semanal** | Semanal (domingo/segunda) | Gera resumo financeiro da semana (IA) e envia |
| **Relatório mensal** | 1º dia do mês | Gera relatório do mês anterior em PDF |
| **Sugerir compra de insumo** | Diário (IA) | Quando insumo essencial está acabando |
| **Sugerir cobrança** | Diário (IA) | Quando há clientes em aberto há muitos dias |
| **Produção parada** | Diário | Avisa produto sem movimento há X dias |

## 2. Canais de notificação

1. **Notificação no app** (Web Push / FCM) — sempre.
2. **WhatsApp** — quando o plano e a configuração permitirem (MVP: link `wa.me`).
3. **E-mail** — resumo e relatórios.

O produtor escolhe os canais em **Configurações → Notificações**.

## 3. Exemplos de mensagens

- 💰 "Você tem **2 contas** vencendo amanhã: Energia (R$ 180) e Ração (R$ 240)."
- 📩 "O cliente **João** está devendo **R$ 150** há 12 dias. Quer cobrar pelo
  WhatsApp?" → botão [Cobrar agora].
- 📦 "Sua **ração** está acabando (resta 1 saco). Hora de comprar."
- 💉 "Amanhã é dia de **vacinar o Lote 2** (12 cabeças)."
- 🌾 "Seu **tomate** está previsto para colher esta semana."
- 📊 "Resumo da semana: vendeu R$ 2.450, gastou R$ 1.300, lucro R$ 1.150."

## 4. Como cada alerta é gerado (técnico)

```
Job diário (Edge Function agendada):
  para cada propriedade ativa:
    - finance_entries (a_pagar, due_date ≤ hoje+3, status=aberto)  → alerta
    - finance_entries (a_receber, status=atrasado)                 → alerta
    - stock_items (quantity ≤ min_quantity)                        → alerta
    - stock_items (expires_at ≤ hoje+7)                            → alerta
    - animal_health_records (next_due_at ≤ hoje+1)                 → alerta + task
    - plantings (expected_harvest_at na semana)                    → alerta
    - tasks (due_date < hoje, status=pendente) → status=atrasada   → alerta
  grava em `alerts` e dispara notificações nos canais escolhidos.
```

## 5. Boas práticas

- **Não spammar:** agrupar avisos do dia numa única notificação quando possível.
- **Acionável:** todo alerta traz um botão de ação (pagar, cobrar, comprar,
  concluir).
- **Respeitar horário:** enviar de manhã, nunca de madrugada.
- **Silenciável:** o produtor pode desligar cada tipo de alerta.
