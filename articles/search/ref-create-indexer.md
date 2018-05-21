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
ms.openlocfilehash: 595502ecf0a78491c73800e8077de65707c7a486
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Criar indexador (versão da api de REST do serviço de pesquisa do Azure = 2017-11-11-pré-visualização)

Esta referência de API é uma versão de pré-visualização específicos da documentação, adicionar [pesquisa cognitivos](cognitive-search-concept-intro.md) elementos para a API de indexador criar. Tal como com a [geralmente disponível](https://docs.microsoft.com/rest/api/searchservice/create-indexer) versão, pode criar um indexador novo dentro de um serviço da Azure Search utilizando um pedido POST de HTTP. 

```http
POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json  
    api-key: [admin key]  
```  
O **chave de api** tem de ser uma chave de administração (por oposição a uma chave de consulta). Consulte a secção de autenticação no [segurança na Azure Search](search-security-overview.md) para saber mais sobre as chaves. [Criar um serviço da Azure Search no portal do](search-create-service-portal.md) explica como obter o URL do serviço e as propriedades chave utilizadas no pedido.

Em alternativa, pode utilizar PUT e especifique o nome de origem de dados no URI. Se a origem de dados não existir, será criado.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
O **api-version** é necessária. Atual, geralmente, a versão disponível é `api-version=2017-11-11`, mas terá da versão de pré-visualização para pesquisa cognitivos: `api-version=2017-11-11-Preview`.  Consulte [versões de API na Azure Search](search-api-versions.md) para obter mais detalhes.

Para obter orientações sobre a criação de indexadores específicos da plataforma de dados, começar a utilizar [descrição geral de indexadores](search-indexer-overview.md), que inclui a lista completa de [artigos relacionados](search-indexer-overview.md#next-steps).

> [!NOTE]  
>  O número máximo de indexadores permitido varia consoante o escalão de preço. O serviço gratuito permite até 3 indexadores. O serviço padrão permite 50 indexadores. Serviços de elevada definição padrão não suportam em todos os indexadores. Consulte [limites de serviço](search-limits-quotas-capacity.md) para obter mais detalhes.    

## <a name="request"></a>Pedir  

A [origem de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source), [índice](https://docs.microsoft.com/rest/api/searchservice/create-index), e [skillset](ref-create-skillset.md) fazem parte de um [indexador](search-indexer-overview.md) definição, mas cada é um componente independente que pode ser utilizado no diferentes combinações. Por exemplo, pode utilizar a mesma origem de dados com o índice de mesmo com vários indexadores ou vários indexadores escrever num índice único ou vários indexadores.

 O corpo do pedido contém uma definição de indexador, com as seguintes partes.

+ [dataSourceName](#dataSourceName)
+ [targetIndexName](#targetIndexName)
+ [skillsetName](#skillset)
+ [schedule](#indexer-schedule)
+ [parameters](#indexer-parameters)
+ [fieldMappings](#field-mappings)
+ [outputFieldMappings](#output-fieldmappings)

## <a name="request-syntax"></a>Sintaxe do pedido

Segue-se a sintaxe para structuring o payload de pedido. Um exemplo de pedido é fornecido neste artigo.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "skillsetName" : "Required for cognitive search enrichment",
    "schedule" : { Optional, but immediately runs once if unspecified. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```
<a name="dataSourceName"></a>

### <a name="datasourcename"></a>"dataSourceName"

A [definição da origem de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source) inclui muitas vezes, as propriedades que pode utilizar um indexador para explorar as características de plataforma de origem. Como tal, a origem de dados que pode passa para o indexador determina a disponibilidade de determinadas propriedades e os parâmetros, esse tipo de conteúdo em blobs do Azure ou o tempo limite de consulta de filtragem para a SQL Database do Azure. 

<a name="targetIndexName"></a>

### <a name="targetindexname"></a>"targetIndexName"

Um [esquema de índice](https://docs.microsoft.com/rest/api/searchservice/create-index) define a coleção de campos que contém as atribuições pesquisáveis, filtráveis, recuperável e outras que determinam a forma como o campo é utilizado. Durante a indexação, o indexador pesquisa a origem de dados, opcionalmente cracks documentos e extrai informações, serializa os resultados JSON e indexa o payload com base no esquema definido para o seu índice.

<a name="skillset"></a>

### <a name="skillsetname"></a>"skillsetName"

[Pesquisa cognitivos (pré-visualização)](cognitive-search-concept-intro.md) refere-se a linguagem natural e imagem processamento capacidades na Azure Search, aplicadas durante a ingestão de dados, a extrair entidades, expressões de chaves, idioma, as informações de imagens e assim sucessivamente. São aplicadas ao conteúdo de transformações através de *competências*, que combine numa única [ *skillset*](ref-create-skillset.md), um por cada indexador. Tal como acontece com as origens de dados e índices, um skillset é um componente independente que anexa a um indexador. Pode reutilizar um skillset com outros indexadores, mas cada indexador só pode utilizar um skillset cada vez.
 
<a name="indexer-schedule"></a>

### <a name="schedule"></a>"agenda"  
Um indexador, opcionalmente, pode especificar uma agenda. Sem uma agenda, o indexador for executado imediatamente quando enviar o pedido: ligar, pesquisa e indexação a origem de dados. Em alguns cenários, incluindo as tarefas de indexação de execução longa, são utilizadas para agendas [expandir a janela de processamento](search-howto-reindex.md#scale-out-indexing) além o máximo de 24 horas. Se estiver presente uma agenda, o indexador for executado periodicamente de acordo com a agenda. O programador baseia-se no; Não é possível utilizar um programador externo. A **agenda** tem os seguintes atributos: 

-   **intervalo**: necessário. Um valor de duração que especifica um intervalo ou o período de indexador é executado. O menor intervalo permitido é de cinco minutos; mais longo-é um dia. Tem de ser formatado como um valor de "dayTimeDuration" XSD (um subconjunto restrito de um [duração ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) valor). O padrão para isto é: `"P[nD][T[nH][nM]]".` exemplos: `PT15M` para a cada 15 minutos, `PT2H` para cada 2 horas.  

-   **startTime**: opcional. Datetime de UTC quando o indexador deve iniciar a execução.  

<a name="indexer-parameters"></a>

### <a name="parameters"></a>"parâmetros de"

Um indexador, opcionalmente, pode demorar parâmetros de configuração que modificam comportamentos de tempo de execução. Parâmetros de configuração são delimitados por vírgulas no pedido do indexador. 

```json
    {
      "name" : "my-blob-indexer-for-cognitive-search",
      ... other indexer properties
      "parameters" : { "maxFailedItems" : "15", "batchSize" : "100", "configuration" : { "parsingMode" : "json", "indexedFileNameExtensions" : ".json, .jpg, .png", "imageAction" : "generateNormalizedImages", "dataToExtract" : "contentAndMetadata" } }
    }
```

#### <a name="general-parameters-for-all-indexers"></a>Parâmetros gerais para todos os indexadores

| Parâmetro | Tipo e os valores permitidos   | Utilização  |
|-----------|------------|--------------------------|--------|
| `"batchSize"` | Número inteiro<br/>Predefinição é específica de origem (1000 para a SQL Database do Azure e Azure Cosmos DB, 10 para armazenamento de Blobs do Azure) | Especifica o número de itens que são lidos a partir da origem de dados e indexada como um único lote para melhorar o desempenho. |
| `"maxFailedItems"` | Número inteiro<br/>A predefinição é 0 | Número de erros para tolerar antes de executar um indexador é considerado uma falha. Defina como -1 se não quiser que os erros para parar o processo de indexação. Pode obter informações sobre itens falhadas utilizando [obter estado do indexador](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).  |
| `"maxFailedItemsPerBatch"` | Número inteiro<br/>A predefinição é 0 | Número de erros para tolerar em cada lote antes de executar um indexador é considerado uma falha. Defina como -1 se não quiser que os erros para parar o processo de indexação. |

#### <a name="blob-configuration-parameters"></a>Parâmetros de configuração de blob

Vários parâmetros são exclusivos para um indexador específica, tal como [indexação de Blobs do Azure](search-howto-indexing-azure-blob-storage.md).

| Parâmetro | Tipo e os valores permitidos   | Utilização  |
|-----------|---------------------------|--------|
| `"parsingMode"` | Cadeia<br/>`"text"`<br/>`"delimitedText"`<br/>`"json"`<br/>`"jsonArray"`  | Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), defina a `text` para melhorar o desempenho de indexação nos ficheiros de texto simples no blob storage. <br/>Para [CSV blobs](search-howto-index-csv-blobs.md), defina a `delimitedText` quando blobs são simples ficheiros CSV. <br/>Para [JSON blobs](search-howto-index-json-blobs.md), defina a `json` para extrair estruturado conteúdo ou a `jsonArray` (pré-visualização), a extrair elementos individuais de uma matriz como documentos separados na Azure Search. |
| `"excludedFileNameExtensions"` | Cadeia<br/>lista delimitada por vírgulas | Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), ignorar quaisquer tipos de ficheiro na lista. Por exemplo, pode excluir "PNG, PNG, mp4" Ignorar esses ficheiros durante a indexação. | 
| `"indexedFileNameExtensions"` | Cadeia<br/>lista delimitada por vírgulas | Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), seleciona os blobs se a extensão de ficheiro se encontra na lista. Por exemplo, pode concentrar indexação na aplicação específica ficheiros ". docx, .pptx, .msg" especificamente incluir esses tipos de ficheiro. | 
| `"failOnUnsupportedContentType"` | true <br/>FALSE (predefinição) | Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), defina a `false` se pretender continuar a indexação quando é encontrado um tipo de conteúdo não suportado, e não souber antecipadamente todas os tipos de conteúdos (extensões de ficheiro). |
| `"failOnUnprocessableDocument"` | true <br/>FALSE (predefinição)| Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), defina a `false` se pretender continuar a indexação se falhar de um documento de indexação. |
| `"indexStorageMetadataOnlyForOversizedDocuments"` | true <br/>FALSE (predefinição)| Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), defina esta propriedade como `true` para armazenamento de metadados para o conteúdo de blob que é demasiado grande para processar o índice ainda.  Por predefinição, blobs de grande dimensão são tratados como erros. Para limites de tamanho do blob, consulte [limites de serviço](search-limits-quotas-capacity.md). |
| `"delimitedTextHeaders"` | Cadeia<br/>lista delimitada por vírgulas| Para [blobs CSV (pré-visualização)](search-howto-index-csv-blobs.md), especifica uma lista delimitada por vírgulas de cabeçalhos de coluna, útil para mapeamento de campos de origem para campos de destino num índice. |
| `"delimitedTextDelimiter"` | Cadeia<br/>Definido pelo utilizador | Para [blobs CSV (pré-visualização)](search-howto-index-csv-blobs.md), especifica o delimitador de fim de linha para ficheiros CSV em cada linha é iniciado um novo documento (por exemplo, ""|"`).  |
| `"firstLineContainsHeaders"` | TRUE (predefinição) <br/>false | Para [blobs CSV (pré-visualização)](search-howto-index-csv-blobs.md), indica que a primeira linha (não em branco) de cada blob contém os cabeçalhos.|
| `"documentRoot"`  | Cadeia<br/>Definido pelo utilizador | Para [matrizes JSON (pré-visualização)](search-howto-index-json-blobs.md#nested-json-arrays), tendo em conta um documento estruturado ou semiestruturado, pode especificar um caminho para a matriz de utilizar esta propriedade. |
| `"dataToExtract"` | Cadeia<br/>`"storageMetadata"`<br/>`"allMetadata"`<br/>`"contentAndMetadata"` (predefinição) | Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md):<br/>Definido como `"storageMetadata"` índice apenas o [propriedades blob padrão e o utilizador especificado metadados](../storage/blobs/storage-properties-metadata.md). <br/>Definido como `"allMetadata"` para extrair metadados fornecido pelo subsistema de armazenamento de Blobs do Azure e o [metadados específicos do tipo de conteúdo](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) (por exemplo, metadados exclusivo para apenas os ficheiros PNG) são indexados. <br/>Definido como `"contentAndMetadata"` para extrair todos os metadados e o conteúdo textual de cada blob. <br/><br/>Para [imagem Analysis Services na pesquisa cognitivos (pré-visualização)](cognitive-search-concept-image-scenarios.md), quando `"imageAction"` está definido como `"generateNormalizedImages"`, a `"dataToExtract"` definição indica o indexador que dados para extrair conteúdo de imagem. Aplica-se a imagem incorporada conteúda um. PDF ou outra aplicação ou ficheiros de imagem, tais como. jpg e PNG, em blobs do Azure.  |
| `"imageAction"` |  Cadeia<br/>`"none"`<br/>`"generateNormalizedImages"` | Para [blobs do Azure](search-howto-indexing-azure-blob-storage.md), defina a`"none"` para ignorar imagens incorporadas ou ficheiros de imagem no conjunto de dados. Esta é a predefinição. <br/><br/>Para [imagem Analysis Services na pesquisa cognitivos](cognitive-search-concept-image-scenarios.md), defina a`"generateNormalizedImages"` extraia o texto a partir de imagens (por exemplo, a palavra "parar de" de uma sessão de parar o tráfego) e incorporar como parte do campo conteúdo. Durante a análise de imagem, o indexador cria uma matriz de imagens normalizadas como parte da cracking do documento e incorpora a informação gerada para o campo de conteúdo. Esta ação exige que `"dataToExtract"` está definido como `"contentAndMetadata"`. Uma imagem normalizada refere-se para processamento adicional, resultando na imagem uniform de saída, tamanho e rotação para promover composição consistente quando incluir imagens nos resultados da pesquisa visual (por exemplo, fotografias do mesmo tamanho num gráfico controlam visto a [Demonstração JFK](https://github.com/Microsoft/AzureSearch_JFK_Files)). Esta informação é gerada para cada imagem quando utilizar |


#### <a name="other-configuration-parameters"></a>Outros parâmetros de configuração

Os seguintes parâmetros são específicos para a SQL Database do Azure.

| Parâmetro | Tipo e os valores permitidos   | Utilização  |
|-----------|---------------------------|--------|
| `"queryTimeout"` | Cadeia<br/>"hh: mm:"<br/>"00: 05:00"| Para [SQL Database do Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), defina este parâmetro para aumentar o limite de tempo para lá da predefinição de 5 minutos.|

<a name="field-mappings"></a>

### <a name="fieldmappings"></a>"fieldMappings"

Definições de indexador contenham associações de campo para mapeamento de um campo de origem para um campo de destino num índice da Azure Search. Existem dois tipos de associações, dependendo se a transferência do conteúdo segue um caminho direto ou avançado:

+ **fieldMappings** são opcionais, aplicado quando não correspondem aos nomes de campo de destino de origem, ou quando pretender especificar uma função.
+ **outputFieldMappings** são necessárias se está a criar [um pipeline sem causa](cognitive-search-concept-intro.md). Um pipeline sem causa, o campo de saída é uma construção definida durante o processo de sem causa. Por exemplo, o campo de saída pode ser uma estrutura composta incorporada durante sem causa de dois campos separados no documento de origem. 

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

<a name="output-fieldmappings"></a>

### <a name="outputfieldmappings"></a>"outputFieldMappings"

No [pesquisa cognitivos](cognitive-search-concept-intro.md) cenários em que um skillset está vinculado a um indexador, terá de adicionar `outputFieldMappings` para associar qualquer saída de um passo de sem causa que fornece conteúdo a um campo no índice pesquisável.

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

### <a name="field-mapping-functions"></a>Funções de mapeamento de campo

Mapeamentos de campo também podem ser utilizados para transformar valores de campo de origem utilizando *campo funções mapeamento*. Por exemplo, um valor de cadeia arbitrária pode ser codificados em base64 para que possam ser utilizado para preencher um campo de chave do documento.

Para obter mais informações sobre quando e como utilizar funções de mapeamento de campo, consulte o artigo [campo mapeamento de funções](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

## <a name="request-examples"></a>Exemplos de pedido  
 O primeiro exemplo cria um indexador que copia dados a partir da tabela referenciada pelo `ordersds` da origem de dados para o `orders` índice com base numa agenda que inicia no 1 de Janeiro de 2015 UTC e é executada hora a hora. Cada invocação do indexador estará concluída com êxito se não mais de 5 itens não ser indexados em cada lote e não mais do que 10 itens não ser indexados no total.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2018-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

O segundo exemplo demonstra uma operação de pesquisa cognitivos indicada pela referência a um skillset e [outputFieldMappings](#output-fieldmappings). [Skillsets](ref-create-skillset.md) são recursos de alto nível, definidos separadamente. Neste exemplo é uma abreviatura da definição de indexador no [tutorial search cognitivos](cognitive-search-tutorial-blob.md).

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
        "sourceFieldName" : "content",
        "targetFieldName" : "content"
    }
   ],
  "outputFieldMappings" : 
  [
    {
        "sourceFieldName" : "/document/organizations", 
        "targetFieldName" : "organizations"
    },
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "configuration": 
    {
    "dataToExtract": "contentAndMetadata",
    "imageAction": "generateNormalizedImages"
    }
  }
}
```

## <a name="response"></a>Resposta  
 201 criado para um pedido com êxito.  

## <a name="see-also"></a>Consulte também

+ [Descrição geral do Indexador](search-indexer-overview.md)
+ [Descrição geral de pesquisa cognitivos](cognitive-search-concept-intro.md)
+ [Início rápido: Tente cognitivos pesquisa](cognitive-search-quickstart-blob.md)
+ [Como mapear os campos](cognitive-search-output-field-mapping.md)
