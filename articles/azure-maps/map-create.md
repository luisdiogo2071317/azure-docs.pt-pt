---
title: Criar um mapa com o Azure Maps | Microsoft Docs
description: Como criar um mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ec85854e5d9b7ee0d5e2c54881a417ba6cbb366e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599818"
---
# <a name="create-a-map"></a>Criar um mapa

Este artigo mostra como criar um mapa.  

## <a name="understand-the-code"></a>Compreender o código

Existem duas formas pode construir um mapa. Pode definir a câmara do mapa, especificando o ponto central e nível de zoom ou defina os limites da câmara do mapa, especificando o southwest delimitadora ponto e nordeste delimitadora ponto.

<a id="setCameraOptions"></a>

### <a name="setting-the-camera"></a>Definir a câmara

<iframe height='310' scrolling='no' title='Criar um mapa de via CameraOptions' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>criar um mapa de via CameraOptions</a> pelo Azure LBS (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, uma [objeto de mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest) é criado através de `new atlas.Map()`. Propriedades do mapa, tais como o nível de centro e zoom fazem parte do [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/cameraoptions?view=azure-iot-typescript-latest). CameraOptions podem ser definidas no construtor do mapa ou através de [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamera) função da classe de mapa.

<a id="setCameraBoundsOptions"></a>

### <a name="setting-the-camera-bounds"></a>Definir os limites da câmara

<iframe height='310' scrolling='no' title='Criar um mapa de via CameraBoundsOptions' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>criar um mapa de via CameraBoundsOptions</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, uma [objeto de mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest) é construído através de `new atlas.Map()`. Propriedades do mapa, tais como delimitadora caixa fazem parte do [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/cameraboundsoptions?view=azure-iot-typescript-latest). CameraBoundsOptions pode ser definido através de [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) função da classe de mapa.

## <a name="try-out-the-code"></a>Experimentar o código 

Observe o código de exemplo acima. Pode editar o código JavaScript no separador JS à esquerda e ver as alterações de vista do mapa no separador resultado à direita. Também pode clique no botão "Editar no CodePen" e editar o código no CodePen. 

<a id="relatedReference"></a>

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e os métodos utilizados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamera)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)
    
Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 
* [Adicionar um pin](./map-add-pin.md)
* [Adicionar um pop-up](map-add-popup.md)
    

