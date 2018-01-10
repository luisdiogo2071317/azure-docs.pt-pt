---
title: "A indexação de uma origem de dados da API do Azure Cosmos BD SQL para a Azure Search | Microsoft Docs"
description: Este artigo mostra como criar um indexador de Azure Search com uma origem de dados de base de dados do Azure Cosmos (API do SQL Server).
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 01/08/2018
ms.author: eugenesh
robot: noindex
ms.openlocfilehash: e449f13adcd1a3651e1cac852b23f21d0227038a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Ligar a BD do Cosmos Azure Search utilizando indexadores

[BD do Azure do Cosmos](../cosmos-db/introduction.md) é base de dados da Microsoft, globalmente distribuído, com vários modelo. Com o respetivo [API do SQL Server](../cosmos-db/sql-api-introduction.md), base de dados do Azure Cosmos fornece avançadas e familiares capacidades de consulta SQL com baixas latências consistentes através de dados JSON sem esquema. A pesquisa do Azure integra-se na perfeição com a API do SQL Server. Pode obter documentos JSON diretamente para um índice da Azure Search utilizando um [indexador de Azure Search](search-indexer-overview.md), concebida especificamente para a API do Azure Cosmos BD do SQL Server. 

Neste artigo, saiba como:

> [!div class="checklist"]
> * Configure a Azure Search para utilizar uma base de dados de API do SQL Server de base de dados do Azure Cosmos como uma origem de dados. Opcionalmente, indique uma consulta para selecionar um subconjunto.
> * Crie um índice de pesquisa com tipos de dados compatível com JSON.
> * Configure um indexador para a pedido e indexação periódica.
> * Atualize progressivamente o índice com base nas alterações nos dados subjacentes.

> [!NOTE]
> API do Azure do SQL Server de base de dados do Cosmos é a próxima geração do DocumentDB. Embora o nome de produto é alterado, o `documentdb` sintaxe de indexadores de pesquisa do Azure ainda existe para efeitos de compatibilidade no APIs de pesquisa do Azure e as páginas do portal. Quando configurar indexadores, é necessário especificar o `documentdb` sintaxe com as instruções neste artigo.

<a name="supportedAPIs"></a>

## <a name="supported-api-types"></a>Tipos de API suportados

Apesar de BD do Cosmos do Azure suporta uma variedade de APIs e modelos de dados, expande o suporte para a API do SQL Server só. 

Suporte para APIs adicionais é lançamento. Para ajudar-na priorizar aqueles para suportar pela primeira vez, converta no web site de voz do utilizador:

* [Suporte de origem de dados de API de tabela](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab)
* [Suporte de origem de dados do gráfico API](https://feedback.azure.com/forums/263029-azure-search/suggestions/13285011-add-graph-databases-to-your-data-sources-eg-neo4)
* [Suporte de origem de dados de MongoDB API](https://feedback.azure.com/forums/263029-azure-search/suggestions/18861421-documentdb-indexer-should-be-able-to-index-mongodb)
* [Suporte de origem de dados do Apache Cassandra API](https://feedback.azure.com/forums/263029-azure-search/suggestions/32857525-indexer-crawler-for-apache-cassandra-api-in-azu)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar um indexador de Azure Cosmos DB, tem de ter um [serviço da Azure Search](search-create-service-portal.md)e criar um índice, origem de dados e, finalmente, o indexador. Pode criar estes objetos utilizando o [portal](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), ou [REST API](/rest/api/searchservice/) para todos os idiomas não .NET. 

Se optar por para o portal, o [Assistente para importar dados](search-import-data-portal.md) orienta-o através da criação de todos estes recursos, incluindo o índice.

> [!TIP]
> Pode iniciar o assistente **Importar dados** a partir do dashboard do Azure Cosmos DB, para simplificar a indexação dessa origem de dados. Na navegação à esquerda, aceda a **Coleções** > **Adicionar Azure Search** para começar.

<a name="Concepts"></a>

## <a name="azure-search-indexer-concepts"></a>Conceitos do indexador de pesquisa do Azure
Suporta a pesquisa a criação e gestão de dados origens (incluindo a API do Azure Cosmos BD do SQL Server) e indexadores que operam contra dessas origens de dados do Azure.

A **origem de dados** Especifica os dados para o índice, credenciais e políticas para identificar as alterações nos dados (por exemplo, documentos modificados ou eliminados no interior da sua coleção). A origem de dados está definida como um recurso independente para que possa ser utilizado por vários indexadores.

Um **indexador** descreve a forma como os dados fluem da sua origem de dados para um índice de pesquisa de destino. Um indexador pode ser utilizado para:

* Execute uma única cópia dos dados para preencher um índice.
* Um índice com as alterações da origem de dados com base numa agenda de sincronização. A agenda é parte da definição de indexador.
* Invocar atualizações a pedido para um índice conforme necessário.

<a name="CreateDataSource"></a>

## <a name="step-1-create-a-data-source"></a>Passo 1: criar uma origem de dados
Para criar uma origem de dados, efetue um pedido POST:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

O corpo do pedido contém a definição de origem de dados, o que deve incluir os seguintes campos:

* **nome**: Escolha um nome para representar a sua base de dados.
* **tipo**: tem de ser `documentdb`.
* **credenciais**:
  
  * **connectionString**: necessário. Especifique as informações de ligação à base de dados do Azure Cosmos DB no seguinte formato:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **contentor**:
  
  * **nome**: necessário. Especifique o id da coleção da base de dados ser indexados.
  * **consulta**: opcional. Pode especificar uma consulta para aplanar um documento JSON arbitrário para um esquema simples que pode índice da Azure Search.
* **dataChangeDetectionPolicy**: recomendado. Consulte [indexar documentos alterado](#DataChangeDetectionPolicy) secção.
* **dataDeletionDetectionPolicy**: opcional. Consulte [indexar documentos eliminado](#DataDeletionDetectionPolicy) secção.

### <a name="using-queries-to-shape-indexed-data"></a>Através de consultas a forma indexada dados
Pode especificar uma consulta SQL para aplanar propriedades aninhadas ou matrizes, as propriedades de JSON do projeto e filtre os dados para ser indexados. 

Documento de exemplo:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Consulta de filtro:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Pelo consulta:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Consultas de projecção:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Matriz flattening consulta:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Passo 2: criar um índice
Se ainda não tiver uma, crie um índice de pesquisa do Azure de destino. Pode criar um índice utilizando o [IU do portal do Azure](search-create-index-portal.md), a [criar API REST do índice](/rest/api/searchservice/create-index) ou [índice classe](/dotnet/api/microsoft.azure.search.models.index).

O exemplo seguinte cria um índice com um campo de id e a descrição:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Certifique-se de que o esquema do seu índice de destino é compatível com o esquema dos documentos JSON origem ou o resultado da sua projecção de consulta personalizada.

> [!NOTE]
> Para coleções particionadas, a chave do documento predefinido é de BD Cosmos Azure `_rid` propriedade, o que obtém o nome mudada para `rid` na Azure Search. Do além disso, Cosmos BD do Azure `_rid` valores contém caracteres inválidos nas chaves de pesquisa do Azure. Por este motivo, o `_rid` os valores são codificados em Base64.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapeamento entre tipos de dados JSON e tipos de dados de pesquisa do Azure
| Tipo de dados JSON | Tipos de campo de índice de destino compatível |
| --- | --- |
| bool |Boolean, EDM |
| Números de aspeto de números inteiros |EDM Edm.Int32, Edm.Int64, |
| Números esse aspeto pontos de vírgula flutuante |Edm.Double, EDM |
| Cadeia |Edm.String |
| Matrizes de tipos primitivos, por exemplo ["a", "b", "c"] |Coleção (Edm.String) |
| Cadeias de aspeto de datas |Edm.DateTimeOffset, EDM |
| Por exemplo GeoJSON objetos {"type": "Ponto", "coordenadas": [lat longo]} |Edm.GeographyPoint |
| Outros objetos JSON |N/A |

<a name="CreateIndexer"></a>

## <a name="step-3-create-an-indexer"></a>Passo 3: Criar um indexador

Depois de criar a origem de dados e índice, está pronto para criar o indexador:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Este indexador for executado a cada duas horas (intervalo de agendamento está definido como "PT2H"). Para executar um indexador cada 30 minutos, defina o intervalo para "PT30M". O mais curto intervalo suportado é 5 minutos. A agenda é opcional - se for omitido, um indexador é executado apenas quando quando for criado. No entanto, pode executar uma indexador a pedido em qualquer altura.   

Para obter mais detalhes sobre a API de indexador criar, veja [criar indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Executar o indexador a pedido
Além de executar periodicamente com base numa agenda, um indexador também pode ser invocado a pedido:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Quando executar API devolve com êxito, a invocação de indexador foi agendada, mas o processamento real acontece de forma assíncrona. 

Pode monitorizar o estado de indexador no portal do ou utilizando a obter indexador Estado API, que iremos descrever seguinte. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Ao obter o estado do indexador
Pode obter o histórico de estado e a execução de um indexador:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

A resposta contém o estado geral de indexador, a invocação de indexador último (ou em curso) e o histórico de invocações de indexador recentes.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Histórico de execução contém até as 50 execuções concluídas mais recentes, que são ordenadas por ordem cronológica inversa (para a execução mais recente vem primeira na resposta).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indexar documentos alterados
O objetivo de uma política de deteção de alteração de dados é de forma eficiente identificar itens de dados alterados. Atualmente, a política de suportados apenas é o `High Water Mark` a política a utilizar o `_ts` propriedade (timestamp) fornecida pelo Azure Cosmos DB, que é especificado da seguinte forma:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Utilizar esta política é altamente recomendável para assegurar o indexador bom desempenho. 

Se estiver a utilizar uma consulta personalizada, certifique-se de que o `_ts` propriedade é projetada pela consulta.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Progresso de incremental e consultas personalizadas
Progresso incremental durante a indexação garante que se a execução do indexador for interrompida por falhas transitórias ou o limite de tempo de execução, o indexador pode recolher onde foi deixada próxima vez que é executado, em vez de ter a nova indexação o conjunto completo de zero. Isto é especialmente importante quando indexação coleções de grandes dimensões. 

Para ativar o progresso incremental quando utiliza uma consulta personalizada, certifique-se de que a sua consulta ordena os resultados pelo `_ts` coluna. Isto permite periódica verificação apontando que utiliza a Azure Search para fornecer incremental progresso na presença de falhas.   

Em alguns casos, mesmo se a consulta contém um `ORDER BY [collection alias]._ts` cláusula, pesquisa do Azure pode não inferir que a consulta está ordenada pelo `_ts`. Pode dizer da Azure Search que os resultados são ordenados por utilizar o `assumeOrderByHighWaterMarkColumn` propriedade de configuração. Para especificar desta sugestão, criar ou atualizar o indexador da seguinte forma: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>A indexação eliminar documentos
Quando as linhas são eliminadas da coleção, que normalmente pretende eliminar as linhas da, bem como o índice de pesquisa. O objetivo de uma política de deteção de eliminação de dados é de forma eficiente identificar itens de dados eliminadas. Atualmente, a política de suportados apenas é o `Soft Delete` política (eliminação está assinalada com um sinalizador de algumas ordenação), que é especificado da seguinte forma:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Se estiver a utilizar uma consulta personalizada, certifique-se de que a propriedade referenciado pelo `softDeleteColumnName` é projetada pela consulta.

O exemplo seguinte cria uma origem de dados com uma política de eliminação de forma recuperável:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Passos seguintes
Parabéns! Aprendeu como integrar a BD do Cosmos do Azure com a Azure Search utilizando um indexador para pesquisar e carregar documentos de um modelo de dados do SQL Server.

* Para saber mais sobre a BD do Cosmos do Azure, consulte o [página do serviço de base de dados do Azure Cosmos](https://azure.microsoft.com/services/cosmos-db/).
* Para saber mais sobre a Azure Search, consulte o [página do serviço de pesquisa](https://azure.microsoft.com/services/search/).
