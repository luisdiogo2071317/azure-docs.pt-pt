---
title: Mostrar as direções com o Azure Maps | Documentos da Microsoft
description: Como exibir as direções entre duas localizações num mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: ed522779f5a86e38ee12a246cea9ac85d0379f9e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729065"
---
# <a name="show-directions-from-a-to-b"></a>Mostrar as direções de A para B

Este artigo mostra-lhe como fazer um pedido de rota e mostrar a rota no mapa.

Existem duas formas de fazê-lo. A primeira maneira é a consulta a [API do Azure Maps rota](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) por meio de um módulo de serviço. A segunda maneira é fazer uma [XMLHttpRequest](https://xhr.spec.whatwg.org/) para a API. Ambas as formas são abordadas abaixo.

## <a name="query-the-route-via-service-module"></a>Consulta a rota através do módulo de serviço

<iframe height='500' scrolling='no' title='Mostrar as direções da para B num mapa (módulo de serviço)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/RBZbep/'>mostrar as direções da para B num mapa (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

A linha no segundo bloco de código cria uma instância de um cliente de serviço.

O terceiro bloco de código inicializa a [camada de cadeia de caracteres de linha](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) no mapa.

O quarto bloco de código cria e adiciona pins no mapa para representar o início e de ponto final da rota. Pode ver [adicionar um pin no mapa](map-add-pin.md) para obter instruções sobre como utilizar [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

O próximo bloco de código utiliza [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) função da classe map para definir a caixa delimitadora do mapa com base no início e de ponto final da rota.

O sexto bloco de código constrói uma consulta de rota.

O último bloco de código consulta o serviço de encaminhamento de mapas do Azure através da [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) método para obter uma rota entre o ponto de início e de destino. A resposta, em seguida, é analisada para utilizar o formato GeoJSON a [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) método. Ele adiciona todas essas linhas para o mapa para processar a rota. Pode ver [adicionar uma linha ao mapa](./map-add-shape.md#addALine) para obter mais informações.

## <a name="query-the-route-via-xmlhttprequest"></a>Consulta a rota via XMLHttpRequest

<iframe height='500' scrolling='no' title='Mostrar as direções da para B num mapa' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>mostrar as direções da para B num mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria e adiciona pins no mapa para representar o início e de ponto final da rota. Pode ver [adicionar um pin no mapa](map-add-pin.md) para obter instruções sobre como utilizar [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

O terceiro bloco de código utiliza [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) função da classe Map para definir a caixa delimitadora do mapa com base no início e de ponto final da rota.

O quarto bloco de código envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de rota do Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

O último bloco de código analisa a resposta recebida. Para uma resposta com êxito, recolhe as informações de latitude e longitude para cada o marco. Ele cria uma matriz de linhas ao ligar o cada marco para seu o marco subsequente. Ele adiciona todas essas linhas para o mapa para processar a rota. Pode ver [adicionar uma linha no mapa](./map-add-shape.md#addALine) para obter instruções.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Veja os artigos seguintes para obter exemplos de código completo:

> [!div class="nextstepaction"]
> [Tráfego de mostrar no mapa](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Interagir com o mapa - eventos de mouse](./map-events.md)
