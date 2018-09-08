---
title: Mostrar informações sobre uma coordenada com o Azure Maps | Documentos da Microsoft
description: Como exibir informações sobre um endereço no mapa, quando um usuário seleciona uma coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: d1baa4adc555e65c4a25928d19f201dba6109142
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157689"
---
# <a name="get-information-from-a-coordinate"></a>Obter informações a partir de uma coordenada

Este artigo mostra-lhe como fazer uma pesquisa inversa de endereços e, após um clique do mouse, mostrar o endereço da localização clicado num pop-up.

Existem duas formas de fazer uma pesquisa inversa de endereço, uma é através da consulta a [API de pesquisa de endereço inversa do Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) através de um serviço de módulo e o outro está fazendo uma [XMLHttpRequest](https://xhr.spec.whatwg.org/) para a API para consultar o endereço. Vamos discutir, ambos abaixo.

## <a name="use-the-service-module-to-make-a-reverse-address-search"></a>Utilizar o módulo de serviço para fazer uma pesquisa inversa de endereço

### <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Obtenha informações a partir de uma coordenada (módulo de serviço)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>obter informações a partir de uma coordenada (módulo de serviço)</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

A linha no segundo bloco de código cria uma instância de um cliente de serviço.

O terceiro bloco de código atualiza o estilo de cursor do mouse num ponteiro.

O quarto bloco de código cria um pop-up. Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código adiciona uma escuta de eventos de cliques do mouse. Após um clique do mouse, ele cria uma consulta de pesquisa com os ordinates conjunta do ponto clicado. Em seguida, ele utiliza o mapa [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) ponto final para consultar o endereço para os ordinates conjunta.

Para uma resposta com êxito, recolhe o endereço para a localização de cliques e define o conteúdo do popup e a posição via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe pop-up.

## <a name="use-xmlhttprequest-to-make-a-reverse-address-search"></a>Usar XMLHTTPRequest para fazer uma pesquisa inversa de endereço

### <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Obter informações a partir de uma coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>obter informações a partir de uma coordenada</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código atualiza o estilo de cursor do mouse num ponteiro.

O terceiro bloco de código cria um pop-up. Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código adiciona uma escuta de eventos de cliques do mouse. Após um clique do mouse, envia uma [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de pesquisa de endereço de inverter mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Para uma resposta com êxito, recolhe o endereço para a localização de cliques e define o conteúdo do popup e a posição via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe pop-up

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
