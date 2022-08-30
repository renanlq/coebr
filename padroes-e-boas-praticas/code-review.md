# Code Review

_Code review_, é uma prática que pode salvar a sua vida, kkkkk em outras palavras, uma validação das alterações realizadas em seu repositório, por meio de um _Pull Request/Merge Request_, o que vem em conjunto com uma necessidade de criação de um conselho para avaliação de todas demandas, a fim de não centralizar tais atividades.

## Processo

1. **Pré-requisito**: Criação de um server-hook e/ou bloqueio das branchs de "integração, por parte do Desenvolvedor: criação do Pull/Merge Request.
2. **Atividade**: Indicação para uma solicitação de horário para avaliação junto a pelo menos 2 devs, sendo 1 do comitê de aprovadores, não mais que 15 minutos para o _code review_. Serão avaliados no Code Review: Classes, Aura, LWC, Validation Rules, Flows, e demais metadados que tem potencial de impacto no nosso ambiente.

## Check-list - básico

1\. `Boas práticas` de codificação: Orientação a Objetos, SOLID, KISS ...;\
2\. Se a entrega deverá estar aderente ao `Doc. de Arquitetura de Referencia` da sua Empresa, principalmente boas práticas Salesforce e nomenclatura;\
3\. `Avaliação estática de código` pelo [Apex PMD](https://github.com/renanlq/salesforce/blob/master/salesforce/padroes/apexpmd), execução de regras do ruleset para avaliação de score;\
&#x20;   3.1. `Sugestão` que esse passo seja executado diariamente durante processo de desenvolvimento;\
4\. `Validação das classes` de teste no ambiente origem (adicionar evidencia no PR com data/hora);\
5\. `Mensagens de commit` e Pull/Merge Request estão OK;\
6\. `Feature branch` está sincronizada com o ambiente destino (rebase); e\
7\. E em NENHUMA circunstancia pedir o `bypass` para "depois arrumar" (vale -100 pontos esse).

### Resultado avaliação

A. **Aprovação** - Qualidade de entrega ok, segue o jogo;\
B. **Qualidade a desejar** - Sem risco ao ambiente, aprovado mediante a aceite de To-Dos para a próxima entrega; e\
C. **Baixa qualidade** - Com risco ao ambiente (query em for por exemplo) não aprovamos e solicitamos a refatoração.

## Referências

1. [Apex Hours - Code review checklist](https://www.apexhours.com/code-review-checklist/)
