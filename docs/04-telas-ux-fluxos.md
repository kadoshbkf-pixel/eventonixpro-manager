# AgroSimples — Telas, UX e Fluxos

## Princípios de UX
Poucos botões por tela · ícones grandes · cores suaves (verde/branco) · nada poluído · alertas bem visíveis · linguagem do produtor · exemplos em todo campo · botão de ajuda em cada tela · funciona muito bem no celular.

**Linguagem (de → para):**
| Técnico | No AgroSimples |
|---|---|
| Lançamento financeiro | Registrar gasto |
| Receita operacional | Dinheiro que entrou |
| Despesa variável | Gastos do mês |
| Conta a receber | Quem me deve |
| Margem de lucro | Quanto sobra de cada venda |

Mensagens: **erro** curto e gentil (“Diga o que foi vendido 🙂”); **sucesso** com confirmação visual (toast verde “Venda salva!”).

---

## Navegação (mobile-first)
Barra inferior com 8 ícones grandes: **Início · Vendas · Gastos · Contas · Tarefas · Relatórios · Assistente · Mais**. No desktop, a mesma navegação vira menu lateral. Botões de ação rápida “+ Venda” e “+ Gasto” sempre a um toque.

---

## Fluxo de telas (mapa)

```
Login ─► Onboarding (1 tela) ─► INÍCIO (Dashboard)
                                  ├─► Vendas ─► [+ Nova venda] (modal)
                                  ├─► Gastos ─► [+ Novo gasto] (modal)
                                  ├─► Contas ─► abas A receber / A pagar ─► [+]
                                  ├─► Tarefas ─► [+ Nova tarefa]
                                  ├─► Relatórios ─► export PDF / CSV
                                  ├─► Assistente (insights + chat)
                                  └─► Mais ─► Propriedade · Produtos · Clientes · Áreas · Plano · Backup
```

---

## Detalhe das 21 telas

| # | Tela | Campos / elementos principais | Ações |
|---|------|-------------------------------|-------|
| 1 | **Login** | telefone/e-mail, senha ou OTP, “esqueci a senha” | Entrar, recuperar senha |
| 2 | **Onboarding** | nome da propriedade, nome do produtor, o que produz, tamanho | Começar / usar dados de exemplo |
| 3 | **Início/Dashboard** | card de lucro do mês, a receber, a pagar, avisos, tarefas de hoje, atalhos +Venda/+Gasto | abrir cada módulo, concluir tarefa |
| 4 | **Cadastro da propriedade** | nome, produtor, CPF/CNPJ, endereço, área total, tipo | salvar |
| 5 | **Áreas/Talhões** | nome, tipo, tamanho (ha), uso atual | adicionar, excluir |
| 6 | **Produtos** | nome, unidade, preço de venda, estoque, categoria | adicionar, excluir |
| 7 | **Clientes** | nome, WhatsApp, tipo, endereço; total comprado, em aberto | adicionar, WhatsApp, excluir |
| 8 | **Lançar venda** | produto (autocompletar), qtd, unidade, preço, total automático, cliente, pagamento, situação | salvar (baixa estoque + gera a receber se fiado) |
| 9 | **Lançar gasto** | valor, categoria, observação, data | salvar |
| 10 | **Contas a pagar** | nome, categoria, valor, vencimento, status | adicionar, marcar pago |
| 11 | **Contas a receber** | cliente, valor, vencimento, referente a, status | adicionar, marcar recebido, WhatsApp cobrança |
| 12 | **Plantio** (F2) | cultura, variedade, área, datas, custos, status | salvar, ver custo/ha e lucro |
| 13 | **Colheita** (F2) | cultura, data, qtd, unidade, perda, destino | salvar (atualiza estoque) |
| 14 | **Estoque** (F2) | produto/insumo, saldo, validade, local, alertas | entrada/saída |
| 15 | **Insumos** (F2) | item, categoria, qtd, mínimo | entrada/saída, alerta de reposição |
| 16 | **Animais** (F3) | identificação, espécie, raça, sexo, nascimento, finalidade, sanitário, produção | cadastrar, registrar vacina/produção |
| 17 | **Agenda de tarefas** | título, tipo, data, prioridade, responsável, status | adicionar, concluir |
| 18 | **Relatórios** | resumo do mês, ranking de produtos, maiores gastos | exportar PDF, Excel/CSV, imprimir |
| 19 | **Assistente de IA** | lista de insights + chat com sugestões rápidas | perguntar, ler resumo |
| 20 | **Configurações** | propriedade, plano, backup, apagar dados, ajuda | editar, backup, sair |
| 21 | **Planos e assinatura** | comparativo de planos, recursos, preço | assinar/upgrade |

---

## Estados de cada tela
- **Vazio:** ícone grande + frase amigável (“Nenhuma venda ainda. Toque em + Nova venda.”).
- **Carregando:** esqueleto leve; nunca tela branca.
- **Offline:** badge “Offline (salvo no aparelho)” e, quando houver fila, “dados pendentes de sincronização”.
- **Sucesso:** toast verde. **Erro:** toast/realce gentil, sem termos técnicos.

---

## Acessibilidade e campo (roça)
- Alvo de toque ≥ 44px, fontes grandes (16px+), alto contraste.
- Funciona com uma mão e com luz do sol (cores sólidas, pouco cinza claro).
- Tudo essencial acessível **sem internet**.
