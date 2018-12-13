---
title: Como itens de página numa página de resultados de pesquisa - Azure Search
description: Paginação no Azure Search, um serviço de pesquisa de nuvem alojada no Microsoft Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 08/29/2016
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 5f36dbb72e2518f7e3a27ef3aadec85312d751c2
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309348"
---
# <a name="how-to-page-search-results-in-azure-search"></a>Como paginar os resultados da pesquisa no Azure Search
Este artigo fornece orientações sobre como usar a API de REST do serviço de pesquisa do Azure para implementar elementos padrão de uma página de resultados de pesquisa, como contagens de totais, obtenção de documento, ordens de classificação e navegação.

Em cada caso mencionado abaixo, as opções relacionadas com a página que contribuem informações à sua página de resultados de pesquisa ou dados são especificadas através da [pesquisar no documento](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) pedidos enviados para o serviço de pesquisa do Azure. Pedidos de incluir um comando GET, caminho e os parâmetros de consulta que informam o serviço, o que está a ser requerido e como a formular a resposta.

> [!NOTE]
> Um pedido válido inclui um número de elementos, como um URL do serviço e o caminho, verbo HTTP, `api-version`e assim por diante. Para fins de brevidade, podemos cortados os exemplos para realçar apenas a sintaxe que é relevante para paginação. Consulte a [API de REST do serviço do Azure Search](https://docs.microsoft.com/rest/api/searchservice) documentação para obter detalhes sobre a sintaxe do pedido.
> 
> 

## <a name="total-hits-and-page-counts"></a>Total de resultados e contagens de página
Que mostra o número total de resultados devolvidos por uma consulta e, em seguida, retornar esses resultados em segmentos mais pequenos, é fundamental para praticamente todas as páginas de pesquisa.

![][1]

Azure Search, vai utilizar o `$count`, `$top`, e `$skip` parâmetros para esses valores de retorno. O exemplo seguinte mostra um pedido de exemplo para cliques total, retornado como `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Obtenha documentos em grupos de 15 e também mostram as visitas total, começando na primeira página:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Paginação de resultados requer `$top` e `$skip`, onde `$top` Especifica quantos itens a devolver no lote, e `$skip` Especifica quantos itens para ignorar. No exemplo a seguir, cada página mostra os itens de 15 em seguida, indicado pelos saltos incrementais no `$skip` parâmetro.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Esquema
Numa página de resultados de pesquisa, pode querer mostrar uma imagem em miniatura, um subconjunto dos campos e uma ligação para uma página de produto completo.

 ![][2]

No Azure Search, usaria `$select` e um comando de pesquisa para implementar esta experiência.

Para retornar um subconjunto dos campos para um esquema de lado a lado:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Imagens e arquivos de suporte de dados não são diretamente pesquisáveis e devem ser armazenados em outra plataforma de armazenamento, como o armazenamento de Blobs do Azure, para reduzir os custos. O índice e documentos, defina um campo que armazena o endereço de URL do conteúdo externo. Em seguida, pode utilizar o campo como uma referência da imagem. O URL para a imagem deve ser o documento.

Para obter uma página de descrição de produto para um **onClick** evento, utilize [documento de pesquisa](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) passar a chave do documento para recuperar. O tipo de dados da chave é `Edm.String`. Neste exemplo, é *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Ordenar por relevância, classificação ou preço
Ordenação orders, muitas vezes, a predefinição para relevância, mas é comum para tornar ordenação alternativa ordens prontamente disponíveis para que os clientes podem rapidamente reshuffle resultados existentes numa ordem de classificação diferente.

 ![][3]

No Azure Search, classificação se baseia o `$orderby` expressão, para todos os campos que são indexados como `"Sortable": true.`

Relevância é fortemente associada com perfis de classificação. Pode usar o padrão de classificação, que baseia-se na análise de texto e estatísticas para a ordem de classificação de todos os resultados, com pontuações mais altas acontecendo para documentos com mais fortes ou mais correspondências um termo de pesquisa.

Ordens de classificação alternativa estão normalmente associadas **onClick** eventos que uma chamada de retorno para um método que baseia-se a sequência de ordenação. Por exemplo, com este elemento de página:

 ![][4]

Deverá criar um método que aceita a opção de ordenação selecionada como entrada e retorna uma lista ordenada para os critérios associados essa opção.

 ![][5]

> [!NOTE]
> Embora o padrão de classificação seja suficiente para muitos cenários, é recomendável baseando relevância num perfil de classificação personalizado em vez disso. Um perfil de classificação personalizado dá-lhe uma forma de itens de boost que são mais úteis para o seu negócio. Ver [adicionar um perfil de classificação](https://docs.microsoft.com/rest/api/searchservice/Add-scoring-profiles-to-a-search-index) para obter mais informações. 
> 
> 

## <a name="faceted-navigation"></a>Navegação por facetas
Navegação de pesquisa é comum numa página de resultados, muitas vezes, localizada no lado ou da parte superior de uma página. No Azure Search, navegação por facetas fornece pesquisa orientada com base nos filtros predefinidos. Ver [navegação por facetas no Azure Search](search-faceted-navigation.md) para obter detalhes.

## <a name="filters-at-the-page-level"></a>Filtros ao nível da página
Se seu design de solução incluído páginas de pesquisa dedicadas para tipos específicos de conteúdo (por exemplo, um aplicativo de varejo online tem departamentos listados na parte superior da página), pode inserir uma expressão de filtro juntamente com um **onClick**evento para abrir uma página num estado pré-filtrada. 

Pode enviar um filtro com ou sem uma expressão de pesquisa. Por exemplo, o pedido seguinte irá filtrar no nome de marca, retornando apenas esses documentos que correspondam a ele.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Ver [documentos sobre pesquisa (API de pesquisa do Azure)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) para obter mais informações sobre `$filter` expressões.

## <a name="see-also"></a>Consultar Também
* [API de REST do serviço Azure Search](https://docs.microsoft.com/rest/api/searchservice)
* [Operações de índice](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
* [Operações de documento](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
* [Navegação por facetas no Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
