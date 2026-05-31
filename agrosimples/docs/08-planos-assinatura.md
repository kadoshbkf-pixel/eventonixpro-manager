# 08 — Planos de Assinatura

Modelo SaaS por assinatura. Cada plano libera módulos e recursos conforme a
necessidade do produtor. Sempre há **7 a 14 dias de teste grátis**.

## 1. Comparativo de planos

| Recurso | Básico | Intermediário | Premium | Cooperativa |
|---------|:------:|:-------------:|:-------:|:-----------:|
| Cadastro de propriedade | ✅ | ✅ | ✅ | ✅ |
| Cadastro de clientes | ✅ | ✅ | ✅ | ✅ |
| Controle de vendas | ✅ | ✅ | ✅ | ✅ |
| Controle financeiro simples | ✅ | ✅ | ✅ | ✅ |
| Agenda de tarefas | ✅ | ✅ | ✅ | ✅ |
| Relatório mensal básico | ✅ | ✅ | ✅ | ✅ |
| Controle de plantio | — | ✅ | ✅ | ✅ |
| Controle de colheita | — | ✅ | ✅ | ✅ |
| Controle de estoque + insumos | — | ✅ | ✅ | ✅ |
| Contas a pagar / a receber | — | ✅ | ✅ | ✅ |
| Relatórios por cultura | — | ✅ | ✅ | ✅ |
| Exportação PDF | — | ✅ | ✅ | ✅ |
| Controle de animais | — | — | ✅ | ✅ |
| Controle por talhão | — | — | ✅ | ✅ |
| Dashboard completo | — | — | ✅ | ✅ |
| IA — análise financeira | — | — | ✅ | ✅ |
| IA — perguntas e respostas | — | — | ✅ | ✅ |
| Múltiplos usuários | — | — | ✅ | ✅ (ilimitado) |
| Backup em nuvem | — | — | ✅ | ✅ |
| Exportação Excel | — | — | ✅ | ✅ |
| Alertas inteligentes | — | — | ✅ | ✅ |
| Suporte prioritário | — | — | ✅ | ✅ |
| Cadastro de vários produtores | — | — | — | ✅ |
| Relatórios consolidados | — | — | — | ✅ |
| Controle individual por propriedade | — | — | — | ✅ |
| Painel administrativo geral | — | — | — | ✅ |
| Comparativo entre propriedades | — | — | — | ✅ |
| Gestão de produção conjunta | — | — | — | ✅ |

## 2. Para quem é cada plano

### 🟢 Plano Básico — produtor pequeno
"Quero parar de anotar no caderno." Vende, registra gastos e vê o resultado.

### 🔵 Plano Intermediário — produtor em crescimento
Já controla produção: plantio, colheita, estoque e contas a pagar/receber.

### 🟣 Plano Premium — produtor organizado / pequena associação
Tudo + animais + IA + dashboard completo + múltiplos usuários e backup.

### 🟠 Plano Cooperativa/Associação — várias propriedades
Painel geral, relatórios consolidados e comparação entre propriedades.

## 3. Lógica de cobrança e limites (regras)

- **Trial:** acesso ao plano Premium por X dias; ao fim, escolhe um plano (ou
  cai para um modo limitado/somente leitura até assinar).
- **Mensal ou anual** (anual com desconto). Pagamento por Pix/cartão.
- **Limites por plano** controlados pelo campo `tenant.plan`:
  - Básico/Intermediário: **1 usuário** (produtor).
  - Premium: **até N usuários** (ex.: 5).
  - Cooperativa: usuários e propriedades **ilimitados**.
- **Gate de recurso:** ao tocar num recurso fora do plano, mostrar tela amigável
  "Esse recurso está no plano Premium — veja os benefícios" (upsell suave).
- **Downgrade:** dados antigos preservados (somente leitura nos módulos que
  saíram do plano), nunca apagados.

## 4. Sugestão de precificação (a validar no mercado)

> Valores ilustrativos — ajustar à realidade regional. A regra de ouro é
> **"ser barato"** para o pequeno produtor.

| Plano | Faixa sugerida (mensal) |
|-------|--------------------------|
| Básico | baixo / acessível |
| Intermediário | médio |
| Premium | médio-alto |
| Cooperativa | por número de propriedades (pacote) |

A âncora de valor é simples: **"custa menos do que uma perda de uma colheita
mal calculada."**
