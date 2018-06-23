---
title: Aplicação de web de página única de pesquisa de entidades do Bing | Microsoft Docs
description: Mostra como utilizar a API de pesquisa do Bing entidade numa aplicação Web de página única.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/08/2017
ms.author: v-jerkin
ms.openlocfilehash: 91c60913cd806baf100e5511cbf59299bf9a84f0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354920"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: Aplicação de web de página única

A API de pesquisa do Bing entidade permite-lhe procurar Web para obter informações sobre *entidades* e *coloca.* Pode pedir o tipo de resultado, ou ambos, numa consulta especificada. As definições de locais e entidades são fornecidas em baixo.

|||
|-|-|
|Entidades|Well-known pessoas, locais e coisas que encontrará por nome|
|Locais|Restaurants, hotéis e outras empresas locais que encontrar ao nome *ou* por tipo (restaurants italiano)|

Neste tutorial, iremos criar uma aplicação de Web de página única que utiliza a API de pesquisa de entidades do Bing para apresentar os resultados da pesquisa à direita na página. A aplicação inclui os componentes HTML, CSS e JavaScript.

A API permite-lhe atribuir prioridades resultados por localização. Na aplicação móvel, pode colocar o dispositivo para a respetiva localização. Numa aplicação Web, pode utilizar o `getPosition()` função. Mas esta chamada só funciona no contextos segurados e, se não fornecer uma localização precisa. Além disso, o utilizador poderá querer pesquisar entidades perto de uma localização que não sejam os seus próprios.

A nossa aplicação chama-se, por conseguinte, após o serviço do Bing Maps para obter a latitude e longitude de uma localização de utilizador introduzido. O utilizador pode, em seguida, introduza o nome de um landmark ("espaço agulha") ou um endereço total ou parcial ("Nova Iorque cidade") e a API do Bing Maps fornece as coordenadas.

<!-- Remove until we can replace with a sanitized version.
![[Single-page Bing Entity Search app]](media/entity-search-spa-demo.png)
-->

> [!NOTE]
> Os cabeçalhos de JSON e HTTP na parte inferior da página revelar a resposta JSON e informações de pedidos de HTTP quando clicado. Estes detalhes são úteis quando explorar o serviço.

A aplicação tutorial ilustra como:

> [!div class="checklist"]
> * Efetuar uma chamada de API de pesquisa do Bing entidade em JavaScript
> * Efetuar uma Bing Maps `locationQuery` chamada de API em JavaScript
> * Opções de pesquisa de passar para as chamadas de API
> * Apresentar os resultados da pesquisa
> * Processar os Bing ID e a API subscrição chaves cliente
> * Lidar com os erros que possam ocorrer

A página tutorial é inteiramente autónomo; Não utilize qualquer estruturas externas, folhas de estilo ou até mesmo os ficheiros de imagem. Utiliza apenas funcionalidades de linguagem JavaScript amplamente suportadas e funciona com versões atuais dos todos os principais browsers.

Neste tutorial, vamos discutir apenas selecionadas partes do código fonte. O código de origem completo está disponível [numa página separada](tutorial-bing-entities-search-single-page-app-source.md). Copie e cole este código para um editor de texto e guarde-o como `bing.html`.

> [!NOTE]
> Este tutorial é substancialmente semelhante para o [tutorial de aplicação de pesquisa na Web Bing de página única](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), mas lida apenas com os resultados da pesquisa de entidade.

## <a name="app-components"></a>Componentes de aplicação

Como qualquer aplicação Web de página única, o tutorial da aplicação inclui três partes:

> [!div class="checklist"]
> * HTML - define a estrutura e conteúdo da página
> * CSS - define o aspecto da página
> * JavaScript - define o comportamento da página

Este tutorial não abrange a maioria dos HTML ou CSS detalhadamente, conforme forem simples.

No código HTML contém o formulário de pesquisa na qual o utilizador introduz uma consulta e escolhe opções de pesquisa. O formulário está ligado à JavaScript que executa efetivamente a pesquisa pelo `<form>` da tag `onsubmit` atributo:

```html
<form name="bing" onsubmit="return newBingEntitySearch(this)">
```

O `onsubmit` processador devolve `false`, que mantém o formulário sejam submetidas ao servidor. O código JavaScript, na verdade, efetua o trabalho de recolher as informações necessárias do formulário e efetuar a pesquisa.

A procura é efetuada em duas fases. Em primeiro lugar, se o utilizador tem de introduzir uma restrição de localização, é feita uma consulta do Bing Maps para convertê-la num coordenadas. A chamada de retorno para esta consulta, em seguida, arranca, a consulta de pesquisa do Bing entidade.

No código HTML também contém de divisões (HTML `<div>` etiquetas) onde os resultados de pesquisa são apresentados.

## <a name="managing-subscription-keys"></a>Gerir chaves de subscrição

> [!NOTE]
> Esta aplicação precisa de chaves de subscrição para a API de pesquisa do Bing e a API do Bing Maps. Pode utilizar um [avaliação chave de pesquisa do Bing](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) e um [chave do Bing Maps básico](https://www.microsoft.com/maps/create-a-bing-maps-key).

Para evitar ter de incluir as chaves de subscrição de pesquisa do Bing e Bing Maps API no código, utilizamos armazenamento persistente do browser para armazená-las. Se a chave não tenha sido armazenado, iremos linha de comandos para a mesma e armazene-a para utilização posterior. Se a chave mais tarde é rejeitada pela API, iremos invalidar a chave armazenada, pelo que é pedido ao utilizador para o mesmo após a sua pesquisa seguinte.

Iremos definir `storeValue` e `retrieveValue` funções que utilizam o `localStorage` objeto (se o browser suportar) ou um cookie. A nossa `getSubscriptionKey()` função utiliza estas funções para armazenar e obter a chave do utilizador.

```javascript
// cookie names for data we store
SEARCH_API_KEY_COOKIE = "bing-search-api-key";
MAPS_API_KEY_COOKIE   = "bing-maps-api-key";
CLIENT_ID_COOKIE      = "bing-search-client-id";

// API endpoints
SEARCH_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/entities";
MAPS_ENDPOINT   = "http://dev.virtualearth.net/REST/v1/Locations";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey(cookie_name, key_length, api_name) {
    var key = retrieveValue(cookie_name);
    while (key.length !== key_length) {
        key = prompt("Enter " + api_name + " API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(cookie_name, key);
    return key;
}

function getMapsSubscriptionKey() {
    return getSubscriptionKey(MAPS_API_KEY_COOKIE, 64, "Bing Maps");
}

function getSearchSubscriptionKey() {
    return getSubscriptionKey(SEARCH_API_KEY_COOKIE, 32, "Bing Search");
}
```

No código HTML `<body>` tag inclui um `onload` atributo que chama `getSearchSubscriptionKey()` e `getMapsSubscriptionKey()` quando a página concluiu o carregamento. Estas chamadas servem para imediatamente solicitar ao utilizador para as respetivas chaves se estes ainda ainda não introduzido-los.

```html
<body onload="document.forms.bing.query.focus(); getSearchSubscriptionKey(); getMapsSubscriptionKey();">
```

## <a name="selecting-search-options"></a>Selecionar opções de pesquisa

![[Formulário de pesquisa do Bing entidade]](media/entity-search-spa-form.png)

O formulário HTML inclui os seguintes controlos:

| | |
|-|-|
|`where`|Um menu pendente para selecionar o mercado (localização e de idioma) utilizado para a pesquisa.|
|`query`|O campo de texto em que a introduza os termos de pesquisa.|
|`safe`|Uma caixa de verificação que indica se SafeSearch está ativada (restringe resultados "para adultos")|
|`what`|Um menu para escolher a procurar entidades, locais ou ambos.|
|`mapquery`|O campo de texto no qual o utilizador pode introduzir um endereço total ou parcial, um landmark, etc., para ajudar os resultados da pesquisa do Bing entidade retorno mais relevantes.|

> [!NOTE]
> Resultados de locais estão atualmente disponíveis apenas nos Estados Unidos. O `where` e `what` menus têm código para impor esta restrição. Se escolher um mercado não-US enquanto locais está selecionado no `what` menu, `what` é alterado para qualquer coisa. Se optar por locais enquanto um mercado não-US é selecionado no `where` menu, `where` é alterado para EUA.

A nossa função JavaScript `bingSearchOptions()` converte estes campos a uma cadeia de consulta parcial para a API de pesquisa do Bing.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.what.selectedIndex) options.push("responseFilter=" + form.what.value);
    return options.join("&");
}
```

Por exemplo, pode ser a funcionalidade de SafeSearch `strict`, `moderate`, ou `off`, com `moderate` a ser a predefinição. Mas nosso formulário utiliza uma caixa de verificação, que tem apenas dois Estados. O código JavaScript converte esta definição para o `strict` ou `off` (não utilizamos `moderate`).

O `mapquery` campo não é processado em `bingSearchOptions()` porque é utilizada para a consulta de localização do Bing Maps, não da pesquisa do Bing entidade.

## <a name="obtaining-a-location"></a>A obtenção de uma localização

A API do Bing Maps oferece um [ `locationQuery` método](//msdn.microsoft.com/library/ff701711.aspx), que são utilizadas para localizar a latitude e longitude de localização do utilizador introduz. Estes coordenadas, em seguida, são transmitidas para a API de pesquisa do Bing entidade com o pedido do utilizador. Os resultados da pesquisa definir prioridades de entidades e locais que estão próximos a localização especificada.

Não é possível aceder a API do Bing Maps utilizando um comum `XMLHttpRequest` consultar numa aplicação Web, porque o serviço não suporta consultas de várias origens. Felizmente, suporta JSONP ("P" é de "preenchido até"). Uma resposta JSONP é uma resposta JSON comum moldada numa chamada de função. O pedido é efetuado através da inserção utilizando um `<script>` etiquetas no documento. (Carregar scripts não está sujeita às políticas de segurança do browser.)

O `bingMapsLocate()` função cria e insere a `<script>` etiqueta para a consulta. O `jsonp=bingMapsCallback` segmento da cadeia de consulta Especifica o nome da função a chamar com a resposta.

```javascript
function bingMapsLocate(where) {

    where = where.trim();
    var url = MAPS_ENDPOINT + "?q=" + encodeURIComponent(where) + 
                "&jsonp=bingMapsCallback&maxResults=1&key=" + getMapsSubscriptionKey();

    var script = document.getElementById("bingMapsResult")
    if (script) script.parentElement.removeChild(script);

    // global variable holds reference to timer that will complete the search if the maps query fails
    timer = setTimeout(function() {
        timer = null;
        var form = document.forms.bing;
        bingEntitySearch(form.query.value, "", bingSearchOptions(form), getSearchSubscriptionKey());
    }, 5000);

    script = document.createElement("script");
    script.setAttribute("type", "text/javascript");
    script.setAttribute("id", "bingMapsResult");
    script.setAttribute("src", url);
    script.setAttribute("onerror", "BingMapsCallback(null)");
    document.body.appendChild(script);

    return false;
}
```

> [!NOTE]
> Se não responder a API do Bing Maps, o `bingMapsCallBack()` função nunca é chamada. Normalmente, tal significa que que `bingEntitySearch()` não é chamado, e os resultados da pesquisa de entidade não aparecem. Para evitar este cenário, `bingMapsLocate()` também define um temporizador para chamar `bingEntitySearch()` após cinco segundos. Não há lógica na função de chamada de retorno para evitar a efetuar a pesquisa de entidade duas vezes.

Quando a consulta estiver concluída, o `bingMapsCallback()` função é chamada, conforme solicitado.

```javascript
function bingMapsCallback(response) {

    if (timer) {    // we beat the timer; stop it from firing
        clearTimeout(timer);
        timer = null;
    } else {        // the timer beat us; don't do anything
        return; 
    }

    var location = "";
    var name = "";
    var radius = 1000;

    if (response) {
        try {
            if (response.statusCode === 401) {
                invalidateMapsKey();
            } else if (response.statusCode === 200) {
                var resource = response.resourceSets[0].resources[0];
                var coords   = resource.point.coordinates;
                name         = resource.name;

                // the radius is the largest of the distances between the location and the corners
                // of its bounding box (in case it's not in the center) with a minimum of 1 km
                try {
                    var bbox    = resource.bbox;
                    radius  = Math.max(haversineDistance(bbox[0], bbox[1], coords[0], coords[1]),
                                       haversineDistance(coords[0], coords[1], bbox[2], bbox[1]),
                                       haversineDistance(bbox[0], bbox[3], coords[0], coords[1]),
                                       haversineDistance(coords[0], coords[1], bbox[2], bbox[3]), 1000);
                } catch(e) {  }
                var location = "lat:" + coords[0] + ";long:" + coords[1] + ";re:" + Math.round(radius);
            }
        }
        catch (e) { }   // response is unexpected. this isn't fatal, so just don't provide location
    }

    var form = document.forms.bing;
    if (name) form.mapquery.value = name;
    bingEntitySearch(form.query.value, location, bingSearchOptions(form), getSearchSubscriptionKey());

}
```

Juntamente com a latitude e longitude, a consulta de pesquisa do Bing entidade requer um *radius* que indica a precisão das informações de localização. Iremos calcular o radius utilizando o *delimitadora caixa* fornecido na resposta do Bing Maps. A caixa delimitadora é um retângulo que rodeia a localização de toda. Por exemplo, se o utilizador introduz `NYC`, o resultado contém aproximadamente central coordenadas de Nova Iorque cidade e uma caixa delimitadora que abrange a cidade. 

Vamos primeiro calcular as distâncias das coordenadas primárias para cada uma de quatro cantos da caixa delimitadora utilizando a função `haversineDistance()` (não apresentado). Utilizamos o maior destas quatro distâncias como o radius. O mínimo radius é um kilometer. Este valor também é utilizado por predefinição, se não for fornecida nenhum caixa delimitadora na resposta.

Ter adquirido as coordenadas e o radius, em seguida, chamamos `bingEntitySearch()` para efetuar a pesquisa real.

## <a name="performing-the-search"></a>Efetuar a pesquisa

Dada a consulta, uma localização, uma cadeia de opções e a chave de API, o `BingEntitySearch()` função efetua o pedido de pesquisa do Bing entidade.

```javascript
// perform a search given query, location, options string, and API keys
function bingEntitySearch(query, latlong, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "error");

    var request = new XMLHttpRequest();
    var queryurl = SEARCH_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

    // open the request
    try {
        request.open("GET", queryurl);
    } 
    catch (e) {
        renderErrorMessage("Bad request (invalid URL)\n" + queryurl);
        return false;
    }

    // add request headers
    request.setRequestHeader("Ocp-Apim-Subscription-Key", key);
    request.setRequestHeader("Accept", "application/json");

    var clientid = retrieveValue(CLIENT_ID_COOKIE);
    if (clientid) request.setRequestHeader("X-MSEdge-ClientID", clientid);

    if (latlong) request.setRequestHeader("X-Search-Location", latlong);

    // event handler for successful response
    request.addEventListener("load", handleBingResponse);
    
    // event handler for erorrs
    request.addEventListener("error", function() {
        renderErrorMessage("Error completing request");
    });

    // event handler for aborted request
    request.addEventListener("abort", function() {
        renderErrorMessage("Request aborted");
    });

    // send the request
    request.send();
    return false;
}
```

Após a conclusão com êxito do pedido HTTP, chamadas JavaScript a nossa `load` processador de eventos, o `handleBingResponse()` função, para processar um pedido de HTTP GET com êxito para a API. 

```javascript
// handle Bing search request results
function handleBingResponse() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // try to parse JSON results
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
    }

    // show raw JSON and HTTP request
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " + 
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // if HTTP response is 200 OK, try to render search results
    if (this.status === 200) {
        var clientid = this.getResponseHeader("X-MSEdge-ClientID");
        if (clientid) retrieveValue(CLIENT_ID_COOKIE, clientid);
        if (json.length) {
            if (jsobj._type === "SearchResponse") {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    if (divHidden("pole") && divHidden("mainline") && divHidden("sidebar")) 
        showDiv("noresults", "No results.<p><small>Looking for restaurants or other local businesses? Those currently areen't supported outside the US.</small>");
    }

    // Any other HTTP status is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSearchKey();

        // some error responses don't have a top-level errors object, so gin one up
        var errors = jsobj.errors || [jsobj];
        var errmsg = [];

        // display HTTP status code
        errmsg.push("HTTP Status " + this.status + " " + this.statusText + "\n");

        // add all fields from all error responses
        for (var i = 0; i < errors.length; i++) {
            if (i) errmsg.push("\n");
            for (var k in errors[i]) errmsg.push(k + ": " + errors[i][k]);
        }

        // also display Bing Trace ID if it isn't blocked by CORS
        var traceid = this.getResponseHeader("BingAPIs-TraceId");
        if (traceid) errmsg.push("\nTrace ID " + traceid);

        // and display the error message
        renderErrorMessage(errmsg.join("\n"));
    }
}
```

> [!IMPORTANT]
> Um pedido HTTP com êxito *não* significam necessariamente que que a pesquisa próprio foi concluída com êxito. Se ocorrer um erro na operação de pesquisa, a API de pesquisa do Bing entidade devolve um código de estado de HTTP não 200 e inclui informações de erro na resposta JSON. Além disso, se o pedido foi taxa limitado, a API devolve uma resposta vazia.

Muito do código em ambas as funções anteriores se encontra dedicada para processamento de erros. Poderão ocorrer erros nas seguintes fases:

|Fase|Potenciais erros|Processado pelo|
|-|-|-|
|Objeto de pedido de JavaScript de criação|URL inválido|`try`/`catch` Bloco|
|Efetuar o pedido|Erros de rede, ligações abortadas|`error` e `abort` processadores de eventos|
|Efetuar a pesquisa|Inválido limites de velocidade do pedido, JSON inválido,|os testes no `load` processador de eventos|

São processados os erros ao chamar `renderErrorMessage()` com os detalhes sobre o erro. Se a resposta passar o gauntlet completo de testes de erro, chamamos `renderSearchResults()` para apresentar os resultados da procura na página.

## <a name="displaying-search-results"></a>Apresentar os resultados da pesquisa

A API de pesquisa do Bing entidade [requer a apresentar os resultados por uma ordem especificada](use-display-requirements.md). Uma vez que a API pode devolver dois tipos diferentes de respostas, não é suficiente iterar através de nível superior `Entities` ou `Places` coleção na resposta JSON e apresenta esses resultados. (Se pretender que apenas um tipo de resultado, utilize o `responseFilter` parâmetro de consulta.)

Em vez disso, utilizamos o `rankingResponse` coleção nos resultados da pesquisa para ordenar os resultados para apresentação. Este objeto faz referência a itens de `Entitiess` e/ou `Places` coleções.

`rankingResponse` pode conter até três coleções dos resultados da pesquisa, designados `pole`, `mainline`, e `sidebar`. 

`pole`, se estiver presente, é o resultado de pesquisa mais relevante e devem ser apresentados de forma destacada. `mainline` refere-se o volume dos resultados da pesquisa. Resultados mainline devem ser apresentados imediatamente após `pole` (ou se primeiro, `pole` não estiver presente). 

Por fim. `sidebar` refere-se os resultados da pesquisa auxiliar. Poderão ser apresentadas numa barra lateral real ou simplesmente depois os resultados mainline. Escolhemos esta última opção para a nossa aplicação tutorial.

Cada item num `rankingResponse` coleção refere-se aos itens do resultado da pesquisa real de duas formas diferentes, mas equivalentes.

| | |
|-|-|
|`id`|O `id` aparente ser um URL, mas não deve ser utilizado para ligações. O `id` corresponde ao tipo de um resultado de classificação de `id` de qualquer um item de resultado da pesquisa numa coleção de resposta, *ou* uma coleção de resposta completo (tais como `Entities`).
|`answerType`<br>`resultIndex`|O `answerType` refere-se para a coleção de nível superior de resposta que contém o resultado (por exemplo, `Entities`). O `resultIndex` refere-se ao índice do resultado nessa coleção. Se `resultIndex` for omitido, o resultado de classificação refere-se para a coleção completa.

> [!NOTE]
> Para obter mais informações sobre esta parte da resposta de pesquisa, consulte [resultados de classificação](rank-results.md).

Pode utilizar qualquer método de localizar o item de resultado da pesquisa referenciado é mais conveniente para a sua aplicação. No nosso código do tutorial, utilizamos o `answerType` e `resultIndex` localizar cada resultado da pesquisa.

Por fim, está na altura para ver a nossa função `renderSearchResults()`. Esta função itera três `rankingResponse` coleções que representam as três secções dos resultados da pesquisa. Para cada secção, vamos chamar `renderResultsItems()` para compor os resultados para essa secção.

```javascript
// render the search results given the parsed JSON response
function renderSearchResults(results) {

    // if spelling was corrected, update search field
    if (results.queryContext.alteredQuery) 
        document.forms.bing.query.value = results.queryContext.alteredQuery;

    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

## <a name="rendering-result-items"></a>Itens do resultado composição

No nosso JavaScript código é um objeto, `searchItemRenderers`, que contém *compositores:* funções que geram HTML para cada tipos de resultados de pesquisa.

```javascript
searchItemRenderers = { 
    entities: function(item) { ... },
    places: function(item) { ... }
}
```

Uma função de compositor pode aceitar os seguintes parâmetros:

| | |
|-|-|
|`item`|O objecto de JavaScript que contém propriedades do item, tal como o respetivo URL e a respetiva descrição.|
|`index`|O índice do item de resultado dentro da respetiva coleção.|
|`count`|O número de itens na coleção de itens de resultado de pesquisa.|

O `index` e `count` parâmetros podem ser utilizados para resultados número, para gerar HTML especial para o início ou fim de uma coleção para inserir quebras de linha depois de um determinado número de itens e assim sucessivamente. Se a um compositor não precisa desta funcionalidade, não é necessário aceitar estes dois parâmetros. Na verdade, iremos não utilizá-los as compositores para a nossa aplicação tutorial.

Vamos observá o `entities` compositor:

```javascript
    entities: function(item) {
        var html = [];
        html.push("<p class='entity'>");
        if (item.image) {
            var img = item.image;
            if (img.hostPageUrl) html.push("<a href='" + img.hostPageUrl + "'>");
            html.push("<img src='" + img.thumbnailUrl +  "' title='" + img.name + "' height=" + img.height + " width= " + img.width + ">");
            if (img.hostPageUrl) html.push("</a>");
            if (img.provider) {
                var provider = img.provider[0];
                html.push("<small>Image from ");
                if (provider.url) html.push("<a href='" + provider.url + "'>");
                html.push(provider.name ? provider.name : getHost(provider.url));
                if (provider.url) html.push("</a>");
                html.push("</small>");
            }
        }
        html.push("<p>");
        if (item.entityPresentationInfo) {
            var pi = item.entityPresentationInfo;
            if (pi.entityTypeHints || pi.entityTypeDisplayHint) {
                html.push("<i>");
                if (pi.entityTypeDisplayHint) html.push(pi.entityTypeDisplayHint);
                else if (pi.entityTypeHints) html.push(pi.entityTypeHints.join("/"));
                html.push("</i> - ");
            }
        }
        html.push(item.description);
        if (item.webSearchUrl) html.push("&nbsp;<a href='" + item.webSearchUrl + "'>More</a>")
        if (item.contractualRules) {
            html.push("<p><small>");
            var rules = [];
            for (var i = 0; i < item.contractualRules.length; i++) {
                var rule = item.contractualRules[i];
                var link = [];
                if (rule.license) rule = rule.license;
                if (rule.url) link.push("<a href='" + rule.url + "'>");
                link.push(rule.name || rule.text || rule.targetPropertyName + " source");
                if (rule.url) link.push("</a>");
                rules.push(link.join(""));
            }
            html.push("License: " + rules.join(" - "));
            html.push("</small>");
        }
        return html.join("");
    }, // places renderer omitted
```

A nossa função compositor de entidade:

> [!div class="checklist"]
> * Baseia-se no código HTML `<img>` etiquetas para apresentar a miniatura de imagem, se aplicável. 
> * Baseia-se no código HTML `<a>` tag que as ligações para a página que contém a imagem.
> * Baseia-se a descrição que apresenta informações sobre a imagem e o site que está ativada.
> * Incorpora a classificação da entidade utilizando as sugestões de apresentação, se aplicável.
> * Inclui uma ligação para uma pesquisa do Bing para obter mais informações sobre a entidade.
> * Mostra informações de licenciamento ou de atribuição de origens de dados.

## <a name="persisting-client-id"></a>ID de cliente persistentes

As respostas a partir de APIs de pesquisa do Bing podem incluir um `X-MSEdge-ClientID` cabeçalho que deve ser enviado para a API com os pedidos sucessivos. Se estão a ser utilizadas várias APIs de pesquisa do Bing, o mesmo ID de cliente deve utilizado com todos eles, se possível.

Fornecer o `X-MSEdge-ClientID` cabeçalho permite que as APIs do Bing para associar todas as pesquisas de um utilizador, que tem duas vantagens importantes.

Em primeiro lugar, permite que o Bing motor de busca aplicar passado contexto a procura para localizar os resultados que melhor satisfazem o utilizador. Se um utilizador tem procurado anteriormente para os termos relacionados com sailing, por exemplo, uma pesquisa posterior para "docks" poderá preferentially devolver informações sobre locais para ancorar um sailboat.

Segundo, o Bing aleatoriamente pode selecionar os utilizadores a experiência de novas funcionalidades antes de serem efetuadas amplamente disponíveis. Fornecer o mesmo ID de cliente com cada pedido garante que os utilizadores que escolheu para ver uma funcionalidade sempre veem-lo. Sem o ID de cliente, o utilizador poderá ver uma funcionalidade são apresentados e desaparecer, seemingly aleatória, os resultados da procura.

Políticas de segurança do browser (CORS) poderão impedir o `X-MSEdge-ClientID` cabeçalho estejam disponíveis para JavaScript. Esta limitação ocorre quando a resposta de pesquisa tem uma origem diferente da página de pedido-lo. Num ambiente de produção, deve abordar esta política ao alojar um script do lado do servidor que faz a chamada de API no mesmo domínio que a página Web. Uma vez que o script tem a mesma origem como a página Web, o `X-MSEdge-ClientID` cabeçalho, em seguida, está disponível para JavaScript.

> [!NOTE]
> Uma aplicação Web de produção, deve efetuar o pedido lado do servidor mesmo assim. Caso contrário, a chave de API de pesquisa do Bing têm de ser incluída na página Web, onde se encontram disponíveis para qualquer pessoa que vistas de origem. É-lhe faturado para utilização de todas as sob a chave de subscrição de API, mesmo pedidos efetuados por entidades confiadoras não autorizadas, pelo que é importante não para expor a sua chave.

Para fins de desenvolvimento, pode efetuar o pedido de API de pesquisa do Bing Web através de um proxy CORS. A resposta de um proxy essa tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta whitelists e disponibiliza-los JavaScript.

É fácil instalar um proxy CORS para permitir que a nossa aplicação tutorial para o cliente de acesso cabeçalho de ID. Primeiro, se ainda não tiver, [instale o Node.js](https://nodejs.org/en/download/). Em seguida, emita o seguinte comando numa janela de comandos:

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de pesquisa na Web Bing no ficheiro HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Por fim, inicie o proxy CORS com o seguinte comando:

    cors-proxy-server

Deixe a janela de comandos aberta e, ao utilizar a aplicação tutorial; fechar a janela interrompe o proxy. Na secção de cabeçalhos de HTTP expansível abaixo os resultados da pesquisa, agora, pode ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada pedido.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API de pesquisa do Bing entidade](//docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)

> [!div class="nextstepaction"]
> [Documentação da API do Bing Maps](//msdn.microsoft.com/library/dd877180.aspx)
