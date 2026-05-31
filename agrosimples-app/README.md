# 🌱 AgroSimples — App (MVP funcional)

Aplicativo de demonstração do **AgroSimples**, implementando o MVP descrito na
documentação (`../agrosimples/docs/06-mvp.md`).

## Como usar

Abra o arquivo **`index.html`** em qualquer navegador (celular ou computador).
Não precisa instalar nada nem ter servidor — funciona offline.

> No celular, use "Adicionar à tela inicial" para instalar como app (PWA).

## O que já funciona

- **Login simples** (só o nome) — dados salvos no próprio aparelho.
- **Dashboard** com **Lucro do mês**, vendas, gastos, quem me deve, a pagar,
  tarefas de hoje e um aviso do Assistente AgroSimples.
- **Registrar venda** (cliente, produto, quantidade, forma de pagamento) com
  baixa para "quem me deve" quando é fiado/pendente.
- **Registrar gasto** por categoria, com opção de "conta a pagar".
- **Quem me deve** (contas a receber) com botão de **cobrança no WhatsApp**.
- **O que tenho a pagar** (contas a pagar) com "marcar pago".
- **Clientes** com total comprado e valor em aberto.
- **Agenda de tarefas** com prioridade e marcação de concluída.
- **Relatório do mês** com exportação por **Imprimir/Salvar PDF**.

## Tecnologia

- React (via CDN) + Babel no navegador — mesmo padrão do projeto.
- Armazenamento local (`localStorage`) → funciona **sem internet**.
- Identidade visual conforme `../agrosimples/docs/14-identidade-visual.md`
  (verde = lucro, vermelho = gasto/dívida).

## Limitações desta versão (demo)

- Dados ficam **apenas neste aparelho** (sem nuvem/sincronização ainda).
- Sem login real/multiusuário, sem backend.
- Para produção: migrar para Next.js + Supabase conforme
  `../agrosimples/docs/07-arquitetura-tecnologias.md` e
  `../agrosimples/docs/17-prompt-tecnico.md`.

## Próximos passos (fases 2 e 3)

Plantio, colheita, estoque, insumos, animais, modo offline com sincronização,
IA avançada e plano cooperativa — ver `../agrosimples/docs/13-roadmap.md`.
