---
title: Adicionar controles de mapa do Azure Maps | Documentos da Microsoft
description: Como adicionar o controle de zoom, controle de argumento de venda, girar controle e um Seletor de estilo para um mapa do Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 08/29/2018
ms.topic: how-to-guides
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b31b709f08c8e8ab4a04a1b70daa48b02f33d32e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345103"
---
# <a name="add-map-controls-to-azure-maps"></a>Adicionar controles de mapa do Azure Maps

Este artigo mostra-lhe como adicionar controles de mapa para um mapa. Também aprenderá como criar um mapa com todos os controles e um Seletor de estilo.

## <a name="add-zoom-control"></a>Adicionar o controle de zoom

<iframe height='500' scrolling='no' title='Adição de um controle de zoom' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>adicionando um controle de zoom</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código no código anterior cria um objeto de mapa. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo bloco de código cria um objeto de controle de zoom usando o atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest).

Controle de zoom acrescenta a capacidade de aplicar zoom dentro e fora do mapa. O bloco de terceiro adiciona o controle de zoom ao mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

## <a name="add-pitch-control"></a>Adicionar controlo de argumento de venda

<iframe height='500' scrolling='no' title='Adição de um controle de argumento de venda' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>adicionando um controle de argumento de venda</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código acima cria um objeto de mapa com o estilo definido como em tons de cinzento. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo bloco de código cria um objeto de controle de argumento de venda usando o atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest).

Controlo de argumento de venda adiciona a capacidade de alterar o argumento de venda do mapa. O bloco de terceiro adiciona o controle de argumento de venda para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

## <a name="add-compass-control"></a>Adicionar controlo de bússola

<iframe height='500' scrolling='no' title='Adição de um controle de rotação' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>adicionando um controle rodar</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código no código anterior cria um objeto de mapa. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo bloco de código cria um objeto de controle de bússola usando o atlas [controle de bússola](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol). Ele também adiciona o controle de bússola para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

## <a name="a-map-with-all-controls"></a>Um mapa com todos os controles

<iframe height='500' scrolling='no' title='Um mapa com todos os controles' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>mapa com todos os controles</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código no código anterior cria um objeto de mapa. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo bloco de código cria um objeto de controle de bússola usando o atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol) e adiciona-o para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

O terceiro bloco de código cria um objeto de controle de zoom usando o atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) e adiciona-o para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

O quarto bloco de código cria um objeto de controle de argumento de venda usando o atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) e adiciona-o para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

O último bloco de código adiciona um objeto de Seletor de estilo ao mapa usando o atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) e adiciona-o para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol)
    * [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest)
    * [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 
* [Adicionar um pin](./map-add-pin.md)
* [Adicionar um pop-up](./map-add-popup.md)