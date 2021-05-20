## Verificações para identificar gargalos

### Bundler Analyser

Cria um análise de todas as dependência buildando a aplicação e verificando o bundle size. Essa verificação é importante para percebermos os locais onde devemos focar para reduzir o bundle size.

### Throttling da rede

É basicamente diminuir no dev tools do chrome a conexão da internet para velocidade "3g", recarregar a aplicação e analisar como ela se comporta. Verificamos que os principais gargalos são os grandes chunks e as imagens.

## Resolução

Para quebrar nosso código em partes menores basta adicionarmos o lazy load nas rotas.

Também podemos ativar o pre fetch das rotas para carregar paralelamente as próximas rotas
