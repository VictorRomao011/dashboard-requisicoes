# Dashboard de Requisições FY26

Dashboard que lê os dados **dinamicamente** do arquivo `dados.xlsx` no mesmo diretório, usando [SheetJS](https://sheetjs.com/). Os dados **não** estão mais embutidos no HTML — para atualizar o dashboard, basta substituir o `dados.xlsx` e recarregar a página.

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | O dashboard. Carrega Chart.js e SheetJS via CDN e lê `dados.xlsx` a cada carregamento da página (com cache desabilitado). |
| `dados.xlsx` | A base de dados. É a planilha de requisições, sem nenhuma modificação de formato. |

## Como atualizar os dados

1. Edite sua planilha normalmente (no SharePoint/OneDrive/Excel).
2. Salve uma cópia com o nome `dados.xlsx`.
3. Suba o arquivo no repositório substituindo o atual (pela interface web do GitHub/GitLab: abrir `dados.xlsx` → *Replace* / *Upload new file*).
4. Recarregue o dashboard. Pronto — KPIs, gráficos e tabela refletem a planilha nova.

## Como publicar (GitHub Pages)

1. No repositório: **Settings → Pages**.
2. Em *Source*, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Salve. Em ~1 minuto o dashboard fica disponível em `https://SEU_USUARIO.github.io/NOME_DO_REPO/`.

No GitLab, o equivalente é GitLab Pages com um `.gitlab-ci.yml` simples (job `pages` que copia os arquivos para `public/`).

> **Importante:** abrir o `index.html` com duplo clique (`file://`) **não funciona** — o navegador bloqueia o `fetch()` do xlsx. Sirva via Pages ou um servidor local (`python -m http.server`).

## Requisitos do formato da planilha

O parser localiza automaticamente a linha de cabeçalho (procura a coluna `SOLICITANTE`), então linhas de lixo acima do cabeçalho são ignoradas. Colunas esperadas (acentos e maiúsculas/minúsculas são tolerados):

`COLUNA1` (ou `DATA`), `SOLICITANTE`, `MODELO`, `DESCRICAO`, `APROVADOR`, `REQUISICAO`, `PEDIDO`, `VIM`, `STATUS VIM`, `NOTA FISCAL`, `FORNECEDOR`, `DESPESA`, `STATUS`

- Datas: aceita data nativa do Excel, `AAAA-MM-DD` ou `DD/MM/AAAA`.
- Despesa: aceita número nativo, `1.234,56` ou `1,234.56`.
- Linhas sem solicitante, requisição e data são descartadas.

## Validação

O parser foi validado contra a planilha real: extrai exatamente **212 registros**, com soma de despesa **R$ 2.962.973,37**, idêntico ao dashboard original com dados embutidos.
