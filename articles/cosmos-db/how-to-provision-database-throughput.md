---
title: Aprovisionar débito da base de dados no Azure Cosmos DB
description: Aprenda a aprovisionar o débito ao nível da base de dados no Azure Cosmos DB
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 4eba0381eb302ca4400fb5669b486fb3ad337993
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262527"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Aprovisione o débito para a base de dados no Azure Cosmos DB

Este artigo explica como aprovisionar débito para uma base de dados no Azure Cosmos DB. Pode aprovisionar débito para um único [contentor](how-to-provision-container-throughput.md) ou para uma base de dados e partilhá-la entre os contentores na mesma. Pode aprovisionar o débito de nível de base de dados com o portal do Azure ou SDKs do Cosmos DB.

## <a name="provision-throughput-using-azure-portal"></a>Aprovisionar débito com o portal do Azure

### <a id="portal-sql"></a>API de SQL (Core)

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. [Crie uma nova conta do Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) ou selecione uma conta existente.

1. Abra o painel **Data Explorer** e selecione **Nova Base de Dados**. Em seguida, preencha o formulário com os seguintes detalhes:

   * Introduzir o ID da Base de Dados. 
   * Selecione Débito de Aprovisionamento.
   * Introduza um débito, por exemplo, 1000 RUs.
   * Selecione **OK**.

![Aprovisionar débito de base de dados da API de SQL](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>Aprovisionar débito com o SDK do .NET

> [!Note]
> Utilize a API de SQL para aprovisionar débito para todas as APIs. Também pode, opcionalmente, utilizar o exemplo abaixo para a API para Cassandra.

### <a id="dotnet-all"></a>Todas as APIs

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>API para Cassandra

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Passos seguintes

Veja os seguintes artigos para saber mais sobre o aprovisionamento de débito no Cosmos DB:

* [Como aprovisionar o débito para um contentor](how-to-provision-container-throughput.md)
* [Unidades de pedido e débito no Azure Cosmos DB](request-units.md)
