---
title: 'Do Azure Cosmos DB: Exemplos de Java para a API do SQL Server | Microsoft Docs'
description: "Localize exemplos de Java no GitHub para tarefas comuns com a API do Azure Cosmos BD do SQL Server, incluindo as operações CRUD."
keywords: Exemplo de NoSQL
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: java
ms.assetid: d824d517-903e-4d82-ab0a-09fc3b984c84
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 02/08/2018
ms.author: mimig
ms.openlocfilehash: a46b6e4d64b44c07fe7a2aa63bc49bea59ad5b53
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2018
---
# <a name="azure-cosmos-db-java-examples-for-the-sql-api"></a>Do Azure Cosmos DB: Exemplos de Java para a API do SQL Server

> [!div class="op_single_selector"]
> * [Exemplos de .NET](sql-api-dotnet-samples.md)
> * [Exemplos de Java](sql-api-java-samples.md)
> * [Exemplos de node.js](sql-api-nodejs-samples.md)
> * [Exemplos de Python](sql-api-python-samples.md)
> * [Galeria de exemplo de código do Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

As aplicações de exemplo mais recentes que as operações CRUD perform outras operações comuns na base de dados do Azure Cosmos recursos estão incluídas no [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java) repositório do GitHub. Este artigo fornece:

* Ficheiros de projeto de ligações para as tarefas em cada um do Java de exemplo. 
* Ligações para a API relacionada fazer referência a conteúdo.

**Pré-requisitos**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- Pode [ativar os benefícios de subscritor do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): O Visual Studio subscrição dá-lhe créditos todos os meses que pode utilizar para serviços pagos do Azure.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

É necessário o seguinte para executar esta aplicação de exemplo:

* Kit de desenvolvimento Java 7
* SDK de DocumentDB Java do Microsoft Azure

Opcionalmente, pode utilizar Maven para obter os binários de Microsoft Azure DocumentDB Java SDK mais recentes para utilização no seu projeto. Maven adiciona automaticamente quaisquer dependências necessárias. Caso contrário, diretamente pode transferir as dependências listadas no ficheiro pom.xml e adicioná-los para o caminho de compilação.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>LATEST</version>
</dependency>
```

**Executar as aplicações de exemplo**

Clone o repositório de exemplo:
```bash
$ git clone https://github.com/Azure/azure-documentdb-java.git

$ cd azure-documentdb-java
```

Pode executar os exemplos a utilizar Eclipse ou na linha de comandos com o Maven.

Para executar a partir do Eclipse:
* Carregar o ficheiro de pom.xml projeto principal principal no Eclipse; -la automaticamente deve carregar exemplos do documentdb.
* Para executar os exemplos, precisa de um ponto de final de base de dados de Cosmos de Azure válido. Os pontos finais são lidos a partir `src/test/java/com/microsoft/azure/documentdb/examples/AccountCredentials.java`.
* Pode transmitir as suas credenciais de ponto final como argumentos de VM na configuração de execução do Eclipse JUnit ou pode colocar as suas credenciais de ponto final no AccountCredentials.java.
    ```bash
    -DACCOUNT_HOST="https://REPLACE_THIS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS"
    ```
* Agora pode executar os exemplos como JUnit testes no Eclipse.

Para executar a linha de comandos:
* A forma como para executar exemplos consiste em utilizar maven:
* Executar Maven e transmita as suas credenciais de ponto final de BD do Cosmos do Azure:
    ```bash
    mvn test -DACCOUNT_HOST="https://REPLACE_THIS_WITH_YOURS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS_WITH_YOURS"
    ```

   > [!NOTE]
   > Cada amostra é autónomo; Isto configura-se e limpa após a próprio. Os exemplos emitir várias chamadas para [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection). Cada vez que isto é feito, a sua subscrição é faturada para 1 hora de utilização para a camada de desempenho da coleção criada. 
   > 
   > 

## <a name="database-examples"></a>Exemplos de base de dados
O [DatabaseCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar e ler uma base de dados](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L64-L79) | [DocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdatabase)<br>[DocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdatabase)<br>[Resource.setId](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._resource.setid) |
| [Criar e eliminar uma base de dados](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L82-L93) | [DocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletedatabase) |
| [Criar e consultar uma base de dados](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L96-L111) | [DocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydatabases) |

## <a name="collection-examples"></a>Exemplos de coleção
O [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar uma coleção de partições únicas](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L74-L84) | [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection) |
| [Criar uma coleção de partição multi personalizada](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L103-L155) | [DocumentCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_collection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._partition_key_definition)<br>[RequestOptions](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._request_options) |
| [Elimina uma coleção](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L97-L99) | [DocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletecollection) |

## <a name="document-examples"></a>Exemplos de documento
O [DocumentCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar, ler e eliminar um documento](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L84-L122) | [DocumentClient.createDocument](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createdocument)<br>[DocumentClient.readDocument](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.readdocument)<br>[DocumentClient.deleteDocument](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) |
| [Criar um documento com uma definição de documento programável](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L126-L147) | [Document](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document)<br>[Resource.setId](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._resource.setid) |

## <a name="indexing-examples"></a>Exemplos de indexação
O [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar um índice e o conjunto de política de indexação](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L125-L141) | [Índice](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._index)<br>[IndexingPolicy](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._indexing_policy) |

Para obter mais informações sobre a indexação, consulte [Azure Cosmos DB indexação políticas](indexing-policies.md).

## <a name="query-examples"></a>Exemplos de consulta
O [DocumentQuerySamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Executar uma consulta simples de partição cruzada de documento](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L108-L129) | [DocumentClient.queryDocuments](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.querydocuments)<br>[FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._feed_options.setenablecrosspartitionquery) |
| [Ordenar por consulta](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L132-L154) | [FeedResponse<T>.getQueryIterator](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._feed_response.getqueryiterator) |

Para obter mais informações sobre como escrever consultas, consulte [consulta SQL na base de dados do Azure Cosmos](sql-api-sql-query.md).

## <a name="offer-examples"></a>Exemplos de oferta
O [OfferCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar uma coleção e defina o débito](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L76-L102) | [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection)<br>[RequestOptions.setOfferThroughput](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._request_options.setofferthroughput) |
| [Ler uma coleção para localizar a oferta associada](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L108-L132) | [Offer.getContent](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._offer.getcontent)<br>[DocumentClient.replaceOffer](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.replaceoffer)<br>[DocumentClient.readCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.readcollection)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.queryoffers) |

## <a name="partition-key-examples"></a>Exemplos de chaves de partição
O [SinglePartitionCollectionDocumentCrudSample](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar uma coleção de partições únicas](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L164-L207) | [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection) |
| [Alterar a oferta de débito para uma coleção de partições únicas](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L209-L223) | [DocumentClient.replaceOffer](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.replaceoffer) |

## <a name="stored-procedure-examples"></a>Exemplos de procedimento armazenado
O [StoredProcedureSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java) ficheiro mostra como realizar as seguintes tarefas:

| Tarefa | Referência da API |
| --- | --- |
| [Criar um procedimento armazenado](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L85-L118) | [StoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._stored_procedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createstoredprocedure) |
| [Executar um procedimento armazenado com argumentos](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L121-L144) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.executestoredprocedure) |
| [Executar um procedimento armazenado com um argumento de objeto](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L147-L177) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.executestoredprocedure) |
