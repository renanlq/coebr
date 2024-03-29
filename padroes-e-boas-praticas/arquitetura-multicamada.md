# Arquitetura multicamada

## Índice

1. Apresentação
2. Pré-requisitos
3. SOC - Separation of Concerns
4. De onde elas vêm, e quais suas responsabilidades?
5. Modelo pretendido
6. Referências

## Apresentação

Intuito desse documento é apresentar a proposta de arquitetura e suas camadas para desenvolvimento das customizações programáticas do Salesforce.

Seja onde estiver trabalhando, sempre é necessário estipularmos padrões e patterns, dessa forma para inicio de conversa o básico que precisamos saber sobre as camadas e suas respectivas responsabilidades serão descritas aqui. Pense na Service Layer como a porta de entrada da sua aplicação, como um exemplo, pense em utilizar ela como o consumo de uma API.

## Pré-requisitos

Ter passado pelos seguintes módulos do Trailhead:

1. Apex Enterprise Patterns: Service Layer (1)
2. Apex Enterprise Patterns: Domain & Selector Layers (2)\
   Inicialmente iremos atuar com as 2 das apresentadas como "Enterprise Patters" pela Salesforce (vide Trailhead), são elas:
3. Service; e
4. Domain e Selector.\
   Fora as classes de responsabilidades de tarefas intrínsecas à solução Salesforce: Batch, Controller, Trigger e patterns, ex.: Factory.

## SOC - Separation of Concerns

Separação das responsabilidades, é entender que "o melhor código é aquele feito fora do teclado", e que todo software é feito de três camadas principais: Armazenamento, lógica e apresentação.

Dessa forma é interessante entender o que cada solução dentro do Salesforce tem como responsabilidade e que camada represeta:

### Presentation

1. Declarative: Layouts, Flow, Record Types, Formulas, Reports, Dashboards
2. Coding: Apex Controllers, VisualForce, Lightning Components

### Business Logic Layer

1. Declarative: Formula, Flow, Validation, Workflow (DEPRECATED), Process Builder (DEPRECATED), Sharing Rules
2. Coding: Apex Services, Apex Custom Actions

### Data Access Layer

1. Declarative: Data Loaders
2. Coding: SOQL, SOSL, Salesforce APIs

### Database Layer

1. Declarative: Custom Objects, Fields, Relationships, RollUps
2. Coding: Apex Triggers

## De onde elas vêm, e quais suas responsabilidades?

### Service

Ajuda no encapsulamento das tarefas de negócio, em outras palavras a lógica de processamentos relacionados ao 'tarefas' de negócio, cálculos e processamentos. Sua implementação deve levar em consideração que essa pode ser utilizadas em diferentes contexto, ex. Web, Mobile e API.&#x20;

#### **Quem consome a camada de serviço?**

\
_\[Imagem contida em módulo do Trailhead - Salesforce]_

![\[Imagem contida em módulo do Trailhead - Salesforce\]](https://user-images.githubusercontent.com/15347353/152427835-e181eb50-a788-4421-a5b9-842458fd0e40.png)

Trigger Apex não estão inclusas pois a lógica de negócio deve pertencer à camada de Domínio, que está alinhado com os Objetos e a manipulação desses em sua aplicação. A lógica de domínio e chamada diretamente e indiretamente pelas camadas de serviço e aplicação (UI e APIs).

### Domain

De forma resumida a camada de domínio encapsula o comportamento e dados de um objeto. Como descrito por Martin Fowler:

"_Um modelo de objeto do domínio que incorpora comportamento e dados. Na pior das hipóteses, a lógica de negócios pode ser muito complexa. Regras e lógica descrevem muitos casos e inclinações diferentes de comportamento, e é essa complexidade que os objetos foram projetados para trabalhar._"

**Quem consome a camada de domínio?**

A camada de domínio deve ser consumida por Triggers e camada de Serviço (Service Layer).

### Selector

![\[Imagem contida em módulo do Trailhead - Salesforce\]](https://user-images.githubusercontent.com/15347353/152427943-207830e0-2b73-4d59-a0c6-9c86b1707895.png)

Analogia rápida, seria a nossa velha e conhecida DAO, mas com umas diferenças sobre responsabilidade, segurança, vindas do pattern Data Mapper, onde a camada de "mappers" é responsável por trafegar os dados entre objetos e "banco de dados", mantendo os independentes entre eles e até na própria camada.

Opte por retornar listas de "sObject", principalmente os métodos Selector devem retornar listas de sObject. Embora o Apex agora suporte mapas e conjuntos determinísticos, nenhum deles pode ser usado para refletir uma ordem de classificação.

Evite usar "with sharing" ou "without sharing" nas classes Selector Apex para garantir que o contexto de chamada herde esse contexto. Essa responsabilidade deve vir da camada de Serviço que provavelmente terá contexto "with sharing".

Se for o caso de um SOQL requerer acesso a registros que não são de visibilidade do usuário, é aconselhado o uso de uma "inner class" privada com esse contexto "without sharing" aplicado.

## Modelo de arquitetura APEX pretendido

\
_\[Imagem contida em módulo do Trailhead - Salesforce]_

![\[Imagem contida em módulo do Trailhead - Salesforce\]](https://user-images.githubusercontent.com/15347353/152428008-9f6f6f3e-47b0-4d9a-92d6-52d56b3c9f8c.png)

## Vantagens e desvantagens
Vamos direto ás desvantagens, de fato o aumento de camadas implica diretamente no tempo de desenvolvimento e um pequeno aumento de complexidade (inicial) na criação de um fluxo de CRUD ou consumo de API externa. E por final aumento na quantidade de customização na Org, dado que para cada nova classe e método temos novas classes e métodos de teste.

Já por outro lado, as vantagens são: separando bem as responsabildades e centralizando a responsabilidade sobre um "dominio" (ex. Account, Case ... ) temos como benefícios a unididade de um processo, onde os possíveis impactos a um registro ou processo estarão nas devidas camadas exercendo suas responsabilidades. Logo, a rastreabilidade e manutenção e muito menor, do que quando temos os códigos todos separados por classes sem padronização.

E sobre a manutenção do código, é muito importante saber o impacto em custo dos bug encontrados ainda em desenvolvimento vs produção, a Salesforce e outras empresas do mercado mesmo tem apresentações em que mostram que o custo para resolver um bug em prudução chega a ser 1000x maior que o custo da feature desenvolvida.

## Classes, nomenclatura e responsabilidades
| Tipo | Exemplo | Descrição |
| ------------ | :--------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Objeto/Entidade	 | AccountDomain | Domain (extends fflib_SObjectDomain - se utilizarmos), em outras palavras, uma TriggerHandler, que controla o comportamento do objeto. Essa classe não deve possuir a notação @AuraEnabled. | 
| Batch | NameBatch.cls |  |
| Builder | NameViewModelBuilder.cls | Classe para separação das necessidades de conversão entre back-end (APEX) e front-end (Aura, LWC), quando ViewModel, e entre back-end e API HTTP (json) quando APIModel. Quando a complexidade da conversão for alta ou muito extensa. |
| Controller | NameController.cls | Presentation layer MVC, faz a gestão da camada VIEW, roteador para a camada de Service e Selector. Essa classe não deve possuir a responsabilidade de negócio (Service) e integração. |
| Invocable | NameInvocable.cls | Classes de invocação por meio de Flow. |
| Mock | NameMock.cls	| Nome é baseado no "CONTEXTO" de APIs, não objeto. Classe para criação de "mocks" para validação de testes em cenários de APIs (callout). |
| Service | NameService.cls | Service layer, consome APIs e serviços externos ao domínio dessa classe, também conhecida como BLL/BO. Deve ser a camada responsável por "commitar" as alterações de DML. |
| Selector | NameSelector.cls | Camada de abstração com a "base de dados", também conhecida como Repository/DAO |
| Test | NameServiceTest.cls	 | Classe para realização de testes "unitários", não apenas cobertura de código. |
| Trigger | NameTrigger.cls	 |  |
| TriggerHandler | ANameTriggerHandler.cls |  |
| WebService | NameWebService.cls | Classe de call in para integrações REST de endpoints customizados. |

## Referências:

1. [Apex Enterprise Patterns: Service Layer](https://trailhead.salesforce.com/en/content/learn/modules/apex\_patterns\_sl)
2. [Apex Enterprise Patterns: Domain & Selector Layers](https://trailhead.salesforce.com/en/content/learn/modules/apex\_patterns\_ds)
3. [Factory method pattern](https://en.wikipedia.org/wiki/Factory\_method\_pattern)
4. [Unit of Work](https://martinfowler.com/eaaCatalog/unitOfWork.html)
5. [Domain Model](https://martinfowler.com/eaaCatalog/domainModel.html)
6. [Data Mapper](https://martinfowler.com/eaaCatalog/dataMapper.html)
7. [Github - FFLib Apex Common](https://github.com/apex-enterprise-patterns/fflib-apex-common)
