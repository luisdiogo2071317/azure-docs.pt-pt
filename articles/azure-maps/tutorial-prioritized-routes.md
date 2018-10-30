---
title: Vários trajetos com o Azure Maps | Microsoft Docs
description: Localizar trajetos para diferentes meios de deslocação com o Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 10/22/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 864f662cd6be3c5929166db92f2dad92b9c6586e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648212"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Localizar trajetos para diferentes meios de deslocação com o Azure Maps

Este tutorial mostra como utilizar a sua conta do Azure Maps e o serviço de rotas para encontrar o caminho para o seu ponto de interesse, priorizado pelo seu meio de transporte. Apresente dois trajetos diferentes no seu mapa, um para carros e outro para camiões que possam ter restrições de trânsito, devido a altura, peso ou carga perigosa. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma nova página Web com a API de controlo de mapas
> * Visualizar o fluxo de tráfego no seu mapa
> * Criar consultas de trajeto que declaram o modo de deslocação
> * Apresentar vários trajetos no seu mapa

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, siga os passos do primeiro tutoria para [criar a sua conta do Azure Maps](./tutorial-search-location.md#createaccount) e [obter a chave da subscrição para a sua conta](./tutorial-search-location.md#getkey).

## <a name="create-a-new-map"></a>Criar um novo mapa

Os passos seguintes mostram como criar uma página HTML estática incorporada com a API de Controlo de Mapas.

1. No seu computador local, crie um novo ficheiro e dê-lhe o nome **MapTruckRoute.html**.
2. Adicione os seguintes componentes de HTML ao ficheiro:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
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
    O cabeçalho HTML incorpora as localizações de recursos para os ficheiros JavaScript e CSS para a biblioteca do Azure Maps. O segmento *script* no corpo de HTML irá conter o código JavaScript inline do mapa.

3. Adicione o seguinte código JavaScript ao bloco *script* do ficheiro HTML. Substitua a cadeia de texto **\<your account key\>** pela chave primária que copiou a partir da sua conta do Maps. Se não disser ao mapa onde é que se deve focar, verá a vista do mundo inteiro. Este código define o ponto central do mapa e declara um nível de zoom que lhe permite concentrar-se numa área específica por predefinição.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var mapCenterPosition = [-73.985708, 40.75773];
    atlas.setSubscriptionKey("<your account key>");
    var map = new atlas.Map("map", {
      center: mapCenterPosition,
      zoom: 11
    });
    ```
    O ficheiro **atlas.Map** fornece o controlo para um mapa Web visual e interativo, sendo um componente da API do Controlo de Mapas do Azure.

4. Guarde o ficheiro e abra-o no browser. Neste momento, tem um mapa básico que pode aprimorar mais. 

   ![Ver mapa básico](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Visualizar o fluxo do tráfego

1. Adicione a apresentação do fluxo de tráfego ao mapa.  Depois de o mapa estar totalmente carregado, o **map.events.add** garante que todas as funções dos mapas adicionadas ao mapa são carregadas.

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    Este código define o fluxo de tráfego como `relative`, que é a velocidade da estrada relativa ao fluxo livre. Também pode defini-lo como velocidade da estrada `absolute` ou `relative-delay`, o que apresenta a velocidade relativa onde difere do fluxo livre.

2. Guarde o ficheiro **MapTruckRoute.html** e atualize a página no browser. Deverá ver as estradas de Los Angeles com os dados do tráfego atual.

   ![Ver mapa do tráfego](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="set-start-and-end-points"></a>Definir pontos de partida e de chegada

Neste tutorial, defina o ponto de partida como uma empresa fictícia em Seattle, chamada Fabrikam, e o ponto de chegada como um escritório da Microsoft.

1. Adicione o seguinte código JavaScript para criar os marcadores para os pontos de início e de fim da rota:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.356099, 47.580045]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Fabrikam, Inc.",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Microsoft",
        icon: "pin-blue"
    });
    ```
    Este código cria dois [objetos GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) para representar os pontos de início e de fim da rota.

2. Adicione o seguinte código JavaScript para adicionar os pontos de início e de fim ao mapa:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 100
    });
    
    map.events.add("load", function() { 
        // Add pins to the map for the start and end point of the route
        map.addPins([startPin, destinationPin], {
            name: "route-pins",
            textFont: "SegoeUi-Regular",
            textOffset: [0, -20]
        });
    });
    ```
    A chamada **map.setCameraBounds** ajusta a janela do mapa de acordo com as coordenadas dos pontos de partida e de chegada. Depois de o mapa estar totalmente carregado, o **map.events.add** garante que todas as funções dos mapas adicionadas ao mapa são carregadas. A API **map.addPins** adiciona os pontos ao Controlo de mapas como componentes visuais.

3. Guarde o ficheiro e atualize o browser para ver os marcadores no seu mapa. Apesar de ter declarado o mapa com um ponto central em Los Angeles, **map.setCameraBounds** moveu a vista para apresentar os pontos de partida e de chegada.

   ![Ver mapa com os pontos de partida e de início](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Compor rotas priorizadas por meio de transporte

Esta secção mostra como utilizar a API de serviço de trajetos do Maps para encontrar vários trajetos a partir de um determinado ponto de partida para um destino com base no seu meio de transporte. O serviço de trajetos oferece APIs para planear os trajetos *mais rápidos*, *mais curtos*, *mais ecológicos* ou *mais emocionantes* entre dois locais, considerando as condições de tráfego atuais. Também permite aos utilizadores planear rotas no futuro através da extensa base de dados de tráfego histórico e da previsão das durações das rotas para qualquer dia e hora. Para obter mais informações, veja [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Obter indicações de trajetos). Todos os blocos de código seguintes devem ser adicionados no **eventListener de carregamento do mapa** para garantir que são carregados depois de o mapa estar totalmente carregado.

1. Primeiro, adicione uma camada nova no mapa para ver o percurso do trajeto, ou *linestring*. Neste tutorial, existem dois trajetos diferentes, **car-route** (trajeto-carro) e **truck-route** (trajeto-camião), cada um com o seu próprio estilo. Adicione o seguinte código JavaScript ao bloco *script*:

    ```JavaScript
    // Place route layers on the map
    var carRouteLayerName = "car-route";
    map.addLinestrings([], {
        name: carRouteLayerName,
        color: "#B76DAB",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });

    var truckRouteLayerName = "truck-route";
    map.addLinestrings([], {
        name: truckRouteLayerName,
        color: "#2272B9",
        width: 9,
        cap: "round",
        join: "round",
        before: carRouteLayerName
    });
    ```

2. Adicione o seguinte código JavaScript ao bloco *script*, para pedir o trajeto para um camião e apresentar os resultados no mapa:

    ```JavaScript
    // Instantiate the service client  
    var client = new atlas.service.Client(MapsAccountKey);

    // Construct the route query string
    var routeQuery = startPoint.coordinates[1] +
        "," +
        startPoint.coordinates[0] +
        ":" +
        destinationPoint.coordinates[1] +
        "," +
        destinationPoint.coordinates[0];

    // Execute the truck route query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery, {
        travelMode: "truck",
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: "USHazmatClass2"
    }).then(response => {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson
            .GeoJsonRouteDirectionsResponse(response);

        // Get the first in the array of routes and add it to the map
        map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
            name: truckRouteLayerName
        });
    });
    ```
    O fragmento de código acima cria uma instância de um cliente de serviço e constrói uma cadeia de consulta do trajeto. Posteriormente, consulta o serviço de encaminhamento do Azure Maps através do método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) e, em seguida, analisa a resposta em formato GeoJSON com o [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest). Em seguida, cria uma matriz de coordenadas para o trajeto devolvido e adiciona-o à camada `truckRouteLayerName` do mapa.

3. Adicione o seguinte código JavaScript para pedir o trajeto para um carro e apresentar os resultados:

    ```JavaScript
    // Execute the car route query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery).then(response => {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson
            .GeoJsonRouteDiraectionsResponse(response);

        // Get the first in the array of routes and add it to the map 
        map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
            name: carRouteLayerName
        });
    });
    ```
    Este fragmento de código utiliza a mesma consulta do trajeto do camião para um carro. Consulta o serviço de encaminhamento do Azure Maps através do método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) e, em seguida, analisa a resposta em formato GeoJSON com o [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest). Em seguida, cria uma matriz de coordenadas para o trajeto devolvido e adiciona-o à camada `carRouteLayerName` do mapa.

4. Guarde o ficheiro **MapTruckRoute.html** e atualize o browser para ver o resultado. Para uma ligação com êxito às APIs do Maps, deverá ver um mapa semelhante ao seguinte.

    ![Rotas priorizadas com o Route Service do Azure](./media/tutorial-prioritized-routes/prioritized-routes.png)

    O trajeto do camião é azul e mais espesso, ao passo que o do carro é roxo e mais fino. O trajeto do carro passa por Lake Washington através da autoestrada interestadual I-90, que passa por túneis em zonas residenciais, pelo que não são permitidas cargas com resíduos perigosos. O trajeto do camião, que especifica uma carga do tipo USHazmatClass2, é direcionado corretamente para outra autoestrada.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma nova página Web com a API de controlo de mapas
> * Visualizar o fluxo de tráfego no seu mapa
> * Criar consultas de trajeto que declaram o modo de deslocação
> * Apresentar vários trajetos no seu mapa

Pode aceder ao código de exemplo deste tutorial aqui:

> [Vários trajetos com o Azure Maps](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/truckRoute.html)

Para saber mais sobre a cobertura e as capacidades do Azure Maps:

> [!div class="nextstepaction"]
> [Níveis de zoom e grelha de mosaico](zoom-levels-and-tile-grid.md)

Para ver mais exemplos de código e uma experiência interativa de programação:

> [!div class="nextstepaction"]
> [Como utilizar o controlo de mapa](how-to-use-map-control.md)