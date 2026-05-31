# AgroSimples — Visão do Produto

> **Slogan:** “O sistema que mostra se sua propriedade está dando lucro de verdade.”
> **Posicionamento:** SaaS simples, barato e mobile-first para **pequenos produtores rurais e agricultura familiar** — não para grandes fazendas.

---

## 1. Descrição completa do sistema

O **AgroSimples** é um sistema de gestão rural pensado para produtores que **não têm facilidade com tecnologia**. Ele troca o jargão técnico por linguagem do dia a dia (“Dinheiro que entrou”, “Gastos do mês”, “Quem me deve”) e concentra-se em responder à pergunta mais importante de qualquer negócio:

> **“Estou ganhando dinheiro ou só girando dinheiro?”**

O produtor registra vendas e gastos em poucos toques, acompanha contas a pagar/receber, organiza tarefas da propriedade e recebe, de um **assistente de IA**, análises práticas em linguagem simples. Tudo funciona no **celular e no computador**, com **modo offline** (os dados ficam no aparelho e sincronizam quando a internet volta).

### Princípios de produto
- **Simplicidade primeiro:** poucos botões por tela, ícones grandes, cores suaves, exemplos em todo campo.
- **Valor rápido:** em 5 minutos o produtor já vê seu lucro do mês.
- **Mobile-first e offline:** a propriedade nem sempre tem internet.
- **Linguagem de produtor, não de contador.**
- **Cresce com o cliente:** começa básico e libera plantio, estoque, animais e IA avançada conforme o plano.

---

## 2. Público-alvo

Pequenos produtores rurais • agricultura familiar • hortaliças, frutas e grãos em pequena escala • gado de corte e leite • aves, suínos, peixes e pequenos animais • quem vende em feiras, mercados, restaurantes ou direto ao consumidor • pequenas associações e cooperativas.

---

## 3. Perguntas que o sistema responde
- Quanto gastei e quanto vendi este mês?
- Estou no lucro ou no prejuízo?
- Qual cultura/animal deu mais lucro? Qual deu prejuízo?
- Tenho produto para vender? O que está acabando?
- Quais contas preciso pagar? Quem ainda me deve?
- O que preciso fazer hoje na propriedade?
- Quanto preciso vender para cobrir meus custos?

---

## 4. Lista de módulos

| # | Módulo | Fase |
|---|--------|------|
| 1 | Cadastro da propriedade e áreas/talhões | MVP |
| 2 | Cadastro de produtos | MVP |
| 3 | Cadastro de clientes | MVP |
| 4 | Controle de vendas | MVP |
| 5 | Controle financeiro (gastos, contas a pagar/receber, fluxo) | MVP |
| 6 | Agenda de tarefas rurais | MVP |
| 7 | Relatórios e dashboard | MVP |
| 8 | Assistente de IA | MVP (básico) → Fase 3 (avançado) |
| 9 | Controle de plantio | Fase 2 |
| 10 | Controle de colheita | Fase 2 |
| 11 | Controle de estoque (produção + insumos) | Fase 2 |
| 12 | Controle de animais (rebanho + sanitário + produtivo) | Fase 3 |
| 13 | Modo offline completo + app mobile nativo | Fase 3 |
| 14 | Plano Cooperativa (multi-propriedade) | Fase 3 |

---

## 5. Funcionalidades por módulo (resumo)

### Módulo 1 — Propriedade e áreas
Cadastro de propriedade (nome, produtor, CPF/CNPJ, endereço, área total, tipo). Áreas internas com tipo (talhão, pasto, estufa, galpão, curral, reservatório), tamanho e uso atual. Máquinas, equipamentos e funcionários (Fase 2+). **Lógica:** uma propriedade tem muitas áreas; cada área pode estar vinculada a um plantio ou a um lote de animais.

### Módulo 2 — Plantio (Fase 2)
Registro por plantio: cultura, variedade, área usada, datas (plantio/previsão de colheita), insumos e seus custos (sementes, adubo, defensivos, mão de obra, máquina, combustível), status. **Cálculos automáticos:** custo total, custo por hectare, produção prevista vs. real, receita esperada, lucro/prejuízo e % de perda.

### Módulo 3 — Colheita (Fase 2)
Cultura colhida, data, quantidade e unidade, quantidade/motivo de perda, destino (venda, consumo, estoque, doação, perda, beneficiamento). **Atualiza o estoque automaticamente.**

### Módulo 4 — Estoque (Fase 2)
**Produção:** produto, quantidade, unidade, entrada, validade, local, vendido, perdido, saldo. **Insumos:** sementes, adubos, calcário, defensivos, ração, medicamentos, combustível, embalagens, ferramentas, peças. **Alertas:** estoque baixo, perto do vencimento, insumo acabando, produção parada, muita perda.

### Módulo 5 — Financeiro (MVP) ⭐
Entradas e saídas, contas a pagar/receber, despesas fixas/variáveis, fluxo de caixa, lucro/prejuízo mensal, custo por cultura/animal/talhão/cliente, margem. Categorias de despesa e receita pré-definidas. Indicadores: total vendido, total gasto, lucro real, maior despesa, melhor produto, produto com prejuízo, clientes em aberto.

### Módulo 6 — Vendas (MVP)
Cliente, produto, quantidade, valor unitário/total, data, forma de pagamento, status, entrega, observações. Gera recibo simples, pedido, relatório de vendas, histórico do cliente e **contas a receber automáticas** quando a venda é fiada/pendente.

### Módulo 7 — Clientes (MVP)
Nome, telefone/WhatsApp, endereço, tipo. Mostra total comprado, última compra, valor em aberto, produtos mais comprados. Botão de WhatsApp para cobrança/contato.

### Módulo 8 — Animais (Fase 3)
Cadastro por animal ou lote (espécie, raça, sexo, nascimento, peso, finalidade, origem, valor). Sanitário (vacinas, vermífugos, tratamentos, próxima aplicação). Produtivo (leite, ovos, ganho de peso, partos, mortalidade). **Cálculos:** custo e lucro por animal/lote, produção média, alertas de vacina.

### Módulo 9 — Agenda de tarefas (MVP)
Tarefas com título, tipo (plantar, irrigar, adubar, vacinar, colher, cobrar, pagar…), data, prioridade, responsável, status. Alertas no app (e WhatsApp/e-mail nas fases seguintes).

### Módulo 10 — Relatórios e dashboard (MVP)
Dashboard com lucro do mês, a pagar/receber, tarefas de hoje, avisos (estoque baixo, contas vencendo). Relatórios: financeiro mensal, vendas, despesas, por cultura/talhão/animal, estoque, clientes, produtividade, lucro/prejuízo. Exportação **PDF, Excel/CSV**.

---

## 6. Diferenciais comerciais
- Simples e barato, feito para quem produz pouco ou está começando.
- Funciona no celular e **offline**.
- Mostra **lucro real**, não só movimentação.
- **IA prática** que fala como consultor rural amigável.
- Agenda rural + controle de produção + implantação assistida.

---

## 7. Texto comercial (pitch)

> **AgroSimples — Gestão Inteligente para Pequenos Produtores**
>
> Você trabalha duro na roça, mas no fim do mês não sabe se sobrou dinheiro? O **AgroSimples** organiza sua propriedade, controla seus gastos e mostra **onde está o lucro de verdade** — tudo pelo celular, com linguagem simples e sem complicação.
>
> Registre uma venda em 10 segundos. Saiba na hora quem te deve, o que precisa pagar e o que colher esta semana. E conte com um assistente inteligente que te diz, em português claro, onde você pode economizar e ganhar mais.
>
> **Comece grátis. Veja seu lucro hoje mesmo.**

**CTAs sugeridos:** “Comece grátis” · “Veja seu lucro do mês” · “Fale no WhatsApp”.

---

## 8. Jornada do usuário (resumo)

1. **Descoberta** (indicação, feira, cooperativa, anúncio) → instala o PWA/app.
2. **Onboarding em 1 tela:** nome da propriedade, nome, o que produz. Pronto para usar.
3. **Primeiro valor:** registra 1 venda + 1 gasto → vê o card de lucro do mês.
4. **Rotina diária:** abre o app, vê tarefas de hoje e avisos, registra vendas/gastos.
5. **Rotina mensal:** abre Relatórios, exporta PDF, lê o resumo da IA.
6. **Expansão:** passa a usar plantio/estoque/animais → faz upgrade de plano.

Fluxo de telas detalhado em [`04-telas-ux-fluxos.md`](04-telas-ux-fluxos.md).
