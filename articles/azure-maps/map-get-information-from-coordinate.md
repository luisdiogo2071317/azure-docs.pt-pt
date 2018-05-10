---
title: Apresentam informações sobre uma coordenada com o Azure Maps | Microsoft Docs
description: Como visualizar informações sobre um endereço no mapa quando um utilizador seleccionar uma coordenada
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: bb8644724cc872a0a8bc331e76251218492fd93d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="get-information-from-a-coordinate"></a>Obter informações a partir de uma coordenada

Este artigo mostra como efetuar uma pesquisa inversa de endereços e, após um clique do rato apresentar o endereço da localização clicked um pop-up. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Obter informações a partir de uma coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>obter informações a partir de uma coordenada</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código atualiza o estilo de cursor do rato para um ponteiro.

O terceiro bloco de código cria um pop-up. Pode ver [adicionar um pop-up no mapa](./map-add-popup.md) para obter instruções.

O último bloco de código adiciona um serviço de escuta de eventos para cliques do rato. Após um clique do rato, envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) para [Maps inverso endereço API da Azure Search](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Para uma resposta com êxito, recolhe o endereço para a localização clicked e define o conteúdo de pop-up e posição através de [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe de pop-up

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 
* [Pesquisa inversa do endereço](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [Abrir](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Fechar](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)
