---
title: Criar um contentor no Azure Cosmos DB
description: Aprenda a criar um contentor no Azure Cosmos DB
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 5558409c3a3b0aef3757ebb73b2046a7018e4150
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088192"
---
# <a name="create-a-container-in-azure-cosmos-db"></a>Criar um contentor no Azure Cosmos DB

Este artigo explica as diferentes formas de criar um contentor (coleção, tabela, gráfico). Um contentor pode ser criado com o portal do Azure, a CLI do Azure ou dos SDKs suportados. Este artigo demonstra como criar um contentor, especificar a chave de partição e o débito de aprovisionamento.

## <a name="create-a-container-using-azure-portal"></a>Criar um contentor com o portal do Azure

### <a id="portal-sql"></a>API de SQL (Core)

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Nova Coleção**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Crie uma nova base de dados ou utilize uma existente.
   * Introduzir o ID da Coleção.
   * Introduza a chave de Partição.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![A API de SQL cria uma coleção](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>API do MongoDB

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-mongodb-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Nova Coleção**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Crie uma nova base de dados ou utilize uma existente.
   * Introduzir o ID da Coleção.
   * Selecione a capacidade de armazenamento **Ilimitada**.
   * Introduza uma chave Shard.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![A API de MongoDB cria uma coleção](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>API para Cassandra

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-cassandra-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Nova Tabela**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Crie um novo Espaço de chaves ou utilize um existente.
   * Introduza um nome de tabela.
   * Introduza as propriedades e especifique uma CHAVE PRIMÁRIA.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![A API para Cassandra cria uma coleção](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Para a API para Cassandra, a chave primária é utilizada como a chave de partição.

### <a id="portal-gremlin"></a>API do Gremlin

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-graph-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Novo Grafo**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Crie uma nova base de dados ou utilize uma existente.
   * Introduza um ID de Grafo.
   * Selecione a capacidade de armazenamento **Ilimitada**.
   * Introduza uma chave de Partição para vértices.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![A API de Gremlin cria uma coleção](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>API de Tabela

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-table-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Nova Tabela**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Introduza um ID de Tabela.
   * Selecione a capacidade de armazenamento **Ilimitada**.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![A API de Tabela cria uma coleção](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Para a API de Tabela, a chave de partição é especificada sempre que adicionar uma nova linha.

## <a name="create-a-container-using-azure-cli"></a>Criar um contentor com a CLI do Azure

### <a id="cli-sql"></a>API de SQL (Core)

```azurecli-interactive
# Create a container with a partition key and provision 1000 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-mongodb"></a>API do MongoDB

```azurecli-interactive
# Create a collection with a shard key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 1000
```

### <a id="cli-cassandra"></a>API para Cassandra

```azurecli-interactive
# Create a table with a partition/primary key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $keyspaceName \
    --partition-key-path /myPrimaryKey \
    --throughput 1000
```

### <a id="cli-gremlin"></a>API do Gremlin

```azurecli-interactive
# Create a graph with a partition key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-table"></a>API de Tabela

```azurecli-interactive
# Create a table with 1000 RU/s
# Note: you don't need to specify partition key in the following command because the partition key is set on each row.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $databaseName \
    --throughput 1000
```

## <a name="create-a-container-using-net-sdk"></a>Criar um contentor com o SDK .NET

### <a id="dotnet-sql-graph"></a>API de SQL e API do Gremlin

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>API do MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> O MongoDB não tem um conceito de unidades de pedido. Para criar uma nova coleção com débito, utilize o Portal do Azure ou a API de SQL, conforme mostrado nos exemplos anteriores.

### <a id="dotnet-cassandra"></a>API para Cassandra

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Passos Seguintes

Veja os seguintes artigos para saber mais sobre a criação de partições no Cosmos DB:

- [Criação de partições no Azure Cosmos DB](partitioning-overview.md)
