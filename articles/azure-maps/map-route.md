---
title: Mostrar as direções com o Azure Maps | Documentos da Microsoft
description: Como exibir as direções entre duas localizações num mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 52462c1c5a2a1a9698a2b51708e63b1bb1664f6e
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745541"
---
# <a name="show-directions-from-a-to-b"></a>Mostrar as direções de A para B 

Este artigo mostra-lhe como fazer um pedido de rota e mostrar a rota no mapa. 

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

* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Tráfego de mostrar no mapa](./map-show-traffic.md)
* [Interagir com o mapa - eventos de mouse](./map-events.md)
