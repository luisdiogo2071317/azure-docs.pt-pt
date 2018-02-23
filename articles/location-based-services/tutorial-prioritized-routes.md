---
title: "Várias rotas com o Azure Location Based Services | Microsoft Docs"
description: "Localizar rotas para diferentes meios de deslocação com o Azure Location Based Services"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: ac57996e7dced5ef0f0a993889bfc13b1d064167
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Localizar rotas para diferentes meios de deslocação com o Azure Location Based Services

Este tutorial mostra como utilizar a sua conta do Azure Location Based Services e o SDK do Route Service para encontrar o caminho para o seu ponto de interesse, priorizado pelo seu meio de transporte. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar uma consulta do Route Service
> * Compor rotas priorizadas por meio de transporte

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, certifique-se de que [cria a sua conta do Azure Location Based Services](./tutorial-search-location.md#createaccount) e [obtém uma chave da sua conta](./tutorial-search-location.md#getkey). Também pode ver como utilizar as APIs do Controlo de Mapas e do Search Service, tal como explicado no tutorial [Procurar pontos de interesse nas proximidades com o Azure Location Based Services](./tutorial-search-location.md), bem como aprender a utilização básica das APIs do Route Service, tal como explicado no tutorial [Encaminhar para um ponto de interesse com o Azure Location Based Services](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Configurar uma consulta do Route Service

Utilize os passos seguintes para criar uma página HTML estática incorporada com a API de Controlo de Mapas do Location Based Services. 

1. No seu computador local, crie um novo ficheiro e dê-lhe o nome **MapTruckRoute.html**. 
2. Adicione os seguintes componentes de HTML ao ficheiro:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
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
    Note que o cabeçalho HTML incorpora as localizações de recurso para ficheiros JavaScript e CSS para a biblioteca do Azure Location Based Services. Repare também no segmento de *script* adicionado ao corpo do HTML, para incluir o código JavaScript inline para aceder à API de Controlo de Mapas do Azure.
3. Adicione o seguinte código JavaScript ao bloco de *script* do ficheiro HTML. Substitua o marcador de posição *<insert-key>* pela chave primária da conta do Location Based Services.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    O ficheiro **atlas.Map** fornece o controlo para um mapa Web visual e interativo, sendo um componente da API do Controlo de Mapas do Azure.

4. Adicione o seguinte código JavaScript ao bloco de *script*, para adicionar a apresentação de fluxo de tráfego ao mapa:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Este código define o fluxo de tráfego como `relative`, que é a velocidade da estrada relativa ao fluxo livre. Também pode defini-lo como velocidade de estrada `absolute` ou `relative-delay` que apresenta a velocidade relativa onde difere do fluxo livre. 

5. Adicione o seguinte código JavaScript para criar os marcadores para os pontos de início e de fim da rota:

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

6. Adicione o seguinte código JavaScript para adicionar camadas de *linestrings* ao Controlo de Mapas, para apresentar as rotas com base no meio de transporte, por exemplo, _carro_ e _camião_.

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

7. Adicione o seguinte código JavaScript para adicionar os pontos de início e de fim ao mapa:

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

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    A API **map.setCameraBounds** ajusta a janela de mapa de acordo com as coordenadas dos pontos de início e de fim. A API **map.addPins** adiciona os pontos ao Controlo de mapas como componentes visuais.

8. Guarde o ficheiro **MapTruckRoute.html** no seu computador. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Compor rotas priorizadas por meio de transporte

Esta secção mostra como utilizar a API Route Service do Azure Location Based Services para localizar várias rotas a partir de um determinado ponto de início para um destino, com base no seu meio de transporte. O Route Service fornece APIs para planear a rota mais rápida, mais curta ou ecológica entre dois locais, considerando as condições de tráfego em tempo real. Também permite aos utilizadores planear rotas no futuro através da extensa base de dados de tráfego histórico e da previsão das durações das rotas para qualquer dia e hora. 

1. Abra o ficheiro **MapTruckRoute.html** criado na secção anterior e adicione o seguinte código JavaScript ao bloco de *script*, para obter a rota para um camião utilizando o Route Service.

    ```JavaScript
    // Perform a request to the route service and draw the resulting truck route on the map
    var xhttpTruck = new XMLHttpRequest();
    xhttpTruck.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: truckRouteLayerName
            });
        }
    };

    var truckRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    truckRouteUrl += "&api-version=1.0";
    truckRouteUrl += "&subscription-key=" + LBSAccountKey;
    truckRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    truckRouteUrl += "&travelMode=truck";
    truckRouteUrl += "&vehicleWidth=2";
    truckRouteUrl += "&vehicleHeight=2";
    truckRouteUrl += "&vehicleLength=5";
    truckRouteUrl += "&vehicleLoadType=USHazmatClass2";

    xhttpTruck.open("GET", truckRouteUrl, true);
    xhttpTruck.send();
    ```
    Este fragmento de código cria um [XMLHttpRequest](https://xhr.spec.whatwg.org/) e adiciona um processador de eventos para analisar a resposta recebida. Para uma resposta com êxito, cria uma matriz de coordenadas para a rota devolvida e adiciona-a à camada `truckRouteLayerName` do mapa. 
    
    Este fragmento de código também envia a consulta para o Route Service, para obter a rota para o ponto de início e de fim especificado, para a sua chave de conta. Os seguintes parâmetros opcionais são utilizados para indicar a rota para um camião pesado:- O parâmetro `travelMode=truck` especifica o meio de transporte como *camião*. Outros meios de transporte suportados são *táxi*, *autocarro*, *carrinha*, *motociclo* e a predefinição *carro*.
        - Os parâmetros `vehicleWidth`, `vehicleHeight` e `vehicleLength` especificam as dimensões do veículo em metros e são considerados apenas se o meio de transporte for *camião*.
        - O `vehicleLoadType` classifica a carga como perigosa e restrita em algumas estradas. Atualmente, esta classificação também só é considerada para o meio *camião*.

2. Adicione o seguinte código JavaScript para obter a rota para um carro utilizando o Route Service:

    ```JavaScript
    // Perform a request to the route service and draw the resulting car route on the map
    var xhttpCar = new XMLHttpRequest();
    xhttpCar.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: carRouteLayerName
            });
        }
    };

    var carRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    carRouteUrl += "&api-version=1.0";
    carRouteUrl += "&subscription-key=" + LBSAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Este fragmento de código cria outro [XMLHttpRequest](https://xhr.spec.whatwg.org/) e adiciona um processador de eventos para analisar a resposta recebida. Para uma resposta com êxito, cria uma matriz de coordenadas para a rota devolvida e adiciona-a à camada `carRouteLayerName` do mapa. 
    
    Este fragmento de código também envia a consulta para o Route Service, para obter a rota para o ponto de início e de fim especificado, para a sua chave de conta. Uma vez que não são utilizados outros parâmetros, é devolvida a rota para o meio de transporte predefinido *carro*. 

3. Guarde o ficheiro **MapTruckRoute.html** localmente e, em seguida, abra-o num browser à escolha e veja o resultado. Para uma ligação com êxito à API do Location Based Services, deverá ver um mapa semelhante ao seguinte. 

    ![Rotas priorizadas com o Route Service do Azure](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Repare que a rota de camião é azul, enquanto a rota de carro é roxa.

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar uma consulta do Route Service
> * Compor rotas priorizadas por meio de transporte

Avance para os artigos **Conceitos** e **Guias de procedimentos** para aprender o SDK do Azure Location Based Services em profundidade. 
