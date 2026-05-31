# AgroSimples — IA e Automações

## 1. Assistente AgroSimples — papel
Um **consultor rural amigável** que fala em linguagem simples, sem termos técnicos, sempre com um próximo passo prático. Nunca despeja números crus: traduz para ação (“que tal cobrar hoje?”, “pense em produzir mais alface”).

### Funções
1. **Análise financeira automática** — “Você gastou mais do que vendeu este mês.”, “Sua maior despesa foi com ração.”, “A cultura mais lucrativa foi alface.”
2. **Perguntas por texto** — “Quanto lucrei?”, “Quem está me devendo?”, “O que tenho para colher?”, “Quanto preciso vender para pagar meus custos?”
3. **Sugestões inteligentes** — comprar insumo antes de acabar, cobrar cliente atrasado, rever frete, separar dinheiro pessoal do da propriedade.
4. **Resumos automáticos** — “Esta semana você vendeu R$ 2.450, gastou R$ 1.300 e teve lucro de R$ 1.150. Há 3 contas a pagar e 2 clientes em atraso.”

---

## 2. Implementação em camadas

### Camada 1 — Regras locais (offline, já no MVP)
A função `analyze(db)` em `agrosimples/index.html` gera insights determinísticos a partir dos dados do mês: lucro/prejuízo, maior despesa, melhor produto, clientes atrasados, estoque baixo, ponto de equilíbrio. O chat responde por palavras-chave (lucro, vendas, dívida, gastos, tarefas, contas). **Funciona sem internet e sem custo.**

### Camada 2 — LLM (Fase 3, online)
Edge Function `ai-analyze` monta um snapshot agregado (somente números + rótulos, sem PII desnecessária) e chama um LLM com o **prompt de persona** abaixo. Respostas curtas, em PT-BR simples. Resultados cacheados em `ai_insights`.

```
Você é o "Assistente AgroSimples", um consultor rural amigável que ajuda
pequenos produtores. Fale em português simples, sem termos técnicos.
Seja direto, gentil e sempre termine com uma sugestão prática.
Use os dados abaixo (em reais) para responder à pergunta do produtor.
Nunca invente números; se faltar dado, diga que ainda não há registro.

DADOS DO MÊS: {json_agregado}
PERGUNTA: {pergunta}
```

> A Camada 1 é o **fallback** quando offline ou sem créditos de IA — o produtor nunca fica sem resposta.

---

## 3. Automações

| Automação | Gatilho | Ação |
|-----------|---------|------|
| Conta vencendo | `due_date` ≤ 3 dias | aviso no app (+ WhatsApp/e-mail nas fases pagas) |
| Cliente atrasado | recebível vencido | sugere cobrança + botão WhatsApp pronto |
| Estoque baixo | saldo ≤ `min_qty` | aviso + sugestão de compra |
| Vacina próxima | `health_records.next_date` ≤ 7 dias | lembrete de vacina/medicamento |
| Colheita prevista | `expected_harvest_date` ≤ 7 dias | tarefa “colher” + aviso |
| Tarefa atrasada | `date < hoje` e pendente | destaque + notificação |
| Resumo semanal | toda segunda | mensagem-resumo (app/WhatsApp) |
| Relatório mensal | dia 1 | gera PDF do mês anterior |
| Sugerir compra de insumo | insumo acabando | item sugerido na lista de compras |
| Sugerir cobrança | recebíveis em aberto | lista priorizada por valor/atraso |

**Canais:** in-app (MVP) → WhatsApp Cloud API e e-mail (fases pagas). Mensagens em linguagem simples e com ação de 1 toque (ex.: link `wa.me` já preenchido para cobrança).

---

## 4. Mensagens-modelo (WhatsApp)
- **Cobrança:** “Olá, {cliente}! Aqui é do {propriedade}. Passando para lembrar do valor de {R$ X} referente a {produto}, com vencimento em {data}. Qualquer dúvida, é só chamar. Obrigado! 🙂”
- **Resumo semanal (produtor):** “📊 Resumo da semana: vendeu {R$}, gastou {R$}, lucro de {R$}. Pendências: {n} contas a pagar e {n} clientes em atraso.”
