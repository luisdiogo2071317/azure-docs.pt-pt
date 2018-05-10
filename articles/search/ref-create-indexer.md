---
title: Criar indexador (versão da api de REST do serviço de pesquisa do Azure = 2017-11-11-pré-visualização)
description: A pré-visualização API, indexadores são expandidas para incluir parâmetros outputFieldMapping utilizados para mapear saída sem causa a um campo num índice da Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.author: luisca
ms.date: 05/01/2018
ms.prod: azure
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.openlocfilehash: d47b14342caf0312e052584d20f1a9c86ca29cad
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Criar indexador (versão da api de REST do serviço de pesquisa do Azure = 2017-11-11-pré-visualização)

Esta referência da API é uma versão de pré-visualização específicos da documentação, que abrangem os melhoramentos de pesquisa cognitivos para indexação.

Tal como com a [geralmente disponível](https://docs.microsoft.com/rest/api/searchservice/create-indexer) versão, pode criar um indexador novo dentro de um serviço da Azure Search utilizando um pedido POST de HTTP. 

```http
POST https://[service name].search.windows.net/indexers?api-version=[api-version]  
    Content-Type: application/json  
    api-key: [admin key]  
```  
O **chave de api** tem de ser uma chave de administração (por oposição a uma chave de consulta). Consulte a secção de autenticação no [segurança na Azure Search](search-security-overview.md) para saber mais sobre as chaves. [Criar um serviço da Azure Search no portal do](search-create-service-portal.md) explica como obter o URL do serviço e as propriedades chave utilizadas no pedido.

Em alternativa, pode utilizar PUT e especifique o nome de origem de dados no URI. Se a origem de dados não existir, será criado.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
O **api-version** é necessária. A versão atual é `2016-09-01`. Consulte [versões de API na Azure Search](search-api-versions.md) para obter mais detalhes.

Para obter orientações sobre a criação de indexadores específicos da plataforma de dados, começar a utilizar [descrição geral de indexadores](search-indexer-overview.md), que inclui a lista completa de [artigos relacionados](search-indexer-overview.md#next-steps).

> [!NOTE]  
>  O número máximo de indexadores permitido varia consoante o escalão de preço. O serviço gratuito permite até 3 indexadores. O serviço padrão permite 50 indexadores. Consulte [limites de serviço](search-limits-quotas-capacity.md) para obter mais detalhes.    

## <a name="request"></a>Pedir  
 O corpo do pedido contém uma definição de indexador que especifica a origem de dados e o índice de destino para indexação, bem como indexação de agenda opcional e os parâmetros.  

 Segue-se a sintaxe para structuring o payload de pedido. Um exemplo de pedido é fornecido posteriormente neste tópico.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "schedule" : { Optional. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```

### <a name="indexer-schedule"></a>Agendamento do indexador  
Um indexador, opcionalmente, pode especificar uma agenda. Se estiver presente uma agenda, o indexador for executado periodicamente de acordo com a agenda. O programador está incorporado; Não é possível utilizar um programador externo. **Agenda** tem os seguintes atributos: 

-   **intervalo**: necessário. Um valor de duração que especifica um intervalo ou o período de indexador é executado. O menor intervalo permitido é de 5 minutos; mais longo-é um dia. Tem de ser formatado como um valor de "dayTimeDuration" XSD (um subconjunto restrito de um [duração ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) valor). O padrão para isto é: `"P[nD][T[nH][nM]]".` exemplos: `PT15M` para a cada 15 minutos, `PT2H` para cada 2 horas.  

-   **startTime**: opcional. Datetime de UTC quando o indexador deve iniciar a execução.  

### <a name="indexer-parameters"></a>Parâmetros do indexador  
 Um indexador, opcionalmente, pode especificar vários parâmetros que afetam o respetivo comportamento. Todos os parâmetros indicados abaixo são opcionais.  

-   **maxFailedItems**: O número de itens que podem não ser indexados antes de um indexador é considerado uma falha. Predefinição é 0. São devolvidas informações sobre itens falhados pelo [obter estado do indexador &#40;API de REST do serviço de pesquisa do Azure&#41; ](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) operação.  

-   **maxFailedItemsPerBatch**: O número de itens que podem não ser indexados em cada lote antes de um indexador é considerado uma falha. Predefinição é 0.  

-   **batchSize:** Especifica o número de itens que são lidos a partir da origem de dados e indexada como um único lote para melhorar o desempenho. A predefinição depende do tipo de origem de dados: é 1000 para o SQL do Azure e a base de dados do Azure Cosmos e 10 para armazenamento de Blobs do Azure.

### <a name="field-mapping-parameters"></a>Parâmetros de campo mapeamento

Definições de indexador contenham associações de campo para mapeamento de um campo de origem para um campo de destino num índice da Azure Search. Existem dois tipos de associações, dependendo se a transferência do conteúdo segue um caminho direto ou avançado:

+ **fieldMappings** são opcionais, aplicado quando não correspondem aos nomes de campo de destino de origem, ou quando pretender especificar uma função.
+ **outputFieldMappings** são necessárias se está a criar [um pipeline sem causa](cognitive-search-concept-intro.md). Um pipeline sem causa, o campo de saída é uma construção definida durante o processo de sem causa. Por exemplo, o campo de saída pode ser uma estrutura composta incorporada durante sem causa de dois campos separados no documento de origem. 

#### <a name="fieldmappings"></a>fieldMappings

No exemplo seguinte, considere uma tabela de origem com um campo `_id`. A pesquisa do Azure não permite um nome de campo começado por um caráter de sublinhado, pelo que o campo tem de ser mudado. Isto pode ser feito utilizando o `fieldMappings` propriedade do indexador da seguinte forma:

```json
"fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
```

Pode especificar mapeamentos campo:

```json
"fieldMappings" : [
    { "sourceFieldName" : "_id", "targetFieldName" : "id" },
    { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" }
]
```

Os nomes de campo de origem e de destino são sensível.

Para saber mais sobre cenários onde os mapeamentos de campo são úteis, consulte [pesquisa indexador campo mapeamentos](https://docs.microsoft.com/azure/search/search-indexer-field-mappings).

#### <a name="outputfieldmappings"></a>outputFieldMappings

Em cenários de pesquisa cognitivos em que um skillset está vinculado a um indexador, terá de adicionar `outputFieldMappings` para associar qualquer saída de um passo de sem causa que fornece conteúdo a um campo no índice pesquisável.

```json
  "outputFieldMappings" : [
        {
          "sourceFieldName" : "/document/organizations", 
          "targetFieldName" : "organizations"
        },
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*", 
          "targetFieldName" : "keyphrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "language",
            "mappingFunction": null
        }      
   ],
```

<a name="FieldMappingFunctions"></a>

#### <a name="field-mapping-functions"></a>Campo mapeamento de funções

Mapeamentos de campo também podem ser utilizados para transformar valores de campo de origem utilizando *campo funções mapeamento*. Por exemplo, um valor de cadeia arbitrária pode ser codificados em base64 para que possam ser utilizado para preencher um campo de chave do documento.

Para obter mais informações sobre quando e como utilizar funções de mapeamento de campo, consulte o artigo [campo mapeamento de funções](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

### <a name="request-body-examples"></a>Exemplos de corpo do pedido  
 O exemplo seguinte cria um indexador que copia dados a partir da tabela referenciada pelo `ordersds` da origem de dados para o `orders` índice com base numa agenda que inicia no 1 de Janeiro de 2015 UTC e é executada hora a hora. Cada invocação do indexador estará concluída com êxito se não mais de 5 itens não ser indexados em cada lote e não mais do que 10 itens não ser indexados no total.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

## <a name="response"></a>Resposta  
 201 criado para um pedido com êxito.  

## <a name="see-also"></a>Consulte também

+ [Descrição geral de pesquisa cognitivos](cognitive-search-concept-intro.md)
+ [Início rápido: Tente cognitivos pesquisa](cognitive-search-quickstart-blob.md)
+ [Como mapear os campos](cognitive-search-output-field-mapping.md)
