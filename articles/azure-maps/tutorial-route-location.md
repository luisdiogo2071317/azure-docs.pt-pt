---
title: Encontrar o trajeto com o Azure Maps | Microsoft Docs
description: Ir para um ponto de interesse com o Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 09/04/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 1ef4467862f47a833e0592c94c662170ca2946d8
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781454"
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>Ir para um ponto de interesse com o Azure Maps

Este tutorial mostra como utilizar a sua conta do Azure Maps e o SDK do Route Service para encontrar o trajeto para o seu ponto de interesse. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma nova página Web com a API de controlo de mapas
> * Definir coordenadas de endereços
> * Consultar o Route Service para obter direções para o ponto de interesse

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, siga os passos do tutorial anterior para [criar a sua conta do Azure Maps](./tutorial-search-location.md#createaccount) e [obter a chave de subscrição para a sua conta](./tutorial-search-location.md#getkey). 

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Criar um novo mapa 
Os passos seguintes mostram como criar uma página HTML estática incorporada com a API de Controlo de Mapas. 

1. No seu computador local, crie um novo ficheiro e dê a este o nome de **MapRoute.html**. 
2. Adicione os seguintes componentes de HTML ao ficheiro:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css"/> 
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script> 
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.min.js?api-version=1"></script> 
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
    O cabeçalho HTML incorpora as localizações de recursos para os ficheiros JavaScript e CSS para a biblioteca do Azure Maps. O segmento *script* no corpo do ficheiro HTML irá conter o código JavaScript inline para aceder às APIs do Azure Maps.

3. Adicione o seguinte código JavaScript ao bloco *script* do ficheiro HTML. Substitua a cadeia de texto **\<your account key\>** pela chave primária que copiou a partir da sua conta do Maps.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    O ficheiro **atlas.Map** fornece o controlo para um mapa Web visual e interativo, sendo um componente da API do Controlo de Mapas do Azure.

4. Guarde o ficheiro e abra-o no browser. Neste momento, tem um mapa básico que pode aprimorar mais. 

   ![Ver mapa básico](./media/tutorial-route-location/basic-map.png)

## <a name="set-start-and-end-points"></a>Definir pontos de partida e de chegada

Para este tutorial, defina o ponto de partida como Microsoft e o ponto de chegada como uma bomba de gasolina em Seattle. 

1. No mesmo bloco *script* do ficheiro **MapRoute.html**, adicione o seguinte código JavaScript para criar pontos de partida e de chegada para o trajeto:

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
    Este código cria dois [objetos GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) para representar os pontos de início e de fim da rota. 

2. Adicione o seguinte código do JavaScript para adicionar alfinetes para os pontos iniciais e finais ao mapa:

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
    **map.setCameraBounds** ajusta a janela de mapa de acordo com as coordenadas dos pontos de partida e de chegada. A API **map.addPins** adiciona os pontos ao Controlo de mapas como componentes visuais.

7. Guarde o ficheiro **MapRoute.html** e atualize o browser. Agora, o mapa está centrado em Seattle e consegue ver o alfinete azul redondo a marcar o ponto de partida e o alfinete azul a marcar o ponto de chegada.

   ![Ver mapa com os pontos de partida e de chegada](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Obter direções

Esta secção mostra como utilizar a API do serviço de trajetos do Maps para encontrar o trajeto de um determinado ponto inicial para um destino. O serviço de trajetos fornece APIs para planear os trajetos *mais rápidos*, *mais curtos*, *mais ecológicos* ou *mais emocionantes* entre dois locais. Também permite aos utilizadores planear rotas no futuro através da extensa base de dados de tráfego histórico e da previsão das durações das rotas para qualquer dia e hora. Para obter mais informações, veja [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Obter indicações de trajetos).


1. Primeiro, adicione uma camada nova no mapa para ver o percurso do trajeto, ou *linestring*. Adicione o seguinte código JavaScript ao bloco *script*.

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

2.  Crie uma instância do serviço de cliente ao adicionar o seguinte código Javascript ao bloco de script.
    ```JavaScript
    var client = new atlas.service.Client(subscriptionKey);
    ```

3. Adicione o seguinte bloco de código para criar uma cadeia de consulta de trajeto.
    ```JavaScript
    // Construct the route query string 
        var routeQuery = startPoint.coordinates[1] + 
            "," + 
            startPoint.coordinates[0] + 
            ":" + 
            destinationPoint.coordinates[1] + 
            "," + 
            destinationPoint.coordinates[0];     
    ```

4. Para obter o trajeto, adicione o seguinte bloco de código ao script. Este consulta o serviço de encaminhamento do Azure Maps através do método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) e, em seguida, analisa a resposta em formato GeoJSON com o [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes). Em seguida, adiciona todas as linhas de resposta ao mapa para compor o trajeto. Pode ver [adicionar uma linha ao mapa](./map-add-shape.md#addALine) para obter mais informações.

    ```JavaScript
    // Execute the query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery).then(response => { 
         // Parse the response into GeoJSON 
         var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response); 
 
         // Get the first in the array of routes and add it to the map 
         map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], { 
             name: routeLinesLayerName 
         }); 
    }); 
    ```

5. Guarde o ficheiro **MapRoute.html** e atualize o browser. Para uma ligação com êxito às APIs do Maps, deverá ver um mapa semelhante ao seguinte.

    ![Route Service e Controlo de Mapas do Azure](./media/tutorial-route-location/map-route.png)


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma nova página Web com a API de controlo de mapas
> * Definir coordenadas de endereços
> * Consultar o serviço de trajetos para obter direções para o ponto de interesse

O tutorial seguinte demonstra como criar uma consulta de trajeto com restrições, como o meio de deslocação ou o tipo de carga, e apresentar vários trajetos no mesmo mapa. 

> [!div class="nextstepaction"]
> [Localizar trajetos para diferentes meios de deslocação](./tutorial-prioritized-routes.md)
