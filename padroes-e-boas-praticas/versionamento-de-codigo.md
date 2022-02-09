# Versionamento de código

#### Requisitos

Para cada nova feature ou projeto, definição da estratégia de _branch_ e definição de utilização de _feature branchs_ e ambientes _sandbox_ (dev).

#### Sistema de versionamento

Fortemente aconselhado a utilização do [git](https://git-scm.com), uma vez que é a ferramenta mais utilizada pela comunidade e com maior integração entre as ferramentas de gestão de repositórios.

**Estratégia de branches**

Isso vai de lugar para lugar, podendo se trabalhar desde um modelo simples como [github flow](https://guides.github.com/introduction/flow/) aos mais complexos como [git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), dado volume de ambientes e fluxos estipulados pela sua compania. Lembrando que para modelos mais complexos como _gitflow_ é interessante pensar na atualização dos ambientes quando possuirem diferentes fluxos até um ponto de conversão final, ex. pensar no "_rollover_" para ambientes anteriores ao mergeado, que não façam parte do seu fluxo.

**Feature Branch**

Por que usar uma branch em separado da sua de seu ambiente dev, por exemplo. Uma vez que utiliza uma _branch_ para separação da sua "_feature_", pode ser muito mais fácil organizar seus _commits_ e validar possíveis impactos nos _merges_ futuros.

Exemplos para fluxos simples:

```
sandbox: devatendimento
featrue branch: feature/cadastrouser
```

Exemplos para fluxo gitflow

```
sandbox: devatendimento
branchs por ambiente:
- ft-atendimento/qa
- ft-atendimento/uat
- ft-atendimento/release
```

#### Ferramentas

As ferramentas utilizadas estão listadas aqui: [Ferramentas](https://github.com/renanlq/salesforce/blob/master/salesforce/padroes/ferramentas).

#### Padrão de mensagem

Indico utilizar a lingua padrão do local de trabalho, sendo assim Português-BR, para Brasil, e qualquer outro lugar fora (seja remoto) Inglês-US. Uma boa prática é pensar nas ferramentas e automações existentes entre as soluções de ALM e Gestão de Repositório, pois dessa forma podem ser aproveitados os _hooks_ de integração entre elas.

Um exemplo é usar o Jira + GitLab, dessa forma seria interessante utilizar o seguinte padrão:

* CÓDIGO \[Gatilho] Mensagem.
  * Código: Número em UPPERCASE, separados por "-", ex. "PROJETO-HISTORIA";
  * Trigger: Para utilização de atualização o item dentro do JIRA, ver: [Gitlab Integrations](https://docs.gitlab.com/ee/integration/jira/); e
  * Mensagem: Texto com pontuação, acentuação, que seja breve e conclusivo.

Exemplos:

```
0001-01 Criação metadados
0001-01 Adicação campos em objeto case
0001-01 Fixes Correção classe case                             // gatilho para conclusão de "bug" no Jira
0001-01 Resolves Conclusão histório de cadastro de atendimento // gatilho atualizar "história", concluindo a mesma

0002 Feature de lista de usuários
0001 Closes Fechamento do projeto                              // gatilho conclusão do "projeto"
```

**IMPORTANTE!** Informações sobre a sintax para mensagens de commit para automação com JIRA, devem segir modelo prédefinido pelas respectivas soluçeos de repositório, seja GitLab, Bitbucket e afins.

#### Não versionáveis

Lista abaixo são dos metadados com suas respectivoas considerações, relacionadas ao versionamento e promoção desses dentro das suas automações de CI/CD:

|      Tipo do metadado     | Razão para excluir do CI/CD                                                                                                                                                                                                                                                                                                                                                                                                      |
| :-----------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       Auth Providers      | Provedores de autenticação geralmente são por ambiente, e os mesmos devem ser mantidos em segurança/segredo                                                                                                                                                                                                                                                                                                                      |
|        Certificate        | Certificados geralmente são por ambiente, e os mesmos devem ser mantidos em segurança/segredo                                                                                                                                                                                                                                                                                                                                    |
|      CleanDataService     | A DEFINIR                                                                                                                                                                                                                                                                                                                                                                                                                        |
|        ConnectedApp       | Connected Apps são geralmente específicos por ambiente. Se você automatizar a publicação desses, você terá que realizar alterações dinâmicas para algums parâmetros                                                                                                                                                                                                                                                              |
|         Dashboard         | Este são alterados frequentemente pelos usuários em produção e não deveriam estar contidos no ciclo de desenvolvimento, a não ser que hajam dependências em seus códigos ou metadados                                                                                                                                                                                                                                            |
|       DelegateGroup       | Específico pela complexidade da org                                                                                                                                                                                                                                                                                                                                                                                              |
|          Document         | Estes são alterados com frequência pelos usuários em produção e não deveriam constar em seu ciclo de desenvolvimento, a não ser que eles sejam uma dependência do seu código ou metadata                                                                                                                                                                                                                                         |
|       EmailTemplate       | Frequentemente alterados pelos usuários em produção e não devem passar pelo ciclo de vida de desenvolvimento, a menos que sejam uma dependência de seu código ou metadados. Uma exceção são os modelos de e-mail do VisualForce ou outros modelos que usam código incorporado e podem exigir um cuidadoso processo de desenvolvimento                                                                                            |
|       FlowDefinition      | Não é mais obrigatória a informação de versão do fluxo, desde o Metadata API version 44, e existe a indicação pela própria Salesforce, para não utilizar esse para ativação e desativação de fluxo. Dessa forma utilize o próprio metadado do fluxo para ativação e desativação do mesmo                                                                                                                                         |
|      InstalledPackage     | Você não controla a ordem de instalação. Utilize comandos de pacote do sfdx para instalar os pacotes de dependência                                                                                                                                                                                                                                                                                                              |
|           Layout          | Quando utilizando do modelo de desenvolvimento por pacote, layouts podem ser problematicos. Eles devem ser gerenciados a nível da org para quaisquer objetos que agranjam vários pacotes                                                                                                                                                                                                                                         |
|      NamedCredential      | São geralmente específicas do ambiente. Se você automatizar sua implantação, você precisará substituir dinamicamente alguns parâmetros                                                                                                                                                                                                                                                                                           |
|   ObjectTranslations \*   | Verificar se há necessidade de tradução de Labels para sua organização                                                                                                                                                                                                                                                                                                                                                           |
| PlatformCachePartition \* | Normalmente, são específicos do ambiente                                                                                                                                                                                                                                                                                                                                                                                         |
|          Profile          | Os perfis são os mais complicados de todos os tipos de metadados. Sem automação especial, você achará mais fácil gerenciá-los manualmente                                                                                                                                                                                                                                                                                        |
|   ProfileSessionSettings  | Configurações de sessão devem ser gerenciadas pelo SSO quando configurado login Federado para a organização                                                                                                                                                                                                                                                                                                                      |
|  ProfilePasswordPolicies  | Políticas de senha devem ser gerenciadas pelo SSO quando configurado login Federado para a organização                                                                                                                                                                                                                                                                                                                           |
|     RemoteSiteSetting     | Normalmente, são específicos do ambiente                                                                                                                                                                                                                                                                                                                                                                                         |
|           Report          | Alterados com frequência pelos usuários em produção, não devem passar pelo ciclo de vida de desenvolvimento, a menos que sejam uma dependência de seu código ou metadados                                                                                                                                                                                                                                                        |
|       SamlSsoConfig       | A configuração de SSO de SAML geralmente é específica do ambiente. Se você automatizar sua implantação, precisará substituir dinamicamente alguns parâmetros                                                                                                                                                                                                                                                                     |
|     Site _DEPRECIADO_     | O tipo de metadados do site representa Comunidades, mas é armazenado como um arquivo binário e pode ser alterado de maneira imprevisível, o que o torna um candidato insatisfatório para CI/CD. Forte sugestão a migração para _Experiences_: [ExperienceBundle for Experience Builder Sites](https://developer.salesforce.com/docs/atlas.en-us.communities\_dev.meta/communities\_dev/communities\_dev\_migrate\_expbundle.htm) |

LEGENDA: \* Específico por projeto/ambiente.
