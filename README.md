# Izzy Test Dashboard

Painel web para rodar e acompanhar testes automatizados do agente de IA "Izzy" (chatbot de atendimento da Zerando o Chile), incluindo um histórico consolidado com métricas de acerto/erro.

## O que é isso

Duas páginas estáticas (HTML/CSS/JS puro, sem build, sem dependências):

- **`index.html`** — painel de execução: escolhe um cenário de teste, dispara a execução e acompanha a resposta do agente em tempo real, com botões para marcar cada resposta como Correto/Errado.
- **`historico.html`** — dashboard com o histórico de todas as execuções: total de execuções, taxa de acerto/erro, ranking de etapas com mais erro, últimas execuções (com link direto pra execução no n8n) e detalhamento por cenário.

Toda a lógica de negócio (rodar os cenários de teste, chamar o agente Izzy via Chatwoot, gravar resultados) roda em workflows do n8n — este projeto é só a interface, hospedada separadamente e consumindo os webhooks do n8n via `fetch()`.

## Como funciona

As páginas fazem chamadas para os webhooks do n8n (`https://z-n8n.zitway.com/webhook/izzy-dashboard-*`):

| Endpoint | Método | Uso |
|---|---|---|
| `/webhook/izzy-dashboard-run` | POST | Inicia uma rodada de testes |
| `/webhook/izzy-dashboard-status` | GET | Consulta o status/resultado de uma rodada |
| `/webhook/izzy-dashboard-stop` | POST | Interrompe uma rodada em andamento |
| `/webhook/izzy-dashboard-marcar` | POST | Marca um resultado como correto/errado |
| `/webhook/izzy-dashboard-historico-data` | GET | Retorna os dados agregados do histórico |

O n8n já libera CORS por padrão nesses webhooks, então não é preciso nenhum backend/proxy adicional — as páginas podem ser hospedadas em qualquer lugar (GitHub Pages, Netlify, etc.) e continuam funcionando.

## Deploy (GitHub Pages)

1. Em **Settings → Pages** deste repositório, selecione a branch `main` e a pasta raiz (`/`) como fonte.
2. Aguarde alguns minutos — o GitHub publica em `http://testes-izzy.zitway.com/`,`https://grupo-zerando.github.io/izzy-test-dashboard/`.
3. Pronto — `index.html` é o painel de execução, `historico.html` é o histórico.

## Desenvolvimento local

Como são páginas estáticas, basta abrir os arquivos direto no navegador ou servir com qualquer servidor HTTP simples, por exemplo:

```bash
python3 -m http.server 8000
```

e acessar `http://localhost:8000`.
