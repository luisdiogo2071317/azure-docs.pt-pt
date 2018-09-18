---
title: Criar um mapa com o Azure Maps | Documentos da Microsoft
description: Como criar um mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9759c4149c6b026837e550dcf3ab0a0156bbb736
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730014"
---
# <a name="create-a-map"></a>Criar um mapa

Este artigo mostra-lhe como criar um mapa.  

## <a name="understand-the-code"></a>Compreender o código

Existem duas formas, pode construir um mapa. Pode definir a câmera do mapa, especificando o ponto central e nível de zoom. Defina os limites de câmera do mapa por especificar Sudoeste delimitação de ponto e nordeste delimitação de ponto.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Definir a câmara

<iframe height='310' scrolling='no' title='Criar um mapa via CameraOptions' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>criar um mapa via `CameraOptions` </a>pelo Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, uma [objeto de mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) é criado através de `new atlas.Map()`. As propriedades do mapa, como o nível de centro e zoom fazem parte da [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). `CameraOptions` podem ser definidos no construtor de mapa ou via [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) função da classe Map.

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Defina os limites de câmara

<iframe height='310' scrolling='no' title='Criar um mapa via CameraBoundsOptions' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>criar um mapa via `CameraBoundsOptions` </a>pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, uma [objeto de mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) é construída por meio de `new atlas.Map()`. As propriedades do mapa, como a caixa delimitadora fazem parte da [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). `CameraBoundsOptions` pode ser definida [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) função da classe Map.

## <a name="try-out-the-code"></a>Experimente o código

Dar uma olhada no código de exemplo acima. Pode editar o código JavaScript no **separador JS** à esquerda e consulte a vista do mapa alterações na **separador de resultado** à direita. Também pode clicar no **editar no CodePen** botão e editar o código na CodePen.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Veja exemplos de código para adicionar funcionalidades à sua aplicação:

> [!div class="nextstepaction"]
> [Escolha um estilo de mapa](choose-map-style.md)

> [!div class="nextstepaction"]
> [Adicionar controles de mapa](map-add-controls.md)
