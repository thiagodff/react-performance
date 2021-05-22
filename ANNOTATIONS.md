## Verificações para identificar gargalos

### Bundler Analyser

Cria um análise de todas as dependência buildando a aplicação e verificando o bundle size. Essa verificação é importante para percebermos os locais onde devemos focar para reduzir o bundle size.

### Throttling da rede

É basicamente diminuir no dev tools do chrome a conexão da internet para velocidade "3g", recarregar a aplicação e analisar como ela se comporta. Verificamos que os principais gargalos são os grandes chunks e as imagens.

### Resolução

Para quebrar nosso código em partes menores basta adicionarmos o lazy load nas rotas.

Também podemos ativar o pre fetch das rotas para carregar paralelamente as próximas rotas

## Verificando desempenho

No chrome dev tools na aba de performance podemos realizar um throttling de CPU, clique em "record" e analise como a aplicação se comporta.

### Renderização da lista

Podemos notar muitas renderizações desnecessárias no Search, nesse caso, por se tratar de uma lista estática (com pouca ou nenhuma alteração) podemos utilizar o react memo.

Devemos tomar cuidado com o React memo pois ele quebrar algumas partes da aplicação, não sendo indicado para todos os usos

## Classes vs Funções

Às classes não são nativas do JS, é necessário transpilar uma classe peo Babel para que o navegador entenda o código, ele adiciona mais código ainda para tudo funcionar, é um overhead desnecessário.

Por isso o ideal é substituir todas as classes por funções, pois assim iremos diminuir o bundle size. Em um teste simples, o bundle size de um componente com classe foi de 4,7 kb para 2,4 kb utilizando função e hooks, ou seja, um ganho de quase de 50%.

## Importações desnecessárias

Em Accounts estamos importando toda a biblioteca do moment para fazer algo simples, podemos utilizar uma ferramenta para nos auxiliar à utilizar apenas o necessário do moment

https://github.com/you-dont-need/You-Dont-Need-Momentjs

que é uma doc inspirada no [You Might Not Need JQuey](http://youmightnotneedjquery.com/).

Ou até mesmo podemos substituir por outra biblioteca mais leve, como o date-fns no nosso caso.

## Single thread

O JS por rodar de forma single thread, quando executamos uma função pesada a nossa aplicação inteira fica travada, que é o que acontece quando clicamos para fazer Download Personal Data na pagina de Account

### Resolução com Web Worker

https://github.com/webpack-contrib/worker-loader

https://github.com/developit/workerize-loader

Conseguimos transformar qualquer pacote da nossa aplicação em um pacote que pode ser utilizado dentro de um web worker.

Realizamos um import para dentro de um módulo utilizando o webpack para rodar o código dentro de um web worker.

Para isso iremos isolar nossa função em src/Utils/worker.js e dentro do Account iremos importar esse worker utilizando o workerize-loader.

E com isso conseguimos pegar toda complexidade da função de gerar o CSV e faze-la rodar em uma thread separada

### Tradeoff

Ao lidar com múltiplas threads surgem alguns problemas:

- Maior consumo de memória
- Maior gasto de processamento
- As threads conversam via emissão de eventos
