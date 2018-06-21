---
title: Tutorial de distribuição global do Azure Cosmos DB para a Graph API | Microsoft Docs
description: Saiba como configurar a distribuição global do Azure Cosmos DB com a Graph API.
services: cosmos-db
keywords: distribuição global, graph, gremlin
author: luisbosquez
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 61004a735f731cfd1303cf40b6e60cba496e942a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763685"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Configurar a distribuição global do Azure Cosmos DB com a Graph API

Neste artigo, vamos mostrar como utilizar o portal do Azure para configurar a distribuição global do Azure Cosmos DB e, em seguida, ligar através da Graph API.

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Configurar a distribuição global com o portal do Azure
> * Configurar a distribuição global com as [Graph APIs](graph-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Ligar a uma região preferencial com a Graph API com o SDK .NET

A Graph API é exposta como uma biblioteca de extensão sobre a API SQL.

Para tirar o máximo partido da [distribuição global](distribute-data-globally.md), as aplicações cliente podem especificar a lista de preferência ordenada de regiões a utilizar nas operações de documentos. Isto pode ser feito ao definir a política de ligação. Com base na configuração da conta do Azure Cosmos DB, disponibilidade regional atual e lista de preferência especificada, o SDK selecionará o ponto final ideal para efetuar operações de escrita e leitura.

Esta lista de preferência é especificada ao inicializar uma ligação com os SDKs. Os SDKs aceitam um parâmetro opcional "PreferredLocations" que é uma lista ordenada de regiões do Azure.

* **Escritas**: o SDK envia automaticamente todas as escritas para a região de escrita atual.
* **Leituras**: todas as leituras são enviadas para a primeira região disponível na lista PreferredLocations. Se o pedido falhar, o cliente falha a lista para a região seguinte e assim sucessivamente. Os SDKs só tentam ler a partir das regiões especificadas em PreferredLocations. Por exemplo, se a Conta do Cosmos DB estiver disponível em três regiões, mas o cliente apenas especificar duas regiões de não escrita para PreferredLocations, não são fornecidas leituras fora da região de escrita, mesmo em caso de ativação pós-falha.

A aplicação pode verificar o ponto final de escrita atual e o ponto final de leitura escolhido pelo SDK ao consultar duas propriedades, WriteEndpoint e ReadEndpoint, disponíveis na versão 1.8 e posterior do SDK. Se a propriedade PreferredLocations não estiver definida, todos os pedidos são fornecidos a partir da região de escrita atual.

### <a name="using-the-sdk"></a>Utilizar o SDK

Por exemplo, no SDK .NET, o parâmetro `ConnectionPolicy` para o construtor `DocumentClient` tem uma propriedade denominada `PreferredLocations`. Esta propriedade pode ser definida para uma lista de nomes de região. Os nomes a apresentar para [Regiões do Azure][regions] podem ser especificados como parte de `PreferredLocations`.

> [!NOTE]
> Os URLs para os pontos finais não devem ser considerados como constantes de longa duração. O serviço pode atualizá-los em qualquer momento. O SDK processa esta alteração automaticamente.
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
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

[regions]: https://azure.microsoft.com/regions/

