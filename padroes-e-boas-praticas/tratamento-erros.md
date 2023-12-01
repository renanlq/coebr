# Tratamento de erros Apex

Práticas para tratamento de erros, boas práticas de uso, transporte da mensagem de exceção entre as camadas, de back-end. Aqui não definimos como a msg será apresentada no front-end, apenas unificando o meio de transporte até ele, sendo assim responsabilidade do front-end como apresentar as mesmas para o usuário.

## Exceções

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

## Disparo de exceções

Como trafegar sua msg de exceção, pelas camadas, até seu LWC ou Aura &#x20;

### Controller

```
public with sharing class ExampleController {
 
    @AuraEnabled
    public static void blockCard() {
        try {
            ExampleService.blockCard();
        }
        catch (ExampleService.ExampleServiceException e) {
            System.debug('ERROR: ' + e.getMessage() + '\n\n' + e.getStackTraceString());
            throw new AuraHandledException(JSON.serialize(e));
        }
        catch (ExampleAPIService.ExampleAPIServiceException e) {
            System.debug('ERROR: ' + e.getMessage() + '\n\n' + e.getStackTraceString());
            throw new AuraHandledException(JSON.serialize(e));
        }
        catch (Exception e) {
            System.debug('ERROR: ' + e.getMessage() + '\n\n' + e.getStackTraceString());
            throw new AuraHandledException('Não foi possível realizar a ação, contate um Administrador.');
        }
    }
}
```

### Service

```
public with sharing class ExampleService {
     
    public static void blockCard() {
 
        // Alguma logica de consulta para validar possibilidade de bloqueio do cartao
        Boolean UsuarioSemPermissao = false;
        if (UsuarioSemPermissao) {
            System.debug('ERROR: CardService.blockCard: ' + UsuarioSemPermissao);
            throw new ExampleServiceException(ExampleServiceException.Type.WARNING,
                                              'Usuário não tem permissão para realizar o bloqueio do cartão');
        }
         
        ExampleAPIService.blockCard();
    }
     
    public class ExampleServiceException extends BaseException {}
}
```

### APIService

```
public with sharing class ExampleAPIService {
 
    public static void blockCard() {
 
        HttpResponse response = HttpClient.post(configmdt, 'url', headers, body);
 
        if (response.getStatusCode() != 200) {
            System.debug('ERROR: ExampleAPIServiceAPIService.blockCard: ' + response.getBody());
             
            // Se for necessário tratar os erros de HTTP, isso pode ser coletado no objeto de Response
            // IMPORTANTE! Exceção na API não estoura exception try-catch do APEX, são contextos diferentes.
            if (response.getStatusCode() >= 500) {
                throw new ExampleAPIServiceException(ExampleAPIServiceException.Type.ERROR, 
                                                     'Erro não esperado da API, tente novamente mais tarde.');
            }
            else if (response.getStatusCode() >= 400) {
                throw new ExampleAPIServiceException(ExampleAPIServiceException.Type.WARNING,
                                                     'Erro na chamada de API, solicitação do cliente não corresponde aos parâmetros do serviço.');
 
                // Aqui se o response.Body tiver msg de erro para ajudar a tratativa, podemos avaliar a possibilidade de concatenar à msg de erro.
                // Ex. Request.Body = 'Campo requerido não informado - Nome do cliente'
                // > throw new ExampleAPIServiceException(ExampleAPIServiceException.Type.WARNING, 'Erro na chamada de API: ' + response.getBody());
                // Ex. do resultado: 'Erro na chamada de API: Campo requerido não informado - Nome do cliente'.
            }
             
            throw new ExampleAPIServiceException('Erro na chamada de API.');
        }
    }
 
    public class ExampleAPIServiceException extends BaseException { }
}
```

### Base Exception

```
public with sharing class BaseException extends Exception {
 
    public Type type;
    public String message;
 
    public BaseException(Type pType, String pMessage) {
        this.type = pType;
        this.message = pMessage;
    }
 
    public Enum Type { INFO, WARNING, ERROR } // Para auxiliar no uso dos diferentes toats pelo LWC e Aura
}
```

## Referências

1. [Apex GovernorLimits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm)
2. [Apex Methods System Limits](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_limits.htm)
3. [Error Handling Best Practices for Lightning and Apex](https://developer.salesforce.com/blogs/2017/09/error-handling-best-practices-lightning-apex)
4. [Error Handling Best Practices for Lightning Web Components](https://developer.salesforce.com/blogs/2020/08/error-handling-best-practices-for-lightning-web-components)
5. [Exception Class and Built-In Exceptions](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_classes_exception_methods.htm)
6. [Returning Errors from an Apex Server-Side Controller](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/controllers_server_apex_custom_errors.htm)
