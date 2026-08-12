# Censo Buona Vita Ribeirão Preto 2026

Página estática do **Censo Residencial Buona Vita 2026**, com acompanhamento público da participação, formulário em etapas e painel de resultados.

## Estrutura

| Arquivo | Finalidade |
|---|---|
| `index.html` | Aplicação completa em HTML, CSS e JavaScript |
| `.nojekyll` | Publicação direta no GitHub Pages sem processamento do Jekyll |
| `VALIDATION.md` | Auditoria comparativa, testes do Make e registro da publicação |

## Integrações utilizadas

A página envia respostas para um webhook do Make e consulta uma planilha do Google Sheets para atualizar os indicadores. As bibliotecas Chart.js, SheetJS e Papa Parse são carregadas por CDN.

## Publicação

O site é publicado pelo GitHub Pages a partir da raiz da branch `main`. O domínio configurado é **[http://www.buonavitaribeirao.com.br/](http://www.buonavitaribeirao.com.br/)**; a URL padrão **[fcascontabilsp.github.io/censobvrp](https://fcascontabilsp.github.io/censobvrp/)** redireciona para ele.

A revisão mais recente está documentada em **[VALIDATION.md](VALIDATION.md)**. O conteúdo já está online, mas o certificado HTTPS do domínio personalizado ainda depende de reprovisionamento no GitHub Pages.

## Observação de segurança

Como se trata de uma aplicação estática, configurações, códigos de acesso e URLs presentes no JavaScript podem ser visualizados pelo navegador. O controle de acesso atual deve ser considerado apenas uma barreira simples de interface, não uma autenticação segura.
