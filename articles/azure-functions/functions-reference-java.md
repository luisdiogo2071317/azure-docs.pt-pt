---
title: Referência para programadores de Java nas funções do Azure | Documentos da Microsoft
description: Aprenda a desenvolver as funções com Java.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure funções, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: routlaw
ms.openlocfilehash: 9e07cddb9d446ea24143d3a6dec5e310d3ed6f1c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802122"
---
# <a name="azure-functions-java-developer-guide"></a>Guia de programadores de Java de funções do Azure

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Modelo de programação 

A função do Azure deve ser um método de classe sem monitoração de estado que processa as entradas e produz um resultado. Embora possa escrever métodos de instância, sua função deve não depender quaisquer campos de instância da classe. Todos os métodos de função tem de ter um `public` modificador de acesso.

## <a name="folder-structure"></a>estrutura de pastas

A estrutura de pastas para um projeto de Java é semelhante ao seguinte:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

Existe um ficheiro partilhado [host. JSON] (funções-host-json.md) que pode ser utilizado para configurar a aplicação de funções. Cada função tem seu próprio arquivo de código (Java) e o ficheiro de configuração de vinculação (Function).

Pode colocar mais do que uma função num projeto. Evite colocar as suas funções em jars separados. FunctionApp no diretório de destino é o que é implementado para a aplicação de funções no Azure.

## <a name="triggers-and-annotations"></a>Acionadores e as anotações

 As funções do Azure são chamadas por um acionador, como um pedido HTTP, um temporizador ou uma atualização de dados. A função precisa processar esse acionador e todas as outras entradas para produzir uma ou mais saídas.

Utilize as anotações de Java incluídas nos [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) pacote para o enlace de entrada e saídas para seus métodos. Usando as anotações de código de exemplo está disponível na [documentos de referência de Java](/java/api/com.microsoft.azure.functions.annotation) cada anotação e na documentação de referência de ligação do funções do Azure, como o existente para [acionadores HTTP](/azure/azure-functions/functions-bindings-http-webhook).

Entradas de Acionador e saídas também podem ser definidas na [Function](/azure/azure-functions/functions-reference#function-code) para a função em vez de por meio de anotações. Usando `function.json` em vez de anotações dessa maneira não é recomendado.

> [!IMPORTANT] 
> Tem de configurar uma conta de armazenamento do Azure no seu [Settings](/azure/azure-functions/functions-run-local#local-settings-file) para executar os acionadores de tabela, fila ou Blob de armazenamento do Azure localmente.

Exemplo de uso anotações:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
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

## <a name="jdk-runtime-availability-and-support"></a>Disponibilidade de tempo de execução do JDK e suporte 

Transferir e utilizar o [Azul Zulu para o Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) JDKs partir [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) para o local de desenvolvimento de aplicações de funções do Java. JDKs estão disponíveis para Windows, Linux e macOS e [suporte do Azure](https://support.microsoft.com/en-us/help/4026305/sql-contact-microsoft-azure-support) está disponível para problemas encontrados durante o desenvolvimento com uma [plano de suporte qualificado](https://azure.microsoft.com/support/plans/).

## <a name="third-party-libraries"></a>Bibliotecas de terceiros 

As funções do Azure suporta a utilização de bibliotecas de terceiros. Por predefinição, todas as dependências especificado no seu projeto `pom.xml` ficheiro serão automaticamente agrupado durante o `mvn package` objetivo. Para bibliotecas não especificadas como dependências a `pom.xml` de ficheiros, colocá-los num `lib` diretório no diretório de raiz da função. Dependências colocadas o `lib` directory será adicionado ao carregador de classe do sistema no tempo de execução.

O `com.microsoft.azure.functions:azure-functions-java-library` dependência é fornecida no classpath por predefinição e não precisa de ser incluídos no `lib` diretório.

## <a name="data-type-support"></a>Tipo de dados de suporte

Pode usar qualquer tipo de dados no Java para os dados de entrada e saídos, incluindo tipos nativos; personalizado Java tipos e especializadas do Azure definidos no `azure-functions-java-library` pacote. Funções do Azure, tempo de execução tenta converter a entrada recebida no tipo solicitado pelo seu código.

### <a name="strings"></a>Cadeias

Valores passados para os métodos de função vão ser convertidos para cadeias de caracteres, se o tipo de parâmetro de entrada correspondente para a função é do tipo `String`. 

### <a name="plain-old-java-objects-pojos"></a>Objetos de Java (POJOs) antigos sem formatação

Cadeias de caracteres formatadas com JSON irão ser convertidas para tipos de Java, se a assinatura de entrada da função espera que esse tipo de Java. Esta conversão permite-lhe transmitir em JSON e trabalhar com tipos de Java.

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

Podem ser vazios valores de entrada `null` como seu argumento de funções, mas uma forma recomendada de lidar com potencial de valores vazios consiste em utilizar `Optional<T>`.


## <a name="function-method-overloading"></a>Sobrecarga de método de função

Pode sobrecarregar métodos de função com o mesmo nome mas com tipos diferentes. Por exemplo, pode ter ambos `String echo(String s)` e `String echo(MyType s)` numa classe. As funções do Azure decide qual o método a invocar com base no tipo de entrada (para o tipo MIME de entrada, HTTP `text/plain` leva à `String` enquanto `application/json` representa `MyType`).

## <a name="inputs"></a>Entradas

Entrada são divididas em duas categorias nas funções do Azure: um é a entrada de Acionador e o outro é a entrada adicional. Embora eles sejam diferentes em `function.json`, a utilização é idêntica no código de Java. Vejamos o seguinte trecho de código como exemplo:

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String in,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") MyObject obj
    ) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    public static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Quando esta função é acionada, o pedido HTTP é passado para a função por `String in`. Uma entrada será recuperada do armazenamento de tabelas do Azure com base no ID no URL de rota e feitas disponíveis como `obj` no corpo da função.

## <a name="outputs"></a>Saídas

Podem ser expresso saídas no valor de retorno ou parâmetros de saída. Se existir apenas uma saída, são recomendadas para usar o valor de retorno. Para várias saídas, precisa usar parâmetros de saída.

Valor de retorno é a forma mais simples de saída, basta retornar o valor de qualquer tipo e tempo de execução de funções do Azure irá tentar o empacota novamente para o tipo real (por exemplo, uma resposta HTTP).  Pode aplicar qualquer anotações de saída para o método de função (a propriedade de nome da anotação tem de ser $return) para definir a saída do valor de retorno.

Para produzir vários valores de saída, utilize `OutputBinding<T>` tipo definido no `azure-functions-java-library` pacote. Se precisar de fazer uma resposta HTTP e enviar uma mensagem para uma fila também, pode escrever algo como:

Por exemplo, um conteúdo de blob de cópia de função pode ser definido como o código a seguir. `@StorageAccount` anotação é utilizada aqui para evitar a duplicação de propriedade de ligação para os dois `@BlobTrigger` e `@BlobOutput`.

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Utilizar `OutputBinding<byte[]`> tornar um binário de saída do valor (para os parâmetros); para valores de retorno, basta utilizar `byte[]`.

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
import com.microsoft.azure.functions.annotation.*;


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

## <a name="execution-context"></a>Contexto de execução

Interagir com o ambiente de execução de funções do Azure através da `ExecutionContext` objeto definido no `azure-functions-java-library` pacote. Utilizar o `ExecutionContext` objeto para usar as informações de invocação e informações de tempo de execução de funções no seu código.

### <a name="custom-logging"></a>Registo personalizado

Acesso para o agente de log de tempo de execução de funções está disponível através do `ExecutionContext` objeto. Este agente de log está associado ao Azure monitor e permite-lhe sinalizador avisos e erros encontrados durante a execução de função.

O seguinte código de exemplo regista uma mensagem de aviso quando o corpo do pedido recebido está vazio.

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Ver registos e de rastreio

Pode utilizar a CLI do Azure para o stream Java padrão horizontalmente e o registo de erros, bem como outros logs do aplicativo. Em primeiro lugar, Configure a sua aplicação de função para escrever o registo de aplicações com a CLI do Azure:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Para transmitir o registo de saída para a sua aplicação de função com a CLI do Azure, abra uma nova linha de comandos, o Bash ou a sessão de Terminal e introduza o seguinte comando:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
O [cauda do az webapp log](/cli/azure/webapp/log) comando tem opções para filtrar a saída com o `--provider` opção. 

Para transferir os ficheiros de registo como um ficheiro ZIP único utilizando a CLI do Azure, abra uma nova linha de comandos, o Bash ou a sessão de Terminal e introduza o seguinte comando:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Tem de ter ativado o sistema de ficheiros de registo no Portal do Azure ou na CLI do Azure antes de executar este comando.

## <a name="environment-variables"></a>Variáveis de ambiente

Manter informações secretas, tais como chaves ou tokens fora do seu código-fonte por motivos de segurança. Utilizar chaves e tokens no código da função, lendo-los a partir de variáveis de ambiente.

Para definir as variáveis de ambiente durante a execução das funções do Azure localmente, pode optar por adicionar essas variáveis para o ficheiro Settings. Se não houver no diretório de raiz do seu projeto de função, pode criar um. Eis o que o ficheiro deverá ser semelhante:

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
> Se estiver atribuída essa abordagem, ser-se de que para adicionar o Settings ficheiro para o seu repositório ignorar ficheiros, para que não está consolidada.

Com o seu código agora consoante estas variáveis de ambiente, pode iniciar sessão portal do Azure para definir a mesma chave / valor pares de contratos nas suas definições de aplicação de função, para que seu código funciona de forma equivalente quando testar localmente e quando implementada no Azure.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o desenvolvimento de Java de função do Azure, consulte os seguintes recursos:

* [Best Practices for Azure Functions (Melhores Práticas para as Funções do Azure)](functions-best-practices.md)
* [Referência para programadores das Funções do Azure](functions-reference.md)
* [Acionadores de funções do Azure e enlaces](functions-triggers-bindings.md)
- Desenvolvimento local e de depuração com [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md), e [Eclipse](functions-create-maven-eclipse.md). 
* [Depuração remota do Java Azure funciona com o Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Plug-in do maven para as funções do Azure](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) -Simplifique a criação da função através do `azure-functions:add` objetivo e preparar um diretório de transição para [implementação de ficheiros ZIP](deployment-zip-push.md).
