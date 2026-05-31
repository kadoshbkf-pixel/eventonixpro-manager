# 🌱 AgroSimples — Gestão Inteligente para Pequenos Produtores

> **“O sistema que mostra se sua propriedade está dando lucro de verdade.”**

AgroSimples é um sistema de gestão rural **simples, mobile-first e offline** para pequenos produtores e agricultura familiar. Em poucos toques o produtor registra vendas e gastos, controla contas a pagar/receber, organiza tarefas e descobre, com a ajuda de um assistente de IA, **se está tendo lucro ou prejuízo**.

---

## 📂 O que tem neste repositório

```
agrosimples/                # APP — MVP funcional (PWA)
├─ index.html               # aplicação React single-file (sem build)
├─ manifest.webmanifest     # PWA instalável
└─ sw.js                    # Service Worker (funciona offline)

docs/                       # DOCUMENTAÇÃO profissional (20 entregáveis)
├─ README.md                # índice
├─ 01-visao-produto.md
├─ 02-banco-de-dados.md
├─ 03-arquitetura-tecnologias.md
├─ 04-telas-ux-fluxos.md
├─ 05-perfis-planos-regras.md
├─ 06-mvp-roadmap.md
├─ 07-ia-automacoes.md
├─ 08-implantacao-checklist.md
└─ 09-prompt-tecnico-identidade.md
```

> Observação: o `index.html` na raiz do repositório é outro projeto (EventPro Manager) e foi **preservado**. O AgroSimples vive na pasta `agrosimples/`.

---

## ▶️ Como rodar o MVP

É um app single-file, sem dependências de build. Basta servir a pasta:

```bash
cd agrosimples
python3 -m http.server 8080
# abra http://localhost:8080 no navegador (ou no celular, na mesma rede)
```

Ou abra `agrosimples/index.html` direto no navegador (o Service Worker exige `http://`/`https://` para o modo offline completo).

**No celular:** abra o endereço, toque em “Adicionar à tela inicial” → o AgroSimples vira um app instalado e funciona **sem internet**.

---

## ✨ O que o MVP já faz

- ✅ Onboarding em 1 tela + dados de exemplo para demonstração
- ✅ **Dashboard** com lucro do mês, a pagar/receber, avisos e tarefas de hoje
- ✅ **Vendas** (baixa estoque automática + gera “a receber” quando fiado)
- ✅ **Gastos** com categorias rurais
- ✅ **Contas a pagar e a receber** (marcar pago/recebido, alerta de atraso, WhatsApp)
- ✅ **Agenda de tarefas** rurais
- ✅ **Relatórios** (resumo, ranking de produtos e gastos) + export **PDF** e **CSV/Excel**
- ✅ **Assistente AgroSimples** — análises por regras + chat de perguntas (offline)
- ✅ Cadastro de **propriedade, áreas, produtos e clientes**
- ✅ **Offline** (PWA) + **backup** export JSON
- ✅ Linguagem simples (“Dinheiro que entrou”, “Quem me deve”), ícones grandes, tema verde

**Próximas fases:** plantio, colheita, estoque/insumos → animais, IA avançada (LLM), app nativo, plano cooperativa. Detalhes em [`docs/06-mvp-roadmap.md`](docs/06-mvp-roadmap.md).

---

## 🧱 Tecnologia
MVP atual: **React (via CDN) single-file + PWA + localStorage**, custo zero de infraestrutura para validar em campo. Evolução planejada: **Next.js + Supabase (PostgreSQL/RLS) + Edge Functions + LLM + WhatsApp**. Ver [`docs/03-arquitetura-tecnologias.md`](docs/03-arquitetura-tecnologias.md).

---

## 📖 Documentação completa
Comece por [`docs/README.md`](docs/README.md).
