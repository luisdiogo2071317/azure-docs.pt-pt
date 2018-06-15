---
title: Procurar com o Azure Maps | Microsoft Docs
description: Procurar um ponto de interesse nas proximidades com o Azure Maps
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 4a414b5b865d31dd99b54ef9693abafb5490a50f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601790"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Procurar pontos de interesse nas proximidades com o Azure Maps

Este tutorial mostra como configurar uma conta com o Azure Maps e, em seguida, utilizar as APIs do Maps para procurar um ponto de interesse. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma conta do Azure Maps
> * Obter a chave primária para a sua conta do Maps
> * Criar uma nova página Web com a API de controlo de mapas
> * Utilizar o serviço de pesquisa do Maps para localizar um ponto de interesse nas proximidades

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [Portal do Azure](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Criar uma conta com o Azure Maps

Crie uma nova conta dos Maps com os seguintes passos:

1. No canto superior esquerdo do [portal do Azure](https://portal.azure.com), clique em **Criar um recurso**.
2. Na caixa *Procurar no Marketplace*, escreva **Maps**.
3. Em *Resultados*, selecione **Maps**. Clique no botão **Criar** que aparece abaixo do mapa. 
4. Na página **Criar Conta do Maps**, introduza os seguintes valores:
    - O *Nome* da nova conta. 
    - A *Subscrição* que quer utilizar para esta conta.
    - O nome do *Grupo de recursos* para esta conta. Pode optar por *Criar um grupo de recursos novo* ou *Utilizar um grupo de recursos existente*.
    - Selecione a *Localização do grupo de recursos*.
    - Leia a *Licença* e a *Declaração de Privacidade*, e selecione a caixa de verificação para aceitar os termos. 
    - Clique no botão **Criar**.
   
    ![Criar a conta do Maps no portal](./media/tutorial-search-location/create-account.png)


<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Obter a chave primária para a sua conta

Depois de a sua conta do Maps ser criada com êxito, obtenha a chave que lhe permite consultar as APIs do Maps.

1. Abra a sua conta do Maps no portal.
2. Na secção das definições, selecione **Chaves**.
3. Copie a **Chave primária** para a área de transferência. Guarde-a localmente para a utilizar mais tarde neste tutorial. 

    ![Obter a Chave Primária no portal](./media/tutorial-search-location/get-key.png)


<a id="createmap"></a>

## <a name="create-a-new-map"></a>Criar um novo mapa 
A API de Controlo de Mapas é uma biblioteca de cliente prática que lhe permite integrar facilmente o Maps na sua aplicação Web. Oculta a complexidade das chamadas de serviço REST bare e aumenta a produtividade com componentes personalizáveis, cujo estilo pode ser definido. Os passos seguintes mostram como criar uma página HTML estática incorporada com a API de Controlo de Mapas. 

1. No seu computador local, crie um novo ficheiro e dê-lhe o nome **MapSearch.html**. 
2. Adicione os seguintes componentes HTML ao ficheiro:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

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
    Repare que o cabeçalho HTML inclui os ficheiros de recursos CSS e JavaScript alojados pela biblioteca de Controlo de Mapas do Azure. Tenha em atenção o segmento de *script* adicionado ao *corpo* do ficheiro HTML. Este segmento irá conter o código JavaScript inline para aceder às APIs do Azure Maps.
 
3. Adicione o seguinte código JavaScript ao bloco *script* do ficheiro HTML. Substitua a cadeia de texto **\<your account key\>** pela chave primária que copiou a partir da sua conta do Maps.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    Este segmento inicia a API de Controlo de Mapas para a sua chave de conta do Azure Maps. **Atlas** é o espaço de nomes que contém a API e os componentes visuais relacionados. Por sua vez, **Atlas.Map** fornece o controlo para um mapa Web interativo e visual. 
    
4. Guarde as alterações efetuadas ao ficheiro e abra a página HTML num browser. Este é o mapa mais básico que pode criar quando chama **atlas.map** e fornece a sua chave de conta. 

   ![Ver o mapa](./media/tutorial-search-location/basic-map.png)


<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Adicionar capacidades de pesquisa

Esta secção mostra como utilizar a API de Pesquisa do Maps para localizar um ponto de interesse no seu mapa. É uma API RESTful concebida para os programadores procurarem endereços, pontos de interesse e outras informações geográficas. O serviço de Pesquisa atribui informações de latitude e longitude a um endereço especificado. 

1. Adicione uma nova camada ao seu mapa para apresentar os resultados da pesquisa. Adicione o seguinte código Javascript ao bloco *script*, a seguir ao código que inicializa o mapa. 

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

2. Crie um [XMLHttpRequest](https://xhr.spec.whatwg.org/) e adicione um processador de eventos para analisar a resposta de JSON enviada pelo serviço de pesquisa do Maps. Este fragmento de código cria o processador de eventos para recolher os endereços, os nomes e as informações de latitude e longitude para cada localização devolvida na variável `searchPins`. Por fim, adiciona esta coleção de pontos de localização ao controlo `map` como marcadores. 

    ```JavaScript
    // Perform a request to the search service and create a pin on the map for each result
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        var searchPins = [];

        if (this.readyState === 4 && this.status === 200) {
            var response = JSON.parse(this.responseText);

            var poiResults = response.results.filter((result) => { return result.type === "POI" }) || [];

            searchPins = poiResults.map((poiResult) => {
                var poiPosition = [poiResult.position.lon, poiResult.position.lat];
                return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                    name: poiResult.poi.name,
                    address: poiResult.address.freeformAddress,
                    position: poiResult.position.lat + ", " + poiResult.position.lon
                });
            });

            map.addPins(searchPins, {
                name: searchLayerName
            });

            var lons = searchPins.map((pin) => { return pin.geometry.coordinates[0] });
            var lats = searchPins.map((pin) => { return pin.geometry.coordinates[1] });

            var swLon = Math.min.apply(null, lons);
            var swLat = Math.min.apply(null, lats);
            var neLon = Math.max.apply(null, lons);
            var neLat = Math.max.apply(null, lats);

            map.setCameraBounds({
                bounds: [swLon, swLat, neLon, neLat],
                padding: 50
            });
        }
    };
    ```

3. Adicione o seguinte código ao bloco *script* para criar a consulta e enviar o XMLHttpRequest para o serviço de Pesquisa do Maps:

    ```JavaScript
    var url = "https://atlas.microsoft.com/search/fuzzy/json?";
    url += "api-version=1.0";
    url += "&query=gasoline%20station";
    url += "&subscription-key=" + MapsAccountKey;
    url += "&lat=47.6292";
    url += "&lon=-122.2337";
    url += "&radius=100000";

    xhttp.open("GET", url, true);
    xhttp.send();
    ``` 
    Este fragmento utiliza a API de pesquisa básica do Search Service, denominada **Fuzzy Search**. Processa as entradas mais difusas, incluindo qualquer combinação de endereço ou tokens de ponto de interesse (POI). Procura **postos de combustível** nas proximidades dentro de um raio concreto das coordenadas de latitude e longitude especificadas. Utiliza a chave primária da sua conta fornecida anteriormente no ficheiro de exemplo para fazer a chamada para o Maps. Devolve os resultados como pares latitude/longitude para as localizações encontradas. 
    
4. Guarde o ficheiro **MapSearch.html** e atualize o browser. Deverá constatar que o mapa está agora centrado em Seattle e que os marcadores azuis marcam as localizações dos postos de combustível na área. 

   ![Ver o mapa com os resultados da pesquisa](./media/tutorial-search-location/pins-map.png)

5. Pode ver os dados não processados que o mapa está a compor ao tirar o XMLHTTPRequest criado no ficheiro e colocá-lo no seu browser. Substitua o texto \<your account key\> pela sua chave primária. 

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<your account key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Neste momento, a página de MapSearch pode apresentar as localizações dos pontos de interesse devolvidos por uma consulta de pesquisa difusa. Vamos adicionar algumas capacidades interativas e mais informações sobre as localizações. 

## <a name="add-interactive-data"></a>Adicionar dados interativos

O mapa criado até ao momento está focado apenas nos dados de latitude/longitude para os resultados da pesquisa. No entanto, se observar o JSON não processado devolvido pelo serviço de Pesquisa dos Maps, é possível reparar que contém informações adicionais sobre cada um dos postos de combustível, incluindo o nome e a morada. Pode incorporar esses dados no mapa com caixas de pop-up interativas. 

1. Adicione as seguintes linhas ao bloco de *script*, para criar pop-ups para os pontos de interesse devolvidos pelo Search Service:

    ```JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    A API **atlas.Popup** fornece uma janela de informações ancorada na posição necessária no mapa. Este fragmento de código define o conteúdo e a posição para o pop-up, e adiciona uma escuta de eventos ao controlo `map`, que aguarda que o _rato_ paire sobre o pop-up. 

4. Guarde o ficheiro e atualize o browser. Agora, o mapa no browser mostra pop-ups de informação quando paira o rato sobre qualquer um dos marcadores de pesquisa. 

    ![Search Service e Controlo de Mapas do Azure](./media/tutorial-search-location/popup-map.png)


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma conta com o Azure Maps
> * Obter a chave primária para a sua conta
> * Criar uma nova página Web com a API de Controlo de Mapas
> * Utilizar o Search Service para localizar pontos de interesse nas proximidades

O próximo tutorial demonstra como apresentar um percurso entre duas localizações. 

> [!div class="nextstepaction"]
> [Encaminhar para um destino](./tutorial-route-location.md)
