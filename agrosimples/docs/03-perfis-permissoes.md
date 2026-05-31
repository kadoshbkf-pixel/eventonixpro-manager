# 03 — Perfis de Usuário e Permissões

## 1. Perfis

| Perfil | Para quem | Resumo do acesso |
|--------|-----------|------------------|
| **Administrador** | Dono da conta / responsável | Acesso completo, gerencia usuários, planos e configurações |
| **Produtor** | O produtor rural | Acesso à propriedade, lançamentos e relatórios |
| **Funcionário** | Ajudante / colaborador | Lança tarefas, colheitas, vendas ou despesas conforme permissão |
| **Contador** | Contabilidade | Apenas visualiza relatórios financeiros e exporta |
| **Consultor** | Consultor/técnico rural | Visualiza dados e registra sugestões/observações |

> Cada usuário pode ter **permissões finas** (campo `permissions` em `users`)
> para liberar ou restringir ações específicas além do papel padrão.

## 2. Matriz de permissões

Legenda: ✅ total · 👁️ somente leitura · ⚙️ conforme permissão · ❌ sem acesso

| Recurso | Admin | Produtor | Funcionário | Contador | Consultor |
|---------|:-----:|:--------:|:-----------:|:--------:|:---------:|
| Dashboard | ✅ | ✅ | 👁️ | 👁️ (financeiro) | 👁️ |
| Cadastro da propriedade | ✅ | ✅ | 👁️ | ❌ | 👁️ |
| Áreas / talhões | ✅ | ✅ | 👁️ | ❌ | 👁️ |
| Produtos | ✅ | ✅ | ⚙️ | ❌ | 👁️ |
| Clientes | ✅ | ✅ | ⚙️ | 👁️ | 👁️ |
| Vendas | ✅ | ✅ | ⚙️ | 👁️ | 👁️ |
| Despesas / financeiro | ✅ | ✅ | ⚙️ | 👁️ | 👁️ |
| Contas a pagar/receber | ✅ | ✅ | ⚙️ | 👁️ | 👁️ |
| Plantio | ✅ | ✅ | ⚙️ | ❌ | 👁️ |
| Colheita | ✅ | ✅ | ⚙️ | ❌ | 👁️ |
| Estoque / insumos | ✅ | ✅ | ⚙️ | ❌ | 👁️ |
| Animais | ✅ | ✅ | ⚙️ | ❌ | 👁️ |
| Agenda de tarefas | ✅ | ✅ | ⚙️ | ❌ | ⚙️ |
| Relatórios | ✅ | ✅ | 👁️ | ✅ (exporta) | 👁️ |
| Assistente IA | ✅ | ✅ | ⚙️ | 👁️ | ✅ |
| Configurações | ✅ | ⚙️ | ❌ | ❌ | ❌ |
| Gestão de usuários | ✅ | ❌ | ❌ | ❌ | ❌ |
| Planos e assinatura | ✅ | 👁️ | ❌ | ❌ | ❌ |

## 3. Regras de acesso

1. **Isolamento por propriedade/tenant** — usuário só vê os dados do seu
   `tenant_id` (ver RLS em [02 — Banco de Dados](02-banco-de-dados.md#4-segurança-rls)).
2. **Funcionário é granular** — o Administrador escolhe na criação do usuário
   exatamente o que ele pode lançar (ex.: só vendas e colheitas).
3. **Contador não edita** — sempre leitura + exportação de relatórios.
4. **Consultor sugere, não altera dados financeiros** — registra observações e
   usa a IA, mas não lança vendas/despesas.
5. **Plano define o teto** — recursos como IA, animais e múltiplos usuários só
   aparecem se o plano permitir (ver [08 — Planos](08-planos-assinatura.md)).
6. **Cooperativa** — perfil Administrador da cooperativa tem um painel
   consolidado e acesso de leitura às propriedades vinculadas.

## 4. Convite e onboarding de usuários

- Administrador convida por e-mail/WhatsApp → usuário define senha.
- Primeiro acesso mostra um **tour guiado** curto (3 a 5 telas).
- Recuperação de senha por e-mail. Senhas sempre criptografadas (hash).
