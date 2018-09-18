---
title: Mostrar o tráfego com o Azure Maps | Documentos da Microsoft
description: Como exibir dados de tráfego num mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6d5c721ab84c28bae9415dceeaa09fd12cc05824
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733027"
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

> [!div class="nextstepaction"]
> [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Veja os artigos seguintes para exemplos de código completo:

> [!div class="nextstepaction"]
> [Página de exemplo de código](http://aka.ms/AzureMapsSamples)

Melhore suas experiências de utilizador:

> [!div class="nextstepaction"]
> [Mapear a interação com eventos de mouse](./map-events.md)

> [!div class="nextstepaction"]
> [Criar um mapa acessível](./map-accessibility.md)
