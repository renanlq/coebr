# Padrão de mensagens

É muito importante a definição e possivel padronização nas suas mensagens de erro, aviso e afins, pois dado o tamanho da sua Org, isso pode virar uma dor de cabeça pela falta de padronização, além de ser um ofensor no rastreio de issues em seu ambiete (vide que quando recebemos um INC, o print vem com a tela e a mensagem de erro que não diz muito sobre ela :P). Aqui iremos exemplificar possíveis definições para os padrões de mensagens de UI e Apex.

## Lightnintg

### Sucesso

```
A/O [ objeto ] foi [ criada/o | alterada/o | deletada/o ]  com sucesso. :-)

# Exempos
Sua solicitação de bloqueio foi enviada com sucesso.
O bloqueio do cartão foi efetivado com sucesso.
O boleto foi gerado com sucesso.
Sua solicitação de nova via de cartão foi enviada com sucesso.
```

### Alerta

```
A informação [ nome ] não foi preenchida, favor [ ação ] antes de prosseguir ;-)

# Exemplos
Falta informar o motivo do caso, favor informar antes de prosseguir ;-)
Fila atual não é compatível para realização da ação, favor encaminhar antes de prosseguir ;-)
```

### Erro

```
Não foi possível realizar a/o [ ação ], contate um Administrador. Info: [ nome da classe (sem extensão) ].
```

### Log de erro

```
console.error('lwc/aura:NomeClasse.metodo: erro');
```

## Apex

### Debug

```
System.debug('DEBUG: ' + informação);
```

IMPORTANTE! Lembrando que não é uma boa prática deixar um MONTE de debug por ai, usar com parcimônia, e remover esses antes de enviar para QA e principalmente UAT e PRD

### Log de erro

```
System.debug('ERROR: NomeClase.Metodo [linha]: Mensagem de erro');
```
