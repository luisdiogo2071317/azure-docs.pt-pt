---
title: A indexação de blobs CSV com o indexador de blob do Azure Search | Microsoft Docs
description: Saiba como índice blobs CSV pesquisa do Azure
author: chaosrealm
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 12/28/2017
ms.author: eugenesh
ms.openlocfilehash: bf65ab7858ba792418e325e7a025ee1bd88bbb27
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>A indexação de blobs CSV com o indexador de blob do Azure Search
Por predefinição, [indexador de blob do Azure Search](search-howto-indexing-azure-blob-storage.md) analisa os blobs de texto delimitado por como um segmento de texto único. No entanto, com blobs que contém os dados CSV, muitas vezes, pretende tratar cada linha no blob como um documento separado. Por exemplo, tendo em conta o seguinte texto delimitado, pode querer analisá-lo em dois documentos, cada que contém campos de "etiquetas", "datePublished" e "id": 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Neste artigo, irá aprender a analisar os blobs CSV com um indexador de blob do Azure Search. 

> [!IMPORTANT]
> Esta funcionalidade está atualmente em pré-visualização pública e não deve ser utilizada em ambientes de produção. Para obter mais informações, consulte [REST api-version = 2017-11-11-Preview](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>Como configurar a indexação de CSV
Índice de blobs CSV, criar ou atualizar uma definição de indexador com o `delimitedText` análise modo:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Para obter mais detalhes sobre a API de indexador criar, veja [criar indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` indica que a primeira linha (não em branco) de cada blob contém os cabeçalhos.
Se os blobs não contém uma linha de cabeçalho inicial, os cabeçalhos devem ser especificados na configuração do indexador: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Pode personalizar o caráter delimitador a utilizar o `delimitedTextDelimiter` definição de configuração. Por exemplo:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Atualmente, é suportada apenas a codificação UTF-8. Se precisar de suporte para outras codificações, votá-lo em [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Quando utiliza o modo de análise de texto delimitado, da Azure Search parte do princípio de que todos os blobs na sua origem de dados será CSV. Se precisar de suportar uma mistura de blobs CSV e sem CSV na mesma origem de dados,. votá-lo em [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Exemplos de pedido
Colocar este todos os em conjunto, Eis os exemplos de payload completo. 

Origem de dados: 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexador:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Ajude-na tornar o melhor da Azure Search
Se tiver de pedidos de funcionalidades ou ideias para melhoramentos, fornecer os dados introduzidos no [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

