# 04 — Jornada do Usuário e Fluxo de Telas

## 1. Jornada do produtor (do zero ao "sei meu lucro")

```
1. CONHECE        → vê anúncio/indicação: "sistema que mostra seu lucro"
2. EXPERIMENTA    → cria conta grátis (trial), responde 5 perguntas rápidas
3. CONFIGURA      → cadastra propriedade e 1ª área (assistido)
4. USA NO DIA     → registra venda e gasto (2 toques cada)
5. ENTENDE        → abre o dashboard e vê "Lucro do mês"
6. CONFIA         → recebe resumo semanal da IA no WhatsApp
7. ASSINA         → escolhe um plano ao fim do trial
8. CRESCE         → ativa plantio, estoque, animais conforme precisa
```

### Jornada diária (a mais importante)
O produtor abre o app **uma vez por dia**, geralmente à noite:

```
Abre app → vê "Tarefas de hoje" + "Lucro do mês"
        → toca em [Registrar venda]  (escolhe cliente, produto, valor) → salva
        → toca em [Registrar gasto]  (escolhe categoria, valor) → salva
        → vê o lucro atualizar na hora
        → marca tarefas concluídas
```

## 2. Fluxo de telas (mapa de navegação)

```
                         ┌──────────────┐
                         │    LOGIN     │
                         └──────┬───────┘
                                │
                         ┌──────▼───────┐
                         │  DASHBOARD   │  (tela inicial)
                         └──────┬───────┘
        ┌───────────────┬──────┼───────┬────────────────┐
        │               │      │       │                │
   ┌────▼────┐   ┌──────▼───┐ │  ┌─────▼─────┐   ┌──────▼──────┐
   │ VENDAS  │   │ FINANCEIRO│ │  │ PRODUÇÃO  │   │  ANIMAIS    │
   ├─────────┤   ├──────────┤ │  ├───────────┤   ├─────────────┤
   │ Vender  │   │ Gasto    │ │  │ Plantio   │   │ Lote/animal │
   │ Recibo  │   │ A pagar  │ │  │ Colheita  │   │ Sanidade    │
   │ Clientes│   │ A receber│ │  │ Estoque   │   │ Produção    │
   └─────────┘   └──────────┘ │  │ Insumos   │   └─────────────┘
                              │  └───────────┘
        ┌─────────────────────┼─────────────────────┐
   ┌────▼────┐         ┌──────▼──────┐        ┌──────▼──────┐
   │ AGENDA  │         │ RELATÓRIOS  │        │ ASSISTENTE  │
   │ Tarefas │         │ PDF/Excel   │        │     IA      │
   └─────────┘         └─────────────┘        └─────────────┘

   Menu inferior fixo (mobile): [Início] [Vender] [Gasto] [Agenda] [Mais]
   "Mais" abre: Produção, Animais, Relatórios, IA, Configurações, Usuários, Planos
```

## 3. Navegação mobile (princípio)

- **Menu inferior fixo** com as 4 ações mais usadas + "Mais".
- Botão central destacado **[+]** para "Registrar venda / gasto" rápido.
- Cada tela tem **botão de ajuda (?)** no canto superior.
- Voltar sempre visível; nunca esconder o caminho de saída.

## 4. Fluxos críticos (passo a passo)

### Fluxo: Registrar uma venda
```
[Vender] → escolhe/cadastra cliente → adiciona produto(s) e quantidade
        → sistema calcula total → escolhe forma de pagamento
        → se "fiado/pendente": cria conta a receber automaticamente
        → salva → baixa estoque → oferece [Gerar recibo] / [Enviar WhatsApp]
```

### Fluxo: Registrar um gasto
```
[Gasto] → escolhe categoria (ração, adubo, frete...) → valor → data
        → (opcional) vincula a talhão/animal/cultura (centro de custo)
        → se "a pagar": vira conta a pagar com vencimento
        → salva → dashboard atualiza
```

### Fluxo: Da colheita ao estoque à venda
```
Plantio (custos) → Colheita (quantidade + destino "Estoque")
   → Estoque de produção atualizado
   → Venda baixa do estoque
   → Financeiro calcula lucro do plantio (receita − custos)
```

### Fluxo: Alerta de vacina
```
Cadastro sanitário do animal define "próxima aplicação"
   → no dia (e véspera) gera Alerta + Tarefa na agenda
   → notificação no app / WhatsApp
   → produtor marca como concluída → agenda próxima
```

## 5. Estados vazios (importante para iniciantes)

Toda tela sem dados mostra **exemplo + botão grande**:
> "Você ainda não registrou nenhuma venda. Toque em **+ Registrar venda** para
> começar. Exemplo: 10 kg de alface para o Mercado do João — R$ 50,00."
