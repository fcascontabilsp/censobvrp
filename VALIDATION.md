# Validação comparativa da publicação

| Metadado | Valor |
|---|---|
| Projeto | Censo Residencial Buona Vita 2026 |
| Data da validação | 12 de agosto de 2026 |
| Responsável técnico | Manus AI |
| Commit anterior | `2881cf585ab97a793b01a8c3c7743e0edcd1fa22` |
| Commit publicado | `61a0fdf680517381d62d1e6f4ff6b7b03d9c09a7` |

## Resultado executivo

A nova versão foi comparada com a versão publicada anteriormente, corrigida, testada em navegador e enviada à branch `main`. O HTML servido pelo GitHub Pages possui SHA-256 `023bac0a99a43177f27831e4283bb6fdfc7e94213bcbdd57a09f438af4a7e258`, exatamente igual ao arquivo `index.html` do commit publicado.

O contrato de envio para o Make foi preservado: o endpoint, o método `POST`, o formato JSON e os 43 caminhos consumidos pelo cenário continuam compatíveis. Nenhuma resposta de teste foi gravada, pois todas as chamadas ao webhook foram interceptadas antes da rede.

| Área verificada | Resultado |
|---|---|
| Estrutura do HTML recebido | Foram corrigidas 11 tags `<div>` de etapas que estavam sem o caractere `>` |
| Wizard | 12 etapas percorridas com sucesso |
| Validação obrigatória | Impediu corretamente o avanço com a primeira etapa vazia |
| Leitura do Google Sheets | `200 OK`, conteúdo CSV e CORS `*` |
| Contrato com Make | 43 de 43 caminhos obrigatórios presentes no payload |
| Envio normal | Confirmação exibida após um POST interceptado |
| Retry | Primeiro POST respondeu `503` no teste; segundo POST concluiu o fluxo |
| POSTs reais de teste | Zero |
| Erros de página | Zero |
| Erros inesperados de console | Zero |
| Publicação | Conteúdo online idêntico ao arquivo versionado |

## Comparação com a versão anterior

A atualização final preservou o formulário, o webhook e o mapeamento do cenário do Make. As mudanças funcionais publicadas estão resumidas abaixo.

| Componente | Versão anterior | Nova versão publicada | Impacto validado |
|---|---|---|---|
| Início da campanha | Não definido | `13/08/2026` | O contador de prazo passou a usar uma data real |
| Encerramento da campanha | Não definido | `31/08/2026` | A tela pública exibe os dias restantes |
| Leitura da planilha | Endpoint `gviz/tq` da planilha | CSV de **Publicar na Web** | Evita o bloqueio de CORS observado no navegador |
| Cache de leitura | Tentativa de consulta ao vivo | Cache do Google de alguns minutos | Uma resposta nova pode demorar alguns minutos para aparecer nos painéis |
| “A voz do morador” | Classe `dir-only` | Disponível também no perfil de respondentes | Respostas textuais deixam de ser exclusivas da diretoria após o acesso ao perfil |
| Envio ao Make | Webhook e payload existentes | Inalterados | Compatibilidade preservada |

> **Observação de privacidade:** a remoção de `dir-only` do painel “A voz do morador” foi mantida por fazer parte do arquivo atualizado recebido. Esse conteúdo não aparece na tela pública de participação, mas passa a aparecer no perfil acessado pelo código geral de respondentes.

## Auditoria da integração com Make

O cenário ativo identificado no Make é **“Censo Buona Vita 2026 — Recebe Resposta e Grava na Planilha”**. Seu blueprint contém o webhook receptor e um módulo Google Sheets que grava 43 colunas na aba `censo`.

| Verificação | Resultado |
|---|---|
| Cenário | Ativo e identificado pelo ID `5889149` |
| Webhook do HTML | Corresponde ao hook do cenário |
| Destino | Planilha `1rG4OgQUKAgfrcPYYQm8y79-FCvMnjuVyitVZSZjIhLM`, aba `censo` |
| Colunas mapeadas | 43 |
| Caminhos exigidos pelo Make | 43 |
| Caminhos presentes no novo payload | 43 |
| Arrays preservados | `pets`, `transportes`, `areasUsadas`, `importancia`, `novosServicos`, `identidade` e demais listas |
| Objetos preservados | `faixas` e `satisfacao` |
| Campos técnicos | `ts` numérico presente |

O histórico do cenário mostra que as três execuções automáticas mais recentes disponíveis, em 8, 10 e 11 de agosto de 2026, terminaram com sucesso (`status: 1`, duas operações cada). As falhas anteriores de 8 de agosto apontavam para a aba inexistente `Página1`; depois da configuração para a aba `censo`, as execuções posteriores passaram a concluir normalmente.

## Testes funcionais seguros

Os testes foram executados contra a versão preparada em servidor local, usando a fonte CSV pública real. O tráfego destinado ao domínio do webhook foi interceptado antes da rede e respondido pelo próprio teste. Assim foi possível validar o fluxo completo sem inserir linhas artificiais na planilha de produção.

| Caso | Passos | POSTs interceptados | Resultado |
|---|---:|---:|---|
| Fluxo normal | 12 | 1 | Tela de agradecimento exibida |
| Falha transitória e retry | 12 | 2 | Primeiro POST `503`; repetição automática concluída |

A inspeção visual confirmou a integridade da tela pública de participação, do mapa por quadra, da chamada para responder e da tela final de agradecimento. A validação estrutural não encontrou erros de parser, ordem de fechamento, conteúdo proibido ou IDs duplicados. Permanecem recomendações não bloqueantes do validador sobre estilos inline, `type` explícito em botões e acessibilidade de formulários.

## Publicação e domínio

O GitHub Pages concluiu a construção a partir da raiz da branch `main`. A URL padrão do repositório redireciona para o domínio personalizado `www.buonavitaribeirao.com.br`. A página respondeu `200 OK` por HTTP e abriu o wizard online no **Passo 1 de 12**.

Há uma pendência externa à aplicação: em 12 de agosto de 2026, o domínio personalizado ainda apresentava o certificado curinga `*.github.io`, que não cobre `www.buonavitaribeirao.com.br`. Por isso, a verificação HTTPS falhava e `https_enforced` permanecia desativado. O DNS observado estava apontando corretamente para `fcascontabilsp.github.io`, e não havia registro CAA bloqueando a Let's Encrypt. A tentativa de reiniciar o provisionamento pela API recebeu `403 Resource not accessible by integration`, sem alterar a configuração.

O GitHub informa que o provisionamento do certificado pode exigir nova verificação do domínio e que remover e adicionar novamente o domínio reinicia o processo. A opção **Enforce HTTPS** só deve ser ativada depois que o certificado válido estiver disponível.[1] [2]

## Referências

[1]: https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https "GitHub Docs — Securing your GitHub Pages site with HTTPS"
[2]: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages "GitHub Docs — Troubleshooting custom domains and GitHub Pages"
