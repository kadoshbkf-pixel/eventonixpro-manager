# AgroSimples — Perfis, Planos e Regras de Negócio

## 1. Perfis de usuário e permissões

| Perfil | Descrição | Vendas | Gastos | Contas | Tarefas | Plantio/Estoque | Animais | Relatórios | Config/Usuários |
|--------|-----------|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| **Administrador** | acesso total à conta | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Produtor** | dono da propriedade | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | parcial |
| **Funcionário** | ajudante de campo | ➕ lançar | ➕ lançar | 👁 | ✅ | ➕ lançar | ➕ lançar | ❌ | ❌ |
| **Contador** | apoio fiscal | 👁 | 👁 | 👁 | ❌ | 👁 | 👁 | ✅ (financeiro) | ❌ |
| **Consultor** | melhoria/IA | 👁 | 👁 | 👁 | 👁 | 👁 | 👁 | ✅ | ❌ |

Legenda: ✅ total · ➕ criar/editar próprios lançamentos · 👁 somente leitura · ❌ sem acesso.

**Implementação:** `memberships.role` define o papel; `memberships.permissions` (JSONB) permite ajustes finos por módulo. RLS no banco garante o isolamento. Permissões verificadas também no cliente (esconder botões) e no servidor (autoridade final).

---

## 2. Planos de assinatura

| Recurso | Básico | Intermediário | Premium | Cooperativa |
|---|:--:|:--:|:--:|:--:|
| Propriedade, clientes, vendas | ✅ | ✅ | ✅ | ✅ |
| Financeiro simples + agenda | ✅ | ✅ | ✅ | ✅ |
| Relatório mensal básico | ✅ | ✅ | ✅ | ✅ |
| Plantio + colheita | — | ✅ | ✅ | ✅ |
| Estoque + insumos | — | ✅ | ✅ | ✅ |
| Contas a pagar/receber avançado | — | ✅ | ✅ | ✅ |
| Relatórios por cultura/talhão + PDF | — | ✅ | ✅ | ✅ |
| Controle de animais | — | — | ✅ | ✅ |
| Dashboard completo + alertas inteligentes | — | — | ✅ | ✅ |
| IA (análise + perguntas) | básica | básica | **avançada** | avançada |
| Múltiplos usuários | 1 | até 3 | até 10 | ilimitado |
| Backup em nuvem + export Excel | — | — | ✅ | ✅ |
| Multi-propriedade + relatórios consolidados | — | — | — | ✅ |
| Suporte | comunidade | e-mail | prioritário | dedicado |

**Sugestão de preço (BR, validar em campo):** Básico gratuito/baixo custo (aquisição), Intermediário ~R$19–29/mês, Premium ~R$49–79/mês, Cooperativa sob proposta por nº de propriedades.

**Gating:** o plano fica em `organizations.plan`. O cliente esconde/branqueia módulos bloqueados com chamada para upgrade (“Disponível no plano Intermediário”). O servidor valida o plano em escritas sensíveis.

---

## 3. Regras de negócio (núcleo)

### Financeiro
1. **Lucro do mês = total de vendas (não canceladas) − total de gastos** do mês corrente.
2. **Margem = lucro / total vendido × 100** (0% se nada vendido).
3. **Ponto de equilíbrio:** “para cobrir custos, falta vender (gastos − vendas)” enquanto vendas < gastos.
4. Venda com pagamento **Fiado** ou status **Pendente** **gera automaticamente uma conta a receber**.
5. Conta a receber/pagar com `due_date < hoje` e não quitada vira **Atrasada** (visual + alerta).

### Vendas e estoque
6. Salvar venda **baixa o estoque** do produto correspondente (nunca abaixo de zero).
7. Total da venda = `qtd × preço unitário` (somado por item quando multi-item).
8. Venda **Cancelada** não entra em receita nem em rankings.

### Produção (Fase 2)
9. Custo total do plantio = soma dos custos lançados; **custo/ha = custo total ÷ área usada**.
10. **% de perda = (produção prevista − produção real) ÷ produção prevista**.
11. Colheita com destino “Estoque” ou “Venda” **aumenta o estoque**; “Perda” registra perda e não soma estoque.

### Animais (Fase 3)
12. Custo por animal/lote = custos vinculados ÷ nº de animais; lucro = receita de venda/produção − custos.
13. `health_records.next_date` próxima (≤ X dias) gera **alerta de vacina/medicamento**.

### Tarefas e alertas
14. Tarefa com `date < hoje` e status Pendente vira **Atrasada**.
15. Estoque ≤ `min_qty` → alerta “estoque baixo”; validade próxima → “perto do vencimento”.

### Dados e segurança
16. Exclusões são **soft delete**; nada some de vez sem confirmação dupla.
17. Toda alteração relevante grava em `audit_log`.
18. Cada usuário só enxerga propriedades às quais pertence (RLS).
