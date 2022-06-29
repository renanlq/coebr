# Tratamento de erros Apex
Práticas para tratamento de erros, boas práticas de uso, transporte da mensagem de exceção entre as camadas, de back-end. Aqui não definimos como a msg será apresentada no front-end, apenas unificando o meio de transporte até ele, sendo assim responsabilidade do front-end como apresentar as mesmas para o usuário.

### Exceções

Utilização do block try-catch e classes de exceções padrões Salesforce
```
try {
    Case caso = CaseSelector.getById(CaseId); // Imagina o ID inexistente

    // Qualquer código após o bloco que gera a exceção não será executado
    System.debug('Esse debug irá aparecer no log :P');
}
catch(QueryException queryException) {
    System.debug('Erro em query' + queryException.getMessage());
    throw queryException;
}
catch(Exception exception) {
    System.debug('Erro genérico' + exception.getMessage()); 
    throw exception; 
}
finally {
    // Realizar algo ao fim
}
```
Ver mais na referência: Apex Methods System Limits.

### Disparo de exceções

Como trafegar sua msg de exceção, pelas camadas, até seu LWC ou Aura   
```
public with sharing class BloqueioCartaoController {
    @AuraEnabled
    public static void blockCard(){
        try {
            CardService.blockCard();
	}
	catch (Exception e) {
	    AuraHandledException auraException = new AuraHandledException(e.getMessage()); // msg é obrigatória no construtor
	    auraException.setMessage(e.getMessage()); // mas se faz necessário passar no SET a msg novamente :S
            throw auraException;
	}
    }
}
```

```
public with sharing class CardService {
    public static void blockCard() {
        try {
            // Alguma logica de consulta para validar possibilidade de bloqueio do cartao
            if (UsuarioSemPermissao) {
	        throw new CardServiceException('Usuário não tem permissão para realizar o bloqueio do cartão');
            }

            CardAPIService.blockCard();
        }
        catch (Exception e) { 
            // Qualquer exceção não tratada
            throw e; 
        }
    }

    private class CardServiceException extends Exception {} 
}
```

```
public with sharing class CardAPIService {
    public static void blockCard() {
        try {
            Map<String, Object> token = HttpService.getTokenMTLS();
            // Monta o header com o token
            Map<String, String> header = new Map<String, String>();

            HttpResponse response = HttpService.get('URL', header); 	
            if (response.getStatusCode() != 200) {
	        throw new CardAPIServiceException('Erro na chamada de API. Erro ' + response.getStatusCode() + ' | ' + response.getBody());
            }
        }
        catch (Exception e) { 
            // Qualquer exceção não tratada
            throw e;
        }
    }

    private class CardAPIServiceException extends Exception {}
}
```

### Referências
1. [Apex Governor Limits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm)  
2. [Apex Methods System Limits](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_limits.htm)  
