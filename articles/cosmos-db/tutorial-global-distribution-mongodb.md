---
title: Tutorial de distribuição global no Azure Cosmos DB para a MongoDB API
description: Saiba como configurar a distribuição global do Azure Cosmos DB com o Azure Cosmos DB para API do MongoDB.
services: cosmos-db
keywords: distribuição global, MongoDB
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 2473d6f16c4464352c2b9def3e451f8dcf1cdf63
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53717003"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-azure-cosmos-db-for-mongodb-api"></a>Configurar a distribuição global do Azure Cosmos DB com o Azure Cosmos DB para API do MongoDB

Neste artigo, vamos mostrar como utilizar o portal do Azure para configurar a distribuição global do Azure Cosmos DB e, em seguida, ligar com o Azure Cosmos DB para a MongoDB API.

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Configurar a distribuição global com o portal do Azure
> * Configurar a distribuição global com o [para a MongoDB API do Azure Cosmos DB](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup"></a>Verificar a configuração regional 
A forma mais simples de verificar a configuração global na API do MongoDB é executar o comando *isMaster()* a partir da Shell de Mongo.

A partir da Shell de Mongo:

   ```
      db.isMaster()
   ```
   
Resultados de exemplo:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region"></a>Ligar a uma região preferencial 

O Azure Cosmos DB para API do MongoDB permite-lhe especificar a preferência de leitura de sua coleção para uma base de dados distribuído globalmente. Para leituras de baixa latência e elevada disponibilidade global, recomendamos a definição da preferência de leitura da coleção como *mais próxima*. Está configurada uma preferência de leitura de *mais próxima* para ler a partir da região mais próxima.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Para aplicações com uma região de leitura/escrita primária e uma região secundária para cenários de recuperação após desastre (DR), recomendamos a definição da preferência de leitura da coleção como *secundária preferida*. Está configurada uma preferência de leitura de *secundária preferida* para ler a partir da região secundária quando a região primária não estiver disponível.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Por último, se quiser especificar manualmente as regiões de leitura. Pode definir a região Etiqueta na sua preferência de leitura.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Já está, isto conclui este tutorial. Pode saber como gerir a consistência da sua conta replicada globalmente ao ler [Níveis de consistência no Azure Cosmos DB](consistency-levels.md). Para obter mais informações sobre como funciona a replicação de base de dados global no Azure Cosmos DB, veja [Distribuir dados globalmente com o Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Configurar a distribuição global com o portal do Azure
> * Configurar a distribuição global com as APIs SQL

Agora, pode avançar para o próximo tutorial para saber como desenvolver localmente com o emulador local do Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Desenvolver localmente com o emulador](local-emulator.md)
