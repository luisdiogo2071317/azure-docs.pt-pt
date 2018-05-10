---
title: Pesquisa cognitivos mapa enriquecidos campos de entrada aos campos de saída no índices da Azure Search | Microsoft Docs
description: Extrair e enriquecer a campos de dados de origem para mapear para campos de saída num índice da Azure Search.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 67e4798070a73eebb8f61b0b260e3104e9ae6237
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Como mapear campos avançados para um índice pesquisável

Neste artigo, irá aprender a mapear os campos de entrada avançados para campos de saída de um índice pesquisável. Assim que tiver [definido um skillset](cognitive-search-defining-skillset.md), tem de mapear os campos de saída de qualquer skill que contribui diretamente os valores para um determinado campo no seu índice de pesquisa. Mapeamentos de campo são necessários para mover o conteúdo de documentos avançados para o índice.


## <a name="use-outputfieldmappings"></a>Utilizar outputFieldMappings
Para mapear os campos, adicione `outputFieldMappings` à sua definição de indexador conforme mostrado abaixo:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

O corpo do pedido está estruturado da seguinte forma:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Para cada mapeamento de campos de saída, defina o nome do campo avançado (sourceFieldName) e o nome do campo como referenciado no índice (targetFieldName).

O caminho num sourceFieldName pode representar um elemento ou vários elementos. No exemplo acima, ```/document/content/sentiment``` representa um valor numérico único, enquanto ```/document/content/organizations/*/description``` representa várias descrições da organização. Nos casos em que existem vários elementos, estes são "nivelados" uma matriz que contém cada um dos elementos. Mais concretely, para o ```/document/content/organizations/*/description``` exemplo, os dados a *descrições* campo seria parece ser uma matriz simples de descrições de antes de que obtém indexada:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Passos Seguintes
Depois de ter mapeado os campos avançados aos campos pesquisáveis, pode definir os atributos de campo para cada um dos campos pesquisáveis [como parte da definição de índice](search-what-is-an-index.md).

Para obter mais informações sobre o mapeamento de campos, consulte [campo mapeamentos de indexadores de Azure Search](search-indexer-field-mappings.md).