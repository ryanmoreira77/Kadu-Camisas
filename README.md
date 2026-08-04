# Kadu Camisas de Time — Sistema de Gestão de Estoque

Versão atual: **Dashboard e Catálogo são públicos** (qualquer cliente pode
acessar direto, sem login). **Produtos e Configurações** pedem apenas uma
**senha** para editar — por trás continua sendo um login real no Supabase
(o e-mail já vem fixo no código), então a proteção é de verdade, no banco
de dados, não só um botão escondido na tela.

## ⚠️ Passo obrigatório antes de usar esta versão

Como antes era necessário estar logado até para *ver* os dados, e agora
não é mais, é preciso **liberar a leitura pública** no banco:

1. No painel do Supabase, vá em **SQL Editor > New query**.
2. Cole todo o conteúdo do arquivo `database/atualizar_acesso_publico.sql`
   e clique em **Run**.

Sem isso, o Dashboard e o Catálogo vão aparecer vazios pra quem não
estiver logado (a leitura continua bloqueada).

> Isso só precisa ser feito uma vez. Cadastrar, editar e excluir continuam
> exigindo a senha — essa parte não muda.

## Estrutura das páginas

- `index.html` → agora só redireciona para `dashboard.html` (não existe
  mais tela de login com e-mail/senha).
- `dashboard.html` → público. Visão geral do estoque (sem preços).
- `catalogo.html` → público. Catálogo de camisas com foto para os
  clientes verem/baixarem em PDF (sem preços).
- `produtos.html` → **protegido por senha**. Cadastro completo das
  camisas (nome, time, categoria, tamanhos, foto etc. — sem preço).
- `configuracoes.html` → **protegido por senha**. Logo, nome da empresa,
  cor de destaque e dados da loja.

## Como funciona a senha

A senha da loja é literalmente a senha da conta já cadastrada no
Supabase (Authentication > Users), com o e-mail
`jenniferrodriguesruffo@gmail.com` fixado no `assets/js/shell.js`. Se um
dia precisar trocar a senha, é só trocar lá no painel do Supabase
(Authentication > Users > editar usuário) — não precisa mexer em nada no
código.

## Configuração do banco (Supabase)

As credenciais já estão preenchidas em `assets/js/supabaseClient.js`. Só
é preciso, uma única vez:

1. Rodar `database/schema.sql` (tabelas, segurança e função
   `registrar_movimentacao` — essa última não é mais usada nessa versão,
   mas não atrapalha manter).
2. Rodar `database/atualizar_acesso_publico.sql` (libera a leitura
   pública, ver acima).
3. Criar o bucket **fotos** no Storage, marcado como público (se ainda
   não tiver criado).

## Estrutura de pastas

```
kadu-sistema/
├── index.html            → redireciona para dashboard.html
├── dashboard.html          → público
├── produtos.html            → protegido por senha
├── catalogo.html              → público
├── configuracoes.html          → protegido por senha
├── database/
│   ├── schema.sql               → schema original (tabelas, RLS, função)
│   └── atualizar_acesso_publico.sql → libera leitura pública (rodar 1x)
└── assets/
    ├── css/style.css
    ├── img/logo.svg
    └── js/
        ├── data.js, supabaseClient.js, auth-cloud.js, brand.js
        ├── shell.js         → menu, cabeçalho e a telinha de senha
        └── dashboard.js, produtos.js, catalogo.js, configuracoes.js
```
