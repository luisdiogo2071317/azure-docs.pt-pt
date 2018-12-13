---
title: Analisadores de linguística e o processamento de texto - Azure Search
description: Analisadores de atribuir a campos de texto pesquisável num índice para substituir a predefinição Lucene padrão com alternativas personalizadas, predefinidas ou específicas de idioma.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 1de3743d6ec37d263e16b168d32d2b56b0a28295
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310543"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Analisadores de processamento no Azure Search de texto

Uma *analisador* é um componente do [pesquisa em texto completo](search-lucene-query-architecture.md) responsável pelo processamento de texto em cadeias de consulta e documentos indexados. As seguintes transformações são característicos durante a análise:

+ Não essencial palavras (palavras de paragem) e pontuação são removidos.
+ Frases e palavras hifenizadas são divididas em componentes.
+ Palavras de maiúsculas estão em minúsculas.
+ Palavras são reduzidas para formulários de raiz, de modo a que pode ser encontrada uma correspondência, independentemente do tempo verbal.

Analisadores linguística convert uma entrada de texto para o primitivo ou formulários de raiz que são eficientes para armazenamento de informações e a recuperação. Conversão ocorre durante a indexação, quando o índice é criado e, em seguida, novamente durante a pesquisa quando o índice é de leitura. Que é mais provável que obtenha os resultados da pesquisa esperado se usar o mesmo analisador de texto para as duas operações.

O Azure Search utiliza a [analisador de Lucene padrão](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) como predefinição. É possível substituir a predefinição de forma de campo por campo. Este artigo descreve a gama de opções e oferece melhores práticas para análise personalizada. Ele também fornece as configurações de exemplo para cenários-chave.

## <a name="supported-analyzers"></a>Analisadores suportados

A lista seguinte descreve os analisadores são suportados no Azure Search.

| Categoria | Descrição |
|----------|-------------|
| [Analisador de Lucene padrão](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Predefinição. Não é necessária nenhum especificação ou configuração. Executa este analyzer para fins gerais bem para a maioria das linguagens e cenários.|
| Analisadores predefinidos | Oferecido como um produto deve ser usado como-é, na personalização limitada. <br/>Existem dois tipos: especializadas e idioma. O que os torna "predefinido" é referenciá-los por nome, sem personalização. <br/><br/>[Especializada analisadores (independente de idioma)](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) são utilizadas quando entradas de texto requerem processamento especializado ou processamento mínimo. Analisadores de idioma não predefinidos incluem **Asciifolding**, **palavra-chave**, **padrão**, **simples**, **parar**, **Espaços em branco**.<br/><br/>[Analisadores de idiomas](https://docs.microsoft.com/rest/api/searchservice/language-support) são utilizados quando precisar de suporte linguístico avançado para idiomas individuais. O Azure Search suporta 35 analisadores de idiomas do Lucene e 50 analisadores de processamento de linguagem natural da Microsoft. |
|[Analisadores personalizados](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Uma configuração definida pelo utilizador de uma combinação de elementos existentes, constituída por um atomizador (obrigatório) e filtros opcionais (char ou token).|

Pode personalizar um analisador predefinido, tais como **padrão** ou **parar**, para utilizar opções alternativas documentadas na [predefinidos de analisador de referência](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable). Apenas alguns dos analisadores predefinidos tem opções que pode definir. Como com qualquer personalização, forneça a nova configuração com um nome, como *myPatternAnalyzer* para distingui-la a partir do analisador de Lucene padrão.

## <a name="how-to-specify-analyzers"></a>Como especificar analisadores

1. (para apenas analisadores personalizados) Criar uma **analyzer** secção na definição do índice. Para obter mais informações, consulte [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) e também [analisadores personalizados > criar](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search#create-a-custom-analyzer).

2. Num [campo definição](https://docs.microsoft.com/rest/api/searchservice/create-index) no índice, definir o **analyzer** propriedade para o nome de um analisador de destino (por exemplo, `"analyzer" = "keyword"`. Valores válidos incluem o nome de um analisador predefinido, o analisador de idioma ou o analisador personalizado também definido no esquema do índice.

3. Opcionalmente, em vez de um **analisador** propriedade, pode definir diferentes analisadores de indexação e consulta utilizando o **indexAnalyzer** e **searchAnalyzer'** campo parâmetros. 

3. Adicionar um analisador para uma definição de campo, incorre numa operação de escrita no índice. Se adicionar um **analyzer** para um índice existente, tenha em atenção os seguintes passos:
 
 | Cenário | Impacto | Passos |
 |----------|--------|-------|
 | Adicionar um novo campo | Mínimo | Se o campo ainda não existe no esquema, não há nenhum revisão do campo porque o campo ainda não tiver uma presença física no seu índice. Uso [índice de atualização](https://docs.microsoft.com/rest/api/searchservice/update-index) e [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) para esta tarefa.|
 | Adicione um analisador para um campo indexado existente. | Reconstruir | O índice invertido para esse campo deve ser recriado desde o backup e o conteúdo para os campos têm de ser reindexado. <br/> <br/>Para índices em permanente desenvolvimento, [elimine](https://docs.microsoft.com/rest/api/searchservice/delete-index) e [criar](https://docs.microsoft.com/rest/api/searchservice/create-index) o índice de retirada a nova definição de campo. <br/> <br/>Para índices em produção, deve criar um novo campo para fornecer a definição revisada e começar a utilizar. Uso [índice de atualização](https://docs.microsoft.com/rest/api/searchservice/update-index) e [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) para incorporar o novo campo. Mais tarde, como parte da manutenção planeada de índice, é possível limpar o índice para remover campos obsoletos. |

## <a name="tips-and-best-practices"></a>Dicas e melhores práticas

Esta secção oferece orientação sobre como trabalhar com os analisadores.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Um analisador para leitura e escrita, a menos que tenha requisitos específicos

O Azure Search permite-lhe especificar diferentes analisadores de indexação e pesquisa via adicionais `indexAnalyzer` e `searchAnalyzer` parâmetros de campo. Se não for especificado, o analisador de conjunto com o `analyzer` propriedade é utilizada para a indexação e a pesquisa. Se `analyzer` é especificado, é utilizado o analisador de Lucene padrão de predefinição.

Regra geral é usar o mesmo analyzer para indexação e consulta, a menos que necessidades específicas determinem o contrário. Certifique-se de que testar minuciosamente. Quando o processamento de texto é diferente no tempo de indexação e pesquisa, corre o risco de erro de correspondência entre os termos de consulta e termos indexados quando a pesquisa e indexação de analisador de configurações não está alinhado.

### <a name="test-during-active-development"></a>Testar durante o desenvolvimento do Active Directory

Substituir o analisador padrão requer uma recompilação de índice. Se possível, decida sobre qual analisadores para utilizar durante o desenvolvimento do Active Directory, antes de implementar um índice em produção.

### <a name="inspect-tokenized-terms"></a>Inspecione os termos com token

Se uma pesquisa não retornar resultados esperados, o cenário mais provável é o token discrepâncias entre as entradas de termo na consulta e com token termos no índice. Se os tokens não forem iguais, correspondências não materializar. Para inspecionar a saída de tokenizer, recomendamos que utilize o [API analisar](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) como uma ferramenta de investigação. A resposta é composta por tokens, à medida que gerados por um analisador específico.

### <a name="compare-english-analyzers"></a>Compare os analisadores em inglês

O [demonstração do Search Analyzer](http://alice.unearth.ai/) é uma aplicação de demonstração de terceiros que mostra uma comparação lado a lado do analisador de Lucene padrão, o analisador de idioma inglês do Lucene e o processador de idioma natural da Microsoft. O índice é fixo; ela contém o texto de uma história popular. Para cada entrada de pesquisa é fornecer, resultados de cada analisador são apresentados nos painéis adjacentes, dando-lhe uma noção de como cada analisador processa a mesma cadeia de caracteres. 

## <a name="examples"></a>Exemplos

Os exemplos abaixo mostram as definições do analyzer para alguns cenários essenciais.

<a name="Example1"></a>
### <a name="example-1-custom-options"></a>Exemplo 1: Opções personalizadas

Este exemplo ilustra uma definição de analisador com opções personalizadas. Opções personalizadas para os filtros de char, tokenizers e filtros de token são especificadas em separado como construções com nome e, em seguida, referenciadas na definição do analyzer. Elementos predefinidos são utilizados como-é e simplesmente referenciado por nome.

Percorrendo neste exemplo:

* Analisadores são uma propriedade da classe de campo para um campo pesquisável.
* Um analisador personalizado faz parte de uma definição de índice. Ele pode ser apenas superficialmente personalizado (por exemplo, personalizar uma única opção de um filtro) ou personalizado em vários locais.
* Neste caso, o analisador personalizado é "my_analyzer", que por sua vez usa um atomizador padrão personalizado "my_standard_tokenizer" e dois filtros de token: filtro de asciifolding em minúsculas e personalizadas "my_asciifolding".
* Também define um filtro de char map_dash"personalizada" para substituir todos os traços com carateres de sublinhado antes de atomização (as padrão atomizador quebras no dash, mas não no caráter de sublinhado).

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Example2"></a>
### <a name="example-2-override-the-default-analyzer"></a>Exemplo 2: Substituir o analisador de predefinição

O analisador padrão é a predefinição. Suponha que pretende substituir a predefinição com um analisador de predefinidos diferente, como o analisador padrão. Se não forem opções personalizadas de definição, só precisa de especificá-lo pelo nome na definição do campo.

O elemento "analyzer" substitui o analisador padrão numa base de campo por campo. Não há nenhuma substituição global. Neste exemplo, `text1` utiliza o analisador padrão e `text2`, que não especifica um analisador, utiliza a predefinição.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Example3"></a>
### <a name="example-3-different-analyzers-for-indexing-and-search-operations"></a>Exemplo 3: Analisadores de diferentes para operações de indexação e pesquisa

As APIs incluem os atributos de índice adicional para especificar diferentes analisadores de indexação e pesquisa. O `searchAnalyzer` e `indexAnalyzer` atributos tem de ser especificados como um par, substituindo o único `analyzer` atributo.


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Example4"></a>
### <a name="example-4-language-analyzer"></a>Exemplo 4: Analisador de idioma

Campos que contenham as cadeias de caracteres em diferentes idiomas podem usar um analisador de idioma, enquanto outros campos mantém a predefinição (ou utilizam outro analisador predefinida ou personalizada). Se usar um analisador de idioma, tem de ser utilizado para operações de indexação e pesquisa. Campos que utilizam um analisador de idioma não é possível ter diferentes analisadores para indexação e pesquisa.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="next-steps"></a>Passos Seguintes

+ Reveja nossa explicação abrangente das [completa como funciona a pesquisa de texto no Azure Search](search-lucene-query-architecture.md). Este artigo utiliza exemplos para explicar os comportamentos que podem parecer achem à primeira vista.

+ Experimente a sintaxe de consulta adicionais do [documentos sobre pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) secção de exemplo ou a partir de [sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) no Explorador de pesquisa no portal.

+ Saiba como aplicar [analisadores lexicais de idioma específico](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Configurar os analisadores personalizados](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) para processamento mínimo ou processamento especializado em campos individuais.

+ [Compare os analisadores de padrão e em inglês](http://alice.unearth.ai/) nos painéis adjacentes neste site da web de demonstração. 

## <a name="see-also"></a>Consulte também

 [Procurar nos documentos de REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

 [Sintaxe de consulta Lucene completa](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 
 [Processar os resultados da pesquisa](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
