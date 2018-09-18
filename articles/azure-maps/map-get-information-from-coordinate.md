---
title: Mostrar informações sobre uma coordenada com o Azure Maps | Documentos da Microsoft
description: Como exibir informações sobre um endereço no mapa, quando um usuário seleciona uma coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 09/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: bf44437f4d0b60a5d56c2be29418b7132346da2e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45732299"
---
# <a name="get-information-from-a-coordinate"></a>Obter informações a partir de uma coordenada

Este artigo mostra como fazer uma pesquisa inversa de endereços que mostra o endereço de uma localização de pop-up clicado.

Existem duas formas de fazer uma pesquisa inversa de endereço. Uma delas é a consulta a [API de pesquisa de endereço de inverter mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) por meio de um módulo de serviço. A outra forma consiste em efetuar uma [XMLHttpRequest](https://xhr.spec.whatwg.org/) para a API para localizar um endereço. Ambas as formas são pesquisadas abaixo.

## <a name="make-a-reverse-search-request-via-service-module"></a>Fazer um pedido de pesquisa inversa através do módulo de serviço

<iframe height='500' scrolling='no' title='Obtenha informações a partir de uma coordenada (módulo de serviço)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>obter informações a partir de uma coordenada (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

A linha no segundo bloco de código cria uma instância de um cliente de serviço.

O terceiro bloco de código atualiza o estilo de cursor do mouse num ponteiro.

O quarto bloco de código cria um pop-up, utilizando [abra](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código [adiciona um serviço de escuta de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) para cliques do mouse. Após um clique do mouse, ele cria uma consulta de pesquisa com os ordinates conjunta do ponto clicado. Em seguida, ele utiliza o mapa [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) ponto final para consultar o endereço para os ordinates conjunta.

Para uma resposta com êxito, recolhe o endereço para a localização de cliques e define o conteúdo do popup e a posição via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe pop-up.

## <a name="make-a-reverse-search-request-via-xmlhttprequest"></a>Fazer um pedido de pesquisa inversa via XMLHttpRequest

<iframe height='500' scrolling='no' title='Obter informações a partir de uma coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>obter informações a partir de uma coordenada</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código atualiza o estilo de cursor do mouse num ponteiro.

O terceiro bloco de código cria um pop-up, utilizando [abra](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código adiciona uma escuta de eventos de cliques do mouse. Após um clique do mouse, envia uma [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de pesquisa de endereço de inverter mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Para uma resposta com êxito, recolhe o endereço para a localização de cliques e define o conteúdo do popup e a posição via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe pop-up

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Veja os artigos seguintes para obter exemplos de código completo:

> [!div class="nextstepaction"]
> [Mostrar as direções da para B](./map-route.md)

> [!div class="nextstepaction"]
> [Mostrar trânsito](./map-show-traffic.md)
