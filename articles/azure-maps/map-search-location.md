---
title: Mostrar resultados da pesquisa com o Azure Maps | Documentos da Microsoft
description: Como efetuar um pedido de pesquisa com o Azure Maps, em seguida, apresentar os resultados num mapa Javascrip
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b0ab271eab45a6f4b05d01713e2e2ddd22a22ea3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746608"
---
# <a name="show-search-results-on-the-map"></a>Mostrar resultados da pesquisa no mapa

Este artigo mostra-lhe como fazer um pedido de pesquisa e mostrar os resultados da pesquisa no mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Mostrar resultados da pesquisa num mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=519&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show pesquisa resulta num mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria e adiciona uma camada de pesquisa fixa no mapa. Pode ver [adicionar um pin no mapa](./map-add-pin.md) para obter instruções.

O terceiro bloco de código envia um [XMLHttpRequest](https://xhr.spec.whatwg.org/) ao [API de pesquisa difusa do Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

O último bloco de código analisa a resposta recebida. Para uma resposta com êxito, recolhe as informações de latitude e longitude para cada localização devolvida. Adiciona todos os pontos de localização ao mapa como marcadores e ajusta os limites do mapa para processar todos os pinos.


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 

* [API de pesquisa difusa de mapas do Azure](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Obtenha informações a partir de uma coordenada](./map-get-information-from-coordinate.md)
* [Mostrar as direções da para B](./map-route.md)
