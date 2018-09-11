---
title: Mostrar resultados da pesquisa com o Azure Maps | Documentos da Microsoft
description: Como efetuar um pedido de pesquisa com o Azure Maps, em seguida, exibir os resultados num mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b324d0a68fde8f47072a087330f2e40a99378984
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299479"
---
# <a name="show-search-results-on-the-map"></a>Mostrar resultados da pesquisa no mapa

Este artigo mostra como procurar a localização de interesse e mostrar os resultados da pesquisa no mapa. 

Existem duas formas de procurar uma localização de interesse, uma delas é através da utilização de um módulo de serviço para fazer um pedido de pesquisa e a outra é fazendo uma solicitação de pesquisa por meio de um [XMLHttpRequest](https://xhr.spec.whatwg.org/) para [APIdepesquisadifusademapasdoAzure](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Vamos discutir, ambos abaixo.

## <a name="making-a-search-request-via-service-module"></a>Fazer uma solicitação de pesquisa através do módulo de serviço

### <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar resultados da pesquisa num mapa (módulo de serviço)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Mostrar resultados da pesquisa num mapa (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa e cria uma instância de um serviço de cliente. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código usa a pesquisa difusa [API de pesquisa difusa do Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para procurar o ponto de interesse. API de pesquisa difusa pode lidar com qualquer combinação de entradas difusas. A resposta do serviço de pesquisa difusa, em seguida, é analisada para utilizar o formato GeoJSON a [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) método. Os pins, em seguida, são adicionados ao mapa para mostrar os pontos de interesse no mapa.

O último bloco de código ajusta os limites de câmera para o mapa através do mapa [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) propriedade.


##  <a name="making-a-search-request-via-xmlhttprequest"></a>Fazer uma solicitação de pesquisa via XMLHttpRequest

### <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar resultados da pesquisa num mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show pesquisa resulta num mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código adiciona uma camada de resultados de pesquisa ao mapa. A camada de resultados da pesquisa serão apresentados os resultados da pesquisa como fixa no mapa.

O terceiro bloco de código envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de pesquisa do Azure Maps difusa](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para procurar o ponto de interesse. API de pesquisa difusa pode lidar com qualquer combinação de entradas difusas.

O último bloco de código analisa a resposta e ajusta o ajusta os limites de câmera para o mapa através do mapa [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) para renderizar os pins de resultado.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 

* [API de pesquisa difusa de mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Obtenha informações a partir de uma coordenada](./map-get-information-from-coordinate.md)
* [Mostrar as direções da para B](./map-route.md)
