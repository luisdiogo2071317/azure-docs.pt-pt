---
title: Mostrar as direções com o Azure Maps | Microsoft Docs
description: Como apresentar as direções entre duas localizações num mapa Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 5e9ab73ddc16517e17894cddd9bc102f3941f00c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "35759800"
---
# <a name="show-directions-from-a-to-b"></a>Mostrar as direções de A para B 

Este artigo mostra como efetuar um pedido de rota e mostrar a rota no mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar as direções da para B num mapa' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>mostrar as direções da para B num mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria e adiciona pins no mapa para representar o início e de ponto final da rota. Pode ver [adicionar um pin no mapa](map-add-pin.md) para obter instruções.

O terceiro bloco de código utiliza [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) função da classe mapa para definir a caixa delimitadora do mapa com base no início e de ponto final da rota.

O bloco de código quarto envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) para [API de rota do Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

O bloco de código pela última vez analisa a resposta de entrada. Para uma resposta com êxito, recolhe as informações de latitude e longitude de cada waypoint. Cria uma matriz de linhas estabelecendo ligação cada waypoint à respetiva waypoint subsequente. Adiciona todos os estas linhas para o mapa para compor a rota. Pode ver [adicionar uma linha no mapa](./map-add-shape.md#addALine) para obter instruções.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 

* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)

Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 
* [Mostrar o tráfego no mapa](./map-show-traffic.md)
* [Interagir com o mapa - eventos de rato](./map-events.md)
