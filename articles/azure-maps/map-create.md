---
title: Criar um mapa com o Azure Maps | Documentos da Microsoft
description: Como criar um mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c5d48e2e7316f33a565fc61a769a29c00834eed5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43343374"
---
# <a name="create-a-map"></a>Criar um mapa

Este artigo mostra-lhe como criar um mapa.  

## <a name="understand-the-code"></a>Compreender o código

Existem duas formas, pode construir um mapa. Pode definir a câmera do mapa, especificando o ponto central e nível de zoom ou definir os limites de câmera do mapa, especificando Sudoeste delimitação de ponto e nordeste delimitação de ponto.

<a id="setCameraOptions"></a>

### <a name="setting-the-camera"></a>Definir a câmara

<iframe height='310' scrolling='no' title='Criar um mapa via CameraOptions' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>criar um mapa via CameraOptions</a> ao Azure LBS (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, uma [objeto de mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) é criado através de `new atlas.Map()`. As propriedades do mapa, como o nível de centro e zoom fazem parte da [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). CameraOptions podem ser definidos no construtor de mapa ou via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) função da classe map.

<a id="setCameraBoundsOptions"></a>

### <a name="setting-the-camera-bounds"></a>Definir os limites de câmara

<iframe height='310' scrolling='no' title='Criar um mapa via CameraBoundsOptions' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>criar um mapa via CameraBoundsOptions</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, uma [objeto de mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) é construída por meio de `new atlas.Map()`. As propriedades do mapa, como a caixa delimitadora fazem parte da [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). CameraBoundsOptions pode ser definida [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) função da classe map.

## <a name="try-out-the-code"></a>Experimente o código 

Dar uma olhada no código de exemplo acima. Pode editar o código JavaScript na guia JS à esquerda e ver as alterações de vista de mapa no separador de resultado à direita. Também pode clicar no botão "Editar no CodePen" e editar o código na CodePen. 

<a id="relatedReference"></a>

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Escolha um estilo de mapa](choose-map-style.md)
* [Adicionar controles de mapa](map-add-controls.md)
    

