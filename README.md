# Dashboard de Requisições FY26

🔗 **Dashboard publicado:** https://dashboard-requisicoes-atualizado-d59e46.code.siemens-energy.io/

Dashboard que lê os dados **dinamicamente** do arquivo `dados.xlsx` no mesmo diretório, usando [SheetJS](https://sheetjs.com/). Os dados **não** estão mais embutidos no HTML — para atualizar o dashboard, basta substituir o `dados.xlsx` e recarregar a página.

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | O dashboard. Carrega Chart.js e SheetJS via CDN e lê `dados.xlsx` a cada carregamento da página (com cache desabilitado). |
| `dados.xlsx` | A base de dados. É a planilha de requisições, sem nenhuma modificação de formato. |

## Como atualizar os dados (pela interface web do GitLab, sem git)

1. Edite sua planilha normalmente (no SharePoint/OneDrive/Excel) e salve uma cópia com o nome `dados.xlsx`.
2. Acesse o repositório: https://code.siemens-energy.com/victor.romao/dashboard-requisicoes_atualizado
3. Na lista de arquivos, clique em **`dados.xlsx`** para abri-lo.
4. No canto superior direito, clique em **"Edit"** (ou no menu/dropdown ao lado) e escolha **"Replace file"**.
5. Clique em **"Upload file"** (ou arraste o arquivo) e selecione o novo `dados.xlsx` do seu computador.
6. Em **"Commit message"**, escreva algo como `Atualiza dados.xlsx`.
7. Confirme que está marcado **"Commit directly to the main branch"** e clique em **"Replace file"** / **"Commit changes"**.
8. Aguarde ~1-2 minutos: o pipeline (**Build → Pipelines**) roda automaticamente e republica o GitLab Pages.
9. Acesse o link do dashboard e recarregue com **Ctrl+F5** (para ignorar o cache) — os novos dados devem aparecer.

## Como publicar (GitLab Pages)

A publicação já está configurada via [`.gitlab-ci.yml`](.gitlab-ci.yml): a cada push na branch `main`, o job `pages` copia `index.html` e `dados.xlsx` para `public/` e o GitLab publica automaticamente em https://dashboard-requisicoes-atualizado-d59e46.code.siemens-energy.io/.

> **Importante:** abrir o `index.html` com duplo clique (`file://`) **não funciona** — o navegador bloqueia o `fetch()` do xlsx. Sirva via Pages ou um servidor local (`python -m http.server`).

## Requisitos do formato da planilha

O parser localiza automaticamente a linha de cabeçalho (procura a coluna `SOLICITANTE`), então linhas de lixo acima do cabeçalho são ignoradas. Colunas esperadas (acentos e maiúsculas/minúsculas são tolerados):

`COLUNA1` (ou `DATA`), `SOLICITANTE`, `MODELO`, `DESCRICAO`, `APROVADOR`, `REQUISICAO`, `PEDIDO`, `VIM`, `STATUS VIM`, `NOTA FISCAL`, `FORNECEDOR`, `DESPESA`, `STATUS`

- Datas: aceita data nativa do Excel, `AAAA-MM-DD` ou `DD/MM/AAAA`.
- Despesa: aceita número nativo, `1.234,56` ou `1,234.56`.
- Linhas sem solicitante, requisição e data são descartadas.

## Validação

O parser foi validado contra a planilha real: extrai exatamente **212 registros**, com soma de despesa **R$ 2.962.973,37**, idêntico ao dashboard original com dados embutidos.
