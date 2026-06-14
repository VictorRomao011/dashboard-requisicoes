# Dashboard de Requisições FY26

🔗 **Dashboard publicado:** https://victorromao011.github.io/dashboard-requisicoes/

Dashboard que lê os dados **dinamicamente** do arquivo `dados.xlsx` no mesmo diretório, usando [SheetJS](https://sheetjs.com/). Os dados **não** estão mais embutidos no HTML — para atualizar o dashboard, basta substituir o `dados.xlsx` e recarregar a página.

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | O dashboard. Carrega Chart.js e SheetJS via CDN e lê `dados.xlsx` a cada carregamento da página (com cache desabilitado). |
| `dados.xlsx` | A base de dados. É a planilha de requisições, sem nenhuma modificação de formato. |

## Como atualizar os dados (pela interface web do GitHub, sem git)

1. Edite sua planilha normalmente (no SharePoint/OneDrive/Excel) e salve uma cópia com o nome `dados.xlsx`.
2. Acesse o repositório: https://github.com/VictorRomao011/dashboard-requisicoes
3. Clique em **"Add file"** → **"Upload files"**.
4. Arraste ou selecione o novo `dados.xlsx` do seu computador (mesmo nome — o GitHub vai substituir o arquivo existente).
5. Em **"Commit message"**, escreva algo como `Atualiza dados.xlsx`.
6. Confirme que está marcado **"Commit directly to the main branch"** e clique em **"Commit changes"**.
7. Aguarde ~1-2 minutos: o workflow (aba **Actions**) roda automaticamente e republica o GitHub Pages.
8. Acesse o link do dashboard e recarregue com **Ctrl+F5** (para ignorar o cache) — os novos dados devem aparecer.

## Como publicar (GitHub Pages)

A publicação já está configurada via [`.github/workflows/deploy-pages.yml`](.github/workflows/deploy-pages.yml): a cada push na branch `main`, o workflow copia os arquivos para `public/` e publica automaticamente em https://victorromao011.github.io/dashboard-requisicoes/.

> **Importante:** abrir o `index.html` com duplo clique (`file://`) **não funciona** — o navegador bloqueia o `fetch()` do xlsx. Sirva via Pages ou um servidor local (`python -m http.server`).

## Requisitos do formato da planilha

O parser localiza automaticamente a linha de cabeçalho (procura a coluna `SOLICITANTE`), então linhas de lixo acima do cabeçalho são ignoradas. Colunas esperadas (acentos e maiúsculas/minúsculas são tolerados):

`COLUNA1` (ou `DATA`), `SOLICITANTE`, `MODELO`, `DESCRICAO`, `APROVADOR`, `REQUISICAO`, `PEDIDO`, `VIM`, `STATUS VIM`, `NOTA FISCAL`, `FORNECEDOR`, `DESPESA`, `STATUS`

- Datas: aceita data nativa do Excel, `AAAA-MM-DD` ou `DD/MM/AAAA`.
- Despesa: aceita número nativo, `1.234,56` ou `1,234.56`.
- Linhas sem solicitante, requisição e data são descartadas.

## Validação

O parser foi validado contra a planilha real: extrai exatamente **212 registros**, com soma de despesa **R$ 2.962.973,37**, idêntico ao dashboard original com dados embutidos.
