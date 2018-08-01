---
title: Consultar os conceitos básicos do Azure Search | Documentos da Microsoft
description: Noções básicas para a criação de uma consulta de pesquisa no Azure Search, utilizando os parâmetros para filtrar, selecione e ordenar os resultados.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/27/2018
ms.openlocfilehash: 4650ad89850f32ae5e83a7ac1cd5eac096b180ed
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366451"
---
# <a name="query-fundamentals-in-azure-search"></a>Conceitos básicos de consulta no Azure Search

Uma definição de consulta no Azure Search é uma especificação completa de um pedido que inclui estas peças: ponto final do URL, o índice de destino, utilizado para autenticar o pedido, a versão de api e uma cadeia de consulta de chave de api de serviço. 

Composição de cadeia de caracteres de consulta consiste em parâmetros conforme definido na [API de pesquisa de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents). Mais importante é o **pesquisa =** parâmetro, o que poderia ser indefinido (`search=*`), mas o mais provável que consiste em termos de expressões e operadores semelhantes ao seguinte exemplo:

```
"search": "seattle townhouse +\"lake\""
```

Outros parâmetros são utilizados para direcionar o processamento de consultas ou melhoram a resposta. Exemplos de como são utilizados os parâmetros incluem o controlo de âmbito de pesquisa a campos específicos, a definição de um modo de pesquisa para variar a tendência de recolhimento de precisão e adicionar uma contagem para que pode manter o controle de resultados. 

```
{  
    "search": "seattle townhouse* +\"lake\"",  
    "searchFields": "description, city",  
    "searchMode": "all",
    "count": "true", 
    "queryType": "simple" 
 } 
```

Embora a definição de consulta é fundamental, seu esquema de índice é igualmente importante na forma como especifica operações permitidas numa base de campo por campo. Durante o desenvolvimento de índice, a atributos em campos determinam operações permitidas. Por exemplo, para se qualificar para a pesquisa em texto completo e inclusão nos resultados da pesquisa, um campo deve ser marcado como ambos os *pesquisável* e *recuperável*.

No momento da consulta, execução é sempre em relação a um índice, autenticado com uma chave de api fornecida no pedido. Não é possível associar a índices ou criar estruturas de dados personalizados ou temporário como um destino de consulta.  

Os resultados da consulta são transmitidos como documentos JSON da API REST, embora se usar APIs do .NET, serialização baseia-se. Pode formatar os resultados ao definir os parâmetros na consulta, selecionar campos específicos para o resultado

Para resumir, a substância do pedido de consulta Especifica âmbito e as operações: os campos a incluir na pesquisa, que campos serão incluídos no resultado definidos, a ordenação ou filtro, e assim por diante. Não especificado, é executada uma consulta em relação a todos os campos pesquisáveis como uma operação de pesquisa de texto completo, retornando um resultado sem pontuação definido na ordem arbitrária.

<a name="types-of-queries"></a>

## <a name="types-of-queries-search-and-filter"></a>Tipos de consultas: Procurar e filtrar

A Azure Search oferece muitas opções para criar consultas extremamente poderosas. Os dois principais tipos de consulta que irá utilizar são `search` e `filter`. 

+ `search` consultas de análise para um ou mais termos em todos os *pesquisável* campos no seu índice e funciona da forma que seria de esperar um motor de busca como o Google ou o Bing. Os exemplos a utilização de introdução a `search` parâmetro.

+ `filter` consultas avalie uma expressão booleana todos os *filtráveis* campos num índice. Ao contrário `search`, um `filter` consulta corresponde aos conteúdos exatos de um campo, incluindo sensibilidade em campos de cadeia de caracteres.

Pode utilizar a pesquisa e filtrar em conjunto ou separadamente. Um filtro de autónoma, sem uma cadeia de consulta, é útil quando a expressão de filtro é capaz de qualificar completamente a documentos de interesse. Sem uma cadeia de consulta, não existe nenhuma análise lexical ou linguística, sem pontuação e nenhuma classificação. Tenha em atenção que a cadeia de pesquisa está vazia.

```
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "count": "true"
    }
```

Utilizados em conjunto, o filtro é aplicado primeiro ao índice completo e, em seguida, a pesquisa é executada nos resultados do filtro. Por conseguinte, os filtros podem ser uma técnica útil para melhorar o desempenho de consultas, uma vez que reduzem o conjunto de documentos necessários à consulta de pesquisa para processamento.

A sintaxe para expressões de filtro é um conjunto do [idioma do filtro de OData](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Para consultas de pesquisa pode utilizar a [sintaxe simplificada](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) ou a [sintaxe de consulta Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) abordadas a seguir.


## <a name="choose-a-syntax-simple-or-full"></a>Escolha uma sintaxe: simples ou completa

O Azure Search encontra-se com base no Apache Lucene e dá-lhe escolher entre dois analisadores de consulta para processar consultas típicas e especializadas. Pedidos de pesquisas típicas são formulados usando o portão [sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search). Essa sintaxe suporta um número de operadores de pesquisa comuns, incluindo o e, em alternativa, não, frase, sufixo e operadores de precedência.

O [sintaxe de consulta Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), habilitado quando adiciona **queryType = full** para o pedido, expõe o idioma de consulta amplamente adotado e expressivo, desenvolvido como parte do [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Usando esta sintaxe de consulta permite consultas especializadas:

+ [Consultas de âmbito de campo](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields)
+ [pesquisa difusa](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy)
+ [pesquisa de proximidade](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity)
+ [prazo adaptativo](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost)
+ [pesquisa de expressão regular](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex)
+ [pesquisa com carateres universais](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard)

Operadores booleanos são principalmente os mesmos em ambas as sintaxes, com formatos adicionais no Lucene completa:

+ [Operadores booleanos numa sintaxe simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search#operators-in-simple-search)
+ [Operadores booleanos na sintaxe de Lucene completa](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean)

## <a name="required-and-optional-elements"></a>Elementos necessários e opcionais

Ao submeter pedidos de pesquisa para a Azure Search, existem vários parâmetros que podem ser especificados em conjunto com as palavras reais que são escritas na caixa de pesquisa da sua aplicação. Esses parâmetros de consulta permitem obter um maior controlo da [experiência de pesquisa em texto completo](search-lucene-query-architecture.md).

Elementos necessários numa solicitação de consulta incluem os seguintes componentes:

+ Ponto final e o índice de coleção de documentos, expressada do serviço aqui como um URL `https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`.
+ Versão da API REST (apenas), expressado como **api-version =**
+ consulta ou um administrador a chave de api, expresso como **a chave de api =**
+ expressado como de cadeia de consulta **pesquisa =**, que pode ser especificada se deseja realizar uma pesquisa em branco. Também pode enviar apenas uma expressão de filtro como **$filter =**.
+ **queryType =**, simples ou completo, que pode ser omitido se desejar usar a sintaxe simples predefinido.

Todos os outros parâmetros de pesquisa são opcionais.

## <a name="apis-and-tools-for-testing"></a>APIs e ferramentas de teste

A tabela seguinte lista as APIs e as abordagens baseadas em ferramenta para o envio de consultas.

| Metodologia | Descrição |
|-------------|-------------|
| [SearchIndexClient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Cliente que pode ser usado para consultar um índice da Azure Search.  <br/>[Saiba mais.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Pesquisa de documentos (REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | GET ou métodos POST num índice, com parâmetros de consulta de entrada adicional.  |
| [Outra ferramenta de teste HTTP, Postman ou fiddler](search-fiddler.md) | Explica como configurar um cabeçalho de pedido e o corpo para enviar consultas para o Azure Search.  |
| [Explorador de pesquisa no portal do Azure](search-explorer.md) | Fornece uma barra de pesquisa e as opções de seleções de índice e a versão de api. Os resultados são devolvidos como documentos JSON. <br/>[Saiba mais.](search-get-started-portal.md#query-index) | 

## <a name="manage-search-results"></a>Gerir os resultados da pesquisa

Parâmetros de consulta podem ser utilizados para estruturar o resultado definido da seguinte maneira:

+ Limitar ou o número de documentos nos resultados (50 por predefinição) de criação de batches
+ Selecionar campos para incluir nos resultados
+ A definição de uma ordem de classificação
+ Prima Adicionar destaca para chamar a atenção para a correspondência de termos no corpo dos resultados da pesquisa

### <a name="tips-for-unexpected-results"></a>Dicas para resultados inesperados

Ocasionalmente, a substância e não a estrutura de resultados são inesperados. Quando os resultados de consulta não que espera ver, pode experimentar estas alterações de consulta para ver se a melhorar os resultados:

+ Alteração `searchMode=any` (predefinição) para `searchMode=all` para exigir correspondências em todos os critérios em vez de um dos critérios. Isso é especialmente verdadeiro quando operadores booleanos são incluídos a consulta.

+ Se o texto ou análise lexical, é necessário, mas o tipo de consulta o processamento linguístico impede, altere a técnica de consulta. Na pesquisa em texto completo, texto ou análise lexical corrige automaticamente para erros ortográficos, formas de palavras de singular plural e até mesmo irregulares verbos ou substantivos. Para algumas consultas, tais como difusa ou pesquisa com carateres universais, a análise de texto não é parte da consulta pipeline de análise. Em alguns cenários, as expressões regulares foram utilizadas como uma solução alternativa. 

### <a name="paging-results"></a>Resultados de paginação
A Azure Search facilita a implementação da paginação dos resultados da pesquisa. Através dos parâmetros `top` e `skip`, pode processar facilmente pedidos de pesquisa que permitem receber o conjunto total de resultados da pesquisa em subconjuntos geríveis e ordenados que permitem facilmente boas práticas da IU de pesquisa. Ao receber esses subconjuntos de resultados mais pequenos, pode também receber a contagem de documentos no conjunto total de resultados da pesquisa.

Pode saber mais sobre a paginação dos resultados da pesquisa no artigo [Como paginar os resultados da pesquisa na Azure Search](search-pagination-page-layout.md).

### <a name="ordering-results"></a>Ordenar resultados
Ao receber resultados de uma consulta de pesquisa, pode pedir à Azure Search para apresentar os resultados ordenados pelos valores num campo específico. Por predefinição, a Azure Search ordena os resultados da pesquisa com base na classificação de pontuação de pesquisa de cada documento, a qual deriva de [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Se pretender que a Azure Search devolva os resultados ordenados por um valor diferente da pontuação de pesquisa, pode utilizar o parâmetro de pesquisa `orderby`. Pode especificar o valor do parâmetro `orderby` para incluir os nomes de campo e as chamadas para a [`geo.distance()` função](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) para valores geoespaciais. Cada expressão pode ser seguida por `asc` para indicar que os resultados foram pedidos por ordem ascendente e por `desc` para indicar que os resultados foram pedidos por ordem descendente. A ordem ascendente de classificação predefinida.


### <a name="hit-highlighting"></a>Detetor de ocorrências
Na Azure Search, o realce da parte exata dos resultados da pesquisa que correspondem à consulta de pesquisa é facilitado através dos parâmetros `highlight`, `highlightPreTag` e `highlightPostTag`. Pode especificar os campos *pesquisáveis* que devem ter o respetivo texto com correspondência realçado, bem como especificar as etiquetas de cadeias exatas a adicionar no início e no fim do texto com correspondência devolvido pela Azure Search.

## <a name="see-also"></a>Consulte também

+ [Cheio funciona a pesquisa de texto no Azure Search (consulta de análise de arquitetura](search-lucene-query-architecture.md)
+ [Explorador de pesquisa](search-explorer.md)
+ [Como consultar no .NET](search-query-dotnet.md)
+ [Como consultar no REST](search-query-rest-api.md)
