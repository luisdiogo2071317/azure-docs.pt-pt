---
title: Mostrar os resultados da pesquisa com o Azure Maps | Microsoft Docs
description: Como efetuar um pedido de pesquisa com o Azure Maps, em seguida, apresentar os resultados num mapa Javascrip
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b77737b16b23ed00c8f12f84e6a8558a665a7d15
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599886"
---
# <a name="show-search-results-on-the-map"></a>Mostrar os resultados da pesquisa no mapa

Este artigo mostra como efetuar um pedido de pesquisa e mostrar os resultados da pesquisa no mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar os resultados da pesquisa num mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=519&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Mostrar pesquisa resulta num mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria e adiciona uma camada de pesquisa pins no mapa. Pode ver [adicionar um pin no mapa](./map-add-pin.md) para obter instruções.

O terceiro bloco de código envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) para [API de pesquisa do Azure Maps difusa](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

O bloco de código pela última vez analisa a resposta de entrada. Para uma resposta com êxito, recolhe as informações de latitude e longitude para cada localização devolvida. Adiciona todos os pontos de localização para o mapa como pins e ajusta dos limites do mapa para compor todos os pins.


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 

* [Azure mapeia pesquisa difusa API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 
* [Obter informações a partir de uma coordenada](./map-get-information-from-coordinate.md)
* [Mostrar as direções da para B](./map-route.md)
