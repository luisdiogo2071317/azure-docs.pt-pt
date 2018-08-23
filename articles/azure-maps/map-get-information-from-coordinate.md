---
title: Mostrar informações sobre uma coordenada com o Azure Maps | Documentos da Microsoft
description: Como exibir informações sobre um endereço no mapa, quando um usuário seleciona uma coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b640346b0d6f490457e1e82a65c0d3f373d658d3
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42057614"
---
# <a name="get-information-from-a-coordinate"></a>Obter informações a partir de uma coordenada

Este artigo mostra-lhe como fazer uma pesquisa inversa de endereços e, após um clique do mouse, mostrar o endereço da localização clicado num pop-up. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Obter informações a partir de uma coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>obter informações a partir de uma coordenada</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código atualiza o estilo de cursor do mouse num ponteiro.

O terceiro bloco de código cria um pop-up. Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código adiciona uma escuta de eventos de cliques do mouse. Após um clique do mouse, envia uma [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de pesquisa de endereço de inverter mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Para uma resposta com êxito, recolhe o endereço para a localização de cliques e define o conteúdo do popup e a posição via [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe pop-up

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Pesquisa inversa de endereço](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [abrir](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Fechar](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Mostrar as direções da para B](./map-route.md)
* [Mostrar trânsito](./map-show-traffic.md)
