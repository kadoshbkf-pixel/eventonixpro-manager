# 10 — Inteligência Artificial: Assistente AgroSimples

O **Assistente AgroSimples** é um consultor rural amigável dentro do sistema.
Fala de forma **simples e prática**, sem termos técnicos, e ajuda o produtor a
entender seu dinheiro e sua produção.

## 1. Quatro funções da IA

### 1.1 Análise financeira automática
A IA olha os dados e avisa, em linguagem simples:
- "Você gastou mais do que vendeu este mês."
- "Sua maior despesa foi com ração."
- "A cultura mais lucrativa foi a alface."
- "O cliente João está com pagamento atrasado."
- "O plantio de tomate teve muita perda."
- "Seu custo por hectare está alto."

### 1.2 Perguntas por texto
O produtor pergunta com as próprias palavras:
- "Quanto lucrei este mês?"
- "Qual produto vendeu mais?"
- "Quem está me devendo?"
- "O que tenho para colher esta semana?"
- "Qual plantio deu prejuízo?"
- "Quanto gastei com ração?"
- "Qual cliente comprou mais?"
- "Quanto preciso vender para pagar meus custos?"

### 1.3 Sugestões inteligentes
- Comprar insumo antes de acabar.
- Reduzir gasto em determinado item.
- Aumentar o preço de venda.
- Cobrar cliente atrasado.
- Evitar plantio em área de baixa produtividade.
- Revisar custo de frete.
- **Separar o dinheiro pessoal do dinheiro da propriedade.**

### 1.4 Resumos automáticos (semanal)
> "Esta semana você vendeu R$ 2.450, gastou R$ 1.300 e teve lucro estimado de
> R$ 1.150. Existem 3 contas a pagar e 2 clientes em atraso."

## 2. Tom de voz da IA

- Como um **amigo que entende de roça e de dinheiro**.
- Frases curtas. Sem jargão. Sempre com um próximo passo prático.
- Nunca culpa o produtor; sugere melhorias com gentileza.
- Usa os termos do glossário ("dinheiro que entrou", "o que sobrou").

**Exemplo de resposta:**
> Pergunta: "Quanto lucrei este mês?"
> Resposta: "Neste mês você vendeu **R$ 3.200** e gastou **R$ 2.500**. Sobrou
> **R$ 700** de lucro 👍. Seu maior gasto foi com **ração (R$ 900)**. Dica: o
> cliente Maria ainda deve R$ 300 — cobrar agora deixaria seu lucro em R$ 1.000."

## 3. Como funciona por dentro (arquitetura da IA)

```
Pergunta do produtor
   │
   ▼
Backend monta um RESUMO AGREGADO seguro:
   - totais do período (vendas, gastos, lucro)
   - maiores despesas por categoria
   - clientes em aberto
   - estoque baixo, colheitas da semana, vacinas pendentes
   │  (NÃO envia a base inteira — só números já calculados)
   ▼
Chamada server-side à API da Anthropic (Claude)
   - system prompt: "Você é o Assistente AgroSimples, consultor rural
     amigável. Responda em português simples, frases curtas, com 1 dica
     prática. Use os dados fornecidos. Não invente números."
   - dados: o resumo agregado em JSON
   ▼
Resposta em linguagem simples → exibida no chat (+ número/gráfico quando útil)
   │
   ▼
Registra em `ai_interactions` (pergunta, resposta, snapshot)
```

**Princípios técnicos:**
- **Cálculo é do sistema, redação é da IA.** Os números vêm do backend (fonte da
  verdade); a IA só explica. Isso evita erros de conta e reduz custo.
- **Privacidade:** enviar o mínimo necessário; não expor dados de outros tenants.
- **Custo:** usar modelo econômico (ex.: Haiku) para resumos/perguntas comuns;
  reservar modelo mais forte para análises mais complexas.
- **Cache:** respostas de perguntas frequentes (ex.: "lucro do mês") em cache
  até novo lançamento.
- **Disponibilidade:** IA é recurso dos planos **Premium** e **Cooperativa**.
- **Offline:** sem internet, a IA fica indisponível, mas os números do dashboard
  (calculados localmente) continuam visíveis.

## 4. Perguntas → fonte de dados (mapa)

| Pergunta | De onde vem a resposta |
|----------|------------------------|
| Quanto lucrei? | `finance_entries` (entradas − saídas no período) |
| Quem me deve? | `finance_entries` (a_receber, atrasado) + `customers` |
| O que colho esta semana? | `plantings.expected_harvest_at` |
| Qual produto vendeu mais? | `sale_items` agregado por produto |
| Quanto gastei com ração? | `finance_entries` (category=racao) |
| Qual plantio deu prejuízo? | `plantings` (profit < 0) |
| Qual cliente comprou mais? | `sales` agregado por cliente |

## 5. Salvaguardas

- A IA **não executa ações financeiras sozinha** (não paga, não cobra) — apenas
  sugere e oferece botões para o produtor confirmar.
- Sempre deixar claro quando um número é **estimativa** (ex.: previsão de
  produção) versus **realizado**.
- Mensagem de rodapé discreta: "Sou um assistente e posso errar; confira sempre
  seus números."
