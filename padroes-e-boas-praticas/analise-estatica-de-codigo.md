# Análise estática de código

## Objetivo

Utilizar de um conjunto de regras sobre práticas de desenvolvimento Apex, com a finalidade de prever possíveis codificações fora de padronizações e até mesmo débitos técnicos e riscos de desenvolvimento.

## Apex rules

Existem uma grande quantidade de regras que podem ser utilizadas para avaliar seu código fonte, utilizando de SCA (_Static Code Analysis_), dessa forma tendo um _feedback_ em tempo de desenvolvimento e/ou até uma barreira em seu _pipeline_ de CI/CD. Os pontos abrangidos:

* Melhores práticas;
* Estilo de código;
* Design de código;
* Documentação;
* Propensão de erros;
* Performance;
* Segurança;
* Regras adicionais; e
* Customização própria de regras. Ver mais em: [Apex Rules](https://pmd.github.io/latest/pmd_rules_apex.html).

## Agentes de IA

Atualmente muitas empresas já estão atuando com a utilização de agentes de IA para atuar como revisores de código, não apenas sobre as práticas e padrões de código estático, mas também para uso na avaliação de itens de segurança e vulnerabilidades (OWASP), que anteriormente eram processamentos executados em ferramentas como SonarQube e Veracode.

Agentes de mercado:

* [Copilot Code Review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)
* [Gemini Code Assist](https://docs.cloud.google.com/gemini/docs/code-review/review-repo-code)

## Como utilizar

### **Local**

Utilizar a IDE Visual Studio Code + Extensão para APEX PMD, seguem links: [VSCode](https://code.visualstudio.com/) e [Extensão Apex PMD](https://marketplace.visualstudio.com/items?itemName=chuckjonas.apex-pmd).

### **Remoto**

Utilize suas ferramentas (Agende de IA, SonarQube, CodeScan, Veracode etc) de análise de código para realizar as avaliações a partir das regras explicitas no _ruleset.xml_, styleguide.yaml ou outro, criado em seu repositório.
