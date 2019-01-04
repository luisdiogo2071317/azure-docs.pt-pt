---
title: Blobs JSON de indexação do indexador de Blobs do Azure para pesquisa em texto completo - Azure Search
description: Pesquise blobs JSON do Azure para o conteúdo de texto com o indexador Blob do Azure Search. Indexadores automatizam ingestão de dados para origens de dados selecionadas, como o armazenamento de Blobs do Azure.
ms.date: 12/21/2018
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 6df8d9a5c1ca1e587834ea08f73b3dd9498f8537
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753154"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexar blobs JSON com o indexador Blob do Azure Search
Este artigo mostra-lhe como configurar um indexador de Blobs do Azure Search para extrair conteúdo estruturado de blobs JSON no armazenamento de Blobs do Azure.

Pode utilizar o [portal](#json-indexer-portal), [REST APIs](#json-indexer-rest), ou [SDK de .NET](#json-indexer-dotnet) para indexar conteúdo JSON. Comum a todas as abordagens é documentos JSON localizados num contentor de BLOBs numa conta de armazenamento do Azure. Para obter orientações sobre como enviar documentos JSON a partir de outras plataformas não pertencente ao Azure, consulte [importação de dados no Azure Search](search-what-is-data-import.md).

Blobs JSON no armazenamento de Blobs do Azure são, normalmente, um único documento JSON ou uma matriz JSON. O indexador blob do Azure Search pode analisar a construção, dependendo de como definir o **parsingMode** parâmetro na solicitação.

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Utilizar o portal

O método mais fácil para indexar documentos JSON é usar um assistente no [portal do Azure](https://portal.azure.com/). Ao analisar metadados no contentor de Blobs do Azure, o [ **importar dados** ](search-import-data-portal.md) assistente pode criar um índice predefinido, mapear campos de origem para campos de índice de destino e carregar o índice numa única operação. Dependendo do tamanho e complexidade da origem de dados, poderia ter um índice de pesquisa de texto completo operacional em minutos.

### <a name="1---prepare-source-data"></a>1 - preparar os dados de origem

Deve ter uma conta de armazenamento do Azure, com armazenamento de BLOBs e um contentor de documentos JSON. Se não estiver familiarizado com qualquer uma destas tarefas, reveja o pré-requisito "Configurar dados de exemplo de serviço e o carregamento de Blobs do Azure" na [cognitivo pesquisa-quickstart](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data).

### <a name="2---start-import-data-wizard"></a>2 - iniciar o Assistente de importação de dados

Pode [iniciar o assistente](search-import-data-portal.md) na barra de comandos na página do serviço de pesquisa do Azure ou ao clicar **adicionar Azure Search** no **serviço Blob** secção da sua conta de armazenamento Painel de navegação esquerdo.

### <a name="3---set-the-data-source"></a>3 - definir a origem de dados

Na **origem de dados** página, a origem tem de ser **armazenamento de Blobs do Azure**, com as seguintes especificações:

+ **Dados a extrair** deve ser *conteúdo e metadados*. Escolher esta opção permite que o Assistente para inferir um esquema de índice e mapear os campos para importação.
   
+ **Modo de análise** deve ser definido como *JSON* ou *matriz JSON*. 

  *JSON* articule toda cada blob como um documento único de pesquisa, aparecer como um item independente nos resultados da pesquisa. 

  *Matriz JSON* é para blobs é composto por vários elementos, onde pretende que cada elemento a ser articulado como autónomo, independente de pesquisar no documento. Se os blobs são complexos e não escolher *matriz JSON* todo o blob é ingerido como um único documento.
   
+ **Contentor de armazenamento** tem de especificar a conta de armazenamento e contentor ou uma cadeia de ligação que é resolvido para o contentor. Pode obter cadeias de ligação na página de portal do serviço Blob.

   ![Definição de origem de dados de blob](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - ignorar a página de "Adicionar a pesquisa cognitiva" no Assistente

Adicionar capacidades cognitivas não é necessário para importação de documentos JSON. A menos que tenha uma necessidade específica de [incluem APIs serviços cognitivos e transformações](cognitive-search-concept-intro.md) para o seu pipeline de indexação, deve ignorar este passo.

Para ignorar o passo, clique na página seguinte **personalizar o índice de destino**.

### <a name="5---set-index-attributes"></a>5 - conjunto de atributos de índice

Na **índice** página, deve ver uma lista de campos com um tipo de dados e uma série de caixas de seleção para a definição de atributos de índice. O assistente pode gerar um índice predefinido com base nos metadados e a origem de dados de amostragem. 

As predefinições, muitas vezes, produzem uma solução funcional: selecionar um campo (como cadeia de caracteres de conversão) para servir como a chave ou ID para identificar exclusivamente cada documento, bem como campos que são bons candidatos para a pesquisa em texto completo e recuperáveis no conjunto de resultados de documentos. Para blobs, a `content` campo é o melhor candidato para conteúdo pesquisável.

Pode aceitar as predefinições ou, reveja a descrição da [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) e [analisadores de idiomas](https://docs.microsoft.com/rest/api/searchservice/language-support) para substituir ou complementar os valores iniciais. 

Dedique uns momentos para rever as suas seleções. Depois de executar o assistente, estruturas de dados físico são criadas e não será possível editar estes campos sem remover e recriar todos os objetos.

   ![Definição de índice de blob](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - criar indexador

Total especificado, o assistente cria 3 objetos distintos no seu serviço de pesquisa. Um objeto de origem de dados e o objeto de índice são guardadas como recursos nomeados do seu serviço Azure Search. A última etapa cria um objeto de indexador. O indexador de nomenclatura permite que ele existe como um recurso de autónomo, o que pode agendar e gerir de forma independente o objeto de origem dados e índice, criado na mesma sequência em Assistente.

Se não estiver familiarizado com indexadores, um *indexador* é um recurso no Azure Search, que pesquisa uma origem de dados externos para conteúdo pesquisável. A saída do **importar dados** assistente é um indexador que rastreia a sua origem de dados JSON, extrai o conteúdo pesquisável e importa-lo para um índice na Azure Search.

   ![Definição de indexador de BLOBs](media/search-howto-index-json/import-wizard-json-indexer.png)

Clique em **OK** para executar o assistente e criar todos os objetos. Indexação começa imediatamente.

Pode monitorizar a importação de dados nas páginas de portais. Notificações de progresso indicam o estado de indexação e o número de documentos são carregados. Quando a indexação é concluída, pode utilizar [Explorador de pesquisa](search-explorer.md) para consultar o índice.

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>Utilizar APIs REST

Indexar JSON blobs é semelhante a extração de documento regulares num fluxo de trabalho de três partes comuns a todos os indexadores na pesquisa do Azure: criar uma origem de dados, criar um índice, criar um indexador.

Para JSON baseadas em código indexação, pode utilizar a API de REST com APIs para o [indexadores](https://docs.microsoft.com/rest/api/searchservice/create-indexer), [origens de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source), e [índices](https://docs.microsoft.com/rest/api/searchservice/create-index). Em contraste com o Assistente do portal, uma abordagem de código requer que tenha um índice no local, pronto para aceitar os documentos JSON ao enviar o **criar indexador** pedido.

Blobs JSON no armazenamento de Blobs do Azure são, normalmente, um único documento JSON ou uma matriz JSON. O indexador blob do Azure Search pode analisar a construção, dependendo de como definir o **parsingMode** parâmetro na solicitação.

| Documento JSON | parsingMode | Descrição | Disponibilidade |
|--------------|-------------|--------------|--------------|
| Um por blob | `json` | Analisa JSON blobs como um único segmento de texto. Cada blob JSON torna-se um único documento de Azure Search. | Disponível em geral nos dois [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) e [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) APIs. |
| Vários por blob | `jsonArray` | Analisa uma matriz JSON no blob, onde cada elemento da matriz se torna um documento separado do Azure Search.  | Disponível em geral nos dois [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) e [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) APIs. |


### <a name="step-1-create-a-data-source"></a>Passo 1: Criar uma origem de dados

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

O índice armazena conteúdo pesquisável no Azure Search. Para criar um índice, fornece um esquema que especifica os campos num documento, atributos e outras construções que moldar a experiência de pesquisa. Se criar um índice com os mesmos nomes de campos e tipos de dados como a origem, o indexador corresponderá aos campos de origem e de destino, poupando o trabalho de ter de mapear explicitamente os campos.

A exemplo a seguir mostra um [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) pedido. O índice terão um pesquisável `content` campo para armazenar os textos extraídos dos blobs:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="step-3-configure-and-run-the-indexer"></a>Passo 3: Configurar e executar o indexador

Até agora, as definições para a origem de dados e índice foram parsingMode independente. No entanto, no passo 3 para a configuração do indexador, o caminho diverges dependendo de como pretende que o blob JSON conteúdo até serem examinados e estruturadas no índice da Azure Search. As opções são `json` ou `jsonArray`:

+ Definir **parsingMode** para `json` para cada blob como um único documento de índice.

+ Definir **parsingMode** para `jsonArray` se os blobs consistem em matrizes JSON, e precisar de cada elemento da matriz para se tornar um documento separado no Azure Search. Pode pensar num documento como um único item nos resultados da pesquisa. Se pretender que cada elemento da matriz ser apresentado nos resultados da pesquisa como um item independente, em seguida, utilize o `jsonArray` opção.

Dentro da definição de indexador, opcionalmente, pode utilizar **mapeamentos de campo** escolher quais propriedades do documento JSON de origem são usadas para preencher o índice de pesquisa de destino. Para matrizes JSON, se a matriz de existir como uma propriedade de nível inferior, pode definir uma raiz do documento que indica em que a matriz é colocada dentro do blob.

> [!IMPORTANT]
> Quando utiliza `json` ou `jsonArray` modo de análise, o Azure Search assume que todos os blobs na sua origem de dados contenham JSON. Se tiver de suportar uma mistura de blobs JSON e não-JSON na mesma origem de dados, fale na [nosso site do UserVoice](https://feedback.azure.com/forums/263029-azure-search).


### <a name="how-to-parse-single-json-blobs"></a>Como analisar únicos de JSON de blobs

Por predefinição, [indexador de Blobs do Azure Search](search-howto-indexing-azure-blob-storage.md) analisa JSON blobs como um único segmento de texto. Muitas vezes, deseja preservar a estrutura dos seus documentos JSON. Por exemplo, suponha que tem o seguinte documento JSON no armazenamento de Blobs do Azure:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

O indexador blob analisa o documento JSON num único documento de Azure Search. O indexador carrega um índice por "text", "datePublished" e "etiquetas" da origem em relação a campos de índice de destino de forma idêntica com nome e com tipos correspondentes.

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

### <a name="how-to-parse-json-arrays"></a>Como analisar as matrizes de JSON

Em alternativa, pode optar por para a funcionalidade de matriz JSON. Esta funcionalidade é útil quando os blobs de conter uma *matriz de objetos JSON*, e pretender que cada elemento para se tornar um documento separado do Azure Search. Por exemplo, tendo em conta o blob JSON seguinte, pode preencher o índice da Azure Search com três documentos separados, cada um com campos "id" e "text".  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Para uma matriz JSON, a definição de indexador deve ter um aspeto semelhante ao seguinte exemplo. Tenha em atenção que o parâmetro parsingMode Especifica o `jsonArray` analisador. Especificando o analisador correto e ter os dados corretos entrada são apenas dois requisitos específicos de matriz para indexar JSON blobs.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Novamente, tenha em atenção que os mapeamentos de campo podem ser omitidos. Supondo que um índice com "id" com nomes idênticos e campos de "text", o indexador blob pode inferir o mapeamento correto sem uma lista de mapeamento de campo explícita.

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

### <a name="using-field-mappings-to-build-search-documents"></a>Através de mapeamentos de campo para criar documentos de pesquisa

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

Os nomes de campos de origem os mapeamentos são especificados com o [ponteiro JSON](https://tools.ietf.org/html/rfc6901) notação. Começar com uma barra de segurança para fazer referência à raiz do seu documento JSON, em seguida, selecionar a propriedade pretendida (no nível arbitrário de aninhamento) com o caminho de barra por vírgulas de encaminhamento.

Também pode consultar elementos individuais da matriz usando um índice baseado em zero. Por exemplo, para escolher o primeiro elemento da matriz "etiquetas" do exemplo acima, utilize um mapeamento de campo como este:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Se um nome de campo de origem num caminho de mapeamento de campo se refere a uma propriedade que não existe em JSON, esse mapeamento é ignorado sem erros. Isso é feito para que podemos oferecer suporte a documentos com um esquema diferente (que é um caso de utilização comuns). Porque não existe nenhuma validação, precisa tomar cuidado para evitar erros de digitação na sua especificação de mapeamento de campo.
>
>

### <a name="rest-example-indexer-request-with-field-mappings"></a>Exemplo do REST: Pedido de indexador com mapeamentos de campo

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

<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>Utilizar o .NET SDK

O SDK de .NET totalmente tem paridade com a API REST. Recomendamos que reveja a secção de REST API anterior para aprender conceitos, fluxo de trabalho e os requisitos. Em seguida, pode consultar a seguinte documentação de referência de .NET API para implementar um indexador JSON em código gerenciado.

+ [Microsoft.Azure.Search.models.DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [Microsoft.Azure.Search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [Microsoft.Azure.Search.models.Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [Microsoft.Azure.Search.models.Indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

## <a name="see-also"></a>Consulte também

+ [Indexadores na Azure Search](search-indexer-overview.md)
+ [Indexação do armazenamento de Blobs do Azure com o Azure Search](search-howto-index-json-blobs.md)
+ [Indexar blobs CSV com o indexador de Blobs do Azure Search](search-howto-index-csv-blobs.md)
+ [Tutorial: Pesquisar dados semiestruturados do armazenamento de Blobs do Azure](search-semi-structured-data.md)
