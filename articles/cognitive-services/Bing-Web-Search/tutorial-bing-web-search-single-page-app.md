---
title: Aplicação de Web de página única de pesquisa na Web Bing | Microsoft Docs
description: Mostra como utilizar a API de pesquisa do Bing Web numa aplicação Web de página única.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: f22e38a1d6ee4042684b9822b58669bed6fe29a0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354602"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: Aplicação de Web de página única

A API de pesquisa do Bing Web permite-lhe procurar Web e obter resultados dos tipos variando relevantes para uma consulta de pesquisa. Neste tutorial, iremos criar uma aplicação de Web de página única que utiliza a API de pesquisa do Bing Web para apresentar os resultados da pesquisa à direita na página. A aplicação inclui os componentes HTML, CSS e JavaScript.

<!-- Remove until this can be replaced with a sanitized version.
![[Single-page Bing Web Search app]](media/cognitive-services-bing-web-api/web-search-spa-demo.png)
-->

> [!NOTE]
> Os cabeçalhos de JSON e HTTP na parte inferior da página revelar a resposta JSON e informações de pedidos de HTTP quando clicado. Estes detalhes são úteis quando explorar o serviço.

A aplicação tutorial ilustra como:

> [!div class="checklist"]
> * Efetuar uma chamada de API de pesquisa do Bing Web JavaScript
> * Opções de pesquisa de passar para a API de pesquisa do Bing Web
> * Apresentar Web, notícias de última hora, a imagem e os resultados da pesquisa de vídeo
> * Página percorrer os resultados de pesquisa
> * Identificador do Bing cliente ID e a API chave de subscrição
> * Processar erros que possam ocorrer

A página tutorial é inteiramente autónomo; Não utilize qualquer estruturas externas, folhas de estilo ou até mesmo os ficheiros de imagem. Utiliza apenas funcionalidades de linguagem JavaScript amplamente suportadas e funciona com versões atuais dos todos os principais browsers.

Neste tutorial, vamos discutir apenas selecionadas partes do código fonte. O código de origem completo está disponível [numa página separada](tutorial-bing-web-search-single-page-app-source.md). Copie e cole este código para um editor de texto e guarde-o como `bing.html`.

## <a name="app-components"></a>Componentes de aplicação

Como qualquer aplicação Web de página única, o tutorial da aplicação inclui três partes:

> [!div class="checklist"]
> * HTML - define a estrutura e conteúdo da página
> * CSS - define o aspecto da página
> * JavaScript - define o comportamento da página

Este tutorial não abrange a maioria dos HTML ou CSS detalhadamente, conforme forem simples.

No código HTML contém o formulário de pesquisa na qual o utilizador introduz uma consulta e escolhe opções de pesquisa. O formulário está ligado à JavaScript que executa efetivamente a pesquisa pelo `<form>` da tag `onsubmit` atributo:

```html
<form name="bing" onsubmit="return newBingWebSearch(this)">
```

O `onsubmit` processador devolve `false`, que mantém o formulário sejam submetidas ao servidor. O código JavaScript, na verdade, efetua o trabalho de recolher as informações necessárias do formulário e efetuar a pesquisa.

No código HTML também contém de divisões (HTML `<div>` etiquetas) onde os resultados de pesquisa são apresentados.

## <a name="managing-subscription-key"></a>Gestão da chave de subscrição

Para evitar ter de incluir a chave de subscrição de API de pesquisa do Bing no código, iremos utilizar o armazenamento persistente do browser para armazenar a chave. Se nenhuma chave é armazenada, iremos linha de comandos para a chave do utilizador e armazene-a para utilização posterior. Se a chave mais tarde é rejeitada pela API, iremos invalidar a chave armazenada para que o utilizador será avisado novamente.

Iremos definir `storeValue` e `retrieveValue` funções que utilizam o `localStorage` objeto (se o browser suportar) ou um cookie. A nossa `getSubscriptionKey()` função utiliza estas funções para armazenar e obter a chave do utilizador.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/search";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey() {
    var key = retrieveValue(API_KEY_COOKIE);
    while (key.length !== 32) {
        key = prompt("Enter Bing Search API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(API_KEY_COOKIE, key);
    return key;
}
```

No código HTML `form` tag `onsubmit` chamadas a `bingWebSearch` função devolver resultados da pesquisa. `bingWebSearch` utiliza `getSubscriptionKey` para autenticar cada consulta. Conforme mostrado na definição do anterior, `getSubscriptionKey` pede ao utilizador para a chave, se a chave ainda não foi introduzida. A chave é armazenada, em seguida, para continuar a utilizar a aplicação.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Selecionar opções de pesquisa

![[Formulário de pesquisa na Web Bing]](media/cognitive-services-bing-web-api/web-search-spa-form.png)

O formulário HTML inclui elementos com os nomes seguintes:

| | |
|-|-|
| `where` | Um menu pendente para selecionar o mercado (localização e de idioma) utilizado para a pesquisa. |
| `query` | O campo de texto em que a introduza os termos de pesquisa. |
| `what` | Caixas de verificação para promover a tipos específicos de resultados. Por exemplo, imagens, a promoção aumenta a classificação de imagens. |
| `when` | Menu pendente para, opcionalmente, limitar a pesquisa para o dia, semana ou mês mais recente. |
| `safe` | Uma caixa de verificação que indica se deve utilizar a funcionalidade de SafeSearch de Bing para filtrar os resultados "para adultos". |
| `count` | Campo oculto. O número de resultados da pesquisa a devolver em cada pedido. Altere para apresentar resultados mais ou menos por página. |
| `offset` | Campo oculto. O deslocamento do resultado da pesquisa primeiro no pedido; utilizado para paginação. Este é reposto a `0` num pedido de novo. |

> [!NOTE]
> Pesquisa na Web Bing oferece muitas mais parâmetros de consulta. Estamos a utilizar apenas alguns dos-las aqui.

A função de JavaScript `bingSearchOptions()` converte estes campos para o formato necessário para a API de pesquisa do Bing.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var what = [];
    for (var i = 0; i < form.what.length; i++) 
        if (form.what[i].checked) what.push(form.what[i].value);
    if (what.length) {
        options.push("promote=" + what.join(","));
        options.push("answerCount=9");
    }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    options.push("textDecorations=true");
    options.push("textFormat=HTML");
    return options.join("&");
}
```

Por exemplo, o `SafeSearch` parâmetro uma chamada à API real pode ser `strict`, `moderate`, ou `off`, com `moderate` a ser a predefinição. No entanto, o nosso formulário, utiliza uma caixa de verificação, que tem apenas dois Estados. O código JavaScript converte esta definição para o `strict` ou `off` (`moderate` não for utilizado).

Se qualquer uma do **promover** caixas de verificação estão marcadas, iremos também adicionar um `answerCount` parâmetro da consulta. `answerCount` é necessário quando utilizar o `promote` parâmetro. Iremos simplesmente defina-o como `9` (o número de tipos de resultado suportado pela API de pesquisa do Bing Web) para se certificar de que é obter o número máximo possível dos tipos de resultado.

> [!NOTE]
> Promover um tipo de resultado não *garantir* que os resultados da pesquisa incluírem esse tipo de resultado. Em vez disso, a promoção aumenta a classificação desses tipos de resultados relativamente à respetiva classificação habitual. Para limitar a procura a tipos específicos de resultados, utilize o `responseFilter` parâmetro de consulta ou chamar um ponto final mais específico, tais como a pesquisa do Bing imagem ou de pesquisa do Bing notícias de última hora.

Também podemos enviar `textDecoration` e `textFormat` parâmetros para fazer com que o termo de pesquisa para boldfaced nos resultados da pesquisa de consulta. Estes valores são codificado no script.

## <a name="performing-the-request"></a>Executar o pedido

A consulta, a cadeia de opções e a chave de API, o `BingWebSearch` funcionar utiliza um `XMLHttpRequest` objeto para efetuar o pedido para o ponto final de pesquisa na Web Bing.

```javascript
// perform a search given query, options string, and API key
function bingWebSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
    var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

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
        return;
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
            if (jsobj._type === "SearchResponse" && "rankingResponse" in jsobj) {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    }

    // Any other HTTP response is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSubscriptionKey();

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
> Um pedido HTTP com êxito *não* significam necessariamente que que a pesquisa próprio foi concluída com êxito. Se ocorrer um erro na operação de pesquisa, a API de pesquisa do Bing Web devolve um código de estado de HTTP não 200 e inclui informações de erro na resposta JSON. Além disso, se o pedido foi taxa limitado, a API devolve uma resposta vazia.

Muito do código em ambas as funções anteriores se encontra dedicada para processamento de erros. Poderão ocorrer erros nas seguintes fases:

|Fase|Potenciais erros|Processado pelo|
|-|-|-|
|Objeto de pedido de JavaScript de criação|URL inválido|`try`/`catch` Bloco|
|Efetuar o pedido|Erros de rede, ligações abortadas|`error` e `abort` processadores de eventos|
|Efetuar a pesquisa|Inválido limites de velocidade do pedido, JSON inválido,|os testes no `load` processador de eventos|

São processados os erros ao chamar `renderErrorMessage()` com os detalhes sobre o erro. Se a resposta passar o gauntlet completo de testes de erro, chamamos `renderSearchResults()` para apresentar os resultados da procura na página.

## <a name="displaying-search-results"></a>Apresentar os resultados da pesquisa

A API de pesquisa do Bing Web [requer a apresentar os resultados por uma ordem especificada](useanddisplayrequirements.md). Uma vez que a API pode devolver os diferentes tipos de respostas, não é suficiente iterar através de nível superior `WebPages` coleção na resposta JSON e apresenta esses resultados. (Se pretender que apenas um tipo de resultados, utilize o `responseFilter` parâmetro de consulta ou de outro ponto final de pesquisa do Bing.)

Em vez disso, utilizamos o `rankingResponse` nos resultados da pesquisa para ordenar os resultados para apresentação. Este objeto faz referência a itens de `WebPages` `News`, `Images`, e/ou `Videos` coleções, ou em outras coleções de nível superior de resposta na resposta JSON.

`rankingResponse` pode conter até três coleções dos resultados da pesquisa, designados `pole`, `mainline`, e `sidebar`. 

`pole`, se estiver presente, é o resultado de pesquisa mais relevante e devem ser apresentados de forma destacada. `mainline` refere-se o volume dos resultados da pesquisa. Resultados mainline devem ser apresentados imediatamente após `pole` (ou se primeiro, `pole` não estiver presente). 

Por fim. `sidebar` refere-se os resultados da pesquisa auxiliar. Muitas vezes, estes resultados estão relacionadas pesquisas ou nas imagens. Se for possível, estes resultados devem ser apresentados numa barra lateral real. Se os limites de ecrã se uma barra lateral impractical (por exemplo, num dispositivo móvel), deve aparecer após o `mainline` resultados.

Cada item num `rankingResponse` coleção refere-se aos itens do resultado da pesquisa real de duas formas diferentes, mas equivalentes.

| | |
|-|-|
|`id`|O `id` aparente ser um URL, mas não deve ser utilizado para ligações. O `id` corresponde ao tipo de um resultado de classificação de `id` de qualquer um item de resultado da pesquisa numa coleção de resposta, *ou* uma coleção de resposta completo (tais como `Images`).
|`answerType`, `resultIndex`|O `answerType` refere-se para a coleção de nível superior de resposta que contém o resultado (por exemplo, `WebPages`). O `resultIndex` refere-se ao índice do resultado nessa coleção. Se `resultIndex` for omitido, o resultado de classificação refere-se para a coleção completa.

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

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

`renderResultsItems()` por sua vez iterar repetidamente os itens em cada `rankingResponse` secção, cada resultado de classificação é mapeado para um resultado de pesquisa utilizando o `answerType` e `resultIndex` campos e chama a função de composição adequado para gerar HTML o resultado. 

Se `resultIndex` não está especificado para um item fornecido classificação, `renderResultsItems()` iterar todos os resultados desse tipo e chama a função de composição para cada item. 

Qualquer forma, o HTML resultante é inserido apropriados `<div>` elemento na página.

```javascript
// render search results from rankingResponse object in specified order
function renderResultsItems(section, results) {

    var items = results.rankingResponse[section].items;
    var html = [];
    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        // collection name has lowercase first letter while answerType has uppercase
        // e.g. `WebPages` rankingResult type is in the `webPages` top-level collection
        var type = item.answerType[0].toLowerCase() + item.answerType.slice(1);
        // must have results of the given type AND a renderer for it
        if (type in results && type in searchItemRenderers) {
            var render = searchItemRenderers[type];
            // this ranking item refers to ONE result of the specified type
            if ("resultIndex" in item) {
                html.push(render(results[type].value[item.resultIndex], section));
            // this ranking item refers to ALL results of the specified type
            } else {
                var len = results[type].value.length;
                for (var j = 0; j < len; j++) {
                    html.push(render(results[type].value[j], section, j, len));
                }
            }
        }
    }
    return html.join("\n\n");
}
```

## <a name="rendering-result-items"></a>Itens do resultado composição

No nosso JavaScript código é um objeto, `searchItemRenderers`, que contém *compositores:* funções que geram HTML para cada tipos de resultados de pesquisa.

```javascript
// render functions for various types of search results
searchItemRenderers = { 
    webPages: function(item) { ... },
    news: function(item) { ... },
    images: function(item, section, index, count) { ... },
    videos: function(item, section, index, count) { ... },
    relatedSearches: function(item, section, index, count) { ... }
}
```

> [!NOTE]
> A nossa aplicação tutorial tem compositores para páginas Web, itens de notícias, imagens, vídeos e pesquisas relacionadas. As suas próprias aplicações necessita compositores para quaisquer tipos de resultados pode receber, que pode incluir computações, sugestões de ortografia, entidades, fusos horários e definições.

Alguns dos nossas funções de composição só aceitam o `item` parâmetro: um objeto de JavaScript que representa um resultado de procura única. Outras pessoas aceitam parâmetros adicionais, que podem ser utilizados para compor itens de forma diferente nos diferentes contextos. (Um compositor que não utiliza estas informações não são necessários aceitar estes parâmetros.)

Os argumentos do contexto são:

| | |
|-|-|
|`section`|A secção de resultados (`pole`, `mainline`, ou `sidebar`) no qual o item é apresentado.
|`index`<br>`count`|Disponível quando o `rankingResponse` item Especifica que todos os resultados de uma determinada coleção estão a ser apresentada; `undefined` caso contrário. Estes parâmetros recebem o índice do item dentro da respetiva coleção e o número total de itens nessa coleção. Pode estas informações para o número de resultados, para gerar o resultado do primeiro ou último código HTML diferentes e assim sucessivamente.|

Na nossa aplicação tutorial, tanto o `images` e `relatedSearches` compositores utilizam os argumentos do contexto para personalizar o HTML que geram. Vamos observá o `images` compositor:

```javascript
searchItemRenderers = { 
    // render image result using thumbnail
    images: function(item, section, index, count) {
        var height = 60;
        var width = Math.round(height * item.thumbnail.width / item.thumbnail.height);
        var html = [];
        if (section === "sidebar") {
            if (index) html.push("<br>");
        } else {
            if (!index) html.push("<p class='images'>");
        }
        html.push("<a href='" + item.hostPageUrl + "'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + " title='" + title + "' alt='" + title + "'>");
        html.push("</a>");
        return html.join("");
    }, // other renderers omitted
}
```

A nossa função compositor de imagem:

> [!div class="checklist"]
> * Calcula o tamanho da imagem em miniatura (largura varia, enquanto altura é fixo em 60 pixéis).
> * Insere HTML que precede o resultado de imagem, dependendo de contexto.
> * Baseia-se no código HTML `<a>` tag que as ligações para a página que contém a imagem.
> * Baseia-se no código HTML `<img>` etiquetas para apresentar a miniatura de imagem. 

A compositor de imagem utiliza o `section` e `index` variáveis para apresentar resultados de forma diferente, dependendo de onde aparecem. Uma quebra de linha (`<br>` tag) é inserido entre os resultados da imagem na barra lateral, para que a barra lateral apresenta uma coluna de imagens. Noutras secções, a primeira imagem resultar `(index === 0)` é precedido por um `<p>` etiquetas. Miniaturas, caso contrário butt com entre si e moldagem conforme necessário na janela do browser.

O tamanho das miniaturas é utilizado em ambos os `<img>` etiqueta e o `h` e `w` campos no URL a miniatura. O [serviço em miniatura do Bing](resize-and-crop-thumbnails.md) , em seguida, fornece uma miniatura do exatamente esse tamanho. O `title` e `alt` atributos (uma descrição da imagem textual) são construídos a partir de nome da imagem e o nome de anfitrião no URL.

Imagens aparecem conforme mostrado aqui nos resultados da pesquisa mainline.

![[Resultados de imagem do Bing]](media/cognitive-services-bing-web-api/web-search-spa-images.png)

## <a name="persisting-client-id"></a>ID de cliente persistentes

As respostas a partir de APIs de pesquisa do Bing podem incluir um `X-MSEdge-ClientID` cabeçalho que deve ser enviado para a API com os pedidos sucessivos. Se estão a ser utilizadas várias APIs de pesquisa do Bing, o mesmo ID de cliente deve utilizado com todos eles, se possível.

Fornecer o `X-MSEdge-ClientID` cabeçalho permite que as APIs do Bing para associar todas as pesquisas de um utilizador, que tem duas vantagens importantes.

Em primeiro lugar, permite que o Bing motor de busca aplicar passado contexto a procura para localizar os resultados que melhor satisfazem o utilizador. Se um utilizador tem procurado anteriormente para os termos relacionados com sailing, por exemplo, uma pesquisa posterior para "knots" poderá preferentially devolver informações sobre knots utilizado no sailing.

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
> [Tutorial de aplicação móvel de pesquisa Visual](computer-vision-web-search-tutorial.md)

> [!div class="nextstepaction"]
> [Referência da API de pesquisa do Bing Web](//docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
