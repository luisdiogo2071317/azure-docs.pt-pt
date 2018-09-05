---
title: Mostrar informações sobre uma coordenada com o Azure Maps | Documentos da Microsoft
description: Como exibir informações sobre um endereço no mapa, quando um usuário seleciona uma coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 331e687c40f21b0bf6074239969848c632682773
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43667486"
---
# <a name="get-information-from-a-coordinate"></a>Obter informações a partir de uma coordenada

Este artigo mostra-lhe como fazer uma pesquisa inversa de endereços e, após um clique do mouse, mostrar o endereço da localização clicado num pop-up.

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Obtenha informações a partir de uma coordenada (módulo de serviço)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>obter informações a partir de uma coordenada (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

A linha no segundo bloco de código cria uma instância de um cliente de serviço.

O terceiro bloco de código atualiza o estilo de cursor do mouse num ponteiro.

O quarto bloco de código cria um pop-up. Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código adiciona uma escuta de eventos de cliques do mouse. Após um clique do mouse, ele cria uma consulta de pesquisa com os ordinates conjunta do ponto clicado. Em seguida, ele utiliza o mapa [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) ponto final para consultar o endereço para os ordinates conjunta.

Para uma resposta com êxito, recolhe o endereço para a localização de cliques e define o conteúdo do popup e a posição via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe pop-up.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Pesquisa inversa de endereço](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [abrir](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [Fechar](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)

Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes:
* [Mostrar as direções da para B](./map-route.md)
* [Mostrar trânsito](./map-show-traffic.md)
