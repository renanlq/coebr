# Ambientes Sandbox

### Objetivo

Ter uma orientação na criação de um fluxo de ambientes para orientar da melhor forma colaboradores no ciclo de desenvolvimento. Auxiliando arquitetos e desenvolvedores, pessoas diretamente relacionadas aos projetos que envolvem Salesforce e com acesso à gestão e manutenção dos ambientes existentes no fluxo de desenvolvimento.

### Sandbox

Uma prática é disponibilizar sandboxes por frende de projeto, por quê? Pensamos na questão de organização e manutenção dos metadados existentes nesse ambiente, pense em ambiente por "feature". E quando deixar tudo junto? Quando tiver uma ou mais equipes atuando em um mesmo produto, mas muito cuidado ao utilizar essa estratégia e gerar complicações nos _retrieves_ para separação dos _commits_ e entregas.

Mais abaixo falaremos de ambientes que possuem outro tipo de licença.

### Registros (dados):

Dado que para ambientes de desenvolvimento é indicada a licença "_developer_", que tem capacidade de 200MB em dados, atuar com migração dos mesmos utilizando das ferramentas disponíveis em sua empresa (Dataloader, Inspector, AutoRabit Dataloader Pro etc). Lembrando que esses dados devem ser previamente mapeados para criação do processo de migração dos dados de produção para a sandbox em questão.

### Exemplo de fluxo de ambientes

\[IMG EXEMPLO]

#### **Projetos**

Uma indicação de fluxo de promoção deploy, visando uma economia quanto à licenças de sandox, é:

> Desenvolvimento > Integração\* > Qualidade > Homologação > Produção

As responsabilidades são divididas por ambiente da seguinte forma:

| Ambiente     |   Licensa  | Objetivo                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------ | :--------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **DEV**      | dev/devpro | Ambiente destinado às atividades de desenvolvimento, utilizado por squads/equipes e/ou desenvolvedores em bodyshop                                                                                                                                                                                                                                                                                                                                   |
| **INT**      | dev/devpro | QUANTO EXISTENTE, ambiente para integração dos códigos fontes, com responsabilidade de atividades de agregação de entregas relacionadas a um projeto macro, ou seja, que tenha mais de um ambiente de desenvolvimento para unificação dos fontes. Uma vez que o objetivo desse ambiente é validar a "junção" das entregas de diferentes frentes de um projeto a nível apenas de metadata, não se faz necessária capacidade de armazenamento de dados |
| **QA**       |   partial  | Ambiente destinado às atividades de "qualidade", onde serão realizadas as atividades de teste se qualidade de software, ambiente com a responsabilidade de ser a primeira barreira na geração de bugfixes. Ambiente geralmente com licença _partial_ para ajudar nas automações de teste com dados amostrais, podendo também ser utilizada a migração de massas de dados para execução de cenários específicos                                       |
| **UAT**      |    full    | Ambiente de "aceite do usuário" (User Acceptance Test), aquele acessado pela área fim para teste/homologação do pacote promovido. A indicação de lisença _full_, dá-se pela possibilidade de realização de testes com maior qualidade de dados, testes de stress e validação de automações com maior similaridade com ambiente produtivo                                                                                                             |
| **Produção** |    full    | Como ambiente "live" de sua empresa, o acesso a administração do mesmo deve ser o mais restrito possível, sendo permitido apenas durante processos de implantação, quando houverem necessidades de atividades pré e pós deploy                                                                                                                                                                                                                       |

#### **Sustentação**

Quando existir? Existe uma indicação para que as _issues_ encontradas devem ser priorizadas e executadas pelas respectivas _squads_ de projetos, seguindo o fluxo de desenvolvimento e gerenciamento de projeto normal. Uma vez que a equipe e P.O. da solução possuem mais autonomia e expetise sobre o assunto.

### Promoção

Ver em: [CI/CD](ci-cd.md).

### Permissão de acesso

Indicamos "fortemente" que sejam aplicadas apenas durante processos de GMUD/CRQ, por meio de solicitações de acesso, e exclusivamente para tarefas de pré/pós deploy em ambientes de QA, UAT e/ou Produção, novamente exclusivamente para esse intuito, nada mais. Uma vez que manter usuários administradores nos ambientes integrados pode gerar um risco na sincronia entre o código fonte e ambiente.
