---
title: Exemplos de informações do Bing | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra exemplos de informações de imagem apresentadas na Bing.com.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/17/2018
ms.author: scottwhi
ms.openlocfilehash: 102bd0e916491738d74956c209829008d779bcbf
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354128"
---
# <a name="examples-of-bing-insights-usage"></a>Exemplos de utilização de informações do Bing

Esta secção contém exemplos de como o Bing pode apresentar o insights na Bing.com.

## <a name="pagesincluding-insight-example"></a>Exemplo de conhecimentos aprofundados PagesIncluding

O seguinte mostra como o Bing pode apresentar páginas Web que inclui a imagem. O exemplo mostra uma ligação à primeira página Web e permite ao utilizador expandir e fechar a lista de outras páginas Web que inclui a imagem.

![Páginas expandidas, incluindo](./media/pages-including.PNG)


## <a name="shoppingsources-insight-example"></a>Exemplo de conhecimentos aprofundados ShoppingSources

O seguinte mostra como o Bing pode apresentar compras origens para produtos vistos na imagem.

![Compras origens](./media/shopping-sources.PNG)


## <a name="visualsearch-insight-example"></a>Exemplo de conhecimentos aprofundados VisualSearch

O seguinte procedimento mostra como o Bing poderá apresentar imagens visualmente semelhantes (consulte **relacionadas com imagens** no exemplo).

![Imagens visualmente semelhantes](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Exemplo de informações de receitas

O seguinte mostra como o Bing pode apresentar receitas para prato mostrado na imagem. O exemplo permite que o utilizador sabe existem receitas disponíveis.

![Receitas e páginas, incluindo](./media/recipes-pages-including.PNG)

 E fornece a ligação para as receitas quando o utilizador expande a lista.

![Páginas de receitas expandido, incluindo](./media/expanded-recipes-pages-including.PNG)


## <a name="relatedsearches-insight-example"></a>Exemplo de conhecimentos aprofundados RelatedSearches

O seguinte mostra como o Bing pode apresentar relacionadas pesquisas de imagens efetuadas por outras pessoas. Se o utilizador clica a imagem, o utilizador é direcionado para a página de resultados de pesquisa Bing.com/images para essa consulta relacionada.

![Procura relacionada imagens](./media/bordered-related-searches.PNG)


## <a name="entity-insight-example"></a>Exemplo de conhecimentos aprofundados de entidade

O seguinte mostra como o Bing poderá apresentar informações sobre a entidade (pessoa, local ou coisa) mostrada na imagem. Se o utilizador clica na ligação de entidade, o utilizador é direcionado para a página de resultados de pesquisa Bing.com para a entidade.

![Entidade mostrada na imagem](./media/entity.PNG)


## <a name="displaying-other-insights-that-the-user-might-explore"></a>Apresentar outras informações que o utilizador pode explorar

O seguinte mostra como o Bing pode apresentar outras informações sobre a imagem que o utilizador pode explorar.

![Explorar outras informações sobre a imagem](./media/apple-pie-more-tags.PNG)


## <a name="bounding-boxes-and-hot-spots"></a>Delimitador caixas e oportunidades de frequente

Etiquetas predefinidas sem incluem a caixa delimitadora que identifica a área de interesse na imagem que se aplica a etiqueta. Se a caixa delimitadora não identifica a imagem completa, utilize a caixa delimitadora para criar um ponto ativo na imagem. O utilizador pode clicar o local de frequente para obter informações relacionadas com o conteúdo localizado em ponto ativo (ou retângulo). Por exemplo, se a imagem é uma imagem de forma elevado, os resultados podem conter etiquetas (e caixas delimitador) para Acessórios mostrados na imagem, tais como um purse, jewelry, scarfs, etc. O exemplo seguinte mostra um retângulo de ponto ativo para sunglasses mostrado na imagem.

![Delimitador caixa e ponto ativo](./media/click-to-search.PNG)



## <a name="next-steps"></a>Passos Seguintes

Para verificar o JSON atrás estes exemplos, consulte o artigo [predefinido insights](default-insights-tag.md) e [resposta JSON](overview.md#the-response).

Para começar a trabalhar rapidamente com o seu primeiro pedido, consulte os inícios rápidos: [c#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md)





