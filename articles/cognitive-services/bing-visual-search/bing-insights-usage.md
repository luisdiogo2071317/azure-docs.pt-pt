---
title: Exemplos de informações do Bing | Documentos da Microsoft
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra exemplos de informações de imagem mostradas no Bing.com.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/17/2018
ms.author: scottwhi
ms.openlocfilehash: 927a02b400c7a7b250b315ad5668feb198fd590a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005694"
---
# <a name="examples-of-bing-insights-usage"></a>Exemplos de utilização de informações do Bing

Esta seção contém exemplos de como o Bing pode apresentar informações no Bing.com.

## <a name="pagesincluding-insight-example"></a>Exemplo de informações de PagesIncluding

A seguir mostra como o Bing pode exibir páginas da Web que incluem a imagem. O exemplo apresenta uma hiperligação para a primeira página da Web e permite que o utilizador expandir e recolher a lista de outras páginas da Web que incluem a imagem.

![Páginas expandidas, incluindo](./media/pages-including.PNG)


## <a name="shoppingsources-insight-example"></a>Exemplo de informações de ShoppingSources

O código a seguir mostra como o Bing pode apresentar fontes de compras para produtos verifica na imagem.

![Origens de compras](./media/shopping-sources.PNG)


## <a name="visualsearch-insight-example"></a>Exemplo de informações de VisualSearch

O seguinte mostra como o Bing pode exibir imagens visualmente similares (veja **imagens relacionadas com** no exemplo).

![Imagens visualmente similares](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Exemplo de informações de receitas

O código a seguir mostra como o Bing pode apresentar receitas para alimentar mostrado na imagem. O exemplo que informa ao usuário há receitas disponíveis.

![Receitas e páginas, incluindo](./media/recipes-pages-including.PNG)

 E fornece a ligação para as receitas quando o utilizador expande a lista.

![Páginas de receita expandido, incluindo](./media/expanded-recipes-pages-including.PNG)


## <a name="relatedsearches-insight-example"></a>Exemplo de informações de RelatedSearches

O código a seguir mostra como o Bing pode apresentar pesquisas relacionadas de imagens realizadas por terceiros. Se o usuário clicar na imagem, o utilizador é direcionado para a página de resultados de pesquisa de Bing.com/images para essa consulta relacionada.

![Pesquisas relacionadas com imagens](./media/bordered-related-searches.PNG)


## <a name="entity-insight-example"></a>Exemplo de informações da entidade

O código a seguir mostra como o Bing pode apresentar informações sobre a entidade (pessoa, local ou coisa) mostrada na imagem. Se o utilizador clica na ligação de entidade, o utilizador é direcionado para a página de resultados de pesquisa de Bing.com para a entidade.

![Entidade mostrada na imagem](./media/entity.PNG)


## <a name="displaying-other-insights-that-the-user-might-explore"></a>Exibir outras informações que o utilizador poderá explorar

A seguir mostra como o Bing pode exibir outras informações sobre a imagem que o utilizador pode explorar.

![Explore outras informações sobre a imagem](./media/apple-pie-more-tags.PNG)


## <a name="bounding-boxes-and-hot-spots"></a>Delimitadora caixas e pontos de acesso

Etiquetas predefinidas não incluem a caixa delimitadora que identifica a área de interesse na imagem que se aplica a marca. Se a caixa delimitadora não identifica a imagem inteira, utilize a caixa delimitadora para criar um ponto de acesso na imagem. O utilizador pode clicar o ponto de acesso para obter informações relacionadas com o conteúdo encontrado sob o ponto de acesso (ou o retângulo). Por exemplo, se a imagem é uma imagem de alto modo, os resultados podem conter tags (e caixas delimitadora) para acessórios, mostrados na imagem, como uma bolsa, joias, scarfs, etc. O exemplo seguinte mostra um retângulo de ponto de acesso para os mostrado na imagem de óculos de sol.

![Caixa delimitadora e de ponto de acesso](./media/click-to-search.PNG)



## <a name="next-steps"></a>Passos Seguintes

Consulte o JSON por trás desses exemplos, consulte [predefinido de insights](default-insights-tag.md) e [resposta JSON](overview.md#the-response).

Para começar rapidamente com o primeiro pedido, veja os inícios rápidos: [c#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node. js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md)





