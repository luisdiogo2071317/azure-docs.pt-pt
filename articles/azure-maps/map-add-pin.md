---
title: Adicionar um pin com o Azure Maps | Microsoft Docs
description: Como adicionar um pin para um mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 094abe08c0c88c7561185675ceb8529be2c87a0a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294653"
---
# <a name="add-pins-to-the-map"></a>Adicionar pins ao mapa

Este artigo mostra como adicionar um pin para um mapa.  

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Adicionar um pin para um mapa' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>adicionar um pin para um mapa</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

No segundo bloco de código, um pin é criado e adicionado ao mapa. Um pin é um [funcionalidade](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) de [ponto](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) com [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/pinproperties?view=azure-iot-typescript-latest) como a respetiva propriedade de funcionalidade. Utilize `new atlas.data.Feature(new atlas.data.Point())` para criar um pin e definir as respetivas propriedades. Uma camada de pin é uma matriz de pins. Utilize [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins) função da classe mapa para adicionar uma camada de pin para o mapa e definir as propriedades da camada de pin. Ver as propriedades de uma camada de pin no [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/pinlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 
* [Adicionar um pop-up](./map-add-popup.md)
* [Adicionar uma forma](./map-add-shape.md)

