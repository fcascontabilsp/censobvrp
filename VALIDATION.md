# Validação local

A página foi aberta no navegador e **carregou corretamente**, com cabeçalho, tela pública de participação, botão de resposta e barra inferior visíveis.

O único erro registrado no console durante o teste via `file://` foi uma falha ao consultar a planilha do Google Sheets. Esse comportamento é compatível com as restrições de requisições externas ao abrir um arquivo diretamente pelo sistema de arquivos. A validação definitiva deve ser feita por HTTP após a publicação no GitHub Pages.

O validador HTML não encontrou erros de fechamento de tags, estrutura duplicada ou falhas de interpretação. Os avisos encontrados referem-se ao uso de estilos inline, botões sem `type` explícito e recomendações de acessibilidade, sem impedir o carregamento da página.
