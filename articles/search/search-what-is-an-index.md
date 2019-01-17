---
title: Definição de índice e conceitos - Azure Search
description: Introdução aos termos de índice e conceitos no Azure Search, incluindo structre físico de um índice invertido.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/08/2017
ms.custom: seodec2018
ms.openlocfilehash: 5a39021367c2f51125876081e9174eb372d7b9c9
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353163"
---
# <a name="indexes-and-indexing-overview-in-azure-search"></a>Índices e a descrição geral de indexação no Azure Search

No Azure Search, um *índice* é um armazenamento persistente de *documentos* e outras construções utilizado para a pesquisa em texto completo e filtrados num serviço Azure Search. Um documento é uma unidade de dados pesquisáveis no índice. Por exemplo, um retalhista de comércio eletrónico pode ter um documento para cada artigo que vende, uma empresa jornalística pode ter um documento para cada artigo, etc. Mapeamento destes conceitos para equivalentes da base de dados mais familiares: um *índice* é, conceptualmente, semelhante a uma *tabela* e os *documentos* são quase equivalentes a *linhas* numa tabela.

Quando adiciona ou carregar documentos ou submete consultas de pesquisa para o Azure Search, estão a enviar pedidos para um índice específico no seu serviço de pesquisa. O processo de adição de documentos para um índice é chamado *indexação*.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Tipos de campo e atributos num índice da Azure Search
Na definição do esquema, tem de especificar o nome, o tipo e os atributos de cada campo do índice. O tipo de campo classifica os dados armazenados nesse campo. Os atributos são definidos em campos individuais para especificar o modo de utilização do campo. As tabelas seguintes enumeram os tipos e os atributos que pode especificar.

### <a name="field-types"></a>Tipos de campo
| Tipo | Descrição |
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

### <a name="field-attributes"></a>Atributos do campo
| Atributo | Descrição |
| --- | --- |
| *Chave* |Uma cadeia que fornece o ID único de cada documento, utilizada para a procura de documentos. Todos os índices devem ter uma chave. Apenas um campo pode ser uma chave e o tipo deve ser definido para Edm.String. |
| *Recuperável* |Especifica se um campo pode ser devolvido num resultado da pesquisa. |
| *Filtrável* |Permite que o campo seja utilizado nas consultas de filtro.  |
| *Ordenável* |Permite a uma consulta ordenar os resultados da pesquisa através deste campo. |
| *Facetável* |Permite a um campo ser utilizado numa estrutura de [navegação por facetas](search-faceted-navigation.md) para filtragem do utilizador auto-direcionada. Geralmente os campos com valores repetitivos que pode utilizar para agrupar vários documentos (por exemplo, vários documentos que se inserem numa única marca ou categoria de serviço) funcionam melhor como facetas. |
| *Pesquisável* |Marca o campo como pesquisável no texto completo. |

Pode encontrar mais informações detalhadas sobre os [atributos de índice aqui](https://docs.microsoft.com/rest/api/searchservice/Create-Index) do Azure Search.

## <a name="guidance-for-defining-an-index-schema"></a>Orientações para definir um esquema de índice
Na criação do índice, reserve tempo na fase de planeamento para analisar cada decisão. É importante ter em consideração as suas necessidades comerciais e experiência do utilizador de pesquisa quando estiver a criar o seu índice, uma vez que cada campo deve receber os [atributos adequados](https://docs.microsoft.com/rest/api/searchservice/Create-Index). A alteração de um índice após a respetiva implementação implica reconstruir e recarregar os dados.

Caso os requisitos de armazenamento de dados alterem ao longo do tempo, pode aumentar ou diminuir a capacidade adicionando ou removendo partições. Para obter detalhes, consulte [Gerir o serviço de Pesquisa no Azure](search-manage.md) ou [Limites de Serviço](search-limits-quotas-capacity.md).

