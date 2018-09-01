---
title: Adicionar html personalizado no Azure Maps | Documentos da Microsoft
description: Como adicionar html personalizado para um mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 5060839be244f55700b40735e598964a0b7b6709
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382050"
---
# <a name="add-custom-html-to-the-map"></a>Adicionar HTML personalizado para o mapa

Este artigo mostra-lhe como adicionar um HTML personalizado, como um ficheiro de imagem ao mapa. 

## <a name="understand-the-code"></a>Compreender o código

<iframe height='466' scrolling='no' title='Adicionar html personalizado para um mapa - png' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>adicionar html personalizado para um mapa - png</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código cria um elemento HTML a partir de uma imagem.

O último bloco de código utiliza [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addhtml) função da classe map para adicionar a imagem para a posição especificada do mapa.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addhtml)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Mostrar resultados da pesquisa](./map-search-location.md)
* [Obtenha informações a partir de uma coordenada](./map-get-information-from-coordinate.md)

