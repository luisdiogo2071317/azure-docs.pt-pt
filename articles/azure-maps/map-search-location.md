---
title: Mostrar resultados da pesquisa com o Azure Maps | Documentos da Microsoft
description: Como efetuar um pedido de pesquisa com o Azure Maps, em seguida, exibir os resultados num mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 08/26/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 31ff57f8a933ac17c6dfaa1a1fb0cf2bab0b6557
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345276"
---
# <a name="show-search-results-on-the-map"></a>Mostrar resultados da pesquisa no mapa

Este artigo mostra como procurar a localização de interesse e mostrar os resultados da pesquisa no mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar resultados da pesquisa num mapa (módulo de serviço)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Mostrar resultados da pesquisa num mapa (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa e cria uma instância de um serviço de cliente. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código usa a pesquisa difusa [API de pesquisa difusa do Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para procurar o ponto de interesse. API de pesquisa difusa pode lidar com qualquer combinação de entradas difusas. A resposta do serviço de pesquisa difusa, em seguida, é analisada em formato GeoJSON e pins são adicionados ao mapa para mostrar os pontos de interesse no mapa. 

O último bloco de código adiciona dos limites da câmara para o mapa através do mapa [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) propriedade.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 

* [API de pesquisa difusa de mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Obtenha informações a partir de uma coordenada](./map-get-information-from-coordinate.md)
* [Mostrar as direções da para B](./map-route.md)
