---
title: Mostrar o tráfego com o Azure Maps | Documentos da Microsoft
description: Como exibir dados de tráfego num mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 2499822db587dbf47dccedf6039d0fb5823c58b5
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746083"
---
# <a name="show-traffic-on-the-map"></a>Tráfego de mostrar no mapa

Este artigo mostra-lhe como mostrar informações de tráfego e incidentes no mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='456' scrolling='no' title='Mostrar o tráfego num mapa' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Mostrar tráfego num mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](map-create.md) para obter instruções.

O segundo bloco de código utiliza [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic) função da classe map para renderizar os fluxos de tráfego e incidentes no mapa.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic)

Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Interagir com o mapa – eventos de mouse](./map-events.md)
* [Criar um mapa acessível](./map-accessibility.md)

Confira nosso [página de exemplo de código](http://aka.ms/AzureMapsSamples) para obter mais cenários de mapeamento.
