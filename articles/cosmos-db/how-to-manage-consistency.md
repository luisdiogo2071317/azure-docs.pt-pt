---
title: Saiba como gerir consistência no Azure Cosmos DB
description: Saiba como gerir consistência no Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: be2c68922221af848c9e484d03527d02808c071a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283823"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Gerir níveis de consistência no Azure Cosmos DB

Este artigo explica as diferentes formas de definir a consistência predefinida, substituir essa consistência no cliente, gerir manualmente tokens de sessões e compreender a métrica de Estagnação Limitada Probabilisticamente (PBS).

## <a name="configure-the-default-consistency-level"></a>Configurar o nível de consistência predefinido

O nível de consistência predefinido é o nível que os clientes de consistência irão utilizar por predefinição. Pode ser substituído pelos clientes.

### <a name="cli"></a>CLI

```bash
# create with a default consistency
az cosmosdb create --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level Strong

# update an existing account's default consistency
az cosmosdb update --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level BoundedStaleness
```

### <a name="powershell"></a>PowerShell

O exemplo a seguir cria uma nova conta do Cosmos DB com multimestre ativado nas regiões E.U.A. Leste e E.U.A. Oeste, ao definir a política de consistência predefinida como Estagnação Limitada, com um intervalo de estagnação máxima de 10 segundos e o número máximo de pedidos de estagnação até 200.

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = ""

$consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

### <a name="portal"></a>Portal

Para ver ou modificar o nível predefinido de consistência, inicie sessão no portal do Azure. Encontre a sua Conta do Cosmos DB e abra o painel **Consistência predefinida**. A partir daí, escolha o nível de consistência que gostaria de ter predefinido e, em seguida, clique em Guardar.

![Imagem do menu de consistência no portal do Azure](./media/how-to-manage-consistency/consistency-settings.png)

## <a name="override-the-default-consistency-level"></a>Substituir o nível de consistência predefinido

Os clientes podem substituir o nível de consistência predefinido que está definido pelo serviço. Isto pode ser feito para o cliente todo ou por pedido.

### <a id="override-default-consistency-dotnet"></a>SDK do .NET

```csharp
// Override consistency at the client level
ConsistencyPolicy consistencyPolicy = new ConsistencyPolicy
    {
        DefaultConsistencyLevel = ConsistencyLevel.BoundedStaleness,
        MaxStalenessIntervalInSeconds = 5,
        MaxStalenessPrefix = 100
    };
documentClient = new DocumentClient(new Uri(endpoint), authKey, connectionPolicy, consistencyPolicy);

// Override consistency at the request level via request options
RequestOptions requestOptions = new RequestOptions { ConsistencyLevel = ConsistencyLevel.Strong };

var response = await client.CreateDocumentAsync(collectionUri, document, requestOptions);
```

### <a id="override-default-consistency-java-async"></a>SDK do Java Async

```java
// Override consistency at the client level
ConnectionPolicy policy = new ConnectionPolicy();

AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConsistencyLevel(ConsistencyLevel.Eventual)
                .withConnectionPolicy(policy).build();
```

### <a id="override-default-consistency-java-sync"></a>SDK do Java Sync

```java
// Override consistency at the client level
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy, ConsistencyLevel.Strong);
```

### <a id="override-default-consistency-javascript"></a>SDK do Node.js/JavaScript/TypeScript

```javascript
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Strong
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

### <a id="override-default-consistency-python"></a>SDK do Python

```python
# Override consistency at the client level
connection_policy = documents.ConnectionPolicy()
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Strong)
```

## <a name="utilize-session-tokens"></a>Utilizar tokens de sessões

Se quiser gerir manualmente os tokens de sessão, pode obtê-los das respostas e configurá-los por pedido. Se não tiver uma necessidade de gerir manualmente os tokens de sessão, não precisa de utilizar os exemplos a seguir. O SDK irá controlar automaticamente os tokens de sessão e utilizar o token de sessão mais recente, se não definir o token de sessão por conta própria.

### <a id="utilize-session-tokens-dotnet"></a>SDK do .NET

```csharp
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"));
string sessionToken = response.SessionToken;

RequestOptions options = new RequestOptions();
options.SessionToken = sessionToken;
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"), options);
```

### <a id="utilize-session-tokens-java-async"></a>SDK do Java Async

```java
// Get session token from response
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey(document.get("mypk")));
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
readObservable.single()           // we know there will be one response
  .subscribe(
      documentResourceResponse -> {
          System.out.println(documentResourceResponse.getSessionToken());
      },
      error -> {
          System.err.println("an error happened: " + error.getMessage());
      });

// Resume the session by setting the session token on RequestOptions
RequestOptions options = new RequestOptions();
requestOptions.setSessionToken(sessionToken);
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
```

### <a id="utilize-session-tokens-java-sync"></a>SDK do Java Sync

```java
// Get session token from response
ResourceResponse<Document> response = client.readDocument(documentLink, null);
String sessionToken = response.getSessionToken();

// Resume the session by setting the session token on the RequestOptions
RequestOptions options = new RequestOptions();
options.setSessionToken(sessionToken);
ResourceResponse<Document> response = client.readDocument(documentLink, options);
```

### <a id="utilize-session-tokens-javascript"></a>SDK do Node.js/JavaScript/TypeScript

```javascript
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

### <a id="utilize-session-tokens-python"></a>SDK do Python

```python
// Get the session token from the last response headers
item = client.ReadItem(item_link)
session_token = client.last_response_headers["x-ms-session-token"]

// Resume the session by setting the session token on the options for the request
options = {
    "sessionToken": session_token
}
item = client.ReadItem(doc_link, options)
```

## <a name="monitor-probabilistically-bounded-staleness-pbs-metric"></a>Monitorizar a métrica de Estagnação Limitada Probabilisticamente (PBS)

Para ver a métrica de PBS, aceda à sua Conta do Cosmos DB no portal do Azure e, em seguida, abra o painel **Métricas**. A partir daí, clique no separador **Consistência** e observe o gráfico com o nome "**Probabilidade de leituras fortemente consistentes com base na carga de trabalho (veja PBS)**".

![Imagem do gráfico PBS no portal do Azure](./media/how-to-manage-consistency/pbs-metric.png)

Utilize o menu de métricas do Cosmos DB para ver esta métrica. Ele não irá aparecer na experiência de métricas de monitorização do Azure.

## <a name="next-steps"></a>Passos seguintes

Pode saber mais sobre a gestão de conflitos de dados ou mover para o próximo conceito-chave no Cosmos DB com os documentos seguintes:

* [Como gerir conflitos entre regiões](how-to-manage-conflicts.md)
* [Criação de partições e distribuição de dados](partition-data.md)
