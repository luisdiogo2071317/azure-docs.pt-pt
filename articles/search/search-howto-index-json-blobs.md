---
title: Indexar blobs JSON com o indexador de Blobs do Azure Search
description: Indexar blobs JSON com o indexador de Blobs do Azure Search
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.openlocfilehash: a4689093508c3287e60da9d4668393e71211fbdd
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405707"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexar blobs JSON com o indexador de Blobs do Azure Search
Este artigo mostra-lhe como configurar um indexador de Blobs do Azure Search para extrair conteúdo estruturado de blobs JSON no armazenamento de Blobs do Azure.

Blobs JSON no armazenamento de Blobs do Azure são, normalmente, um único documento JSON ou uma matriz JSON. O indexador blob do Azure Search pode analisar a construção, dependendo de como definir o **parsingMode** parâmetro na solicitação.

| Documento JSON | parsingMode | Descrição | Disponibilidade |
|--------------|-------------|--------------|--------------|
| Um por blob | `json` | Analisa JSON blobs como um único segmento de texto. Cada blob JSON torna-se um único documento de Azure Search. | Disponível em geral nos dois [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) e [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) APIs. |
| Vários por blob | `jsonArray` | Analisa uma matriz JSON no blob, onde cada elemento da matriz se torna um documento separado do Azure Search.  | Em pré-visualização, no [REST api-version =`2017-11-11-Preview` ](search-api-2017-11-11-preview.md) e [pré-visualização do .NET SDK](https://aka.ms/search-sdk-preview). |

> [!Note]
> As APIs de pré-visualização destinam-se para teste e avaliação e não devem ser usadas em ambientes de produção.
>

## <a name="setting-up-json-indexing"></a>Configuração do JSON de indexação
Indexar JSON blobs é semelhante a extração de documento regulares num fluxo de trabalho de três partes comuns a todos os indexadores na Azure Search.

### <a name="step-1-create-a-data-source"></a>Passo 1: criar uma origem de dados

A primeira etapa é fornecer informações de ligação de origem de dados o indexador. Os dados da origem de tipo, especificado aqui como `azureblob`, determina quais comportamentos de extração de dados são invocados o indexador. Para JSON a indexação de BLOBs, a definição de origem de dados é o mesmo para documentos JSON e matrizes. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Passo 2: Criar um índice de pesquisa de destino 

Indexadores são associados um esquema de índice. Se estiver a utilizar a API (em vez do portal), prepare um índice com antecedência para que pode especificá-lo sobre a operação de indexador. 

> [!Note]
> Indexadores são expostos no portal do através da **importação** ação para um número limitado de indexadores geralmente disponíveis. Muitas vezes, o fluxo de trabalho de importação, muitas vezes, pode construir um índice preliminar, com base nos metadados na origem. Para obter mais informações, consulte [importar dados para o Azure Search no portal do](search-import-data-portal.md).

### <a name="step-3-configure-and-run-the-indexer"></a>Passo 3: Configurar e executar o indexador

Até agora, as definições para a origem de dados e índice foram parsingMode independente. No entanto, no passo 3 para a configuração do indexador, o caminho diverges dependendo de como pretende que o blob JSON conteúdo até serem examinados e estruturadas no índice da Azure Search.

Ao chamar o indexador, faça o seguinte:

+ Definir o **parsingMode** parâmetro para `json` (para cada blob como um único documento de índice) ou `jsonArray` (se os blobs contêm matrizes JSON, e terá de cada elemento de uma matriz deve ser tratada como um documento separado).

+ Opcionalmente, utilize **mapeamentos de campo** escolher quais propriedades do documento JSON de origem são usadas para preencher o índice de pesquisa de destino. Para matrizes JSON, se a matriz de existir como uma propriedade de nível inferior, pode definir uma raiz do documento que indica em que a matriz é colocada dentro do blob.

> [!IMPORTANT]
> Quando utiliza `json` ou `jsonArray` modo de análise, o Azure Search assume que todos os blobs na sua origem de dados contenham JSON. Se tiver de suportar uma mistura de blobs JSON e não-JSON na mesma origem de dados, fale na [nosso site do UserVoice](https://feedback.azure.com/forums/263029-azure-search).


## <a name="how-to-parse-single-json-blobs"></a>Como analisar únicos de JSON de blobs

Por predefinição, [indexador de Blobs do Azure Search](search-howto-indexing-azure-blob-storage.md) analisa JSON blobs como um único segmento de texto. Muitas vezes, deseja preservar a estrutura dos seus documentos JSON. Por exemplo, suponha que tem o seguinte documento JSON no armazenamento de Blobs do Azure:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

### <a name="indexer-definition-for-single-json-blobs"></a>Definição de indexador para blobs JSON únicos

Com o indexador de Blobs do Azure Search, um documento JSON semelhante ao exemplo anterior é analisado num único documento de Azure Search. O indexador carrega um índice ao corresponder a "text", "datePublished" e "etiquetas" a partir da origem de campos de forma idêntica com nome e tipo de destino.

Configuração é fornecida no corpo de uma operação de indexador. Lembre-se de que o objeto de origem de dados, definido anteriormente, especifica as informações de ligação e tipo da origem de dados. Além disso, o índice de destino também tem de existir como um recipiente vazio no seu serviço. Agenda e os parâmetros são opcionais, mas se omiti-los, o indexador é executado imediatamente, usando `json` como o modo de análise.

Um pedido totalmente especificado será semelhante ao seguinte:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Como observado, os mapeamentos de campo não são necessários. Tendo em conta um índice com "text", "datePublished e"etiquetas"campos, o blob indexador pode inferir o mapeamento correto sem um campo de mapeamento presentes no pedido.

## <a name="how-to-parse-json-arrays-preview"></a>Como analisar as matrizes de JSON (pré-visualização)

Em alternativa, pode optar pela funcionalidade de pré-visualização de matriz JSON. Esta funcionalidade é útil quando os blobs de conter uma *matriz de objetos JSON*, e pretender que cada elemento para se tornar um documento separado do Azure Search. Por exemplo, tendo em conta o blob JSON seguinte, pode preencher o índice da Azure Search com três documentos separados, cada um com campos "id" e "text".  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

### <a name="indexer-definition-for-a-json-array"></a>Definição de indexador para uma matriz JSON

Para uma matriz JSON, o pedido de indexador utiliza a API de pré-visualização e o `jsonArray` analisador. Estes são apenas dois requisitos específicos de matriz para indexar JSON blobs.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Novamente, tenha em atenção que os mapeamentos de campo não são necessários. Devido um índice com campos "id" e "text", o indexador blob pode inferir o mapeamento correto sem uma lista de mapeamento de campo.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>Matrizes JSON aninhadas
E se deseja indexar uma matriz de objetos JSON, mas essa matriz está aninhada em algum lugar dentro do documento? Pode escolher qual propriedade contém a matriz usando o `documentRoot` propriedade de configuração. Por exemplo, se os blobs ter este aspeto:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Utilize esta configuração para indexar a matriz contida no `level2` propriedade:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="using-field-mappings-to-build-search-documents"></a>Através de mapeamentos de campo para criar documentos de pesquisa

Quando os campos de origem e destino não estão alinhados perfeitamente, pode definir uma secção de mapeamento de campo no corpo do pedido para associações de campo a campo explícitas.

Atualmente, do Azure Search não consegue indexar documentos JSON arbitrários diretamente, uma vez que suporta tipos de dados apenas primitivos, as matrizes de seqüência de caracteres e pontos de GeoJSON. No entanto, pode usar **mapeamentos de campo** para escolher partes do documento JSON e "lift-los" nos campos de nível superior do documento de pesquisa. Para saber mais sobre noções básicas de mapeamentos de campo, veja [mapeamentos de campo em indexadores do Azure Search](search-indexer-field-mappings.md).

Revisitando nosso documento JSON de exemplo:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Partem do princípio de um índice de pesquisa com os seguintes campos: `text` typu `Edm.String`, `date` typu `Edm.DateTimeOffset`, e `tags` do tipo `Collection(Edm.String)`. Tenha em atenção a discrepância entre "datePublished" na origem e `date` campo no índice. Para mapear o seu JSON para o formato pretendido, utilize os seguintes mapeamentos de campo:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Os nomes de campos de origem os mapeamentos são especificados com o [ponteiro JSON](http://tools.ietf.org/html/rfc6901) notação. Começar com uma barra de segurança para fazer referência à raiz do seu documento JSON, em seguida, selecionar a propriedade pretendida (no nível arbitrário de aninhamento) com o caminho de barra por vírgulas de encaminhamento.

Também pode consultar elementos individuais da matriz usando um índice baseado em zero. Por exemplo, para escolher o primeiro elemento da matriz "etiquetas" do exemplo acima, utilize um mapeamento de campo como este:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Se um nome de campo de origem num caminho de mapeamento de campo se refere a uma propriedade que não existe em JSON, esse mapeamento é ignorado sem erros. Isso é feito para que podemos oferecer suporte a documentos com um esquema diferente (que é um caso de utilização comuns). Porque não existe nenhuma validação, precisa tomar cuidado para evitar erros de digitação na sua especificação de mapeamento de campo.
>
>

## <a name="example-indexer-request-with-field-mappings"></a>Exemplo: Pedido de indexador com mapeamentos de campo

O exemplo seguinte é um payload de indexador totalmente especificado, incluindo os mapeamentos de campo:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }


## <a name="help-us-make-azure-search-better"></a>Ajude-na melhorar o Azure Search
Se tiver de pedidos de funcionalidades ou ideias para melhorias, contacte-na nossa [site do UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="see-also"></a>Consulte também

+ [Indexadores na Azure Search](search-indexer-overview.md)
+ [Indexação do armazenamento de Blobs do Azure com o Azure Search](search-howto-index-json-blobs.md)
+ [Indexar blobs CSV com o indexador de Blobs do Azure Search](search-howto-index-csv-blobs.md)
+ [Tutorial: Pesquisar dados semiestruturados do armazenamento de Blobs do Azure ](search-semi-structured-data.md)
