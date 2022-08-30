# Versionamento de código

## Requisitos

Para cada nova feature ou projeto, definição da estratégia de _branch_ e definição de utilização de _feature branchs_ e ambientes _sandbox_ (dev).

## Sistema de versionamento

Fortemente aconselhado a utilização do [git](https://git-scm.com/), uma vez que é a ferramenta mais utilizada pela comunidade e com maior integração entre as ferramentas de gestão de repositórios.

### **Estratégia de branches**

Isso varia de lugar para lugar, podendo se trabalhar desde um modelo simples como [github flow](https://guides.github.com/introduction/flow/) aos mais complexos como [git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), dado volume de ambientes e fluxos estipulados pela sua compania. Lembrando que para modelos mais complexos como _gitflow_ é interessante pensar na atualização dos ambientes quando possuirem diferentes fluxos até um ponto de conversão final, ex. pensar no "_rollover_" para ambientes anteriores ao mergeado, que não façam parte do seu fluxo.

#### **Feature branch**

Por que usar uma branch em separado por entrega, ex. Issue no JIRA. Uma vez que utiliza uma _branch_ para separação da sua "_feature_", fica muito mais fácil organizar seus _commits_ e validar possíveis impactos nos _merges_ (_pull/merge requests_) futuros.

Exemplos nomenclatura:

```
sandbox: devatendimento
featrue branch: feature/atd-180
```

Flixo para promoção dessa feature:

```
sandbox: devatendimento
branchs por ambiente:
- feature/atd-180
- origin/qa
- origin/uat
- origin/master
```

Dessa forma, para promoção, os _pull requests_ para os ambientes futuros (qa, uat ...) se da pelo _rebase_ da _branch_ feature/atd-180 e _pull requests_ nos ambientes subsequentes.

#### Sandbox branch

Nesse modelo temos uma única _branch_ para o ambiente de dev, ou seja, uma _branch_ recebendo _commits_ sobre todas atualizações ocorridas na _sandbox_. Esse modelo traz a facilidade de centralizar tudo em uma única branch, mas aumenta a complexidade da promoção das _features_ uma vez que os modelos das ferramentas de gestão de repos, usam o _merge_ entre _branches_. Ex. se temos 10 _commits_ em DEV, porém só queremos enviar 3 para QA, em ferramentas como Bitbucket, Gitlab entre outras, quando for criado o _pull request_, todos os 10 _commits_, que são difernetes entre as _branches_ serão listados no PR. Segue um modelinho que pode ajudar nessas tratativas.

Exemplos nomenclatura:

```
sandbox: devatendimento
featrue branch: feature/devatendimento
```

Flixo para promoção dessa feature:

```
sandbox: devatendimento
branchs por ambiente:
- feature/devatendimento
- feature/devatendimento-qa
- origin/qa
- feature/devatendimento-uat
- origin/uat
- feature/devatendimento-release
- origin/master
```

Dessa forma, pode se isolar os _commits_ via _cherry-pick_ e assim, sua feature/devatendimento-qa, é atualizada conforme _branch_ de QA, e isolamos os 3 _commits_ pretendidos para o PR, no inicio do exemplo. Esses passos são repetidos até o fim da promoção do pacote.

## Ferramentas

As ferramentas utilizadas estão listadas aqui: [Ferramentas](https://github.com/renanlq/salesforce/blob/master/salesforce/padroes/ferramentas).

## Padrão de mensagem

Indico utilizar a lingua padrão do local de trabalho, sendo assim Português-BR, para Brasil, e qualquer outro lugar fora (seja remoto) Inglês-US. Uma boa prática é pensar nas ferramentas e automações existentes entre as soluções de ALM e Gestão de Repositório, pois dessa forma podem ser aproveitados os _hooks_ de integração entre elas.

Um exemplo é usar o Jira + GitLab, dessa forma seria interessante utilizar o seguinte padrão:

* CÓDIGO \[Gatilho] Mensagem.
  * Código: Número em UPPERCASE, separados por "-", ex. "PROJETO-HISTORIA";
  * Trigger (se configurado): Para utilização de atualização o item dentro do JIRA, ex.: [Gitlab Integrations](https://docs.gitlab.com/ee/integration/jira/); e
  * Mensagem: Texto com pontuação, acentuação, que seja breve e conclusivo.

Exemplos:

```
ATD-001 Criação metadados
ATD-001 Adicação campos em objeto case
ATD-001 Fixes Correção classe case                             // gatilho para conclusão de "bug" no Jira
ATD-001 Resolves Conclusão histório de cadastro de atendimento // gatilho atualizar "história", concluindo a mesma

COE-002 Feature de lista de usuários
COE-001 Closes Fechamento do projeto                              // gatilho conclusão do "projeto"
```

**IMPORTANTE!** Informações sobre a sintax para mensagens de commit para automação com JIRA, devem segir modelo prédefinido pelas respectivas soluçeos de repositório, seja GitLab, Bitbucket e afins.

## Não versionáveis

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
