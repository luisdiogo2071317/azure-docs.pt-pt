---
title: Mostrar tráfego com o Azure Maps | Microsoft Docs
description: Como apresentar dados de tráfego num mapa Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6ff7a0270509c244fc97bd04d8ba648fd262dc58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600124"
---
# <a name="show-traffic-on-the-map"></a>Mostrar o tráfego no mapa

Este artigo mostra como mostrar as informações de tráfego e incidentes no mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='456' scrolling='no' title='Mostrar tráfego num mapa' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Mostrar tráfego num mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](map-create.md) para obter instruções.

O segundo bloco de código utiliza [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic) função da classe mapa para compor os fluxos de tráfego e incidentes no mapa.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic)

Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 
* [Interagir com o mapa – eventos de rato](./map-events.md)
* [Criar um mapa acessível](./map-accessibility.md)

Consulte a nossa [página de código de exemplo](http://aka.ms/AzureMapsSamples) para cenários de mapeamento mais.
