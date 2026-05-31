# 11 — Regras de Negócio (Cálculos e Validações)

Este documento define **como o sistema calcula** lucro, custos e indicadores, e
as **validações** que garantem dados confiáveis. É a referência para o
desenvolvimento e para a IA (que só explica estes números, nunca os recalcula).

## 1. Financeiro

### Lucro do mês (indicador central)
```
lucro_mes = total_entradas_pagas(mes) − total_saidas_pagas(mes)
```
- "Entradas" = vendas recebidas + outras receitas no período.
- "Saídas" = despesas pagas no período.
- O dashboard pode mostrar **2 visões**: *caixa* (só o que entrou/saiu de fato) e
  *competência* (tudo do mês, mesmo a receber/pagar). Padrão para o produtor:
  **caixa** ("o que sobrou no bolso"), com opção de ver o previsto.

### Margem de lucro
```
margem = (lucro / total_vendido) × 100        (em %)
```

### Fluxo de caixa
Saldo acumulado dia a dia = saldo_anterior + entradas_do_dia − saídas_do_dia.

### Ponto de equilíbrio ("quanto preciso vender para pagar meus custos")
```
ponto_equilibrio = total_custos_do_periodo
```
A IA responde: "Para empatar este mês, você precisa vender R$ X. Já vendeu R$ Y."

## 2. Plantio

```
custo_total      = cost_seeds + cost_fertilizer + cost_pesticide
                   + cost_labor + cost_machine + cost_fuel
custo_por_ha     = custo_total / area_used_ha
receita_esperada = expected_yield × expected_price
receita_real     = produção_vendida × preço_de_venda  (do estoque/vendas)
lucro_plantio    = receita_real − custo_total
perda_percent    = (qty_lost / (qty_harvested + qty_lost)) × 100
```
- Status `perda_total` → receita_real = 0; lucro = −custo_total.
- `actual_yield` é preenchido pela soma das colheitas do plantio.

## 3. Colheita e estoque

- Ao salvar colheita com destino **Estoque** ou **Venda**:
  ```
  stock_item(producao).quantity += qty_harvested
  registra stock_movement(entrada)
  planting.actual_yield += qty_harvested
  ```
- Ao salvar **venda**:
  ```
  stock_item.quantity −= sale_item.quantity
  registra stock_movement(saida)
  ```
- **Saldo de estoque** = entradas − saídas − perdas.
- **Validação:** não permitir venda acima do saldo (ou avisar "estoque
  insuficiente" e permitir ajuste).

### Gatilhos de alerta (estoque)
- `quantity ≤ min_quantity` → "estoque baixo".
- `expires_at ≤ hoje + 7 dias` → "vencendo".
- Sem `stock_movement` há > X dias → "produção parada".
- `perda_acumulada / entradas > 20%` → "muita perda".

## 4. Vendas e contas

- `total_value = Σ(sale_item.quantity × unit_price)`.
- Forma de pagamento **fiado** ou status **pendente/parcial/atrasado** →
  cria/atualiza `finance_entries(nature=a_receber)`.
- `saldo_devedor_venda = total_value − paid_value`.
- Venda vira **atrasada** quando `due_date < hoje` e `saldo_devedor > 0`.
- **Validação:** `paid_value ≤ total_value`.

### Indicadores do cliente
```
total_comprado = Σ sales.total_value
valor_em_aberto = Σ saldo_devedor das vendas do cliente
ultima_compra = max(sales.sold_at)
produtos_mais_comprados = top N por quantidade em sale_items
```

## 5. Animais

```
custo_animal  = purchase_value + Σ custos_sanitarios + Σ rateio_racao/manejo
custo_lote    = Σ custo dos animais do lote
producao_media= Σ producao / nº de registros (leite/ovos/dia)
lucro_animal  = receita_venda + valor_producao − custo_animal
lucro_lote    = Σ lucro dos animais do lote
```
- Alerta de vacina: `next_due_at ≤ hoje + 1` → alerta + tarefa.
- `mortalidade_%` = mortes / cabeças iniciais × 100.

## 6. Centros de custo

Toda despesa pode ser vinculada a um **centro de custo**
(`cost_center_type` + `cost_center_id`): cultura, animal, talhão ou cliente.
Isso permite relatórios "lucro por talhão", "custo por animal", etc.
```
lucro_por_talhao = receitas(talhao) − despesas(talhao)
```

## 7. Tarefas

- `due_date < hoje` e `status = pendente` → `status = atrasada` (job diário).
- Tarefa de "vacinar/colher/pagar" pode ser **gerada automaticamente** por
  alertas (sanidade, plantio, contas).

## 8. Validações gerais (entrada de dados)

| Campo | Regra |
|-------|-------|
| Valores monetários | ≥ 0; aceitar vírgula decimal (pt-BR) |
| Quantidades | ≥ 0 |
| Datas de colheita | ≥ data de plantio |
| Venda | exige cliente, ≥ 1 item e valor > 0 |
| Despesa | exige categoria e valor > 0 |
| `paid_value` | ≤ `total_value` |
| Estoque na venda | quantidade ≤ saldo (ou aviso) |
| CPF/CNPJ | formato válido (opcional preencher) |
| Exclusão | sempre confirmação + soft delete |

## 9. Regras de moeda, unidades e idioma

- Moeda: **Real (R$)**, formato pt-BR (`R$ 1.234,56`).
- Unidades de medida configuráveis por produto (kg, caixa, saco, unidade, litro).
- Datas em pt-BR (`dd/mm/aaaa`).
- Arredondamento financeiro: 2 casas decimais.

## 10. Multi-tenant e planos (regras de acesso)

- Toda query filtra por `tenant_id` (RLS).
- Recurso fora do plano → bloqueado com tela de upsell (não erro).
- Downgrade preserva dados (somente leitura nos módulos perdidos).
