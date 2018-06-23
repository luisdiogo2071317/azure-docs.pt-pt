---
title: Aplicação de Web de página única de pesquisa de imagem do Bing | Microsoft Docs
description: Mostra como utilizar a API de pesquisa do Bing imagem numa aplicação Web de página única.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: d0e1dc24513c8fc3a405cf1c18f531a0c58fad13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354601"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: Aplicação de Web de página única

A API de pesquisa do Bing imagem permite-lhe procurar Web e obter resultados de imagem relevantes para a consulta de pesquisa. Neste tutorial, iremos criar uma aplicação de Web de página única que utiliza a API de pesquisa de imagem do Bing para apresentar os resultados da pesquisa à direita na página. A aplicação inclui os componentes HTML, CSS e JavaScript.

<!-- Remove until we can sanitize images
![[Single-page Bing Image Search app]](media/cognitive-services-bing-images-api/image-search-spa-demo.png)
-->

> [!NOTE]
> Os cabeçalhos de JSON e HTTP na parte inferior da página revelar a resposta JSON e informações de pedidos de HTTP quando clicado. Estes detalhes são úteis quando explorar o serviço.

A aplicação tutorial ilustra como:

> [!div class="checklist"]
> * Efetuar uma chamada de API de pesquisa do Bing imagem em JavaScript
> * Opções de pesquisa de passar para a API de pesquisa do Bing imagem
> * Apresentar os resultados da pesquisa
> * Página percorrer os resultados de pesquisa
> * Identificador do Bing cliente ID e a API chave de subscrição
> * Processar erros que possam ocorrer

A página tutorial é inteiramente autónomo; Não utilize qualquer estruturas externas, folhas de estilo ou até mesmo os ficheiros de imagem. Utiliza apenas funcionalidades de linguagem JavaScript amplamente suportadas e funciona com versões atuais dos todos os principais browsers.

Neste tutorial, vamos discutir apenas selecionadas partes do código fonte. O código de origem completo está disponível [numa página separada](tutorial-bing-image-search-single-page-app-source.md). Copie e cole este código para um editor de texto e guarde-o como `bing.html`.

> [!NOTE]
> Este tutorial é substancialmente semelhante para o [tutorial de aplicação de pesquisa na Web Bing de página única](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), mas lida apenas com os resultados da pesquisa de imagem.

## <a name="app-components"></a>Componentes de aplicação

Como qualquer aplicação Web de página única, o tutorial da aplicação inclui três partes:

> [!div class="checklist"]
> * HTML - define a estrutura e conteúdo da página
> * CSS - define o aspecto da página
> * JavaScript - define o comportamento da página

Este tutorial não abrange a maioria dos HTML ou CSS detalhadamente, conforme forem simples.

No código HTML contém o formulário de pesquisa na qual o utilizador introduz uma consulta e escolhe opções de pesquisa. O formulário está ligado à JavaScript que executa efetivamente a pesquisa pelo `<form>` da tag `onsubmit` atributo:

```html
<form name="bing" onsubmit="return newBingImageSearch(this)">
```

O `onsubmit` processador devolve `false`, que mantém o formulário sejam submetidas ao servidor. O código JavaScript, na verdade, efetua o trabalho de recolher as informações necessárias do formulário e efetuar a pesquisa.

No código HTML também contém de divisões (HTML `<div>` etiquetas) onde os resultados de pesquisa são apresentados.

## <a name="managing-subscription-key"></a>Gestão da chave de subscrição

Para evitar ter de incluir a chave de subscrição de API de pesquisa do Bing no código, iremos utilizar o armazenamento persistente do browser para armazenar a chave. Se nenhuma chave é armazenada, iremos linha de comandos para a chave do utilizador e armazene-a para utilização posterior. Se a chave mais tarde é rejeitada pela API, iremos invalidar a chave armazenada para que o utilizador seja novamente.

Iremos definir `storeValue` e `retrieveValue` funções que utilizam o `localStorage` objeto (se o browser suportar) ou um cookie. A nossa `getSubscriptionKey()` função utiliza estas funções para armazenar e obter a chave do utilizador.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";

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

No código HTML `<form>` tag `onsubmit` chamadas a `bingWebSearch` função devolver resultados da pesquisa. `bingWebSearch` utiliza `getSubscriptionKey` para autenticar cada consulta. Conforme mostrado na definição do anterior, `getSubscriptionKey` pede ao utilizador para a chave, se a chave ainda não foi introduzida. A chave é armazenada, em seguida, para continuar a utilizar a aplicação.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Selecionar opções de pesquisa

![[Formulário de pesquisa do Bing imagem]](media/cognitive-services-bing-images-api/image-search-spa-form.png)

O formulário HTML inclui os seguintes controlos:

| | |
|-|-|
|`where`|Um menu pendente para selecionar o mercado (localização e de idioma) utilizado para a pesquisa.|
|`query`|O campo de texto em que a introduza os termos de pesquisa.|
|`aspect`|Botões de opção botões para escolher as proporções das imagens encontradas: aproximadamente quadrado, ao nível ou altura.|
|`color`|Seleciona cor ou black-and-white ou uma cor predominant.
|`when`|Menu pendente para, opcionalmente, limitar a pesquisa para o dia, semana ou mês mais recente.|
|`safe`|Uma caixa de verificação que indica se deve utilizar a funcionalidade de SafeSearch de Bing para filtrar os resultados "para adultos".|
|`count`|Campo oculto. O número de resultados da pesquisa a devolver em cada pedido. Altere para apresentar resultados mais ou menos por página.|
|`offset`|Campo oculto. O deslocamento do resultado da pesquisa primeiro no pedido; utilizado para paginação. Este é reposto a `0` num pedido de novo.|
|`nextoffset`|Campo oculto. Ao receber um resultado de pesquisa, este campo está definido para o valor da `nextOffset` na resposta. Através deste campo evita sobrepostos resultados nas páginas sucessivas.|
|`stack`|Campo oculto. Uma lista de desvios de precedente páginas dos resultados da pesquisa, para navegar para páginas anteriores codificada em JSON.|

> [!NOTE]
> Pesquisa do Bing imagem oferece muitas mais parâmetros de consulta. Estamos a utilizar apenas alguns dos-las aqui.

A nossa função JavaScript `bingSearchOptions()` converte estes campos a uma cadeia de consulta parcial no formato necessário para a API de pesquisa do Bing.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var aspect = "all";
    for (var i = 0; i < form.aspect.length; i++) {
        if (form.aspect[i].checked) {
            aspect = form.aspect[i].value;
            break;
        }
    }
    options.push("aspect=" + aspect);
    if (form.color.value) options.push("color=" + form.color.value);
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Por exemplo, pode ser a funcionalidade de SafeSearch `strict`, `moderate`, ou `off`, com `moderate` a ser a predefinição. Mas nosso formulário utiliza uma caixa de verificação, que tem apenas dois Estados. O código JavaScript converte esta definição para o `strict` ou `off` (não utilizamos `moderate`).

## <a name="performing-the-request"></a>Executar o pedido

A consulta, a cadeia de opções e a chave de API, o `BingImageSearch` funcionar utiliza um `XMLHttpRequest` objeto para efetuar o pedido para o ponto final de pesquisa do Bing imagem.

```javascript
// perform a search given query, options string, and API key
function bingImageSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

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
            if (jsobj._type === "Images") {
                if (jsobj.nextOffset) document.forms.bing.nextoffset.value = jsobj.nextOffset;
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
> Um pedido HTTP com êxito *não* significam necessariamente que que a pesquisa próprio foi concluída com êxito. Se ocorrer um erro na operação de pesquisa, a API de pesquisa do Bing imagem devolve um código de estado de HTTP não 200 e inclui informações de erro na resposta JSON. Além disso, se o pedido foi taxa limitado, a API devolve uma resposta vazia.

Muito do código em ambas as funções anteriores se encontra dedicada para processamento de erros. Poderão ocorrer erros nas seguintes fases:

|Fase|Potenciais erros|Processado pelo|
|-|-|-|
|Objeto de pedido de JavaScript de criação|URL inválido|`try`/`catch` Bloco|
|Efetuar o pedido|Erros de rede, ligações abortadas|`error` e `abort` processadores de eventos|
|Efetuar a pesquisa|Inválido limites de velocidade do pedido, JSON inválido,|os testes no `load` processador de eventos|

São processados os erros ao chamar `renderErrorMessage()` com os detalhes sobre o erro. Se a resposta passar o gauntlet completo de testes de erro, chamamos `renderSearchResults()` para apresentar os resultados da procura na página.

## <a name="displaying-search-results"></a>Apresentar os resultados da pesquisa

A função principal para apresentar os resultados da pesquisa é `renderSearchResults()`. Esta função aceita o JSON devolvido pelo serviço de pesquisa do Bing imagem e composições as imagens e pesquisas relacionadas, se aplicável.

```javascript
function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    showDiv("results", renderImageResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```

São apresentados os resultados de pesquisa de imagem principal de nível superior como `value` objeto na resposta JSON. Iremos os passar para a nossa função `renderImageResults()`, que itera através de-los e chama uma função separada para cada item de composição em HTML. O HTML resultante é devolvido ao `renderSearchResults()`, onde é inserido o `results` divisão na página.

```javascript
function renderImageResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.images(items[i], i, len));
    }
    return html.join("\n\n");
}
```

Devolve a API de pesquisa do Bing imagem até quatro diferentes tipos de resultados relacionados, cada uma no seu próprio objeto de nível superior. São:

|||
|-|-|
|`pivotSuggestions`|Consultas que substituir uma palavra pivot na pesquisa original com outro. Por exemplo, se procurar "red flowers", uma palavra dinâmica pode ser "red" e uma sugestão de dinâmica pode ser "amarelo flowers."|
|`queryExpansions`|Consultas que limitar a pesquisa original adicionando mais termos. Por exemplo, se procurar "Microsoft superfície", uma expansão de consulta pode ser "Microsoft Surface Pro."|
|`relatedSearches`|Consultas que também tenham sido introduzidas por outros utilizadores que introduziu a pesquisa original. Por exemplo, se procurar "Montagem Rainier", uma pesquisa relacionada poderão "Mt. São Helens."|
|`similarTerms`|Consultas que são semelhantes na significado da procura original. Por exemplo, se procurar "kittens", um termo semelhante poderão "cute".|

Como anteriormente visto na `renderSearchResults()`, iremos compor apenas o `relatedItems` sugestões e coloque resultante hiperligações na barra lateral da página.

## <a name="rendering-result-items"></a>Itens do resultado composição

No nosso JavaScript código é um objeto, `searchItemRenderers`, que contém *compositores:* funções que geram HTML para cada tipos de resultados de pesquisa.

```javascript
searchItemRenderers = { 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```

Uma função de compositor pode aceitar os seguintes parâmetros:

| | |
|-|-|
|`item`|O objecto de JavaScript que contém propriedades do item, tal como o respetivo URL e a respetiva descrição.|
|`index`|O índice do item de resultado dentro da respetiva coleção.|
|`count`|O número de itens na coleção de itens de resultado de pesquisa.|

O `index` e `count` parâmetros podem ser utilizados para resultados número, para gerar HTML especial para o início ou fim de uma coleção para inserir quebras de linha depois de um determinado número de itens e assim sucessivamente. Se a um compositor não precisa desta funcionalidade, não é necessário aceitar estes dois parâmetros.

Vamos observá o `images` compositor:

```javascript
    images: function (item, index, count) {
        var height = 120;
        var width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
        var html = [];
        if (index === 0) html.push("<p class='images'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<p class='images' style='max-width: " + width + "px'>");
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + "'>");
        html.push("<br>");
        html.push("<nobr><a href='" + item.contentUrl + "'>Image</a> - ");
        html.push("<a href='" + item.hostPageUrl + "'>Page</a></nobr><br>");
        html.push(title.replace("\n", " (").replace(/([a-z0-9])\.([a-z0-9])/g, "$1.<wbr>$2") + ")</p>");
        return html.join("");
    }, // relatedSearches renderer omitted
```

A nossa função compositor de imagem:

> [!div class="checklist"]
> * Calcula o tamanho da imagem em miniatura (largura varia, com um mínimo de 120 pixéis, enquanto altura é fixo no 90 pixéis).
> * Baseia-se no código HTML `<img>` etiquetas para apresentar a miniatura de imagem. 
> * Baseia-se no código HTML `<a>` etiquetas com ligação para a imagem e a página que o contém.
> * Baseia-se a descrição que apresenta informações sobre a imagem e o site que está ativada.

Iremos testar o `index` variável para poder inserir uma `<p>` etiquetas antes do resultado de imagem primeiro. Miniaturas, caso contrário butt com entre si e moldagem conforme necessário na janela do browser.

O tamanho das miniaturas é utilizado em ambos os `<img>` etiqueta e o `h` e `w` campos no URL a miniatura. O [serviço em miniatura do Bing](resize-and-crop-thumbnails.md) , em seguida, fornece uma miniatura do exatamente esse tamanho.

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
> [Referência da API de pesquisa do Bing imagem](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

