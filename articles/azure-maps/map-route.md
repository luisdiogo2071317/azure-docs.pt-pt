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
ms.openlocfilehash: 37323bacf47613c0faf7769701808ecef2645115
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157958"
---
# <a name="show-directions-from-a-to-b"></a>Mostrar as direções de A para B 

Este artigo mostra-lhe como fazer um pedido de rota e mostrar a rota no mapa.

Existem duas formas de fazer isso, é uma forma por consultando o [API do Azure Maps rota](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) através de um serviço de módulo e o outro está fazendo um [XMLHttpRequest](https://xhr.spec.whatwg.org/) para a API. Ambos são abordadas abaixo.

## <a name="use-service-module-to-query-for-a-route"></a>Utilizar o módulo de serviço para a consulta para uma rota

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar as direções da para B num mapa (módulo de serviço)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/RBZbep/'>mostrar as direções da para B num mapa (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

A linha no segundo bloco de código cria uma instância de um cliente de serviço.

O terceiro bloco de código inicializa a [camada de cadeia de caracteres de linha](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) no mapa.

O quarto bloco de código cria e adiciona pins no mapa para representar o início e de ponto final da rota. Pode ver [adicionar um pin no mapa](map-add-pin.md) para obter instruções.

O próximo bloco de código utiliza [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) função da classe map para definir a caixa delimitadora do mapa com base no início e de ponto final da rota.

O sexto bloco de código constrói uma consulta de rota.

O último bloco de código consulta o serviço de encaminhamento de mapas do Azure através da [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) método para obter uma rota entre o ponto de início e de destino. A resposta, em seguida, é analisada para utilizar o formato GeoJSON a [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) método. Ele adiciona todas essas linhas para o mapa para processar a rota. Pode ver [adicionar uma linha no mapa](./map-add-shape.md#addALine) para obter mais informações.

## <a name="use-xmlhttprequest-to-query-for-a-route"></a>Utilizar o XMLHTTPRequest para consultar uma rota

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar as direções da para B num mapa' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>mostrar as direções da para B num mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria e adiciona pins no mapa para representar o início e de ponto final da rota. Pode ver [adicionar um pin no mapa](map-add-pin.md) para obter instruções.

O terceiro bloco de código utiliza [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) função da classe map para definir a caixa delimitadora do mapa com base no início e de ponto final da rota.

O quarto bloco de código envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de rota do Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

O último bloco de código analisa a resposta recebida. Para uma resposta com êxito, recolhe as informações de latitude e longitude para cada o marco. Ele cria uma matriz de linhas ao ligar o cada marco para seu o marco subsequente. Ele adiciona todas essas linhas para o mapa para processar a rota. Pode ver [adicionar uma linha no mapa](./map-add-shape.md#addALine) para obter instruções.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 

* [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)
* [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes)
* [Camada de cadeia de caracteres de linha](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Tráfego de mostrar no mapa](./map-show-traffic.md)
* [Interagir com o mapa - eventos de mouse](./map-events.md)
