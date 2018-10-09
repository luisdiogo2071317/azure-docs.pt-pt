---
title: Mapear as funcionalidades de estilo no Azure Maps | Documentos da Microsoft
description: Saiba mais sobre o Azure Maps funcionalidades relacionados com o estilo.
author: walsehgal
ms.author: v-musehg
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: d0614f55b9666f6d5d7e95529fd78fdf1c19e615
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857623"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Escolha um estilo de mapa do Azure Maps

Mapas do Azure tem quatro estilos de mapas diferentes à sua escolha. Para mais informações sobre os estilos de mapa, consulte [suportado estilos de mapa do Azure Maps](./supported-map-styles.md). Este artigo mostra como utilizar as funcionalidades relacionadas ao estilo para definir um estilo na carga de mapa, definir um novo estilo e utilizar o controlo de Seletor de estilo.

## <a name="set-style-on-map-load"></a>Estilo de conjunto na carga de mapa

<iframe height='500' scrolling='no' title='Definir o estilo na carga de mapa' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>definindo o estilo na carga de mapa</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O bloco de código acima define a chave de subscrição e cria um objeto de mapa, com o estilo definido como em tons de cinzento. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

## <a name="update-the-style"></a>Atualizar o estilo

<iframe height='500' scrolling='no' title='A atualizar o estilo' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>atualizar o estilo</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O bloco de código acima define a chave de subscrição e cria um objeto de mapa sem previamente definir o estilo. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo período de código utiliza o mapa [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) método para definir o estilo de mapa de satélite.

## <a name="add-the-style-picker"></a>Adicionar o Seletor de estilo

<iframe height='500' scrolling='no' title='Adicionar o Seletor de estilo' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>adicionando o Seletor de estilo</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código no código anterior define a chave de subscrição e cria um objeto de mapa, o estilo de mapa previamente está definido como grayscale_dark. Ver [criar um mapa](./map-create.md) para obter instruções sobre como criar um mapa.

O segundo bloco de código constrói um Seletor de estilo usando o atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) construtor.

Um Seletor de estilo permite a seleção de estilo para o mapa. O terceiro bloco de código adiciona o Seletor de estilo para o mapa através do mapa [control.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) método. Todos os controles estão dentro o serviço de escuta de eventos de mapa para garantir que todos os controles são carregados depois dos mapas é totalmente carregado.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Adicione controlo a seus mapas:

> [!div class="nextstepaction"]
> [Adicionar controles de mapa](./map-add-controls.md)

Adicione um pin de mapa:

> [!div class="nextstepaction"]
> [Adicionar um pin](./map-add-pin.md)
