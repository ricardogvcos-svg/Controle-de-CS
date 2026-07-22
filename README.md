# Dashboard de Governança & Retenção CS — NewSun Energy

## Estrutura
- `index.html` — o dashboard (abre pelo link do GitHub Pages)
- `ns_dashboard_data.js` — snapshot de fallback (modo demonstração), usado quando `data/ns_dashboard_data_live.json` não existe ou não carrega
- `data/ns_dashboard_data_live.json` — dados ao vivo, atualizados automaticamente pelo workflow `.github/workflows/sync-data.yml`
- `.github/workflows/sync-data.yml` — GitHub Action agendada que busca o JSON publicado pelo Power Automate e o commita neste repositório

## Como funciona a atualização automática
1. **Power Automate** (na conta Microsoft 365 da NewSun): lê a planilha Excel no SharePoint em intervalos regulares e publica um JSON atualizado (ex: em um arquivo do OneDrive/SharePoint com link "qualquer pessoa com o link pode visualizar").
2. **GitHub Action** (este repositório, `sync-data.yml`): roda a cada 30 minutos, baixa esse JSON e o salva em `data/ns_dashboard_data_live.json`, commitando a mudança.
3. **Dashboard** (`index.html`): a cada abertura, busca `data/ns_dashboard_data_live.json` no próprio repositório (mesma origem, sem problema de CORS). Se falhar, usa o snapshot embutido em `ns_dashboard_data.js`.

## Configuração pendente
Em **Settings → Secrets and variables → Actions → New repository secret**, crie o secret `SHAREPOINT_JSON_URL` com o link direto de download do JSON publicado pelo Power Automate.

## ⚠️ Aviso de dados sensíveis
Este repositório é público e contém dados reais de clientes (CNPJ, situação financeira, observações jurídicas). Isso foi uma decisão consciente registrada na conversa com o Claude em 22/07/2026. Se isso mudar, o caminho recomendado é migrar para GitHub Pages com repositório privado (plano GitHub Pro/Team) ou Cloudflare Pages + Cloudflare Access.
