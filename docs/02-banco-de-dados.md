# AgroSimples — Estrutura de Banco de Dados

Banco recomendado: **PostgreSQL** (via Supabase). Multi-tenant por `farm_id`/`org_id` com **Row Level Security (RLS)**. Todas as tabelas têm `created_at`, `updated_at` e `synced` (para o modo offline). Chaves primárias em `uuid`.

---

## 1. Diagrama de relacionamentos (texto)

```
organizations (cooperativa/conta)        users (auth)
      │ 1                                    │
      │ N                                    │ N
   farms (propriedades) ──────────── memberships (user↔farm + role)
      │ 1
      ├── N areas (talhões/pastos/estufas/galpões)
      ├── N products (produtos de venda)
      ├── N clients
      ├── N sales ───< sale_items >─── products
      │        └── 1 receivable (se fiado/pendente)
      ├── N expenses
      ├── N payables
      ├── N receivables ──── clients
      ├── N tasks
      ├── N plantings ──── areas        (Fase 2)
      │        └── N planting_costs
      │        └── N harvests ──> atualiza stock_items
      ├── N stock_items (produção + insumos)  (Fase 2)
      ├── N animals / animal_lots          (Fase 3)
      │        ├── N health_records
      │        └── N production_records
      └── N ai_insights (cache de análises)
```

---

## 2. Tabelas — MVP

### organizations
| coluna | tipo | nota |
|--------|------|------|
| id | uuid PK | |
| name | text | nome da conta/cooperativa |
| plan | text | `basico` \| `intermediario` \| `premium` \| `cooperativa` |
| created_at | timestamptz | |

### users *(gerenciado pelo Supabase Auth)*
`id, email, phone, full_name, created_at`. Perfil/role fica em `memberships`.

### memberships
| coluna | tipo | nota |
|--------|------|------|
| id | uuid PK | |
| user_id | uuid FK→users | |
| farm_id | uuid FK→farms | |
| role | text | `admin` \| `produtor` \| `funcionario` \| `contador` \| `consultor` |
| permissions | jsonb | overrides finos (ver módulo de permissões) |

### farms (propriedades)
| coluna | tipo |
|--------|------|
| id | uuid PK |
| org_id | uuid FK→organizations |
| name | text |
| owner_name | text |
| doc | text (CPF/CNPJ) |
| address | text |
| area_total | numeric (hectares) |
| production_type | text |

### areas
`id, farm_id FK, name, kind (Talhão/Pasto/Estufa/Galpão/Curral/Reservatório/Outro), size_ha numeric, current_use text`

### products
`id, farm_id FK, name, category, unit (kg/caixa/saco/unidade/dúzia/litro/maço), sale_price numeric, stock numeric, min_stock numeric`

### clients
`id, farm_id FK, name, phone, whatsapp, address, type (Consumidor final/Mercado/Restaurante/Feira/Distribuidor/Cooperativa/Outro produtor/Outro)`

### sales
| coluna | tipo |
|--------|------|
| id | uuid PK |
| farm_id | uuid FK |
| client_id | uuid FK→clients (nullable) |
| date | date |
| total | numeric |
| pay_method | text (Dinheiro/Pix/Cartão/Boleto/Fiado/Transferência/Outro) |
| status | text (Pago/Pendente/Parcialmente pago/Atrasado/Cancelado) |
| delivery_date | date |
| notes | text |

### sale_items
`id, sale_id FK, product_id FK (nullable), product_name, qty numeric, unit text, unit_price numeric, line_total numeric`
> No MVP a venda pode ter um único item embutido em `sales`; `sale_items` formaliza o multi-item.

### expenses
`id, farm_id FK, date, category (enum de despesa), description, value numeric, area_id FK (opcional), planting_id FK (opcional), animal_id FK (opcional)`
> Os FKs opcionais permitem **custo por talhão / cultura / animal**.

### payables (contas a pagar)
`id, farm_id FK, name, category, value numeric, due_date date, status (Pendente/Pago/Atrasado), paid_at`

### receivables (contas a receber)
`id, farm_id FK, client_id FK, sale_id FK (opcional), description, value numeric, due_date date, status (Pendente/Recebido/Atrasado), received_at`

### tasks
`id, farm_id FK, title, type (enum de tarefa), date, time, priority (Baixa/Média/Alta), assignee_id FK→users, status (Pendente/Em andamento/Concluída/Atrasada/Cancelada), notes`

### ai_insights
`id, farm_id FK, period (YYYY-MM), kind (alerta/sugestao/resumo), icon, text, created_at`

---

## 3. Tabelas — Fase 2 (produção)

### plantings
`id, farm_id FK, area_id FK, crop, variety, area_used_ha numeric, plant_date, expected_harvest_date, seed_qty, status (Planejado/Plantado/Em desenvolvimento/Pronto para colher/Colhido/Cancelado/Perda parcial/Perda total), expected_yield, real_yield, expected_revenue`
> **Derivados (view):** custo_total = Σ planting_costs; custo_por_ha = custo_total / area_used_ha; lucro = receita − custo_total; perda_% = (prevista−real)/prevista.

### planting_costs
`id, planting_id FK, type (Sementes/Adubo/Defensivo/Mão de obra/Máquina/Combustível/Outro), value numeric, date`

### harvests
`id, farm_id FK, planting_id FK, date, qty numeric, unit, lost_qty numeric, loss_reason, destination (Venda/Consumo/Estoque/Doação/Perda/Beneficiamento)`
> Trigger: ao inserir colheita com destino “Estoque/Venda”, cria/atualiza `stock_items`.

### stock_items
`id, farm_id FK, kind (producao/insumo), name, category, unit, qty numeric, min_qty numeric, entry_date, expiry_date, location, sold_qty, lost_qty`
> `saldo = qty − sold_qty − lost_qty`. Alertas comparam `saldo` com `min_qty` e `expiry_date`.

---

## 4. Tabelas — Fase 3 (animais)

### animals / animal_lots
`id, farm_id FK, tag, name, species (Bovino/Suíno/Ave/Peixe/Caprino/Ovino/Outro), breed, sex, birth_date, weight, purpose (Corte/Leite/Reprodução/Ovos/Venda/Consumo/Outro), entry_date, origin, purchase_value, status, is_lot bool, lot_count int`

### health_records
`id, animal_id FK, type (Vacina/Medicamento/Vermífugo/Consulta/Doença/Tratamento), product, date, next_date, notes`
> `next_date` alimenta alertas de vacina/medicamento.

### production_records
`id, animal_id FK, type (Leite/Ovos/Peso/Parto/Mortalidade), date, qty numeric, unit`

---

## 5. Regras de integridade e automações no banco
- **Triggers:** venda fiada/pendente → cria `receivable`; colheita → atualiza `stock_items`; venda → baixa `products.stock`.
- **Views/materialized views** para o dashboard: `v_month_summary(farm_id, month, sold, spent, profit, margin)`, `v_product_ranking`, `v_expense_ranking`, `v_overdue_receivables`.
- **RLS:** todo SELECT/INSERT/UPDATE filtrado por `farm_id` pertencente a uma `membership` do usuário autenticado; `contador`/`consultor` recebem políticas somente-leitura.
- **Soft delete** (`deleted_at`) para proteção contra exclusão acidental + histórico.
- **Auditoria:** tabela `audit_log(user_id, farm_id, table, row_id, action, diff jsonb, at)`.

---

## 6. Mapeamento offline (cliente)
O app guarda as mesmas coleções em armazenamento local (IndexedDB no app nativo; `localStorage` no MVP atual). Cada registro carrega `local_id`, `synced (bool)` e `op (create/update/delete)`. Ao reconectar, uma fila envia as operações pendentes; conflitos resolvidos por `updated_at` mais recente (last-write-wins) com aviso ao usuário quando houver divergência.
