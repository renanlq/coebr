# Code Review

_Code review_, é uma prática que pode salvar a sua vida, kkkkk em outras palavras, uma validação das alterações realizadas em seu repositório, por meio de um _Pull Request/Merge Request_, o que vem em conjunto com uma necessidade de criação de um conselho para avaliação de todas demandas, a fim de não centralizar tais atividades.

## Processo

1. **Pré-requisito**: Criação de um server-hook e/ou bloqueio das branchs de "integração, por parte do Desenvolvedor: criação do Pull/Merge Request.
2. **Atividade**: Indicação para uma solicitação de horário para avaliação junto a pelo menos 2 devs, sendo 1 do comitê de aprovadores, não mais que 15 minutos para o _code review_. Serão avaliados no Code Review: Classes, Aura, LWC, Validation Rules, Flows, e demais metadados que tem potencial de impacto no nosso ambiente.

## Check-list - exemplo
```
# Pull request checklist
## GIT
( ) Branch origem sincronizada via rebase com branch destino (Git, Alto)
 
## Permissionamento
( ) Permissão de Objetos, campos, connected apps, apex, pages, tabs e custom permissions em "Permission Set"? (Profile, Alto)
 
## Arquitetura e governança
( ) Envolve refatoração de legado? Se sim, cabe a utilização do recurso de Deployment Strategy - FeatureFlag/CanaryRelease (Arquitetura, Alto)
( ) Classes APEX seguem definição da Arq. de Camada, Controller, Service, Selector, Domain ... ? (Arquitetura, Alto)
( ) Nomenclatura de classes, métodos e variáveis estão em acordo com Arq. de Ref.? (Nomenclatura, Médio)
 
## Análise estática de código
( ) Não existe comentário em componentes LWC e Aura (.cmp, .html)? (SonarCube, Médio)
( ) Componente HTML Fielset segue boa prática com a utilização da tag <legened>? (SonarCube, Baixo)
( ) Arquivo de estilização (.css) do Aura não está sem declarações para o .this {} ? (SonarCube, Baixo)
 
## Programáticas
( ) Para classes de teste, existe cobertura de pelo 90% das respectivas classes testadas? (Classe, Alto)
( ) Blocos de try catch estão sendo utilizados para tratativa de exceções no nível mais alto? Nas Controllers (Classe/trigger, Crítico)
( ) Chamadas de callouts são realizadas antes de DML's? (Classe/trigger, Crítico)
( ) Controller possuem métodos de serialização e deserialização? Classe Builder (Classe, Alto)
( ) Regra de negócio sempre na camada de Domínio? (Classe, Alto)
( ) Metodos possuem somente uma declaração de retorno? (Classe/trigger, Crítico)
( ) Não possui DML dentro de loop? (Classe/trigger, Crítico)
( ) Não possui future dentro de loop? (Classe/trigger, Crítico)
( ) Não possui SOQL dentro de loop? (Classe/trigger, Crítico)
( ) Não possui valores (ex. ID) em Hard Code? (Classe/trigger, Crítico)
( ) Não possui criação de objeto dentro de LOOP? (Classe/trigger, Alto)
( ) Não possui mais de 3 loopings aninhados? (Classe/trigger, Alto)
( ) SOQL sem LIMIT ou WHERE? (Classe/trigger, Crítico)
( ) Todos os métodos da classe de teste possuem start(), stop() e system.Assert()? (Classe, Crítico)
( ) Triggers só devem declarar os eventos que serão usados e devem ser livres de lógica? (Classe/trigger, Crítico)
( ) Variavel contem um nome informativo, não abreviado e usa o padrão lowerCamelCase? (Classe/trigger, Crítico)
( ) Campo utilizado em query é único e/ou indexado (para objetos com alto volume)? (Classe, Alto)
( ) Não é utilizado SeeAllData em classes? (Classe, Alto)
( ) Não possui alert() e console.log() nas classes, fora de contexto de erro? (Javascript, Alto)
( ) Não possui campo fórmula em QUERY? (Classe, Alto)
( ) Não tem mais de 1 trigger e possuir a trigger handler? (Trigger, Alto)
( ) Todas as váriaveis da classe são utilizadas? (Classe/trigger, Alto)
( ) Todos os códigos possuem classes de teste própria? (Classe/trigger, Alto)
( ) Todos os métodos da classe são utilizados? (Classe/trigger, Alto)
( ) Utiliza etiquetas personalizadas para armazenar o valor quando necessário? (Classe/trigger, Alto)
( ) Utiliza configurações personalizadas para armazenar o valor quando necessário? (Classe/trigger, Alto)
( ) Utiliza metadados personalizados para armazenar o valor quando necessário? (Classe/trigger, Alto)
( ) Trigger aceita carga de dados? (Trigger, Alto)
( ) Web service (REST/SOAP) possui endpoint claro e versão, path em modelo RESTFUL? (Web Service, Alto)
```

### Resultado avaliação

A. **Aprovação** - Qualidade de entrega ok, segue o jogo;\
B. **Qualidade a desejar** - Sem risco ao ambiente, aprovado mediante a aceite de To-Dos para a próxima entrega; e\
C. **Baixa qualidade** - Com risco ao ambiente (query em for por exemplo) não aprovamos e solicitamos a refatoração.

## Referências

1. [Apex Hours - Code review checklist](https://www.apexhours.com/code-review-checklist/)
