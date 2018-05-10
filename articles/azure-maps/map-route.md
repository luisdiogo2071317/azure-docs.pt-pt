---
title: Mostrar as direções com o Azure Maps | Microsoft Docs
description: Como apresentar as direções entre duas localizações num mapa Javascript
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
ms.openlocfilehash: 9007afd1bc4d2361addc2a554fab1330174e88e7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="show-directions-from-a-to-b"></a>Mostrar as direções da para B 

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
