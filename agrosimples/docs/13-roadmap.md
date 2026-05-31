# 13 — Roadmap de Desenvolvimento por Fases

Estratégia: **entregar valor cedo** (Fase 1), depois aprofundar produção
(Fase 2) e por fim escalar com animais, offline completo e mobile (Fase 3).

## Fase 1 — MVP (valor rápido)

**Meta:** produtor registra venda e gasto e vê o lucro do mês.

- Login + recuperação de senha (Auth)
- Cadastro de propriedade
- Cadastro de produtos e clientes
- Registro de vendas e despesas
- Contas a pagar e a receber
- Agenda de tarefas
- Dashboard básico (vendas, gastos, **lucro**)
- Relatório financeiro simples + exportação PDF
- IA básica (resumo financeiro)
- PWA instalável + base multi-tenant + RLS
- Cobrança de assinatura (planos Básico/Intermediário)

➡️ Critérios de pronto em [06 — MVP](06-mvp.md).

## Fase 2 — Produção

**Meta:** controlar o que se planta, colhe e guarda.

- Controle de plantio (custos, status, lucro por plantio)
- Controle de colheita (atualiza estoque)
- Controle de estoque de produção
- Controle de insumos + alertas
- Relatórios por cultura e por talhão
- Exportação Excel/CSV
- Automações de estoque e colheita
- Plano Premium liberado (parcial)

## Fase 3 — Escala e inteligência

**Meta:** propriedade completa, mobilidade e IA avançada.

- Controle de animais (sanidade, produção, custo/lucro por lote)
- **Modo offline completo** (app)
- App mobile nativo (React Native/Expo)
- IA avançada (análises, sugestões, perguntas abertas)
- Dashboard completo + alertas inteligentes
- Backup em nuvem + relatórios avançados
- Integração WhatsApp (API oficial)
- **Plano Cooperativa** (multipropriedade, painel consolidado, comparativos)

## Visão em linha do tempo

```
Trimestre 1   ████ Fase 1 — MVP (lançar e validar)
Trimestre 2   ████ Fase 2 — Produção (plantio/colheita/estoque)
Trimestre 3   ████ Fase 3 — Animais, offline, mobile, IA avançada
Trimestre 4   ████ Cooperativa + otimizações + expansão comercial
```

## Critérios para avançar de fase

- **F1 → F2:** MVP estável, primeiros assinantes ativos, métricas de ativação OK.
- **F2 → F3:** produtores usando produção/estoque com regularidade.
- **F3 → Cooperativa:** demanda real de associações + estabilidade do app mobile.

## Riscos e mitigação

| Risco | Mitigação |
|-------|-----------|
| Produtor achar complexo | UX simples, treinar só 5 ações, implantação assistida |
| Internet ruim no campo | Offline-first, PWA leve, sync por fila |
| Custo de IA | Resumos agregados, modelo econômico, cache |
| Dados financeiros errados | Cálculo no backend (fonte da verdade), validações |
| Abandono após trial | Resumo semanal por WhatsApp + análise mensal assistida |
