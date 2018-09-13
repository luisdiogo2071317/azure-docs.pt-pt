---
title: Mapear as funcionalidades de estilo no Azure Maps | Documentos da Microsoft
description: Saiba mais sobre o Azure Maps funcionalidades relacionados com o estilo.
author: walsehgal
ms.author: v-musehg
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 160752cd0467ef307f7a45b1e0d703c7ddd5d773
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720808"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Escolha um estilo de mapa do Azure Maps
Mapas do Azure tem quatro estilos de mapas diferentes à sua escolha. Para mais informações sobre os estilos de mapa, consulte [suportado estilos de mapa do Azure Maps](./supported-map-styles.md). Este artigo mostra como utilizar as funcionalidades relacionadas ao estilo para definir um estilo na carga de mapa, definir um novo estilo e utilizar o controlo de Seletor de estilo.

## <a name="setting-style-on-map-load"></a>A definição de estilo na carga de mapa

<iframe height='500' scrolling='no' title='Definir o estilo na carga de mapa' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>definindo o estilo na carga de mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O código acima cria um objeto de mapa com o estilo definido como em tons de cinzento. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

## <a name="updating-the-style"></a>A atualizar o estilo

<iframe height='500' scrolling='no' title='A atualizar o estilo' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>atualizar o estilo</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código no código anterior cria um objeto de mapa sem previamente definir o estilo. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo período de código utiliza o mapa [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) método para definir o estilo de mapa de satélite.

## <a name="adding-the-style-picker"></a>Adicionar o Seletor de estilo

<iframe height='500' scrolling='no' title='Adicionar o Seletor de estilo' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>adicionando o Seletor de estilo</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código no código anterior cria um objeto de mapa sem previamente definir o estilo. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo bloco de código constrói um Seletor de estilo usando o atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) construtor.

Um Seletor de estilo permite a seleção de estilo para o mapa. O terceiro bloco de código adiciona o Seletor de estilo para o mapa através do mapa [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo: 
* [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes:
* [Adicionar controles de mapa](./map-add-controls.md)
* [Adicionar um pin](./map-add-pin.md)
