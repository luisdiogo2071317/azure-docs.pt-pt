---
title: Criar uma definição de índice e conceitos - Azure Search
description: Introdução aos termos de índice e conceitos no Azure Search, incluindo partes de componente e estrutura física.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/13/2019
ms.custom: seodec2018
ms.openlocfilehash: 9cd43172fc57443cc89f238e1d4ffaae45301936
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330567"
---
# <a name="create-a-basic-index-in-azure-search"></a>Criar um índice básico no Azure Search

No Azure Search, um *índice* é um armazenamento persistente de *documentos* e outras construções utilizado para a pesquisa em texto completo e filtrados num serviço Azure Search. Conceitualmente, um documento é uma única unidade de dados pesquisáveis no índice. Por exemplo, um retalhista de comércio eletrónico pode ter um documento para cada artigo que vende, uma empresa jornalística pode ter um documento para cada artigo, etc. Mapeamento destes conceitos para equivalentes da base de dados mais familiares: um *índice* é, conceptualmente, semelhante a uma *tabela* e os *documentos* são quase equivalentes a *linhas* numa tabela.

Quando adiciona ou carregar um índice, o Azure Search cria estruturas físicas com base no esquema que fornecer. Por exemplo, se um campo no seu índice é marcado como pesquisável, é criado um índice invertido para esse campo. Mais tarde, quando adiciona ou carregar documentos ou submete consultas de pesquisa para o Azure Search, enviar pedidos para um índice específico no seu serviço de pesquisa. A carregar campos com valores de documento é chamada *indexação* ou uma ingestão de dados.

Pode criar um índice no portal, [REST API](search-create-index-rest-api.md), ou [SDK do .NET](search-create-index-dotnet.md).

## <a name="recommended-workflow"></a>Fluxo de trabalho recomendado

Como estruturas físicas são criadas durante a indexação, será necessário para [remover e recriar índices](search-howto-reindex.md) sempre que fizer alterações materiais a uma definição de campo existente. Isso significa que durante o desenvolvimento, deve planejar a recompilações frequentes. Pode optar por trabalhar com um subconjunto dos seus dados para tornar recria ir mais rápido. 

Código em vez de indexação portal também é recomendado. Se contar com o portal para a definição de índice, terá de preencher a definição do índice em cada reconstrução. Como alternativa, usando uma ferramenta como [Postman e a API REST](search-fiddler.md) são úteis para quando os projetos de desenvolvimento ainda se encontram em fases iniciais de teste de prova de conceito. Pode fazer as alterações incrementais para uma definição de índice no corpo do pedido, enviar o pedido para o seu serviço para recriar um índice com um esquema atualizado.

## <a name="components-of-an-index"></a>Componentes de um índice

Schematically, um índice da Azure Search é composta pelos seguintes elementos. 

O [ *coleção de campos* ](#fields-collection) é, normalmente, a maior parte de um índice, onde cada campo com o nome, escreveu e atribuída com comportamentos permitidos que determinam como são utilizadas. Outros elementos incluem [sugestores](#suggesters), [perfis de classificação](#scoring-profiles), [analisadores](#analyzers) com componentes para oferecer suporte a personalização, e [CORS](#cors) opções.

```json
{  
  "name": (optional on PUT; required on POST) "name_of_index",  
  "fields": [  
    {  
      "name": "name_of_field",  
      "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",  
      "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),  
      "filterable": true (default) | false,  
      "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),  
      "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),  
      "key": true | false (default, only Edm.String fields can be keys),  
      "retrievable": true (default) | false,  
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }  
  ],  
  "suggesters": [  
    {  
      "name": "name of suggester",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": ["field1", "field2", ...]  
    }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "name of scoring profile",  
      "text": (optional, only applies to searchable fields) {  
        "weights": {  
          "searchable_field_name": relative_weight_value (positive #'s),  
          ...  
        }  
      },  
      "functions": (optional) [  
        {  
          "type": "magnitude | freshness | distance | tag",  
          "boost": # (positive number used as multiplier for raw score != 1),  
          "fieldName": "...",  
          "interpolation": "constant | linear (default) | quadratic | logarithmic",  
          "magnitude": {  
            "boostingRangeStart": #,  
            "boostingRangeEnd": #,  
            "constantBoostBeyondRange": true | false (default)  
          },  
          "freshness": {  
            "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)  
          },  
          "distance": {  
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)  
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)  
          },  
          "tag": {  
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)  
          }  
        }  
      ],  
      "functionAggregation": (optional, applies only when functions are specified)   
        "sum (default) | average | minimum | maximum | firstMatching"  
    }  
  ],  
  "analyzers":(optional)[ ... ],
  "charFilters":(optional)[ ... ],
  "tokenizers":(optional)[ ... ],
  "tokenFilters":(optional)[ ... ],
  "defaultScoringProfile": (optional) "...",  
  "corsOptions": (optional) {  
    "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],  
    "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)  
  }  
}
```

<a name="fields-collection"></a>

## <a name="fields-collection-and-field-attributes"></a>Atributos de campo e de coleção de campos

Na definição do esquema, tem de especificar o nome, o tipo e os atributos de cada campo do índice. O tipo de campo classifica os dados armazenados nesse campo. Os atributos são definidos em campos individuais para especificar o modo de utilização do campo. As tabelas seguintes enumeram os tipos e os atributos que pode especificar.

### <a name="data-types"></a>Tipos de dados
| Type | Descrição |
| --- | --- |
| *Edm.String* |Texto que pode, opcionalmente, ser atomizado para pesquisa em texto completo (separação de palavras, lematização, etc.). |
| *Collection(Edm.String)* |Uma lista de cadeias que pode, opcionalmente, ser atomizada para pesquisa em texto completo. Não existe um limite superior teórico do número de itens numa coleção, contudo, o limite superior de 16 MB de tamanho de payload aplica-se às coleções. |
| *Edm.Boolean* |Contém valores verdadeiro/falso. |
| *Edm.Int32* |Valores inteiros de 32 bits. |
| *Edm.Int64* |Valores inteiros de 64 bits. |
| *Edm.Double* |Dados numéricos de dupla precisão. |
| *Edm.DateTimeOffset* |Valores de data/hora representados no formato OData V4 (por ex. `yyyy-MM-ddTHH:mm:ss.fffZ` ou `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Um ponto que representa uma localização geográfica no mundo. |

Pode encontrar mais informações detalhadas sobre os [tipos de dados suportados aqui](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) do Azure Search.

### <a name="index-attributes"></a>Atributos de índice
| Atributo | Descrição |
| --- | --- |
| *Chave* |Uma cadeia que fornece o ID único de cada documento, utilizada para a procura de documentos. Todos os índices devem ter uma chave. Apenas um campo pode ser uma chave e o tipo deve ser definido para Edm.String. |
| *Recuperável* |Especifica se um campo pode ser devolvido num resultado da pesquisa. |
| *Filtrável* |Permite que o campo seja utilizado nas consultas de filtro.  |
| *Ordenável* |Permite a uma consulta ordenar os resultados da pesquisa através deste campo. |
| *Facetável* |Permite a um campo ser utilizado numa estrutura de [navegação por facetas](search-faceted-navigation.md) para filtragem do utilizador auto-direcionada. Geralmente os campos com valores repetitivos que pode utilizar para agrupar vários documentos (por exemplo, vários documentos que se inserem numa única marca ou categoria de serviço) funcionam melhor como facetas. |
| *Pesquisável* |Marca o campo como pesquisável no texto completo. |

Pode encontrar mais informações detalhadas sobre os [atributos de índice aqui](https://docs.microsoft.com/rest/api/searchservice/Create-Index) do Azure Search.

## <a name="storage-implications"></a>Implicações de armazenamento

Os atributos que selecionar tem um impacto no armazenamento. Captura de ecrã seguinte é uma ilustração de padrões de armazenamento de índice resultante de várias combinações de atributos. O índice baseia-se no [exemplo de realestate incorporada](search-get-started-portal.md) fonte de dados, o que permite-lhe indexar e consultas no portal.

Filtrar e ordenar a consulta de operações em correspondências exatas para que os documentos são armazenados intacto. Campos pesquisáveis ative a pesquisa difusa e de texto completo. Invertida índices são criados para campos pesquisáveis e preenchidos com os termos com token. Não marcar um campo como um campo recuperável tem nenhum impacto considerável no tamanho do índice.

![Tamanho com base na seleção de atributos de índice](./media/search-what-is-an-index/realestate-index-size.png "tamanho com base na seleção de atributos de índice")

Vários dessas combinações são artificial, útil para que esclarecem de que um ponto, mas não iria resultar num índice viável. Na prática, nunca seria adicionar cada campo único para um sugestor ou criar um índice pesquisável, mas não recuperável.

Arquitetura de armazenamento é considerada um detalhe de implementação do Azure Search e pode ser alteradas sem aviso prévio. Não é garantido que o comportamento atual serão mantidas no futuro.

## <a name="suggesters"></a>Sugestões
Um sugestor é uma seção do esquema que define os campos num índice são usados para oferecer suporte a consultas de conclusão automática ou antecipada em pesquisas. Normalmente, cadeias de procura parciais são enviadas para as sugestões (API do REST de serviço de pesquisa do Azure) enquanto o usuário está digitando uma consulta de pesquisa e a API devolve um conjunto de expressões sugeridas. 

Um sugestor que definir no índice determina quais os campos são utilizados para criar os termos de pesquisa antecipada. Para obter mais informações, consulte [adicionar sugestores](index-add-suggesters.md) para obter detalhes de configuração.

## <a name="scoring-profiles"></a>Perfis de classificação

Um perfil de classificação é uma seção do esquema que define a classificação comportamentos personalizados que permitem-lhe influenciar os itens apareçam mais em cima nos resultados da pesquisa. Perfis de classificação são constituídos por peso de campo e funções. Para usá-los, especifique um perfil de por nome na seqüência de consulta.

Um padrão de perfil de classificação funciona em segundo plano para computar uma pontuação de pesquisa para todos os itens num conjunto de resultados. Pode usar interno, sem nome de perfil de classificação. Em alternativa, defina defaultScoringProfile para utilizar um perfil personalizado como padrão, invocado sempre que um perfil personalizado não é especificado na cadeia de consulta.

Para obter mais informações, consulte [adicionar perfis de classificação](index-add-scoring-profiles.md).

## <a name="analyzers"></a>Analisadores

O elemento de analisadores define o nome do analisador de idioma a utilizar para o campo. Para o conjunto permitido de valores, consulte [analisadores de idiomas no Azure Search](index-add-language-analyzers.md). Esta opção pode ser utilizada apenas com campos pesquisáveis e não pode ser definido em conjunto com um **searchAnalyzer** ou **indexAnalyzer**. Depois do analyzer for escolhido, não pode ser alterado para o campo.

## <a name="cors"></a>CORS

JavaScript do lado do cliente não é possível chamar qualquer API por predefinição, uma vez que o navegador irá impedir que todos os pedidos de várias origens. Para permitir consultas de várias origens para o seu índice, ative CORS (partilha de recursos de várias origens), definindo a **corsOptions** atributo. Por motivos de segurança, apenas a consulta as APIs suportam o CORS. 

As seguintes opções podem ser definidas para CORS:

+ **allowedOrigins** (required): Esta é uma lista de origens que será concedido acesso ao seu índice. Isso significa que qualquer código JavaScript servido a partir destas origens terá permissão para consultar o índice (assumindo que fornece a chave de api correta). Cada origem costuma ter o formato `protocol://<fully-qualified-domain-name>:<port>` Embora `<port>` é frequentemente omitido. Ver [partilha (Wikipédia) de recursos de várias origens](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) para obter mais detalhes.

  Se pretender permitir o acesso a todas as origens, incluem `*` como um único item do **allowedOrigins** matriz. *Isso não é recomendável prática para os serviços de pesquisa de produção* mas, muitas vezes, é útil para desenvolvimento e depuração.

+ **maxAgeInSeconds** (opcional): Browsers utilizam este valor para determinar a duração (em segundos) para respostas de simulação de CORS do cache. Tem de ser um número inteiro não negativo. Este valor é maior, será o melhor desempenho, mas mais tempo demorará para alterações de política CORS entrem em vigor. Se não for definida, será utilizada uma duração predefinida de 5 minutos.

## <a name="next-steps"></a>Passos Seguintes

Com uma compreensão da composição do índice, pode continuar no portal para criar seu primeiro índice.

> [!div class="nextstepaction"]
> [Adicionar um índice (portal)](search-create-index-portal.md)