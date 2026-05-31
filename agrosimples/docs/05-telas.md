# 05 — Telas do Sistema

Padrão de todas as telas: **simples, limpa, responsiva**, com poucos botões,
ícones grandes, botão de **ajuda (?)** e mensagens de sucesso/erro claras.

Convenções de mensagens:
- ✅ **Sucesso:** "Pronto! Sua venda foi registrada."
- ⚠️ **Erro de validação:** "Faltou preencher o valor. Toque aqui para corrigir."
- ❌ **Erro de sistema:** "Não conseguimos salvar agora. Guardamos seus dados e
  vamos enviar quando a internet voltar." (modo offline)

---

## 1. Login
- **Campos:** e-mail/telefone, senha. Links: "Esqueci minha senha", "Criar conta".
- **Botões:** [Entrar] (grande), [Criar conta grátis].
- **Extra:** opção "Continuar conectado". Biometria no app.
- **Erros:** "E-mail ou senha incorretos."

## 2. Dashboard (Início)
- **Cards grandes:** Vendas do mês · Gastos do mês · **Lucro do mês** (destaque).
- **Listas rápidas:** Tarefas de hoje · Contas a pagar · Quem me deve · Estoque baixo.
- **Avisos da IA:** "Esta semana você gastou mais do que vendeu."
- **Botão central [+]:** Registrar venda / gasto.

## 3. Cadastro da Propriedade
- **Campos:** nome da propriedade, nome do produtor, CPF/CNPJ, endereço,
  tamanho (ha), tipo de produção.
- **Botões:** [Salvar], [Adicionar área].
- **Ajuda:** "A propriedade é a sua base. Depois você cadastra os talhões."

## 4. Cadastro de Áreas / Talhões
- **Campos:** nome (ex.: Talhão 1), tipo (talhão/pasto/estufa/galpão/curral/
  reservatório), tamanho (ha), uso atual.
- **Lista:** mostra todas as áreas com cor por tipo.
- **Botões:** [+ Nova área], [Editar], [Excluir] (com confirmação).

## 5. Cadastro de Produtos
- **Campos:** nome, categoria, unidade padrão (kg/caixa/saco/unidade/litro),
  preço de venda.
- **Uso:** alimenta vendas e estoque de produção.

## 6. Cadastro de Clientes
- **Campos:** nome, telefone, WhatsApp, endereço, tipo de cliente.
- **Perfil do cliente:** total comprado, última compra, **valor em aberto**,
  produtos mais comprados, histórico de pagamentos.
- **Botões:** [+ Novo cliente], [Cobrar pelo WhatsApp].

## 7. Lançamento de Venda
- **Campos:** cliente, produto(s) + quantidade + valor unitário (total
  automático), data, forma de pagamento, status, data de entrega, observações.
- **Botões:** [Salvar venda], [Gerar recibo], [Enviar no WhatsApp].
- **Lógica:** baixa estoque; fiado/pendente vira conta a receber.

## 8. Lançamento de Despesa (Registrar gasto)
- **Campos:** categoria, valor, data, (opcional) centro de custo
  (talhão/animal/cultura/cliente), à vista ou a pagar, observação.
- **Botões:** [Salvar gasto].
- **Ajuda:** "Registre tudo que sai: ração, adubo, combustível, frete..."

## 9. Contas a Pagar (O que tenho a pagar)
- **Lista:** descrição, valor, vencimento, status (em dia/atrasado).
- **Filtros:** vence hoje, esta semana, atrasadas.
- **Botões:** [Marcar como pago], [+ Nova conta].

## 10. Contas a Receber (Quem me deve)
- **Lista:** cliente, valor, vencimento, dias em atraso.
- **Botões:** [Marcar como recebido], [Cobrar no WhatsApp].

## 11. Controle de Plantio
- **Campos:** cultura, variedade, área (talhão), data plantio, previsão de
  colheita, sementes/mudas, custos (sementes, adubo, defensivos, mão de obra,
  máquinas, combustível), status, observações.
- **Mostra calculado:** custo total, custo/ha, previsão x produção, **lucro/prejuízo**, % perda.
- **Botões:** [Salvar plantio], [Registrar colheita].

## 12. Controle de Colheita
- **Campos:** plantio/cultura, data, quantidade colhida, unidade, quantidade
  perdida, motivo da perda, destino.
- **Lógica:** atualiza estoque e produção real do plantio.

## 13. Controle de Estoque (produção)
- **Lista:** produto, saldo atual, unidade, validade, local.
- **Alertas:** estoque baixo, próximo do vencimento, parado há muitos dias.

## 14. Controle de Insumos
- **Lista:** item, categoria, quantidade, unidade, custo, estoque mínimo, validade.
- **Alertas:** insumo acabando, vencendo.
- **Botões:** [+ Entrada de insumo], [Registrar uso].

## 15. Controle de Animais
- **Lista:** identificação, espécie, finalidade, status, próxima vacina.
- **Detalhe do animal/lote:** dados, sanidade (vacinas/medicamentos com próxima
  data), produção (leite/ovos/peso), custo e **lucro por animal/lote**.
- **Botões:** [+ Animal/Lote], [Registrar vacina], [Registrar produção].

## 16. Agenda de Tarefas
- **Visão:** lista de hoje / semana, com cor por prioridade.
- **Campos da tarefa:** título, tipo, data, horário, prioridade, responsável,
  status, observação.
- **Botões:** [+ Nova tarefa], [Concluir].
- **Alertas:** notificação/WhatsApp; vencidas viram "Atrasada".

## 17. Relatórios
- **Tipos:** financeiro mensal, vendas, despesas, por cultura, por talhão, por
  animal/lote, estoque, clientes, produtividade, lucro/prejuízo.
- **Filtros:** período, área, cliente, cultura.
- **Botões:** [Gerar], [Exportar PDF], [Exportar Excel], [Exportar CSV].

## 18. Assistente de IA
- **Interface:** chat simples. Campo "Pergunte aqui...".
- **Sugestões prontas (chips):** "Quanto lucrei este mês?", "Quem me deve?",
  "O que colho esta semana?".
- **Respostas:** linguagem simples + 1 gráfico/numero quando útil.

## 19. Configurações
- **Itens:** dados da conta, preferências de notificação (app/WhatsApp/e-mail),
  backup/exportar dados, tema, idioma, termos e privacidade (LGPD).

## 20. Gestão de Usuários (Admin)
- **Lista:** usuários, papel, status.
- **Botões:** [+ Convidar usuário], [Definir permissões], [Desativar].

## 21. Planos e Assinatura
- **Mostra:** plano atual, recursos, data de cobrança.
- **Botões:** [Mudar de plano], [Ver benefícios], [Falar com suporte].
- **Comparativo:** Básico × Intermediário × Premium × Cooperativa.

---

## Padrões de UX aplicados a todas as telas
- Poucos botões, linguagem simples, ícones grandes, cores suaves.
- Alertas bem visíveis; relatórios fáceis de entender.
- Sempre um exemplo no estado vazio e um **botão de ajuda (?)**.
- Confirmação antes de excluir; nada é apagado de fato (soft delete).
