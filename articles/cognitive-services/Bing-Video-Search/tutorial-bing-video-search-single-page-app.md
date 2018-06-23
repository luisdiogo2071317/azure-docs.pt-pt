---
title: Aplicação de pesquisa de vídeo de página única Bing | Microsoft Docs
description: Explica como utilizar a API de pesquisa do Bing vídeo numa aplicação Web de página única.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 11/01/2017
ms.author: v-gedod
ms.openlocfilehash: 55f662721e007e03c8f43f19d8b905e755cfe1d8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354619"
---
# <a name="tutorial-single-page-video-search-app"></a>Tutorial: Aplicação de pesquisa de vídeo de página única
A API do Bing vídeo pesquisa permite pesquisar Web e obter resultados vídeos relevantes para uma consulta de pesquisa. Neste tutorial, iremos criar uma aplicação de Web de página única que utiliza a API de pesquisa do Bing para apresentar os resultados da procura na página. A aplicação inclui os componentes HTML, CSS e JavaScript.

<!-- Remove until it can be replaced with a sanitized version.
![Single-page Bing Video Search app](./media/video-search-singlepage.png)
-->

> [!NOTE]
> Os cabeçalhos de JSON e HTTP na parte inferior da página quando clicado mostram as informações de pedido HTTP e a resposta JSON. Estes detalhes podem ser útil quando explorar o serviço.

![JSON, os resultados em bruto de HTTP](./media/json-http-raw-results.png)

Esta aplicação tutorial ilustra como:
> [!div class="checklist"]
> * Efetuar uma chamada de API de pesquisa do Bing vídeo em JavaScript
> * Opções de pesquisa de passar para a API de pesquisa do Bing
> * Apresentar os resultados da pesquisa de vídeo ou, opcionalmente, inclui páginas Web, notícias de última hora ou imagens
> * Tempo de pesquisa-frames de 24 horas, da última semana, mês ou tempo disponível
> * Página percorrer os resultados de pesquisa
> * Identificador do Bing cliente ID e a API chave de subscrição
> * Processar erros que possam ocorrer

A página tutorial é inteiramente autónomo; Não utilize qualquer estruturas externas, folhas de estilo ou ficheiros de imagem. Utiliza apenas funcionalidades de linguagem JavaScript amplamente suportadas e funciona com versões atuais dos todos os principais browsers.

Neste tutorial, vamos discutir selecionadas partes do código fonte. A concluir [código fonte](tutorial-bing-video-search-single-page-app-source.md) está disponível. Para executar o exemplo, copie e cole o código de origem para um editor de texto e guarde-o como `bing.html`.

## <a name="app-components"></a>Componentes de aplicação
Como qualquer aplicação Web de página única, este tutorial da aplicação inclui três partes:

> [!div class="checklist"]
> * HTML - define a estrutura e conteúdo da página
> * CSS - define o aspecto da página
> * JavaScript - define o comportamento da página

A maioria dos HTML e CSS é convencionais, para que o tutorial não discuti-lo. No código HTML contém o formulário de pesquisa na qual o utilizador introduz uma consulta e escolhe opções de pesquisa. O formulário está ligado à JavaScript que suporta a procura utilizando o `onsubmit` atributo do `<form>` etiqueta:

```html
<form name="bing" onsubmit="return bingWebSearch(this)">
```
O `onsubmit` processador devolve `false`, que mantém o formulário sejam submetidas ao servidor. O código JavaScript efetua o trabalho de recolher as informações necessárias do formulário e efetuar a pesquisa.

No código HTML também contém de divisões (HTML `<div>` etiquetas) onde os resultados de pesquisa são apresentados.

## <a name="managing-subscription-key"></a>Gestão da chave de subscrição

Para evitar ter de incluir a chave de subscrição de API de pesquisa do Bing no código, iremos utilizar o armazenamento persistente do browser para armazenar a chave. Antes da chave é armazenada, iremos solicitar a chave do utilizador. Se a chave mais tarde é rejeitada pela API, iremos invalidar a chave armazenada para que o utilizador será avisado novamente.

Iremos definir `storeValue` e `retrieveValue` funções que utilizam o `localStorage` objeto (nem todos os browsers suportam) ou um cookie. O `getSubscriptionKey()` função utiliza estas funções para armazenar e obter a chave do utilizador.

``` javascript
// Cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

// ... omitted definitions of store value and retrieve value
// Browsers differ in their support for persistent storage by 
// local HTML files. See the source code for browser-specific
// options.

// Get stored API subscription key, or prompt if it's not found.
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
No código HTML `<form>` tag `onsubmit` chamadas a `bingWebSearch` função devolver resultados da pesquisa. `bingWebSearch` utiliza `getSubscriptionKey()` para autenticar cada consulta. Conforme mostrado na definição do anterior, `getSubscriptionKey` pede ao utilizador para a chave, se a chave ainda não foi introduzida. A chave é armazenada, em seguida, para continuar a utilizar a aplicação.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```
## <a name="selecting-search-options"></a>Selecionar opções de pesquisa
A figura seguinte mostra a caixa de texto de consulta e as opções que definem uma pesquisa.

![Opções de pesquisa do Bing notícias de última hora](media/video-search-options.png)

O formulário HTML inclui elementos com os nomes seguintes:

|Elemento|Descrição|
|-|-|
| `where` | Um menu pendente para selecionar o mercado (localização e de idioma) utilizado para a pesquisa. |
| `query` | O campo de texto para introduzir os termos de pesquisa. |
| `modules` | Caixas de verificação para promover módulos específicos de resultados, todos os resultados ou vídeos relacionados. |
| `when` | Menu pendente para, opcionalmente, limitar a pesquisa para o dia, semana ou mês mais recente. |
| `safe` | Uma caixa de verificação que indica se deve utilizar a funcionalidade de Bing SafeSearch para filtrar os resultados "para adultos". |
| `count` | Campo oculto. O número de resultados da pesquisa a devolver em cada pedido. Altere para apresentar resultados mais ou menos por página. |
| `offset`|  Campo oculto. O deslocamento do resultado da pesquisa primeiro no pedido; utilizado para paginação. Este é reposto a `0` num pedido de novo. |

> [!NOTE]
> Pesquisa na Web Bing oferece outros parâmetros de consulta. Estamos a utilizar apenas alguns dos mesmos.

``` javascript
// build query options from the HTML form
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
        options.push("modules=" + what.join(","));
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

## <a name="performing-the-request"></a>Executar o pedido
A consulta, a cadeia de opções e a chave de API, o `BingWebSearch` funcionar utiliza um `XMLHttpRequest` objeto para efetuar o pedido para o ponto final de pesquisa do Bing.

```javascript
// Search on the query, using search options, authenticated by the key.
function bingWebSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_headers", "paging1", "paging2", "error");

    var endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search";
    var request = new XMLHttpRequest();
    var queryurl = endpoint + "?q=" + encodeURIComponent(query) + "&" + options;

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
    request.addEventListener("load", handleOnLoad);

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
Após a conclusão com êxito do pedido HTTP, chamadas JavaScript a `load` processador de eventos, `handleOnLoad()`, para processar um pedido de HTTP GET com êxito para a API. 

```javascript
// handle Bing search request results
function handleOnLoad() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // try to parse JSON results
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
    }

    // show raw JSON and headers
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " + 
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // if HTTP response is 200 OK, try to render search results
    if (this.status === 200) {
        var clientid = this.getResponseHeader("X-MSEdge-ClientID");
        if (clientid) retrieveValue(CLIENT_ID_COOKIE, clientid);
        if (json.length) {
            if (jsobj._type === "Videos") {//"SearchResponse" && "rankingResponse" in jsobj) {
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
> Se ocorrer um erro na operação de pesquisa, a API de pesquisa do Bing notícias devolve um código de estado de HTTP não 200 e inclui informações de erro na resposta JSON. Além disso, se o pedido foi taxa limitado, a API devolve uma resposta vazia.
Um pedido HTTP com êxito *não* significam necessariamente que que a pesquisa próprio foi concluída com êxito. 

Muito do código em ambas as funções anteriores se encontra dedicada para processamento de erros. Poderão ocorrer erros nas seguintes fases:

|Fase|Potenciais erros|Processado pelo|
|-|-|-|
|Criar o objeto de pedido de JavaScript|URL inválido|`try`/`catch` Bloco|
|Efetuar o pedido|Erros de rede, ligações abortadas|`error` e `abort` processadores de eventos|
|Efetuar a pesquisa|Inválido limites de velocidade do pedido, JSON inválido,|os testes no `load` processador de eventos|

São processados os erros ao chamar `renderErrorMessage()` com os detalhes sobre o erro. Se a resposta passar o gauntlet completo de testes de erro, chamamos `renderSearchResults()` para apresentar os resultados da procura na página.

## <a name="displaying-search-results"></a>Apresentar os resultados da pesquisa
A função principal para apresentar os resultados da pesquisa é `renderSearchResults()`. Esta função aceita JSON devolvido pelo serviço de pesquisa do Bing notícias e apresenta os resultados de notícias e pesquisas relacionadas, se aplicável.

```javascript
// render the search results given the parsed JSON response
function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);

    // Render the resuts to the mainline section
    for (section in { mainline: 0 }) {
         showDiv(section, renderResultsItems(section, results));
    }
}
```
Os resultados da pesquisa são devolvidos como de nível superior `value` objeto na resposta JSON. Iremos os passar para a nossa função `renderResultsItems()`, que itera através de-los e chama uma função para compor cada item no HTML. O HTML resultante é devolvido ao `renderSearchResults()`, onde é inserido o `results` divisão na página.

```javascript
// render search results
    function renderResultsItems(section, results) {   

        var items = results.value;
        var html = [];
        for (var i = 0; i < items.length; i++) { 
            var item = items[i];
            // collection name has lowercase first letter
            var type = "videos";
            var render = searchItemRenderers[type];
            html.push(render(item, section));  
        }
    return html.join("\n\n");
}
```

Devolve a API de pesquisa do Bing notícias até quatro diferentes tipos de resultados relacionados, cada uma no seu próprio objeto de nível superior. São:

|Relação|Descrição|
|-|-|
|`pivotSuggestions`|Consultas que substituir uma palavra pivot na pesquisa original com outro. Por exemplo, se procurar "red flowers", uma palavra dinâmica pode ser "red" e uma sugestão de dinâmica pode ser "amarelo flowers."|
|`queryExpansions`|Consultas que limitar a pesquisa original adicionando mais termos. Por exemplo, se procurar "Microsoft superfície", uma expansão de consulta pode ser "Microsoft Surface Pro."|
|`relatedSearches`|Consultas que também tenham sido introduzidas por outros utilizadores que introduziu a pesquisa original. Por exemplo, se procurar "Montagem Rainier", uma pesquisa relacionada poderão "Mt. São Helens."|
|`similarTerms`|Consultas que são semelhantes na significado da procura original. Por exemplo, se procurar "escolas", um termo semelhante poderão "education."|

Como anteriormente visto na `renderSearchResults()`, iremos compor apenas o `relatedItems` sugestões e coloque resultante hiperligações na barra lateral da página.

## <a name="rendering-result-items"></a>Itens do resultado composição

No JavaScript code o objeto, `searchItemRenderers`, pode contém *compositores:* funções que geram HTML para cada tipos de resultados de pesquisa. A pesquisa de vídeo página utiliza apenas `videos`. Consulte os outros tutoriais de vários tipos de compositores.

```javascript
searchItemRenderers = {
    news: function(item) { ... },
    webPages: function (item) { ... }, 
    images: function(item, index, count) { ... },
    videos: function (item, section, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```
Uma função de compositor pode aceitar os seguintes parâmetros:

|Parâmetro|Descrição|
|-|-|
|`item`| O objecto de JavaScript que contém propriedades do item, tal como o respetivo URL e a respetiva descrição.|
|`index`| O índice do item de resultado dentro da respetiva coleção.|
|`count`| O número de itens na coleção de itens de resultado de pesquisa.|

O `index` e `count` parâmetros podem ser utilizados para resultados número, para gerar HTML especial para o início ou fim de uma coleção para inserir quebras de linha depois de um determinado número de itens e assim sucessivamente. Se a um compositor não precisa desta funcionalidade, não é necessário aceitar estes dois parâmetros.

O `video` compositor é apresentado na excerpt de javascript seguinte. Utilizar o ponto final de vídeos, todos os resultados são do tipo `Videos`. O `searchItemRenderers` são apresentadas no segmento de código seguinte.

```javascript
// render functions for various types of search results
    searchItemRenderers = {

    videos: function (item, section, index, count) {
        var height = 60;
        var width = Math.round(height * item.thumbnail.width / item.thumbnail.height);
        var html = [];

        html.push("<p class='images'>");
        html.push("<a href='" + item.hostPageUrl + "'>");
        var title = escapeQuotes(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<img src='" + item.thumbnailUrl + "&h=" + height + "&w=" + width +
            "' height=" + height + " width=" + width + " title='" + title + "' alt='" + title + "'>");
        html.push("</a>");
        html.push("<br>");
        html.push("<nobr><a href='" + item.contentUrl + "'>Video page source</a> - ");
        html.push(title.replace("\n", " (").replace(/([a-z0-9])\.([a-z0-9])/g, "$1.<wbr>$2") + ")</p>");
        return html.join("");
    }
}
```

A função de compositor:
> [!div class="checklist"]
> * Cria uma etiqueta de parágrafo, atribui-o para o `images` classe e envia-a para a matriz de html.
> * Calcula o tamanho das miniaturas de imagem (largura corrigida em 60 pixels, altura calculado proportionately).
> * Baseia-se no código HTML `<img>` etiquetas para apresentar a miniatura de imagem. 
> * Baseia-se no código HTML `<a>` etiquetas com ligação para a imagem e a página que o contém.
> * Baseia-se a descrição que apresenta informações sobre a imagem e o site que está ativada.

O tamanho das miniaturas é utilizado em ambos os `<img>` etiqueta e o `h` e `w` campos no URL a miniatura. O [serviço em miniatura do Bing](resize-and-crop-thumbnails.md) , em seguida, fornece uma miniatura do exatamente esse tamanho.

## <a name="persisting-client-id"></a>ID de cliente persistentes
As respostas a partir de APIs de pesquisa do Bing podem incluir um `X-MSEdge-ClientID` cabeçalho que deve ser enviado para a API com os pedidos sucessivos. Se estão a ser utilizadas várias APIs de pesquisa do Bing, o mesmo ID de cliente deve utilizado com todos eles, se possível.

Fornecer o `X-MSEdge-ClientID` cabeçalho permite que as APIs do Bing para associar todas as pesquisas de um utilizador, que tem duas vantagens importantes.

Em primeiro lugar, permite que o Bing motor de busca aplicar passado contexto a procura para localizar os resultados que melhor satisfazem o utilizador. Se um utilizador tem procurado anteriormente para os termos relacionados com sailing, por exemplo, uma pesquisa posterior para "knots" poderá preferentially devolver informações sobre knots utilizado no sailing.

Segundo, o Bing aleatoriamente pode selecionar os utilizadores a experiência de novas funcionalidades antes de serem efetuadas amplamente disponíveis. Fornecer o mesmo ID de cliente com cada pedido garante que os utilizadores que ver a funcionalidade sempre veem-lo. Sem o ID de cliente, o utilizador poderá ver uma funcionalidade são apresentados e desaparecer, seemingly aleatória, os resultados da procura.

Políticas de segurança do browser (CORS) poderão impedir o `X-MSEdge-ClientID` cabeçalho estejam disponíveis para JavaScript. Esta limitação ocorre quando a resposta de pesquisa tem uma origem diferente da página de pedido-lo. Num ambiente de produção, deve abordar esta política ao alojar um script do lado do servidor que faz a chamada de API no mesmo domínio que a página Web. Uma vez que o script tem a mesma origem como a página Web, o `X-MSEdge-ClientID` cabeçalho, em seguida, está disponível para JavaScript.

> [!NOTE]
> Uma aplicação Web de produção, deve executar o pedido lado do servidor. Caso contrário, a chave de API de pesquisa do Bing têm de ser incluída na página Web, onde se encontram disponíveis para qualquer pessoa que vistas de origem. É-lhe faturado para utilização de todas as sob a chave de subscrição de API, mesmo pedidos efetuados por entidades confiadoras não autorizadas, pelo que é importante não para expor a sua chave.

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
> [Referência da API de pesquisa do Bing vídeo](//docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference)