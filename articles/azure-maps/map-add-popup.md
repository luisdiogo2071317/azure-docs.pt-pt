---
title: Adicionar um pop-up com o Azure Maps | Microsoft Docs
description: Como adicionar um pop-up com o mapa de Javascript
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
ms.openlocfilehash: 7425081597bfa9379594597277555ee30809c4e6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="add-a-popup-to-the-map"></a>Adicionar um pop-up para o mapa

Este artigo mostra como adicionar um pop-up para um mapa.  

## <a name="understand-the-code"></a>Compreender o código

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Adicionar um pop-up para um mapa' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>adicionar um pop-up para um mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria um pin e adicione-o para o mapa. Pode ver [adicionar um pin para o mapa](./map-add-pin.md) para obter instruções.

O terceiro bloco de código cria o conteúdo que será apresentado dentro de um pop-up. O conteúdo de pop-up é o elemento HTML. 

O quarto bloco de código cria um [objeto de pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) através de `new atlas.Popup()`. Propriedades de pop-up, tais como o conteúdo e posição fazem parte do [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popupoptions?view=azure-iot-typescript-latest). PopupOptions podem ser definidas no construtor de pop-up ou através de [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) função da classe de pop-up.

O última bloco de código utiliza [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener) função da classe mapa para escutar eventos mouseover no pins e utiliza [abrir](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open) função da classe de pop-up para abrir o pop-up, se o evento ocorre.


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 

* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [Abrir](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Fechar](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)
