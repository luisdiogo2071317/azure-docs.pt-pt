---
title: Procurar com o Azure Maps | Microsoft Docs
description: Procurar um ponto de interesse nas proximidades com o Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 12/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 1396260ef703ce22f8e0309bd2c8df691d0af86e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120466"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Procurar pontos de interesse nas proximidades com o Azure Maps

Este tutorial mostra como configurar uma conta com o Azure Maps e, em seguida, utilizar as APIs do Maps para procurar um ponto de interesse. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma conta do Azure Maps
> * Obter a chave primária para a sua conta do Maps
> * Criar uma nova página Web com a API de controlo de mapas
> * Utilizar o serviço de pesquisa do Maps para localizar um ponto de interesse nas proximidades

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Criar uma conta com o Azure Maps

Crie uma nova conta dos Maps com os seguintes passos:

1. No canto superior esquerdo do [portal do Azure](https://portal.azure.com), clique em **Criar um recurso**.
2. Na caixa *Procurar no Marketplace*, escreva **Maps**.
3. Em *Resultados*, selecione **Maps**. Clique no botão **Criar** que aparece abaixo do mapa.
4. Na página **Criar Conta do Maps**, introduza os seguintes valores:
    * O *Nome* da nova conta.
    * A *Subscrição* que quer utilizar para esta conta.
    * O nome do *Grupo de recursos* para esta conta. Pode optar por *Criar um grupo de recursos novo* ou *Utilizar um grupo de recursos existente*.
    * Selecione a *Localização do grupo de recursos*.
    * Leia a *Licença* e a *Declaração de Privacidade*, e selecione a caixa de verificação para aceitar os termos.
    * Clique no botão **Criar**.

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
   <html>
   <head>
      <title>Map Search</title>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

      <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
      <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
      <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

      <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
      <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

      <script>      
         var map, datasource, client, popup;

         function GetMap(){
            //Add Map Control JavaScript code here.
         }
      </script>
      <style>
      html,
      body {
         width: 100%;
         height: 100%;
         padding: 0;
         margin: 0;
      }

      #myMap {
         width: 100%;
         height: 100%;
      }
      </style>
   </head>
   <body onload="GetMap()">
      <div id="myMap"></div>
   </body>
   </html>
   ```

   Repare que o cabeçalho HTML inclui os ficheiros de recursos CSS e JavaScript alojados pela biblioteca de Controlo de Mapas do Azure. Observe o evento `onload` no corpo da página, que irá chamar a função `GetMap` quando o corpo da página for carregada. Esta função irá conter o código JavaScript inline para aceder às APIs do Azure Maps.

3. Adicione o seguinte código JavaScript à função `GetMap` do ficheiro HTML. Substitua a cadeia **\<A Sua Chave do Azure Maps\>** pela chave primária que copiou a partir da sua conta do Maps.

   ```JavaScript
   //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
   atlas.setSubscriptionKey('<Your Azure Maps Key>');

   //Initialize a map instance.
   map = new atlas.Map('myMap');
   ```

   Este segmento inicializa a API de Controlo de Mapas para a sua chave de conta do Azure Maps. **atlas** é o espaço de nomes que contém a API e os componentes visuais relacionados. **atlas.Map** fornece o controlo para um mapa Web interativo e visual. 

4. Guarde as alterações efetuadas ao ficheiro e abra a página HTML num browser. Este é o mapa mais básico que pode criar quando chama **atlas.map** com a sua chave de conta.

   ![Ver o mapa](./media/tutorial-search-location/basic-map.png)

5. Na função `GetMap`, após inicializar o mapa, adicione o seguinte código JavaScript. 

   ```JavaScript
   //Wait until the map resources have fully loaded.
   map.events.add('load', function () {

      //Create a data source and add it to the map.
      datasource = new atlas.source.DataSource();
      map.sources.add(datasource);

      //Add a layer for rendering point data.
      var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
         iconOptions: {
            iconImage: 'pin-round-darkblue',
            anchor: 'center',
            allowOverlap: true
         }
      });
      map.layers.add(resultLayer);

      //Create a popup but leave it closed so we can update it and display it later.
      popup = new atlas.Popup();

      //Add a mouse over event to the result layer and display a popup when this event fires.
      map.events.add('mouseover', resultLayer, showPopup);
   });
   ```

   Um evento de carregamento é adicionado ao mapa, que será acionado quando os recursos do mapa forem totalmente carregados. No processador de eventos de carregamento do mapa, é criada uma origem de dados para armazenar os dados de resultado. É criada e anexada uma camada de símbolo à origem de dados. Esta camada especifica como os dados de resultado na origem de dados devem ser compostos, neste caso com um ícone de pino redondo azul escuro centrado sobre a coordenada de resultados e que permite que outros ícones se sobreponham. 

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Adicionar capacidades de pesquisa

Esta secção mostra como utilizar a API de Pesquisa do Maps para localizar um ponto de interesse no seu mapa. É uma API RESTful concebida para os programadores procurarem endereços, pontos de interesse e outras informações geográficas. O serviço de Pesquisa atribui informações de latitude e longitude a um endereço especificado. O **Módulo Serviço** explicado abaixo pode ser utilizado para procurar uma localização com a API de Pesquisa do Maps.

### <a name="service-module"></a>Módulo Serviço

1. No processador de eventos de carregamento do mapa, crie uma instância do serviço de cliente ao adicionar o seguinte código Javascript.

    ```JavaScript
    //Create an instance of the services client.
     client = new atlas.service.Client(atlas.getSubscriptionKey());
    ```

2. Em seguida, adicione o seguinte bloco de script para criar a consulta de pesquisa. Utiliza o Fuzzy Search Service, que é uma API de pesquisa básica do Search Service. O Fuzzy Search Service processa a maioria das entradas difusas, como quaisquer endereços, lugares e pontos de interesse (POI). Este código procura Postos de Gasolina nas proximidades dentro do raio especificado. A resposta, em seguida, é analisada em formato GeoJSON e adicionada à origem de dados, o que resulta automaticamente nos dados que estão a ser compostos no mapa através da camada de símbolo. A última parte do script define a vista da câmera dos mapas com a caixa delimitadora dos resultados através da propriedade [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) do Mapa. É adicionado um preenchimento para compensar as dimensões de píxel dos ícones de símbolo, dado que a caixa delimitadora é calculada com base nas coordenadas. 
 
   ```JavaScript
   //Execute a POI search query then add the results to the map.
    client.search.getSearchPOI('gasoline station', {
        lat: 47.6292,
        lon: -122.2337,
        radius: 100000
    }).then(response => {
        //Parse the response into GeoJSON so that the map can understand.
        var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);
        var results = geojsonResponse.getGeoJsonResults();

        //Add the results to the data source so they can be rendered. 
        datasource.add(results);

        // Set the camera bounds
        map.setCamera({
            bounds: results.bbox,
            padding: 50
        });
    });
   ```
 
3. Guarde o ficheiro **MapSearch.html** e atualize o browser. Deverá constatar que o mapa está agora centrado em Seattle, com os marcadores azuis a marcar as localizações dos postos de combustível na área.

   ![Ver o mapa com os resultados da pesquisa](./media/tutorial-search-location/pins-map.png)

4. Pode ver os dados não processados que o mapa está a compor, introduzindo o seguinte HTTPRequest no seu browser. Substitua a \<Sua Chave do Azure Maps\> pela chave primária.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<Your Azure Maps Key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Neste momento, a página de MapSearch pode apresentar as localizações dos pontos de interesse devolvidos por uma consulta de pesquisa difusa. Vamos adicionar algumas capacidades interativas e mais informações sobre as localizações.

## <a name="add-interactive-data"></a>Adicionar dados interativos

O mapa criado até ao momento está focado apenas nos dados de longitude/latitude para os resultados da pesquisa. No entanto, se observar o JSON não processado devolvido pelo serviço de Pesquisa dos Maps, é possível reparar que contém informações adicionais sobre cada um dos postos de combustível, incluindo o nome e a morada. Pode incorporar esses dados no mapa com caixas de pop-up interativas.

1. Adicione as seguintes linhas de código no processador de eventos de carregamento do mapa depois do código, para consultar o serviço de pesquisa difusa. Isto irá criar uma instância de uma Pop-up e adicionar um evento de mouseover para a camada de símbolo.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```
    
    A API **atlas.Popup** fornece uma janela de informações ancorada na posição necessária no mapa. 
      
2. Na etiqueta do *script*, após a função `GetMap`, adicione o seguinte código para mostrar as informações do resultado com o rato parado sobre a pop-up. 

   ```JavaScript
   function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occurred on.
        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = ['<div style="padding:5px"><div><b>', p.poi.name,
            '</b></div><div>', p.address.freeformAddress,
            '</div><div>', position[1], ', ', position[0], '</div></div>'];

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html.join(''),
            position: position
        });

        //Open the popup.
        popup.open(map);
   }
   ```

2. Guarde o ficheiro e atualize o browser. Agora, o mapa no browser mostra pop-ups de informação quando paira o rato sobre qualquer um dos marcadores de pesquisa.

    ![Search Service e Controlo de Mapas do Azure](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma conta com o Azure Maps
> * Obter a chave primária para a sua conta
> * Criar uma nova página Web com a API de Controlo de Mapas
> * Utilizar o Search Service para localizar pontos de interesse nas proximidades

Pode aceder ao código de exemplo deste tutorial aqui:

> [Procurar localização com o Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

[Veja o exemplo aqui ao vivo](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

O próximo tutorial demonstra como apresentar um percurso entre duas localizações.

> [!div class="nextstepaction"]
> [Encaminhar para um destino](./tutorial-route-location.md)
