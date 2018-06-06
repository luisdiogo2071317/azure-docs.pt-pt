---
title: Azure Cosmos DB enlaces de funções de 2. x (pré-visualização)
description: Compreenda como utilizar o Azure Cosmos DB acionadores e enlaces das funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: das funções do Azure, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 0367f1fc942cf8fa43e2b4b6b0b21bc0ec9f377c
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796915"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x-preview"></a>Enlaces de Cosmos BD do Azure para as funções do Azure 2 (pré-visualização)

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versão 1 - GA](functions-bindings-cosmosdb.md)
> * [Versão 2 - Pré-visualização](functions-bindings-cosmosdb-v2.md)

Este artigo explica como trabalhar com [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) enlaces de funções do Azure 2. x. Funções do Azure suporta acionam, de entrada e saída enlaces para Azure Cosmos DB.

> [!NOTE]
> Este artigo destina-se [versão das funções do Azure 2](functions-versions.md), que está em pré-visualização.  Para obter informações sobre como utilizar estas enlaces nas funções de 1. x, consulte [enlaces de BD do Cosmos do Azure para as funções do Azure 1. x](functions-bindings-cosmosdb.md).
>
> Este enlace foi originalmente denominado DocumentDB. Na versão de funções 2. x, o acionador, enlaces e pacote são todos os com o nome base de dados do Cosmos.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pacotes - funciona 2. x

Os enlaces de BD do Cosmos do Azure para a versão de funções 2 são fornecidos no [Microsoft.Azure.WebJobs.Extensions.CosmosDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) pacote NuGet, versão 3. Código de origem para os enlaces está a ser o [azure-webjobs-sdk-extensões](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) repositório do GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Acionador

O acionador de base de dados do Azure Cosmos utiliza o [Azure Cosmos DB alteração Feed](../cosmos-db/change-feed.md) para escutar inserções e atualizações em partições. O feed de alteração publica inserções e atualizações, eliminações não.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de específicas do idioma:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Exemplos de Acionador de ignorar](#trigger---attributes)

### <a name="trigger---c-example"></a>Acionador - c# exemplo

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que é invocada quando existem insere ou atualizações na base de dados especificado e a coleção.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[Exemplos de Acionador de ignorar](#trigger---attributes)

### <a name="trigger---c-script-example"></a>Acionador - exemplo de script do c#

O exemplo seguinte mostra um acionador de base de dados do Cosmos enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função escreve mensagens de registo quando são modificados registos da base de dados do Cosmos.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Eis o código de script do c#:
 
```cs 
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
      log.Verbose("Documents modified " + documents.Count);
      log.Verbose("First document Id " + documents[0].Id);
    }
```

[Exemplos de Acionador de ignorar](#trigger---attributes)

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra um acionador de base de dados do Cosmos enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função escreve mensagens de registo quando são modificados registos da base de dados do Cosmos.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Eis o código JavaScript:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

## <a name="trigger---attributes"></a>Acionador - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) atributo.

O construtor do atributo tem o nome de base de dados e o nome da coleção. Para obter informações sobre essas definições e outras propriedades que pode configurar, consulte [acionador - configuração](#trigger---configuration). Eis um `CosmosDBTrigger` exemplo de atributo na assinatura do método:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Para obter um exemplo completado, consulte [acionador - c# exemplo](#trigger---c-example).

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `CosmosDBTrigger` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** || tem de ser definido como `cosmosDBTrigger`. |
|**direção** || tem de ser definido como `in`. Este parâmetro é definido automaticamente quando criar o acionador no portal do Azure. |
|**name** || O nome da variável utilizado no código de função que representa a lista de documentos com as alterações. | 
|**connectionStringSetting**|**ConnectionStringSetting** | O nome de uma definição de aplicação que contém a cadeia de ligação utilizada para ligar à conta de base de dados do Azure Cosmos a ser monitorizada. |
|**databaseName**|**DatabaseName**  | O nome da base de dados do Azure Cosmos DB com a coleção que está a ser monitorizada. |
|**collectionName** |**CollectionName** | O nome da coleção que está a ser monitorizado. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Opcional) O nome de uma definição de aplicação que contenha a cadeia de ligação ao serviço que contém a coleção de concessão. Quando não definido, o `connectionStringSetting` valor é utilizado. Este parâmetro é automaticamente definido quando o enlace é criado no portal. A cadeia de ligação para a coleção de concessões tem de ter permissões de escrita.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Opcional) O nome da base de dados que contém a coleção utilizada para armazenar concessões. Quando não definido, o valor da `databaseName` definição é utilizada. Este parâmetro é automaticamente definido quando o enlace é criado no portal. |
|**leaseCollectionName** | **LeaseCollectionName** | (Opcional) O nome da coleção utilizado para armazenar concessões. Quando não definido, o valor `leases` é utilizado. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Opcional) Quando definido como `true`, a coleção de concessões é criada automaticamente quando já não existir. O valor predefinido é `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (Opcional) Define a quantidade de unidades de pedido para atribuir quando é criada a coleção de concessões. Esta definição é apenas utilizado quando `createLeaseCollectionIfNotExists` está definido como `true`. Este parâmetro é automaticamente definido quando o enlace é criado utilizando o portal.
|**LeaseCollectionPrefix**| **LeaseCollectionPrefix**| (Opcional) Se estiver definida, adiciona um prefixo concessões criadas na coleção de concessão para esta função, efetivamente, permitindo duas funções de Azure separado partilhar a mesma coleção de concessão utilizando prefixos diferentes.
|**FeedPollDelay**| **FeedPollDelay**| (Opcional) Quando o conjunto, define, em milissegundos, o atraso entre uma partição para novas alterações do feed de consulta atuais depois de todas as alterações são drained. Predefinição é 5000 (5 segundos).
|**LeaseAcquireInterval**| **LeaseAcquireInterval**| (Opcional) Se estiver definida, define, em milissegundos, o intervalo para iniciar uma tarefa para calcular se as partições são distribuídas uniformemente entre instâncias de anfitrião conhecido. Predefinição é 13000 (13 segundos).
|**LeaseExpirationInterval**| **LeaseExpirationInterval**| (Opcional) Se estiver definida, define, em milissegundos, o intervalo para o qual o período de concessão é criado numa concessão que representa uma partição. Se a concessão for renovada não dentro deste intervalo, fará com que expire e propriedade da partição irá mudar para outra instância. Predefinição é 60000 (60 segundos).
|**LeaseRenewInterval**| **LeaseRenewInterval**| (Opcional) Se estiver definida, define, em milissegundos, o intervalo de renovação para todas as concessões para partições atualmente retido por uma instância. Predefinição é 17000 (17 segundos).
|**CheckpointFrequency**| **CheckpointFrequency**| (Opcional) Se estiver definida, define, em milissegundos, o intervalo entre pontos de verificação de concessão. Predefinição é sempre após uma chamada de função com êxito.
|**MaxItemsPerInvocation**| **MaxItemsPerInvocation**| (Opcional) Se estiver definida, customizes a quantidade máxima de itens recebidos por chamadas de função.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Acionador - utilização

O acionador requer uma segunda coleção, que utiliza para armazenar _concessões_ através de partições. A coleção que está a ser monitorizado e a coleção que contém os concessões tem de estar disponíveis para acionar a funcionar.

>[!IMPORTANT]
> Se várias funções estão configuradas para utilizar um acionador de base de dados do Cosmos para a mesma coleção, cada uma das funções deve utilizar uma coleção de concessão dedicado ou especifique outro `LeaseCollectionPrefix` para cada função. Caso contrário, será acionada apenas uma das funções. Para obter informações sobre o prefixo, consulte o [secção de configuração](#trigger---configuration).

O acionador não indica se um documento foi atualizado ou inserir, fornece apenas o documento de si próprio. Se precisar de lidar com as atualizações e inserções de forma diferente, pode fazê-lo implementando timestamp campos para inserção ou atualização.

## <a name="input"></a>Input

O enlace de entrada de base de dados do Azure Cosmos obtém um ou mais documentos de base de dados do Azure Cosmos e transmite-os para o parâmetro de entrada da função. Os parâmetros de consulta ou ID do documento podem ser determinados com base no acionador que invoca a função. 

>[!NOTE]
> Não utilize a base de dados do Azure Cosmos entrada ou saída enlaces se estiver a utilizar a API do MongoDB numa conta de base de dados do Cosmos. É possível danos nos dados.

## <a name="input---examples"></a>Entrada - exemplos

Consulte os exemplos de específicas do idioma, especificando um valor de ID de leitura de um único documento:

* [C#](#input---c-examples)
* [Script do c# (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Exemplos de entrada ignorados](#input---attributes)

### <a name="input---c-examples"></a>Entrada - c# exemplos

Esta secção contém os exemplos seguintes:

* [Acionador da fila, procure um ID de JSON](#queue-trigger-look-up-id-from-json-c)
* [Acionador HTTP, procure um ID de cadeia de consulta](#http-trigger-look-up-id-from-query-string-c)
* [Acionador HTTP, procure um ID de dados da rota](#http-trigger-look-up-id-from-route-data-c)
* [Acionador HTTP, procure um ID de dados de rota, utilizando SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP acionar, obter documentos múltiplos, utilizando SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP acionar, obter documentos múltiplos, utilizando o DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c)

Os exemplos de fazer referência a uma simples `ToDoItem` tipo:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Acionador da fila, procure um ID de JSON (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que obtém um único documento. A função é acionada por uma mensagem de fila que contém um objeto JSON. O acionador de fila analisa o JSON para um objeto com o nome `ToDoItemLookup`, que contém o ID para procurar. Se o ID é utilizado para obter um `ToDoItem` documento da coleção e base de dados especificada.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>Acionador HTTP, procure um ID de cadeia de consulta (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que obtém um único documento. A função é acionada por um pedido HTTP que utiliza uma cadeia de consulta para especificar o ID para procurar. Se o ID é utilizado para obter um `ToDoItem` documento da coleção e base de dados especificada.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>Acionador HTTP, procure um ID de dados da rota (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que obtém um único documento. A função é acionada por um pedido HTTP que utiliza encaminhar dados para especificar o ID para procurar. Se o ID é utilizado para obter um `ToDoItem` documento da coleção e base de dados especificada.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>Acionador HTTP, procure um ID de dados de rota, utilizando SqlQuery (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que obtém um único documento. A função é acionada por um pedido HTTP que utiliza encaminhar dados para especificar o ID para procurar. Se o ID é utilizado para obter um `ToDoItem` documento da coleção e base de dados especificada. 

O exemplo mostra como utilizar uma expressão de enlace no `SqlQuery` parâmetro. Pode passar dados da rota para o `SqlQuery` parâmetro conforme mostrado, mas atualmente [não é possível transmitir valores de cadeia de consulta](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items", 
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP acionar, obter documentos múltiplos, utilizando SqlQuery (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que obtém uma lista de documentos. A função é acionada por um pedido HTTP. A consulta é especificada no `SqlQuery` propriedade de atributo.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP acionar, obter documentos múltiplos, utilizando o DocumentClient (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que obtém uma lista de documentos. A função é acionada por um pedido HTTP. O código utiliza um `DocumentClient` instância fornecida pelo enlace BD do Cosmos do Azure para ler uma lista de documentos. O `DocumentClient` instância também pode ser utilizada para operações de escrita.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.Info($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[Exemplos de entrada ignorados](#input---attributes)

### <a name="input---c-script-examples"></a>Entrada - exemplos de script do c#

Esta secção contém os seguintes exemplos de leitura de um único documento, especificando um valor de ID de várias origens:

* Acionador da fila, procure um ID da mensagem da fila
* Acionador da fila, procure um ID de mensagem de fila, utilizando SqlQuery

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-c-script"></a>Acionador da fila, procure um ID de mensagem de fila (script do c#)

O exemplo seguinte mostra um enlace de entrada do Cosmos DB num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função lê um único documento e atualiza o valor de texto do documento.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
O [configuração](#input---configuration) secção explica estas propriedades.

Eis o código de script do c#:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding 
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="queue-trigger-look-up-id-queue-message-using-sqlquery-c-script"></a>Acionador da fila, procura de ID de mensagem de fila, utilizando SqlQuery (script do c#)

O exemplo seguinte mostra um enlace de entrada da base de dados do Azure Cosmos num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função obtém vários documentos especificados por uma consulta de SQL Server, com um acionador de fila para personalizar os parâmetros de consulta.

O acionador de filas fornece um parâmetro `departmentId`. Uma mensagem de fila de `{ "departmentId" : "Finance" }` iria devolver todos os registos para o departamento financeiro. 

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

O [configuração](#input---configuration) secção explica estas propriedades.

Eis o código de script do c#:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {   
        foreach (var doc in documents)
        {
            // operate on each document
        }    
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Exemplos de entrada ignorados](#input---attributes)

### <a name="input---javascript-examples"></a>Entrada - exemplos de JavaScript

Esta secção contém os seguintes exemplos de leitura de um único documento, especificando um valor de ID de várias origens:

* Acionador da fila, procure um ID da mensagem da fila
* Acionador da fila, procure um ID de mensagem de fila, utilizando SqlQuery

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-javascript"></a>Acionador da fila, procure um ID de mensagem de fila (JavaScript)

O exemplo seguinte mostra um enlace de entrada do Cosmos DB num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função lê um único documento e atualiza o valor de texto do documento.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
O [configuração](#input---configuration) secção explica estas propriedades.

Eis o código JavaScript:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Exemplos de entrada ignorados](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-using-sqlquery-javascript"></a>Acionador da fila, procure um ID de mensagem de fila, utilizando SqlQuery (JavaScript)

O exemplo seguinte mostra um enlace de entrada da base de dados do Azure Cosmos num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função obtém vários documentos especificados por uma consulta de SQL Server, com um acionador de fila para personalizar os parâmetros de consulta.

O acionador de filas fornece um parâmetro `departmentId`. Uma mensagem de fila de `{ "departmentId" : "Finance" }` iria devolver todos os registos para o departamento financeiro. 

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

O [configuração](#input---configuration) secção explica estas propriedades.

Eis o código JavaScript:

```javascript
    module.exports = function (context, input) {    
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }       
        context.done();
    };
```

[Exemplos de entrada ignorados](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Entrada - F # exemplos

O exemplo seguinte mostra um enlace de entrada do Cosmos DB num *function.json* ficheiro e uma [F # função](functions-reference-fsharp.md) que utiliza o enlace. A função lê um único documento e atualiza o valor de texto do documento.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```

O [configuração](#input---configuration) secção explica estas propriedades.

Eis o código F #:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

Este exemplo requer um `project.json` ficheiro que especifica o `FSharp.Interop.Dynamic` e `Dynamitey` NuGet dependências:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Para adicionar um `project.json` de ficheiros, consulte [gestão de pacotes F #](functions-reference-fsharp.md#package).

## <a name="input---attributes"></a>Entrada - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) atributo.

O construtor do atributo tem o nome de base de dados e o nome da coleção. Para obter informações sobre essas definições e outras propriedades que pode configurar, consulte [a secção de configuração seguintes](#input---configuration). 

## <a name="input---configuration"></a>Entrada - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `CosmosDB` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo**     || tem de ser definido como `cosmosDB`.        |
|**direção**     || tem de ser definido como `in`.         |
|**name**     || Nome do parâmetro de enlace que representa o documento na função.  |
|**databaseName** |**DatabaseName** |A base de dados que contém o documento.        |
|**collectionName** |**CollectionName** | O nome da coleção que contém o documento. |
|**id**    | **Id** | O ID do documento para obter. Esta propriedade suporta [expressões de enlace](functions-triggers-bindings.md#binding-expressions-and-patterns). Não definir ambos os **id** e **sqlQuery** propriedades. Se não definir uma, é obtida a coleção completa. |
|**sqlQuery**  |**SqlQuery**  | Uma consulta de base de dados SQL do Azure Cosmos utilizada para obter vários documentos. A propriedade suporta os enlaces de tempo de execução, tal como neste exemplo: `SELECT * FROM c where c.departmentId = {departmentId}`. Não definir ambos os **id** e **sqlQuery** propriedades. Se não definir uma, é obtida a coleção completa.|
|**connectionStringSetting**     |**ConnectionStringSetting**|O nome da definição de aplicação que contém a cadeia de ligação de base de dados do Azure Cosmos.        |
|**partitionKey**|**PartitionKey**|Especifica o valor de chave de partição para a pesquisa. Pode incluir parâmetros de enlace.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Entrada - utilização

Em c# e F # funções, quando a função sai com sucesso, quaisquer alterações efetuadas para o documento de entrada através dos parâmetros de entrada com nome são mantidas automaticamente. 

Nas funções de JavaScript, as atualizações não são efetuadas automaticamente após a saída da função. Em alternativa, utilize `context.bindings.<documentName>In` e `context.bindings.<documentName>Out` para disponibilizar atualizações. Consulte o [JavaScript exemplo](#input---javascript-example).

## <a name="output"></a>Saída

A saída de base de dados do Azure Cosmos enlace permite escrever um novo documento para uma base de dados do Azure Cosmos DB. 

>[!NOTE]
> Não utilize a base de dados do Azure Cosmos entrada ou saída enlaces se estiver a utilizar a API do MongoDB numa conta de base de dados do Cosmos. É possível danos nos dados.

## <a name="output---example"></a>De saída - exemplo

Veja o exemplo de específicas do idioma:

* [C#](#output---c-examples)
* [Script do c# (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

Consulte também o [entrado de exemplo](#input---c-examples) que utiliza `DocumentClient`.

[Ignorar os exemplos de saída](#output---attributes)

### <a name="ouput---c-examples"></a>Saída - c# exemplos

Esta secção contém os exemplos seguintes:

* Acionador da fila, uma documento escrita
* Acionador da fila, docs escrita utilizando IAsyncCollector

Os exemplos de fazer referência a uma simples `ToDoItem` tipo:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Ignorar os exemplos de saída](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Acionador da fila, escrita um documento do (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que adiciona um documento a uma base de dados, com dados fornecidos na mensagem a partir do armazenamento de filas.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[Ignorar os exemplos de saída](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Acionador da fila, docs escrita utilizando IAsyncCollector (c#)

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que adiciona uma coleção de documentos para uma base de dados, com dados fornecidos numa mensagem de fila JSON.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Ignorar os exemplos de saída](#output---attributes)

### <a name="output---c-script-examples"></a>Saída - exemplos de script do c#

O exemplo seguinte mostra uma saída de base de dados do Azure Cosmos enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função utiliza um enlace de fila de entrada para uma fila que recebe JSON no seguinte formato:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

A função cria documentos de base de dados do Azure Cosmos no seguinte formato para cada registo:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

O [configuração](#output---configuration) secção explica estas propriedades.

Eis o código de script do c#:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
      log.Info($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

Para criar vários documentos, é possível vincular ao `ICollector<T>` ou `IAsyncCollector<T>` onde `T` é um dos tipos suportados.

[Ignorar os exemplos de saída](#output---attributes)

### <a name="output---javascript-examples"></a>Saída - exemplos de JavaScript

O exemplo seguinte mostra uma saída de base de dados do Azure Cosmos enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função utiliza um enlace de fila de entrada para uma fila que recebe JSON no seguinte formato:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

A função cria documentos de base de dados do Azure Cosmos no seguinte formato para cada registo:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

O [configuração](#output---configuration) secção explica estas propriedades.

Eis o código JavaScript:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({ 
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Ignorar os exemplos de saída](#output---attributes)

### <a name="output---f-examples"></a>Saída - F # exemplos

O exemplo seguinte mostra uma saída de base de dados do Azure Cosmos enlace num *function.json* ficheiro e uma [F # função](functions-reference-fsharp.md) que utiliza o enlace. A função utiliza um enlace de fila de entrada para uma fila que recebe JSON no seguinte formato:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

A função cria documentos de base de dados do Azure Cosmos no seguinte formato para cada registo:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```
O [configuração](#output---configuration) secção explica estas propriedades.

Eis o código F #:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
      log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

Este exemplo requer um `project.json` ficheiro que especifica o `FSharp.Interop.Dynamic` e `Dynamitey` NuGet dependências:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Para adicionar um `project.json` de ficheiros, consulte [gestão de pacotes F #](functions-reference-fsharp.md#package).

## <a name="output---attributes"></a>Saída - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) atributo.

O construtor do atributo tem o nome de base de dados e o nome da coleção. Para obter informações sobre essas definições e outras propriedades que pode configurar, consulte [de saída - configuração](#output---configuration). Eis um `CosmosDB` exemplo de atributo na assinatura do método:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Para obter um exemplo completado, consulte [resultado - c# exemplo](#output---c-example).

## <a name="output---configuration"></a>De saída - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `CosmosDB` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo**     || tem de ser definido como `cosmosDB`.        |
|**direção**     || tem de ser definido como `out`.         |
|**name**     || Nome do parâmetro de enlace que representa o documento na função.  |
|**databaseName** | **DatabaseName**|A base de dados que contém a coleção onde o documento é criado.     |
|**collectionName** |**CollectionName**  | O nome da coleção em que o documento é criado. |
|**createIfNotExists**  |**CreateIfNotExists**    | Um valor booleano para indicar se a coleção é criada quando não existe. A predefinição é *falso* porque são criadas novas coleções com débito reservado, que tem custos implicações. Para obter mais informações, veja a [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |Quando `CreateIfNotExists` for VERDADEIRO, define o caminho de chave de partição para a coleção criada.|
|**collectionThroughput**|**CollectionThroughput**| Quando `CreateIfNotExists` for VERDADEIRO, define o [débito](../cosmos-db/set-throughput.md) da coleção criada.|
|**connectionStringSetting**    |**ConnectionStringSetting** |O nome da definição de aplicação que contém a cadeia de ligação de base de dados do Azure Cosmos.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - utilização

Por predefinição, quando escreve para o parâmetro de saída na sua função, um documento é criado na base de dados. Este documento tem um GUID automaticamente gerado como o ID do documento. Pode especificar o ID do documento do documento de saída, especificando o `id` propriedade no objeto JSON transmitido para o parâmetro de saída. 

> [!Note]  
> Quando especificar o ID de um documento existente, obtém substituído pelo novo documento de saída. 

## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Vínculo | Referência |
|---|---|
| CosmosDB | [Códigos de erro CosmosDB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ir para um guia de introdução que utiliza um acionador de base de dados do Cosmos](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre a base de dados sem servidor informática com Cosmos DB](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
