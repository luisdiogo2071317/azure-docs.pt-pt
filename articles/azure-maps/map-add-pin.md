---
title: Adicionar um símbolos e marcadores com o Azure Maps | Documentos da Microsoft
description: Como adicionar símbolos e marcadores a um mapa de Javascript
author: walsehgal
ms.author: v-musehg
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c56ac35f49c364b7b0f2ad26b82b178411419414
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282690"
---
# <a name="add-symbols-and-markers-to-a-map"></a>Adicionar símbolos e marcadores para um mapa

Este artigo mostra-lhe como adicionar símbolos e marcadores a um mapa a utilizar uma origem de dados.

## <a name="add-a-symbol-marker"></a>Adicionar um marcador de símbolo

<iframe height='500' scrolling='no' title='Fixar localização de comutador' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>fixar localização de comutador</a> ao Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O primeiro bloco de código acima constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código, um objeto de origem de dados é criado utilizando o [origem de dados](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) classe. Um ponto, em seguida, é criado e adicionado à origem de dados. É um ponto de um [funcionalidade](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) dos [ponto](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest).

O terceiro bloco de código cria um [serviço de escuta de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) e atualizações coordenadas do ponto de após rato clique usando a classe shape [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#setcoordinates) método.

Uma [camada de símbolo](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa ou ícones de texto para processar dados com base no ponto encapsulados no [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos no mapa.  A origem de dados, o serviço de escuta de eventos de clique e a camada de símbolo são criadas e adicionadas ao mapa dentro do [serviço de escuta de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) função para garantir que o ponto é apresentado depois que o mapa for totalmente carregada.

## <a name="add-a-custom-symbol"></a>Adicionar um símbolo de personalizado

<iframe height='500' scrolling='no' title='Origem de dados do HTML' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>origem de dados do HTML</a> através do Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código adiciona uma [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) para o mapa, com o [marcadores](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) propriedade do [mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) classe. O HtmlMarker é adicionado ao mapa dentro do [serviço de escuta de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) função para garantir que é apresentado depois do mapa carrega totalmente.

## <a name="add-bubble-markers"></a>Adicionar marcadores de bolhas

<iframe height='500' scrolling='no' title='Origem de dados de BubbleLayer' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> através do Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

No código acima, o primeiro bloco de código constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código, é definida uma matriz de posições e um [MultiPoint](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.multipoint?view=azure-iot-typescript-latest) objeto é criado. Um objeto de origem de dados, em seguida, é criado com o [origem de dados](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) classe e o MultiPoint de objeto é adicionado à origem de dados.

Uma [camada de bolhas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) processa os dados com base no ponto encapsulados no [origem de dados](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como faz um movimento no mapa. O último bloco de código cria uma camada de bolhas e adiciona-o ao mapa. Ver as propriedades de uma camada de bolhas na [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

O objeto de MultiPoint, a origem de dados e a camada de bolhas são criadas e adicionadas ao mapa dentro do [serviço de escuta de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) função para garantir que o círculo é apresentado depois que o mapa for totalmente carregada.

## <a name="add-bubble-markers-with-label"></a>Adicionar marcadores de bolhas com etiqueta

<iframe height='500' scrolling='no' title='Origem de dados multiLayer' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>MultiLayer DataSource</a> através do Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

O código acima mostra-lhe como visualizar e dados da etiqueta do mapa. O primeiro bloco de código acima constrói um objeto de mapa. Pode ver [criar um mapa](./map-create.md) para obter instruções.

O segundo bloco de código, cria um [apontar](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) objeto. Em seguida, cria uma origem de dados objeto utilizando a [origem de dados](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) de classe e adiciona o ponto à origem de dados.

Uma [camada de bolhas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) processa os dados com base no ponto encapsulados no [origem de dados](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como faz um movimento no mapa. O terceiro bloco de código cria uma camada de bolhas e adiciona-o ao mapa. Ver as propriedades de uma camada de bolhas na [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Uma [camada de símbolo](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa ou ícones de texto para processar dados com base no ponto encapsulados no [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos no mapa. O último bloco de código cria e adiciona uma camada de símbolo ao mapa que processa o rótulo de texto para a bolha. Ver as propriedades de uma camada de símbolo em [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

A origem de dados e as camadas são criadas e adicionadas ao mapa dentro do [serviço de escuta de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) função para garantir que os dados são apresentados depois do mapa é totalmente carregado.


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [Mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Veja os artigos seguintes para obter mais amostras de código adicionar a seus mapas:

> [!div class="nextstepaction"]
> [Adicionar um pop-up](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Adicionar uma forma](./map-add-shape.md)