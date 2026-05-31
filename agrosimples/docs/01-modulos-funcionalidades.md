# 01 — Módulos e Funcionalidades

Cada módulo abaixo traz **o que faz**, **funcionalidades principais**, **campos**
e a **lógica de negócio** (cálculos e automações). As fórmulas detalhadas estão
em [11 — Regras de Negócio](11-regras-de-negocio.md).

---

## Módulo 1 — Cadastro da Propriedade

**Objetivo:** ser a base de tudo. Toda informação se conecta à propriedade.

**Cadastro da propriedade:** Nome da propriedade, Nome do produtor, CPF/CNPJ,
Endereço, Tamanho total da área (ha), Tipo de produção.

**Áreas internas** (uma propriedade tem várias): Talhões, Pastos, Estufas,
Galpões, Currais, Reservatórios. Cada área tem: nome, tipo, tamanho e uso atual.

**Máquinas e equipamentos:** nome, tipo, valor, data de aquisição, status.

**Funcionários/ajudantes:** nome, função, contato, tipo de vínculo, custo.

**Exemplo:**
- Talhão 1 — 3 ha — milho
- Talhão 2 — 1 ha — mandioca
- Estufa 1 — alface
- Pasto 1 — gado de corte
- Galpão 1 — galinhas poedeiras

**Lógica:** cada área vira "centro de custo" — o sistema soma custos e receitas
por talhão/pasto para mostrar **qual área dá mais lucro**.

---

## Módulo 2 — Controle de Plantio

**Objetivo:** saber **quanto custou** plantar e **quanto rendeu**.

**Campos:** cultura, variedade, área utilizada (vincula a um talhão), data do
plantio, previsão de colheita, quantidade de sementes/mudas, e custos: sementes,
adubo, defensivos, mão de obra, máquinas, combustível, observações, status.

**Status:** Planejado · Plantado · Em desenvolvimento · Pronto para colher ·
Colhido · Cancelado · Perda parcial · Perda total.

**Cálculos automáticos:**
- Custo total do plantio = soma de todos os custos
- Custo por hectare = custo total ÷ área
- Previsão de produção (estimativa) e produção real (vinda da colheita)
- Receita esperada e **lucro/prejuízo**
- Percentual de perda

---

## Módulo 3 — Controle de Colheita

**Objetivo:** registrar o que foi colhido e **atualizar o estoque automaticamente**.

**Campos:** cultura colhida (vincula ao plantio), data, quantidade colhida,
unidade (kg, caixa, saco, unidade, litro), quantidade perdida, motivo da perda,
destino.

**Destinos:** Venda · Consumo próprio · Estoque · Doação · Perda · Beneficiamento.

**Lógica:** ao salvar uma colheita com destino "Estoque" ou "Venda", o
**estoque de produção é atualizado**. A produção real alimenta o cálculo de
lucro do plantio (Módulo 2).

---

## Módulo 4 — Controle de Estoque

### Estoque de produção
Produto, quantidade disponível, unidade, data de entrada, validade, local de
armazenamento, quantidade vendida, quantidade perdida, **saldo atual**.

### Estoque de insumos
Sementes, adubos, calcário, defensivos, ração, medicamentos, combustível,
embalagens, ferramentas, peças, materiais de manutenção. Campos: item, categoria,
quantidade, unidade, custo unitário, estoque mínimo, validade.

**Alertas gerados:**
- Estoque baixo (abaixo do mínimo)
- Produto próximo ao vencimento
- Insumo acabando
- Produção parada há muitos dias
- Produto com muita perda

---

## Módulo 5 — Controle Financeiro Rural ⭐

**Objetivo:** o coração do sistema — mostrar **lucro real**.

**Controla:** entradas, saídas, contas a pagar, contas a receber, despesas fixas
e variáveis, lucro/prejuízo mensal, fluxo de caixa, e **custo por** cultura,
animal, talhão e cliente, além de margem de lucro.

**Categorias de despesa:** sementes, adubos, defensivos, ração, medicamentos,
energia, água, combustível, manutenção, funcionários, frete, embalagens,
impostos, máquinas, equipamentos, outros.

**Categorias de receita:** venda de produtos, animais, leite, ovos, frutas,
hortaliças, venda para mercado, feira, venda direta, serviços rurais, outros.

**Mostra de forma simples:** total vendido, total gasto, **lucro real**, margem,
maior despesa, melhor produto, produto com prejuízo, clientes em aberto.

---

## Módulo 6 — Controle de Vendas

**Campos:** cliente, produto vendido (baixa no estoque), quantidade, valor
unitário, valor total, data, forma de pagamento, status do pagamento, data de
entrega, observações, comprovante/recibo.

**Formas de pagamento:** Dinheiro · Pix · Cartão · Boleto · Fiado ·
Transferência · Outro.

**Status:** Pago · Pendente · Parcialmente pago · Atrasado · Cancelado.

**Gera:** pedido de venda, recibo simples, relatório de vendas, histórico do
cliente, contas a receber (vendas "fiado"/pendentes viram automaticamente
**contas a receber**).

---

## Módulo 7 — Cadastro de Clientes

**Campos:** nome, telefone, WhatsApp, endereço, tipo de cliente.

**Tipos:** consumidor final, mercado, restaurante, feira, distribuidor,
cooperativa, outro produtor, outro.

**Mostra no perfil:** total comprado, última compra, valor em aberto, produtos
mais comprados, histórico de pagamentos.

---

## Módulo 8 — Controle de Animais

**Espécies:** bovinos, suínos, aves, peixes, caprinos, ovinos, outros. Permite
controlar **animal individual** ou **lote**.

**Campos:** identificação (nome/número), espécie, raça, sexo, data de nascimento,
peso, finalidade, data de entrada, origem, valor de compra, status.

**Finalidades:** corte, leite, reprodução, ovos, venda, consumo, outro.

**Controle sanitário:** vacinas, medicamentos, vermífugos, consultas
veterinárias, doenças, tratamentos, **data da próxima aplicação** (gera alerta).

**Controle produtivo:** produção de leite, produção de ovos, ganho de peso,
reprodução, partos, mortalidade, venda de animais.

**Cálculos:** custo por animal, custo por lote, produção média, lucro por
animal, lucro por lote, alertas de vacina/medicamento.

---

## Módulo 9 — Agenda de Tarefas Rurais

**Tipos de tarefa:** plantar, irrigar, adubar, aplicar defensivo, capinar,
colher, vacinar, alimentar animais, comprar insumos, fazer manutenção, entregar
pedido, receber cliente, pagar conta, cobrar cliente, registrar venda, verificar
estoque.

**Campos por tarefa:** título, data, horário, prioridade, responsável, status,
observação.

**Status:** Pendente · Em andamento · Concluída · Atrasada · Cancelada.

**Alertas:** notificação no app, WhatsApp (se possível), e-mail (se possível).
Tarefas vencidas viram "Atrasada" automaticamente.

---

## Módulo 10 — Relatórios e Dashboard

**Dashboard (tela inicial):** vendas do mês, gastos do mês, **lucro do mês**,
contas a pagar, contas a receber, estoque baixo, tarefas de hoje, plantios em
andamento, colheitas previstas, animais com vacina pendente, produtos mais
lucrativos, maiores despesas.

**Relatórios:** financeiro mensal, vendas, despesas, por cultura, por talhão,
por animal/lote, estoque, clientes, produtividade, lucro e prejuízo.

**Exportação:** PDF · Excel · CSV.

---

## Módulo transversal — Assistente AgroSimples (IA)

Atravessa todos os módulos: análise financeira automática, perguntas por texto,
sugestões inteligentes e resumos automáticos, sempre em linguagem simples.
Detalhes em [10 — Inteligência Artificial](10-inteligencia-artificial.md).
