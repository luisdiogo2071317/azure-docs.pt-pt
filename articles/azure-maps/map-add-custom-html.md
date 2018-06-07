---
title: Adicionar html personalizada no Azure Maps | Microsoft Docs
description: Como adicionar html personalizado para um mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 3ee1f394536061cecaa5d775650149b30098560e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600226"
---
# <a name="add-custom-html-to-the-map"></a>Adicionar HTML personalizado para o mapa

Este artigo mostra como adicionar um HTML personalizado, tais como um ficheiro de imagem para o mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='466' scrolling='no' title='Adicionar html personalizado para um mapa - png' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>adicionar html personalizado para um mapa - png</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria um elemento HTML a partir de uma imagem.

O última bloco de código utiliza [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addhtml) função da classe mapa para adicionar a imagem à posição especificada do mapa.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addhtml)
    
Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 
* [Mostrar os resultados da pesquisa](./map-search-location.md)
* [Obter informações a partir de uma coordenada](./map-get-information-from-coordinate.md)

