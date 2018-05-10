---
title: Adicionar uma forma com o Azure Maps | Microsoft Docs
description: Como adicionar uma forma para um mapa de Javascript
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
ms.openlocfilehash: fb9ec0713d3db465cf835346465e70c4455b38ff
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="add-a-shape-to-a-map"></a>Adicionar uma forma para um mapa

Este artigo mostra como adicionar uma linha, um círculo e um polígono ao mapa. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Adicionar uma linha

<iframe height='500' scrolling='no' title='Adicionar uma linha para um mapa' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/qomaKv/'>adicionar uma linha para um mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código, é criada uma linha. Uma linha é uma [funcionalidade](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) de LineString com LineStringProperties como a respetiva propriedade de funcionalidade. Utilize `new atlas.data.Feature(new atlas.data.LineString())` para criar uma linha e definir as respetivas propriedades. 

Uma camada de linha é uma matriz de linhas. O última bloco de código utiliza [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings) função da classe mapa para adicionar a camada de linha para o mapa e definir as propriedades da camada de linha. Ver as propriedades de uma camada de linha em [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Adicionar um círculo

<iframe height='500' scrolling='no' title='Adicionar um círculo para um mapa' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>adicionar um círculo para um mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

No segundo bloco de código, é criado um círculo. É um círculo um [funcionalidade](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) de [ponto](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) com [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circleproperties?view=azure-iot-typescript-latest) como a respetiva propriedade de funcionalidade. Utilize `new atlas.data.Feature(new atlas.data.Point())` para criar um círculo e definir as respetivas propriedades.

Uma camada de círculo é uma matriz de circles. O última bloco de código utiliza [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addcircles) função da classe mapa para adicionar a camada de círculo ao mapa e definir as propriedades da camada do círculo. Ver as propriedades de uma camada de círculo em [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Adicionar um polígono
<iframe height='500' scrolling='no' title='Adicionar um polígono para um mapa ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>adicionar um polígono para um mapa </a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

No segundo bloco de código, é criado um polígono. É um polígono um [funcionalidade](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) de [polígono](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygon?view=azure-iot-typescript-latest) com [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonproperties?view=azure-iot-typescript-latest) como a respetiva propriedade de funcionalidade. Utilize `new atlas.data.Feature(new atlas.data.Polygon())` para criar um polígono e definir as respetivas propriedades. Fornece ordenadas coordenadas do caminho de polígono no construtor do polígono.

Uma camada de polígono é uma matriz de polígonos. O última bloco de código utiliza [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpolygons) função da classe mapa para adicionar a camada de polígono ao mapa e definir as respetivas propriedades. Ver as propriedades de uma camada de polígono no [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonlayeroptions?view=azure-iot-typescript-latest). 
