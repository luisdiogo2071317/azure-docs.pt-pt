---
title: Encontrar caminho com o Azure Location Based Services | Microsoft Docs
description: Encaminhar para um ponto de interesse nas proximidades com o Azure Location Based Services
services: location-based-services
keywords: 
author: kgremban
ms.author: kgremban
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: b54d97afb4e762a7d252acf7cf53a5deac01d43f
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Encaminhar para um ponto de interesse nas proximidades com o Azure Location Based Services

Este tutorial mostra como utilizar a sua conta do Azure Location Based Services e o SDK do Route Service para encontrar o caminho para o seu ponto de interesse. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Obter coordenadas de endereços
> * Consultar o Route Service para obter direções para o ponto de interesse

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, certifique-se de que [cria a sua conta do Azure Location Based Services](./tutorial-search-location.md#createaccount) e [obtém a chave de subscrição para a sua conta](./tutorial-search-location.md#getkey). Também pode observar como utilizar as APIs do Search Service e Controlo de Mapas, conforme abordado no tutorial [Procurar pontos de interesse nas proximidades com o Azure Location Based Services](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Obter coordenadas de endereços

Utilize os seguintes passos para criar uma página HTML estática incorporada com a API de Controlo de Mapas dos Serviços com Base em Localização. 

1. No seu computador local, crie um novo ficheiro e dê a este o nome de **MapRoute.html**. 
2. Adicione os seguintes componentes HTML ao ficheiro:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>

    <body>
        <div id="map"></div>
        <script>
            // Embed Map Control JavaScript code here
        </script>
    </body>

    </html>
    ```
    Note que o cabeçalho HTML incorpora as localizações de recurso para ficheiros JavaScript e CSS para a biblioteca do Azure Location Based Services. Repare também que o segmento *script* no corpo do ficheiro HTML, que contém o código JavaScript inline para aceder às APIs do Azure Location Based Services.

3. Adicione o seguinte código JavaScript ao bloco *script* do ficheiro HTML. Utilize a chave primária da sua conta do Location Based Services no script.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    O ficheiro **atlas.Map** fornece o controlo para um mapa Web visual e interativo, sendo um componente da API do Controlo de Mapas do Azure.

4. Adicione o seguinte código JavaScript ao bloco *script*. Isto adiciona uma camada de *linestrings* ao Controlo de Mapas para mostrar o caminho:

    ```JavaScript
    // Initialize the linestring layer for routes on the map
    var routeLinesLayerName = "routes";
    map.addLinestrings([], {
        name: routeLinesLayerName,
        color: "#2272B9",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });
    ```

5. Adicione o seguinte código do JavaScript para criar pontos de início e fim para o caminho:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Microsoft",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.3352, 47.61397]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Contoso Oil & Gas",
        icon: "pin-blue"
    });
    ```
    Este código cria dois [objetos GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) para representar os pontos iniciais e finais do caminho. O ponto final é a combinação de latitude/longitude para uma das *estações de gasolina* procuradas no tutorial anterior [Procurar pontos de interesse nas proximidades com o Azure Location Based Services](./tutorial-search-location.md).

6. Adicione o seguinte código do JavaScript para adicionar alfinetes para os pontos iniciais e finais ao mapa:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 50
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    A API **map.setCameraBounds** ajusta a janela de mapa segundo as coordenadas dos pontos inicial e final. A API **map.addPins** adiciona os pontos ao Controlo de mapas como componentes visuais.

7. Guarde o ficheiro **MapRoute.html** no seu computador. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Consultar o Route Service para obter direções para o ponto de interesse

Esta secção mostra como utilizar a API Route Service do Azure Location Based Services para encontrar o caminho de um determinado ponto inicial para um destino. O serviço Route Service fornece APIs para planear o caminho mais rápido, mais curto ou ecológico entre dois locais, considerando as condições de tráfego em tempo real. Isto também permite que os utilizadores possam planear caminhos no futuro com recurso à extensa base de dados de tráfego histórico e ao prever as durações dos caminhos em qualquer dia ou hora. 

1. Abra o ficheiro **MapRoute.html**, criado na secção anterior, e adicione o seguinte código JavaScript ao bloco *script*, para ilustrar o Route Service.

    ```JavaScript
    // Perform a request to the route service and draw the resulting route on the map
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(xhttp.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], { name: routeLinesLayerName });
        }
    };
    ```
    Este fragmento de código cria um [XMLHttpRequest](https://xhr.spec.whatwg.org/) e adiciona um processador de eventos para analisar a resposta recebida. Para obter uma resposta com êxito, constrói uma matriz de coordenadas para os segmentos de linha do primeiro caminho devolvido. Depois, adiciona este conjunto de coordenadas deste caminho à camada *linestrings* do mapa.

2. Adicione o seguinte código ao bloco *script* para enviar o XMLHttpRequest para o Route Service do Azure Location Based Services:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    O pedido acima mostra os parâmetros necessários: a sua chave de conta e as coordenadas dos pontos inicial e final, na ordem apresentada. 

3. Guarde o ficheiro **MapRoute.html** localmente e, em seguida, abra-o num browser à escolha e veja o resultado. Para uma ligação com êxito à API do Location Based Services, deverá ver um mapa semelhante ao seguinte. 

    ![Route Service e Controlo de Mapas do Azure](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Obter coordenadas de endereços
> * Consultar o Route Service para obter direções para o ponto de interesse

Avance para o tutorial [Encontrar caminhos para diferentes modos de viagem com o Azure Location Based Services](./tutorial-prioritized-routes.md) para saber como utilizar o Azure Location Based Serivces para priorizar caminhos para o seu ponto de interesse, com base no meio de transporte. 
