---
title: Aprovisionar débito do contentor no Azure Cosmos DB
description: Aprenda a aprovisionar débito ao nível do contentor no Azure Cosmos DB
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 4c17c972a39cd336ab33f6be8bd3db5b8f0f2c92
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/28/2018
ms.locfileid: "53808422"
---
# <a name="provision-throughput-for-an-azure-cosmos-db-container"></a>Aprovisionar débito para um contentor do Azure Cosmos DB

Este artigo explica como aprovisionar débito para um contentor (coleção, gráfico, tabela) no Azure Cosmos DB. Pode aprovisionar débito para um único contentor ou [aprovisionar uma base de dados](how-to-provision-database-throughput.md) e partilhá-la entre os contentores na base de dados. Pode aprovisionar débito para um contentor com o portal do Azure, a CLI do Azure ou SDKs do CosmosDB.

## <a name="provision-throughput-using-azure-portal"></a>Aprovisionar débito com o portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Nova Coleção**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Crie uma nova base de dados ou utilize uma existente.
   * Introduza um Id de Coleção (ou tabela, grafo).
   * Introduza um valor de chave de partição, por exemplo `/userid`.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![Débito de contentor de aprovisionamento da API SQL](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Aprovisionar débito com a CLI do Azure

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

Se estiver a aprovisionar o débito de uma conta do Cosmos configurado com a API do Azure Cosmos DB para o MongoDB, utilize "/ myShardKey' para o caminho da chave de partição e quando aprovisionar o débito de uma conta do Cosmos configurado para a API de Cassandra, utilize" / myPrimaryKey' para o caminho da chave de partição.

## <a name="provision-throughput-using-net-sdk"></a>Aprovisionar débito com o SDK do .NET

> [!Note]
> Utilize a API de SQL para aprovisionar débito para todas as APIs, exceto para a API para Cassandra.

### <a id="dotnet-most"></a>SQL, MongoDB, Gremlin e APIs de Tabela

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>API para Cassandra

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Passos Seguintes

Veja os seguintes artigos para saber mais sobre o aprovisionamento de débito no Cosmos DB:

* [Como aprovisionar débito para uma base de dados](how-to-provision-database-throughput.md)
* [Unidades de pedido e débito no Azure Cosmos DB](request-units.md)
