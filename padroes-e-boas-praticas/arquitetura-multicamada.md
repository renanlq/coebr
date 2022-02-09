# Arquitetura multicamada

### Índice

1. Apresentação
2. Pré-requisitos
3. SOC - Separation of Concerns
4. De onde elas vêm, e quais suas responsabilidades?
5. Modelo pretendido
6. Referências

### Apresentação

Intuito desse documento é apresentar a proposta de arquitetura e suas camadas para desenvolvimento das customizações programáticas do Salesforce.

Seja onde estiver trabalhando, sempre é necessário estipularmos padrões e patterns, dessa forma para inicio de conversa o básico que precisamos saber sobre as camadas e suas respectivas responsabilidades serão descritas aqui. Pense na Service Layer como a porta de entrada da sua aplicação, como um exemplo, pense em utilizar ela como o consumo de uma API.

### Pré-requisitos

Ter passado pelos seguintes módulos do Trailhead:

1. Apex Enterprise Patterns: Service Layer (1)
2. Apex Enterprise Patterns: Domain & Selector Layers (2)\
   Inicialmente iremos atuar com as 2 das apresentadas como "Enterprise Patters" pela Salesforce (vide Trailhead), são elas:
3. Service; e
4. Domain e Selector.\
   Fora as classes de responsabilidades de tarefas intrínsecas à solução Salesforce: Batch, Controller, Trigger e patterns, ex.: Factory.

### SOC - Separation of Concerns

Separação das responsabilidades, é entender que "o melhor código é aquele feito fora do teclado", e que todo software é feito de três camadas principais: Armazenamento, lógica e apresentação.

Dessa forma é interessante entender o que cada solução dentro do Salesforce tem como responsabilidade e que camada represeta:

#### Presentation

1. Declarative: Layouts, Flow, Record Types, Formulas, Reports, Dashboards
2. Coding: Apex Controllers, VisualForce, Lightning Components

#### Business Logic Layer

1. Declarative: Formula, Flow, Validation, Workflow (DEPRECATED), Process Builder (DEPRECATED), Sharing Rules
2. Coding: Apex Services, Apex Custom Actions

#### Data Access Layer

1. Declarative: Data Loaders
2. Coding: SOQL, SOSL, Salesforce APIs

#### Database Layer

1. Declarative: Custom Objects, Fields, Relationships, RollUps
2. Coding: Apex Triggers

### De onde elas vêm, e quais suas responsabilidades?

#### Service

Ajuda no encapsulamento das tarefas de negócio, em outras palavras a lógica de processamentos relacionados ao 'tarefas' de negócio, cálculos e processamentos. Sua implementação deve levar em consideração que essa pode ser utilizadas em diferentes contexto, ex. Web, Mobile e API.&#x20;

**Quem consome a camada de serviço?**

[![image](https://user-images.githubusercontent.com/15347353/152427835-e181eb50-a788-4421-a5b9-842458fd0e40.png)](https://user-images.githubusercontent.com/15347353/152427835-e181eb50-a788-4421-a5b9-842458fd0e40.png)\
_\[Imagem contida em módulo do Trailhead - Salesforce]_

Trigger Apex não estão inclusas pois a lógica de negócio deve pertencer à camada de Domínio, que está alinhado com os Objetos e a manipulação desses em sua aplicação. A lógica de domínio e chamada diretamente e indiretamente pelas camadas de serviço e aplicação (UI e APIs).

#### Domain

De forma resumida a camada de domínio encapsula o comportamento e dados de um objeto. Como descrito por Martin Fowler:

"_Um modelo de objeto do domínio que incorpora comportamento e dados. Na pior das hipóteses, a lógica de negócios pode ser muito complexa. Regras e lógica descrevem muitos casos e inclinações diferentes de comportamento, e é essa complexidade que os objetos foram projetados para trabalhar._"

**Quem consome a camada de domínio?**

A camada de domínio deve ser consumida por Triggers e camada de Serviço (Service Layer).

[![image](https://user-images.githubusercontent.com/15347353/152427943-207830e0-2b73-4d59-a0c6-9c86b1707895.png)](https://user-images.githubusercontent.com/15347353/152427943-207830e0-2b73-4d59-a0c6-9c86b1707895.png)\
_\[Imagem contida em módulo do Trailhead - Salesforce]_

#### Selector

Analogia rápida, seria a nossa velha e conhecida DAO, mas com umas diferenças sobre responsabilidade, segurança, vindas do pattern Data Mapper, onde a camada de "mappers" é responsável por trafegar os dados entre objetos e "banco de dados", mantendo os independentes entre eles e até na própria camada.

Opte por retornar listas de "sObject", principalmente os métodos Selector devem retornar listas de sObject. Embora o Apex agora suporte mapas e conjuntos determinísticos, nenhum deles pode ser usado para refletir uma ordem de classificação.

Evite usar "with sharing" ou "without sharing" nas classes Selector Apex para garantir que o contexto de chamada herde esse contexto. Essa responsabilidade deve vir da camada de Serviço que provavelmente terá contexto "with sharing".

Se for o caso de um SOQL requerer acesso a registros que não são de visibilidade do usuário, é aconselhado o uso de uma "inner class" privada com esse contexto "without sharing" aplicado.

### Modelo de arquitetura APEX pretendido

[![image](https://user-images.githubusercontent.com/15347353/152428008-9f6f6f3e-47b0-4d9a-92d6-52d56b3c9f8c.png)](https://user-images.githubusercontent.com/15347353/152428008-9f6f6f3e-47b0-4d9a-92d6-52d56b3c9f8c.png)\
_\[Imagem contida em módulo do Trailhead - Salesforce]_

### Referências:

1. [Apex Enterprise Patterns: Service Layer](https://trailhead.salesforce.com/en/content/learn/modules/apex\_patterns\_sl)
2. [Apex Enterprise Patterns: Domain & Selector Layers](https://trailhead.salesforce.com/en/content/learn/modules/apex\_patterns\_ds)
3. [Factory method pattern](https://en.wikipedia.org/wiki/Factory\_method\_pattern)
4. [Unit of Work](https://martinfowler.com/eaaCatalog/unitOfWork.html)
5. [Domain Model](https://martinfowler.com/eaaCatalog/domainModel.html)
6. [Data Mapper](https://martinfowler.com/eaaCatalog/dataMapper.html)
7. [Github - FFLib Apex Common](https://github.com/apex-enterprise-patterns/fflib-apex-common)
