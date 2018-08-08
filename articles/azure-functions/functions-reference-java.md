---
title: Referência para programadores de Java nas funções do Azure | Documentos da Microsoft
description: Aprenda a desenvolver as funções com Java.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure funções, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw
ms.openlocfilehash: 65964372cf2a0aa42be967f7c93749c58a9f56dd
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621774"
---
# <a name="azure-functions-java-developer-guide"></a>Guia de programadores de Java de funções do Azure

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Modelo de programação 

A função do Azure deve ser um método de classe sem monitoração de estado que processa as entradas e produz um resultado. Embora têm permissão para escrever métodos de instância, sua função deve não depender quaisquer campos de instância da classe. Todos os métodos de função tem de ter um `public` modificador de acesso.

## <a name="triggers-and-annotations"></a>Acionadores e as anotações

Normalmente, uma função do Azure é invocada devido a um acionador externo. A função precisa processar esse acionador e das respetivas entradas associadas e produzir um ou mais saídas.

Anotações de Java estão incluídas no `azure-functions-java-core` pacote para o enlace de entrada e saídas para seus métodos. Os acionadores de entrada suportados e anotações de enlace de saída estão incluídos na tabela a seguir:

Vínculo | Anotação
---|---
CosmosDB | N/A
HTTP | <ul><li>`HttpTrigger`</li><li>`HttpOutput`</li></ul>
Mobile Apps | N/A
Hubs de Notificação | N/A
Blob de Armazenamento | <ul><li>`BlobTrigger`</li><li>`BlobInput`</li><li>`BlobOutput`</li></ul>
Fila de Armazenamento | <ul><li>`QueueTrigger`</li><li>`QueueOutput`</li></ul>
Tabela de armazenamento | <ul><li>`TableInput`</li><li>`TableOutput`</li></ul>
Temporizador | <ul><li>`TimerTrigger`</li></ul>
Twilio | N/A

Entradas de Acionador e saídas também podem ser definidas na [Function](/azure/azure-functions/functions-reference#function-code) para a sua aplicação.

> [!IMPORTANT] 
> Tem de configurar uma conta de armazenamento do Azure no seu [Settings](/azure/azure-functions/functions-run-local#local-settings-file) para executar os acionadores de tabela, fila ou Blob de armazenamento do Azure localmente.

Exemplo de uso anotações:

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

A mesma função escrita sem anotações:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

com o correspondente `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="data-types"></a>Tipos de Dados

Tem liberdade para utilizar todos os tipos de dados em Java para os dados de entrada e saídos, incluindo tipos nativos; personalizado Java tipos e especializadas do Azure definidos no `azure-functions-java-core` pacote. Funções do Azure, tempo de execução tenta converter a entrada recebida no tipo solicitado pelo seu código.

### <a name="strings"></a>Cadeias

Valores passados para os métodos de função vão ser convertidos para cadeias de caracteres, se o tipo de parâmetro de entrada correspondente para a função é do tipo `String`. 

### <a name="plain-old-java-objects-pojos"></a>Objetos de Java (POJOs) antigos sem formatação

Cadeias de caracteres formatadas com JSON irão ser convertidas para tipos de Java, se a entrada do método função espera que esse tipo de Java. Esta conversão permite-lhe passar entradas JSON para as suas funções e trabalhar com tipos de Java no seu código sem ter de implementar a conversão em seu próprio código.

Tipos POJO utilizados como entradas para as funções têm o mesmo `public` modificador de acesso como os métodos de função estão a ser utilizados na. Não é necessário declarar os campos de classe POJO `public`. Por exemplo, uma cadeia de caracteres do JSON `{ "x": 3 }` é capaz de ser convertida no tipo POJO seguinte:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>Dados binários

Dados binários são representados como uma `byte[]` em seu código de funções do Azure. Vincular o binárias entradas ou saídas para as suas funções, definindo a `dataType` campo no seu Function para `binary`:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

Em seguida, utilize-o em seu código de função:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Utilize `OutputBinding<byte[]>` tipo para tornar um enlace de saída binário.


## <a name="function-method-overloading"></a>Sobrecarga de método de função

Pode sobrecarregar métodos de função com o mesmo nome mas com tipos diferentes. Por exemplo, pode ter ambos `String echo(String s)` e `String echo(MyType s)` numa classe e as funções do Azure o runtime decide qual para invocar ao examinar o tipo de entrada real (para o tipo MIME de entrada, HTTP `text/plain` leva à `String` enquanto `application/json` representa `MyType`).

## <a name="inputs"></a>Entradas

Entrada são divididas em duas categorias nas funções do Azure: um é a entrada de Acionador e o outro é a entrada adicional. Embora eles sejam diferentes em `function.json`, a utilização é idêntica no código de Java. Vejamos o seguinte trecho de código como exemplo:

```java
package com.example;

import com.microsoft.azure.serverless.functions.annotation.BindingName;
import java.util.Optional;

public class MyClass {
    public static String echo(Optional<String> in, @BindingName("item") MyObject obj) {
        return "Hello, " + in.orElse("Azure") + " and " + obj.getKey() + ".";
    }

    private static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

O `@BindingName` anotação aceita um `String` propriedade que representa o nome do acionador/ligação definida no `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "put" ],
      "route": "items/{id}"
    },
    {
      "type": "table",
      "name": "item",
      "direction": "in",
      "tableName": "items",
      "partitionKey": "Example",
      "rowKey": "{id}",
      "connection": "ExampleStorageAccount"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```

Portanto, quando esta função é invocada, o payload de pedido HTTP passa opcional `String` para o argumento `in` e um armazenamento de tabelas do Azure `MyObject` tipo passado para o argumento `obj`. Utilize o `Optional<T>` tipo para lidar com entradas para as suas funções que podem ser nulas.

## <a name="outputs"></a>Saídas

Podem ser expresso saídas no valor de retorno ou parâmetros de saída. Se existir apenas uma saída, são recomendadas para usar o valor de retorno. Para várias saídas, precisa usar parâmetros de saída.

Valor de retorno é a forma mais simples de saída, basta retornar o valor de qualquer tipo e tempo de execução de funções do Azure irá tentar o empacota novamente para o tipo real (por exemplo, uma resposta HTTP). Na `functions.json`, utilize `$return` como o nome da ligação de saída.

Para produzir vários valores de saída, utilize `OutputBinding<T>` tipo definido no `azure-functions-java-core` pacote. Se precisar de fazer uma resposta HTTP e enviar uma mensagem para uma fila também, pode escrever algo como:

```java
package com.example;

import com.microsoft.azure.serverless.functions.OutputBinding;
import com.microsoft.azure.serverless.functions.annotation.BindingName;

public class MyClass {
    public static String echo(String body, 
    @QueueOutput(queueName = "messages", connection = "AzureWebJobsStorage", name = "queue") OutputBinding<String> queue) {
        String result = "Hello, " + body + ".";
        queue.setValue(result);
        return result;
    }
}
```

qual deve definir o enlace de saída no `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "queue",
      "name": "queue",
      "direction": "out",
      "queueName": "messages",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```
## <a name="specialized-types"></a>Tipos de especializadas

Por vezes, uma função tem têm detalhados controle sobre entradas e saídas. Especializada tipos no `azure-functions-java-core` pacote são fornecidos manipular as informações de pedido e adaptar o status de retorno de um acionador HTTP:

| Tipo especializado      |       Destino        | Utilização normal                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    Acionador HTTP     | Obter o método, cabeçalhos ou consultas |
| `HttpResponseMessage<T>` | Enlace de saída de HTTP | Status de retorno diferente de 200   |

> [!NOTE] 
> Também pode utilizar `@BindingName` anotação para obter cabeçalhos HTTP e consultas. Por exemplo, `@BindingName("name") String query` itera os cabeçalhos de pedido HTTP e as consultas e passar esse valor para o método. Por exemplo, `query` serão `"test"` se o URL do pedido é `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Metadados

Metadados é proveniente de diferentes origens, como cabeçalhos HTTP, consultas HTTP, e [acionar metadados](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Utilize o `@BindingName` anotação junto com o nome de metadados para obter o valor.

Por exemplo, o `queryValue` no código a seguir o trecho de código será `"test"` se o URL pedido for `http://{example.host}/api/metadata?name=test`.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.serverless.functions.annotation.*;

public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="functions-execution-context"></a>Contexto de execução de funções

Interagir com o ambiente de execução de funções do Azure através da `ExecutionContext` objeto definido no `azure-functions-java-core` pacote. Utilizar o `ExecutionContext` objeto para usar as informações de invocação e informações de tempo de execução de funções no seu código.

### <a name="logging"></a>Registo

Acesso para o agente de log de tempo de execução de funções está disponível através do `ExecutionContext` objeto. Este agente de log está associado ao Azure monitor e permite-lhe sinalizador avisos e erros encontrados durante a execução de função.

O seguinte código de exemplo regista uma mensagem de aviso quando o corpo do pedido recebido está vazio.

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="environment-variables"></a>Variáveis de ambiente

Muitas vezes, é desejável para extrair informações secretas do código-fonte por motivos de segurança. Isso permite que o código a ser publicado para repositórios de código de origem, sem fornecer acidentalmente as credenciais para outros desenvolvedores. Isso pode ser conseguido simplesmente usando variáveis de ambiente, ao executar as funções do Azure localmente e quando implementar as suas funções no Azure.

Para definir facilmente as variáveis de ambiente durante a execução das funções do Azure localmente, pode optar por adicionar essas variáveis para o ficheiro Settings. Se não houver no diretório de raiz do seu projeto de função, fique à vontade criar um. Eis o que o ficheiro deverá ser semelhante:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Cada chave / valor mapeamento no `values` mapa estarão disponível em tempo de execução como uma variável de ambiente, acessível ao chamar `System.getenv("<keyname>")`, por exemplo, `System.getenv("AzureWebJobsStorage")`. Adicionar chave adicional / pares valor são aceites e recomendado prática.

> [!NOTE]
> Se estiver atribuída essa abordagem, ser-se de que a ter em consideração se adicionar o Settings ficheiro para o seu repositório ignorar ficheiros, para que não está consolidada.

Com o seu código agora consoante estas variáveis de ambiente, pode iniciar sessão Portal do Azure para definir a mesma chave / valor pares de contratos nas suas definições de aplicação de função, para que seu código funciona de forma equivalente quando testar localmente e quando implementada no Azure.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte os seguintes recursos:

* [Best Practices for Azure Functions (Melhores Práticas para as Funções do Azure)](functions-best-practices.md)
* [Referência para programadores das Funções do Azure](functions-reference.md)
* [Acionadores de funções do Azure e enlaces](functions-triggers-bindings.md)
* [Depuração remota do Java Azure funciona com o Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
