# Teste de integração

## Teste de integração

Utilização das soluções Postman (API) e Newman (Test) para geração de report a partir de testes de integração REST nas APIs Salesforce.

## Ferramentas

### Postman

Ferramenta gráfica para desenvolvimento e teste de APIs.&#x20;

![\[Collection do Postman com lista de testes executados por endpoint\]](https://user-images.githubusercontent.com/15347353/152425266-8747491b-188f-4397-be3a-f319ec3b755f.png)

### Newman

Biblioteca para execução de collections do Postman a partir de linha de comando (CLI).

![\[Resultado dos testes executados na collection do Postman, apresentação pelo Newman-html\]](https://user-images.githubusercontent.com/15347353/152426139-72a6a44b-4035-41c5-8da3-9eba77888574.png)

### Visual Studio Code

IDE de desenvolvimento que possibilita utilização de extensões e funcionalidades de linha de comando (CLI).

![\[Resultado dos testes executados na collection do Postman, apresentação pelo Newman via CLI\]](https://user-images.githubusercontent.com/15347353/152425345-00e8ddf5-5864-4693-b4a8-c47c1bb4929b.png)

## Utilização

### Pré-requisitos

1. Collection do Postman, (.json), npm (node), newman e newman-reporter-htmlextra

### Execução

Passos para rodar os testes da coleção gerada pelo Postman, em: Postman + Newman.

Dessa forma utilizando as mesmas soluções, dentro do Visual Studio Code podemos utilizar o terminal para executar os comandos node e gerar os reports, sobre versionamento das coleções, os mesmos irão ser realizados dentro da pasta "test" na raiz de nosso projeto.

Configuração de ambiente para execução será baseado em branch, ainda a definir execução do mesmo como usuário de integração e variáveis de ambiente no pipeline dentro do Jenkins.

## Referências

1. [Postman - API Platform](https://www.postman.com/)
2. [Running collections on the command line with Newman](https://learning.postman.com/docs/running-collections/using-newman-cli/command-line-integration-with-newman/)
3. [NPM - newman-reporter-htmlextra](https://www.npmjs.com/package/newman-reporter-htmlextra)
