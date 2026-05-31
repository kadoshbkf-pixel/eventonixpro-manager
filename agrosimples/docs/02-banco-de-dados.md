# 02 — Estrutura do Banco de Dados

Banco recomendado: **PostgreSQL** (via Supabase). Todas as tabelas usam:
- `id` UUID (chave primária, default `gen_random_uuid()`)
- `created_at` / `updated_at` timestamptz
- **Multi-tenant** por `tenant_id` (organização/assinante) — base para o plano
  Cooperativa e para o isolamento de dados via Row Level Security (RLS).
- Soft delete: `deleted_at` (proteção contra exclusão acidental).

> Convenção: nomes de tabela no plural, em `snake_case`.

---

## 1. Diagrama de Entidade-Relacionamento (visão geral)

```
tenants
  └─< users (perfis e permissões)
  └─< properties
        ├─< areas (talhões, pastos, estufas, galpões, currais, reservatórios)
        ├─< machines
        ├─< employees
        ├─< plantings >── areas
        │     └─< harvests
        ├─< animals (individual ou lote)
        │     ├─< animal_health_records
        │     └─< animal_production_records
        ├─< products
        │     └─< stock_items (produção e insumos)
        │           └─< stock_movements
        ├─< customers
        │     └─< sales
        │           └─< sale_items >── products
        ├─< finance_entries (entradas/saídas, contas a pagar/receber)
        ├─< tasks
        └─< ai_interactions / alerts / reports
```

---

## 2. Tabelas principais (campos essenciais)

### `tenants` (assinante / organização)
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| name | text | nome da conta/cooperativa |
| plan | text | basico \| intermediario \| premium \| cooperativa |
| status | text | ativo \| trial \| suspenso \| cancelado |
| trial_ends_at | timestamptz | fim do período de teste |

### `users`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK (ligado ao auth) |
| tenant_id | uuid | FK tenants |
| name | text | |
| email | text | único |
| phone | text | WhatsApp |
| role | text | administrador \| produtor \| funcionario \| contador \| consultor |
| permissions | jsonb | overrides finos de permissão |
| active | boolean | |

### `properties`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| tenant_id | uuid | FK |
| name | text | nome da propriedade |
| owner_name | text | nome do produtor |
| document | text | CPF/CNPJ |
| address | text | |
| total_area_ha | numeric | tamanho total |
| production_type | text | tipo de produção |

### `areas`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| name | text | ex.: "Talhão 1" |
| type | text | talhao \| pasto \| estufa \| galpao \| curral \| reservatorio |
| size_ha | numeric | |
| current_use | text | ex.: "milho" |

### `machines`
property_id, name, type, value, acquired_at, status.

### `employees`
property_id, name, role, contact, contract_type, cost.

### `plantings`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| area_id | uuid | FK areas |
| crop | text | cultura |
| variety | text | variedade |
| area_used_ha | numeric | |
| planted_at | date | |
| expected_harvest_at | date | |
| seed_qty | numeric | sementes/mudas |
| cost_seeds / cost_fertilizer / cost_pesticide / cost_labor / cost_machine / cost_fuel | numeric | custos |
| expected_yield | numeric | previsão de produção |
| actual_yield | numeric | produção real (vem da colheita) |
| expected_price | numeric | preço esperado |
| status | text | planejado \| plantado \| em_desenvolvimento \| pronto_colher \| colhido \| cancelado \| perda_parcial \| perda_total |
| notes | text | |

> **Campos calculados (view ou no app):** `total_cost`, `cost_per_ha`,
> `expected_revenue`, `profit`, `loss_percent`.

### `harvests`
planting_id (FK), property_id, crop, harvested_at, qty_harvested, unit
(kg/caixa/saco/unidade/litro), qty_lost, loss_reason, destination
(venda \| consumo \| estoque \| doacao \| perda \| beneficiamento).

### `products`
property_id, name, category, default_unit, sale_price.
(produtos vendáveis — usado em vendas e estoque de produção)

### `stock_items`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| kind | text | producao \| insumo |
| product_id | uuid | FK products (quando produção) |
| name | text | (insumos) |
| category | text | semente, adubo, racao, defensivo... |
| quantity | numeric | saldo atual |
| unit | text | |
| unit_cost | numeric | |
| min_quantity | numeric | estoque mínimo (gera alerta) |
| entry_date | date | |
| expires_at | date | validade (gera alerta) |
| location | text | local de armazenamento |

### `stock_movements`
stock_item_id (FK), type (entrada \| saida \| perda \| ajuste), quantity,
reason, related_id (venda/colheita), moved_at.

### `customers`
property_id, name, phone, whatsapp, address, type
(consumidor \| mercado \| restaurante \| feira \| distribuidor \| cooperativa \|
produtor \| outro). Campos derivados: total_bought, last_purchase_at, open_amount.

### `sales`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| customer_id | uuid | FK customers |
| sold_at | date | |
| total_value | numeric | |
| payment_method | text | dinheiro \| pix \| cartao \| boleto \| fiado \| transferencia \| outro |
| payment_status | text | pago \| pendente \| parcial \| atrasado \| cancelado |
| paid_value | numeric | quanto já foi pago |
| delivery_date | date | |
| notes | text | |
| receipt_url | text | comprovante |

### `sale_items`
sale_id (FK), product_id (FK), quantity, unit_price, subtotal.
(baixa o estoque de produção ao salvar)

### `finance_entries`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| direction | text | entrada \| saida |
| nature | text | a_vista \| a_pagar \| a_receber |
| category | text | racao, adubo, frete, venda_produtos... |
| description | text | |
| amount | numeric | |
| due_date | date | vencimento (a pagar/receber) |
| paid_at | date | quitação |
| status | text | aberto \| pago \| atrasado \| cancelado |
| cost_center_type | text | cultura \| animal \| talhao \| cliente \| geral |
| cost_center_id | uuid | id da entidade do centro de custo |
| related_sale_id | uuid | quando gerada por venda |

### `animals`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| is_lot | boolean | individual ou lote |
| identifier | text | nome/número |
| species | text | bovino, suino, ave, peixe, caprino, ovino, outro |
| breed | text | raça |
| sex | text | |
| birth_date | date | |
| weight | numeric | |
| purpose | text | corte \| leite \| reproducao \| ovos \| venda \| consumo \| outro |
| entry_date | date | |
| origin | text | |
| purchase_value | numeric | |
| head_count | int | (quando lote) |
| status | text | ativo \| vendido \| morto \| consumido |

### `animal_health_records`
animal_id (FK), type (vacina \| medicamento \| vermifugo \| consulta \| doenca \|
tratamento), description, applied_at, next_due_at (gera alerta), cost.

### `animal_production_records`
animal_id (FK), type (leite \| ovos \| ganho_peso \| parto \| mortalidade \|
venda), value, recorded_at, notes.

### `tasks`
| campo | tipo | notas |
|-------|------|-------|
| id | uuid | PK |
| property_id | uuid | FK |
| title | text | |
| type | text | plantar, irrigar, vacinar, cobrar_cliente... |
| due_date | date | |
| due_time | time | |
| priority | text | baixa \| media \| alta |
| assigned_to | uuid | FK users |
| status | text | pendente \| em_andamento \| concluida \| atrasada \| cancelada |
| notes | text | |

### Tabelas de apoio
- `alerts` — property_id, type, message, severity, related_id, read, created_at.
- `ai_interactions` — property_id, user_id, question, answer, context_snapshot, created_at.
- `reports` — property_id, type, period, file_url, format (pdf/excel/csv).
- `sync_queue` — para modo offline: device_id, entity, payload jsonb,
  operation (insert/update/delete), synced (bool), client_created_at. Ver
  [07 — Arquitetura](07-arquitetura-tecnologias.md#modo-offline).
- `audit_logs` — quem alterou o quê (histórico de alterações / LGPD).

---

## 3. Relacionamentos-chave (regras)

- `harvests.planting_id → plantings.id` — colheita atualiza `actual_yield`.
- `sale_items.product_id → products.id` — venda gera `stock_movements` de saída.
- Venda **fiado/pendente** cria `finance_entries` com `nature = a_receber`.
- Compra de insumo a prazo cria `finance_entries` com `nature = a_pagar`.
- `animal_health_records.next_due_at` alimenta `alerts` (vacina pendente).
- Custos de plantio/animal alimentam centros de custo em `finance_entries`.

---

## 4. Segurança (RLS)

Toda consulta filtra por `tenant_id` do usuário autenticado. Políticas de Row
Level Security garantem que um produtor **só vê os dados da sua propriedade**, e
o painel da cooperativa enxerga as propriedades vinculadas ao seu `tenant_id`.
