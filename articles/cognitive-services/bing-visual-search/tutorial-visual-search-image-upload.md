---
title: 'Tutorial: Como carregar uma imagem - Pesquisa Visual do Bing'
titleSuffix: Azure Cognitive Services
description: Discrimina o processo de carregamento de uma imagem para o Bing obter informações sobre a mesma e, depois, a análise e apresentação da resposta.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: scottwhi
ms.openlocfilehash: a5bc5197ecd1f35b4d0026caa076a844c9d57c40
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221329"
---
# <a name="tutorial-breaking-down-bing-visual-search-upload"></a>Tutorial: Discriminar o carregamento na Pesquisa Visual do Bing

Este tutorial discrimina o processo de carregamento de uma imagem para o Bing e da obtenção de informações. Também mostra como aceder e apresentar as informações na resposta JSON. Para obter um exemplo completo de HTML e JavaScript, veja [Código completo](#complete-code).

Este tutorial é disponibilizado aos programadores que querem explorar os conteúdo das respostas da Pesquisa Visual do Bing. Não aplica todos os requisitos de utilização e apresentação (por exemplo, não disponibiliza uma ligação para a política de privacidade da Microsoft). Para ver todos os requisitos de utilização, veja [Bing Use and Display Requirements](./use-and-display-requirements.md) (Requisitos de Utilização e Apresentação do Bing).


## <a name="where-to-start"></a>Por onde começar?

Vamos começar com uma página HTML que envia uma imagem ao Bing e recebe informações, apresentando-as. No seu editor preferido, crie um ficheiro denominado uploaddemo.html. Adicione a seguinte estrutura de HTML básica ao ficheiro.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Visual Search Upload Demo</title>
    </head>

    <body>
    </body>
</html>      
```

Para começar, vamos dividir a página numa seção de pedido, na qual o utilizador fornece todas as informações necessárias para fazer o pedido, e numa secção de resposta, na qual as informações são apresentadas. Adicione as seguintes tags \<div\> ao \<corpo\>. A tag \<hr\> delimita visualmente a secção de pedido da de resposta.

```html
        <div id="requestSection"></div>

        <hr />
        
        <div id="responseSection"></div>
```

## <a name="get-the-file-to-upload"></a>Obter o ficheiro a carregar

Para permitir que o utilizador selecione a imagem a carregar, a demonstração utiliza a tag \<input\> com o atributo de tipo definido como “file”. A IU tem de deixar claro que a demonstração utiliza o Bing para obter os resultados da pesquisa. 

Adicione a seguinte tag \<div\> à tag div requestSection. A entrada de ficheiro aceita um ficheiro individual de qualquer tipo de imagem (por exemplo, .jpg, .gif, .png). O evento `onchange` especifica o processador que é chamado quando um utilizador seleciona um ficheiro.

A tag \<output\> é utilizada para apresentar uma miniatura da imagem selecionada.


```html
            <div>
                <p>Select image to get insights from Bing:
                    <input type="file" accept="image/*" id="uploadImage" name="files[]" size=40 onchange="handleFileSelect('uploadImage')" />
                </p>

                <output id="thumbnail"></output>
            </div>
```

Antes de adicionar o processador, adicione uma tag \<script\> à tag \<head\>.

```html
        <script>
        <\script>
```

A imagem abaixo mostra o processador que captura a imagem selecionada. O processador inclui lógica para garantir que o ficheiro selecionado é um ficheiro de imagem e que o tamanho do mesmo é 1 MB ou menos. Pode permitir que o utilizador selecione ficheiros maiores, mas tem de reduzir o tamanho da imagem para menos de 1 MB antes de a carregar para o Bing. A última coisa que o processador faz é apresentar uma miniatura da imagem, para que o utilizador tenha um lembrete visual do ficheiro que selecionou.

```javascript
        function handleFileSelect(selector) {
            var files = document.getElementById(selector).files; // FileList object

            // The list will contain only one file.
            for (var i = 0, f; f = files[i]; i++) {
      
                // Only process image files.
                if (!f.type.match('image.*')) {
                    alert("Selected file must be an image file.");
                    document.getElementById("uploadImage").value = null;
                    continue;
                }
      
                // Image must be <= 1 MB and should be about 1500px.
                if (f.size > 1000000) {
                    alert("Image must be less than 1 MB.");
                    document.getElementById("uploadImage").value = null;
                    continue;
                }


                var reader = new FileReader();
      
                // Capture the file information.
                reader.onload = (function(theFile) {
                    return function(e) {
                        var fileOutput = document.getElementById('thumbnail');

                        if (fileOutput.childElementCount > 0) {
                            fileOutput.removeChild(fileOutput.lastChild);  // Remove the current pic, if it exists
                        }

                        // Render thumbnail.
                        var span = document.createElement('span');
                        span.innerHTML = ['<img class="thumb" src="', e.target.result,
                                          '" title="', escape(theFile.name), '"/>'].join('');
                        fileOutput.insertBefore(span, null);
                    };
                })(f);
      
                // Read in the image file as a data URL.
                reader.readAsDataURL(f);
            }
        }
```


## <a name="what-else-is-needed-before-making-the-call-to-bing"></a>O que mais é necessário antes de fazer a chamada para o Bing?

A demonstração ainda precisa de uma chave de subscrição. Na prática, deveria receber a chave de subscrição a partir do armazenamento protegido. Contudo, por uma questão de simplicidade da demonstração, tem de a indicar na IU. Adicione a seguinte tag \<input\> (com o atributo de tipo definido como “text”) a \<corpo\>, imediatamente abaixo da tag \<output\>.

```html
        <div>
            <p>Subscription key: 
                <input type="text" id="key" name="subscription" size=40 maxlength="32" />
            </p>
        </div>
```

Tendo a imagem e a chave de subscrição à mão, pode fazer a chamada para a Pesquisa Visual do Bing obter informações sobre a imagem. A chamada utilizará os valores predefinidos de mercado e safesearch (en-us e moderado respetivamente).

Esta demonstração dá ao utilizador a opção de mudar esses valores. Adicione a seguinte tag \<div\> abaixo da tag div da chave de subscrição. A demonstração utiliza uma tag \<select\> para fornecer uma lista pendente de valores de mercado e safesearch. Ambas as listas apresentam o valor predefinido do Bing.

 
```html
        <div>
            <p><a href="#" onclick="expandCollapse(options.id)">Query options</a></p>

            <div id="options" style="display:none">
                <p style="margin-left: 20px">Market: 
                    <select id="mkt">
                        <option value="es-AR">Argentina (Spanish)</option>
                        <option value="en-AU">Australia (English)</option>
                        <option value="de-AT">Austria (German)</option>
                        <option value="nl-BE">Belgium (Dutch)</option>
                        <option value="fr-BE">Belgium (French)</option>
                        <option value="pt-BR">Brazil (Portuguese)</option>
                        <option value="en-CA">Canada (English)</option>
                        <option value="fr-CA">Canada (French)</option>
                        <option value="es-CL">Chile (Spanish)</option>
                        <option value="da-DK">Denmark (Danish)</option>
                        <option value="fi-FI">Finland (Finnish)</option>
                        <option value="fr-FR">France (French)</option>
                        <option value="de-DE">Germany (German)</option>
                        <option value="zh-HK">Hong Kong (Traditional Chinese)</option>
                        <option value="en-IN">India (English)</option>
                        <option value="en-ID">Indonesia (English)</option>
                        <option value="it-IT">Italy (Italian)</option>
                        <option value="ja-JP">Japan (Japanese)</option>
                        <option value="ko-KR">Korea (Korean)</option>
                        <option value="en-MY">Malaysia (English)</option>
                        <option value="es-MX">Mexico (Spanish)</option>
                        <option value="nl-NL">Netherlands (Dutch)</option>
                        <option value="en-NZ">New Zealand (English)</option>
                        <option value="no-NO">Norway (Norwegian)</option>
                        <option value="zh-CN">People's Republic of China (Chinese)</option>
                        <option value="pl-PL">Poland (Polish)</option>
                        <option value="pt-PT">Portugal (Portuguese)</option>
                        <option value="en-PH">Philippines (English)</option>
                        <option value="ru-RU">Russia (Russian)</option>
                        <option value="ar-SA">Saudi Arabia (Arabic)</option>
                        <option value="en-ZA">South Africa (English)</option>
                        <option value="es-ES">Spain (Spanish)</option>
                        <option value="sv-SE">Sweden (Swedish)</option>
                        <option value="fr-CH">Switzerland (French)</option>
                        <option value="de-CH">Switzerland (German)</option>
                        <option value="zh-TW">Taiwan (Traditional Chinese)</option>
                        <option value="tr-TR">Turkey (Turkish)</option>
                        <option value="en-GB">United Kingdom (English)</option>
                        <option value="en-US" selected>United States (English)</option>
                        <option value="es-US">United States (Spanish)</option>
                    </select>
                </p>
                <p style="margin-left: 20px">Safe search: 
                    <select id="safesearch">
                        <option value="moderate" selected>Moderate</option>
                        <option value="strict">Strict</option>
                        <option value="off">off</option>
                    </select>
                </p>
            </div>
        </div>
```

A demonstração oculta as listas numa tag div expansível que é controlada pela ligação Opções de consulta. Se clicar na ligação Opções de consulta, a tag div expande-se e pode ver e modificar as opções de consulta. Se clicar outra vez na ligação, a tag fecha-se e fica oculta. A imagem abaixo mostra o processador onclick da ligação Opções de consulta. O processador controla se a tag div é expandida ou fechada. Adicione o processador à secção \<script\>. Todas as tags div expansíveis nesta demonstração utilizam o processador.

```javascript
        // Contains the toggle state of divs.
        var divToggleMap = {};  // divToggleMap['foo'] = 0;  // 1 = show, 0 = hide


        // Toggles between showing and hiding the specified div.
        function expandCollapse(divToToggle) {
            var div = document.getElementById(divToToggle);

            if (divToggleMap[divToToggle] == 1) {   // if div is expanded
                div.style.display = "none";
                divToggleMap[divToToggle] = 0;
            }
            else {                                  // if div is collapsed
                div.style.display = "inline-block";
                divToggleMap[divToToggle] = 1;
            }
        }
```


## <a name="making-the-call"></a>Fazer a chamada

Adicione o seguinte botão “Get insights” (“Obter informações”) abaixo da tag div options no corpo. O botão permite ao utilizador iniciar a chamada. Quando o utilizador clica no botão, o cursor muda para o cursor de espera giratório e o processador onclick é chamado.

```html
        <p><input type="button" id="query" value="Get insights" onclick="document.body.style.cursor='wait'; handleQuery()" /></p>
```

Adicione o processador onclick do botão à tag \<script\>. O processador garante que a chave de subscrição está presente e que tem 32 carateres de comprimento e, ainda, que foi selecionada uma imagem. Também limpa todas as informações relativas a uma consulta anterior. Se estiver tudo bem, chama a função sendRequest para fazer a chamada.

```javascript
        function handleQuery() {
            var subscriptionKey = document.getElementById('key').value;

            // Make sure user provided a subscription key and image.
            // For this demo, the user provides the key but typically you'd 
            // get it from secured storage.
            if (subscriptionKey.length !== 32) {
                alert("Subscription key length is not valid. Enter a valid key.");
                document.getElementById('key').focus();
                return;
            }

            var imagePath = document.getElementById('uploadImage').value;

            if (imagePath.length === 0)
            {
                alert("Please select an image to upload.");
                document.getElementById('uploadImage').focus();
                return;
            }

            var responseDiv = document.getElementById('responseSection');

            // Clear out the response from the last query.
            while (responseDiv.childElementCount > 0) {
                responseDiv.removeChild(responseDiv.lastChild);
            }

            // Send the request to Bing to get insights about the image.
            var f = document.getElementById('uploadImage').files[0];
            sendRequest(f, subscriptionKey);
        }
```

A função sendRequest formata o URL do ponto final, define o cabeçalho Ocp-Apim-Subscription-Key como a chave de subscrição, anexa o binário da imagem que vai ser carregada, especifica o processador de resposta e faz a chamada. 

```javascript
        function sendRequest(file, key) {
            var market = document.getElementById('mkt').value;
            var safeSearch = document.getElementById('safesearch').value;
            var baseUri = `https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=${market}&safesearch=${safeSearch}`;

            var form = new FormData();
            form.append("image", file);

            var request = new XMLHttpRequest();

            request.open("POST", baseUri);
            request.setRequestHeader('Ocp-Apim-Subscription-Key', key);
            request.addEventListener('load', handleResponse);
            request.send(form);
        }
```

## <a name="handling-the-response"></a>Processar a resposta

A função handleResponse processa a resposta da chamada para a Pesquisa Visual do Bing. Se a chamada for bem-sucedida, a função analisa a resposta JSON nas tags individuais, as quais contêm as informações. Depois, adiciona a cadeia, os resultados da pesquisa de Internet do Bing, à página, para que o utilizador saiba que os dados vêm do Bing.

A demonstração poderia largar todas as informações na página, mas algumas imagens devolvem muitos dados, o que dificultaria o consumo. Em vez disso, a demonstração cria uma tag div expansível para cada tag, para que o utilizador possa gerir a quantidade de dados que vê.

Adicione o processador à secção \<script\>.

```javascript
        function handleResponse() {
            if(this.status !== 200){
                alert("Error calling Bing Visual Search. See console log for details.");
                console.log(this.responseText);
                return;
            }

            var tags = parseResponse(JSON.parse(this.responseText));
            var h4 = document.createElement('h4');
            h4.textContent = 'Bing internet search results';
            document.getElementById('responseSection').appendChild(h4);
            buildTagSections(tags);

            document.body.style.cursor = 'default'; // reset the wait curor set by query insights button
        }

        function parseResponse(json) {
            var dict = {};

            for (var i =0; i < json.tags.length; i++) {
                var tag = json.tags[i];
             
                if (tag.displayName === '') {
                    dict['Default'] = JSON.stringify(tag);
                }
                else {
                    dict[tag.displayName] = JSON.stringify(tag);
                }
            }

            return(dict);
        }
```

A função buildTagSections itera nas tags JSON analisadas e chamada a função buildDiv para criar uma tag div para cada tag. Tal como com a Opção de consulta, cada tag é apresentada como uma ligação. Quando um utilizador clica na ligação, a tag expande-se e mostra as informações associadas à mesma. Se o utilizador clicar outra vez na ligação, a secção fecha-se e oculta as informações.

```javascript
        function buildTagSections(tags) {
            for (var tag in tags) {
                if (tags.hasOwnProperty(tag)) {
                    var tagSection = buildDiv(tags, tag);
                    document.getElementById('responseSection').appendChild(tagSection);
                }
            }  
        }


        function buildDiv(tags, tag) {
            var tagSection = document.createElement('div');
            tagSection.setAttribute('class', 'subSection');

            var link = document.createElement('a');
            link.setAttribute('href', '#');
            link.setAttribute('style', 'float: left;')
            link.text = tag;
            tagSection.appendChild(link);

            var contentDiv = document.createElement('div');
            contentDiv.setAttribute('id', tag);
            contentDiv.setAttribute('style', 'clear: left;')
            contentDiv.setAttribute('class', 'section');
            tagSection.appendChild(contentDiv);

            link.setAttribute('onclick', `expandCollapse("${tag}")`);
            divToggleMap[tag] = 0;  // 1 = show, 0 = hide

            addDivContent(contentDiv, tag, tags[tag]);

            return tagSection;
        }
```

A função buildDiv chama a função addDivContent para criar os conteúdos da tag div expansível de cada tag.

Os conteúdos das tags incluem o JSON das resposta das mesmas. A demonstração inclui o JSON para os programadores que querem ver o JSON atrás da resposta. Inicialmente, só são mostrados os primeiros cem carateres do JSON, mas pode clicar na cadeia JSON para o ver na íntegra. Se clicar outra vez, a cadeia JSON retrai-se novamente para os cem carateres.

Em seguida, adicione os tipos de ações que estão na tag. Para cada tipo de ação, chame as várias funções para adicionar as informações correspondentes.

```javascript
        function addDivContent(div, tag, json) {

            // Adds the first 100 characters of the json that contains
            // the tag's data. The user can click the text to show the
            // full json. They can click it again to collapse the json.
            var para = document.createElement('p');
            para.textContent = String(json).substr(0, 100) + '...';
            para.setAttribute('title', 'click to expand');
            para.setAttribute('style', 'cursor: pointer;')
            para.setAttribute('data-json', json);
            para.addEventListener('click', function(e) {
                var json = e.target.getAttribute('data-json');

                if (e.target.textContent.length <= 103) {  // 100 + '...'
                    e.target.textContent = json;
                    para.setAttribute('title', 'click to collapse');
                }
                else {
                    para.textContent = String(json).substr(0, 100) + '...';
                    para.setAttribute('title', 'click to expand');
                }
            });
            div.appendChild(para); 

            var parsedJson = JSON.parse(json);

            // Loop through all the actions in the tag and display them.
            for (var j = 0; j < parsedJson.actions.length; j++) {
                var action = parsedJson.actions[j];

                var subSectionDiv = document.createElement('div');
                subSectionDiv.setAttribute('class', 'subSection');
                div.appendChild(subSectionDiv);

                var h4 = document.createElement('h4');
                h4.innerHTML = action.actionType;
                subSectionDiv.appendChild(h4);

                if (action.actionType === 'ImageResults') {
                    addImageWithWebSearchUrl(subSectionDiv, parsedJson.image, action);
                }
                else if (action.actionType === 'DocumentLevelSuggestions') {
                    addRelatedSearches(subSectionDiv, action.data.value);
                }
                else if (action.actionType === 'RelatedSearches') {
                    addRelatedSearches(subSectionDiv, action.data.value);
                }
                else if (action.actionType === 'PagesIncluding') {
                    addPagesIncluding(subSectionDiv, action.data.value);
                }
                else if (action.actionType === 'VisualSearch') {
                    addRelatedImages(subSectionDiv, action.data.value);
                }
                else if (action.actionType === 'Recipes') {
                    addRecipes(subSectionDiv, action.data.value);
                }
                else if (action.actionType === 'ShoppingSources') {
                    addShopping(subSectionDiv, action.data.offers);
                }
                else if (action.actionType === 'ProductVisualSearch') {
                    addProducts(subSectionDiv, action.data.value);
                }
                else if (action.actionType === 'TextResults') {
                    addTextResult(subSectionDiv, action);
                }
                else if (action.actionType === 'Entity') {
                    addEntity(subSectionDiv, action);
                }
            }
        }
```

As funções abaixo são todas as funções que apresentam informações das diferentes ações. A maioria destas funções são bastante simples &mdash; ou disponibilizam uma imagem clicável ou uma ligação clicável que encaminham o utilizador para uma página Web onde ele pode obter mais informações sobre a imagem (em Bing.com ou na página Web do anfitrião da imagem). O tutorial não apresenta todos os dados associados às informações. Para ver todos os campos disponíveis para uma informação, veja [Bing Visual Search Reference](https://aka.ms/bingvisualsearchreferencedoc) (Referência da Pesquisa Visual do Bing).

Não se esqueça de que tem de mostrar uma quantidade mínima de dados; o resto é consigo. Para garantir que está em conformidade, veja [Bing Use and Display Requirements](./use-and-display-requirements.md) (Requisitos de Utilização e Apresentação do Bing).


```javascript
        function addRelatedImages(div, images) {
            var length = (images.length > 10) ? 10 : images.length;

            // Set the title to the website that hosts the image. The title displays 
            // when the user hovers over the image. 

            // Make the image clickable. If the user clicks the image, they're taken
            // to the image in Bing.com.

            for (var j = 0; j < length; j++) {
                var img = document.createElement('img');
                img.setAttribute('src', images[j].thumbnailUrl + '&w=120&h=120');
                img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
                img.setAttribute('title', images[j].hostPageDisplayUrl);
                img.setAttribute('data-webSearchUrl', images[j].webSearchUrl)

                img.addEventListener('click', function(e) {
                    var url = e.target.getAttribute('data-webSearchUrl');
                    window.open(url, 'foo');
                })

                div.appendChild(img);
            }
        }


        // Display links to the first 5 webpages that include the image.
        // TODO: Add 'more' link in case the user wants to see all of them.
        function addPagesIncluding(div, pages) {
            var length = (pages.length > 5) ? 5 : pages.length;

            for (var j = 0; j < length; j++) {
                var page = document.createElement('a');
                page.text = pages[j].name;
                page.setAttribute('href', pages[j].hostPageUrl);
                page.setAttribute('style', 'margin: 20px 20px 0 0');
                page.setAttribute('target', '_blank')
                div.appendChild(page);

                div.appendChild(document.createElement('br'));
            }
        }


        // Display the first 10 related searches. Include a link with the image
        // that when clicked, takes the user to Bing.com and displays the 
        // related search results.
        // TODO: Add 'more' link in case the user wants to see all of them.
        function addRelatedSearches(div, relatedSearches) {
            var length = (relatedSearches.length > 10) ? 10 : relatedSearches.length;

            for (var j = 0; j < length; j++) {
                var childDiv = document.createElement('div');
                childDiv.setAttribute('class', 'stackLink');
                div.appendChild(childDiv);

                var img = document.createElement('img');
                img.setAttribute('src', relatedSearches[j].thumbnail.url + '&w=120&h=120');
                img.setAttribute('style', 'margin: 20px 20px 0 0;');
                childDiv.appendChild(img);

                var relatedSearch = document.createElement('a');
                relatedSearch.text = relatedSearches[j].displayText;
                relatedSearch.setAttribute('href', relatedSearches[j].webSearchUrl);
                relatedSearch.setAttribute('target', '_blank');
                childDiv.appendChild(relatedSearch);

            }
        }


        // Display links to the first 10 recipes. Include the recipe's rating,
        // if available. 
        // TODO: Add 'more' link in case the user wants to see all of them.
        function addRecipes(div, recipes) {
            var length = (recipes.length > 10) ? 10 : recipes.length;

            for (var j = 0; j < length; j++) {
                var para = document.createElement('p');

                var recipe = document.createElement('a');
                recipe.text = recipes[j].name;
                recipe.setAttribute('href', recipes[j].url);
                recipe.setAttribute('style', 'margin: 20px 20px 0 0');
                recipe.setAttribute('target', '_blank')
                para.appendChild(recipe);

                if (recipes[j].hasOwnProperty('aggregateRating')) {
                    var span = document.createElement('span');
                    span.textContent = 'rating: ' + recipes[j].aggregateRating.text;
                    para.appendChild(span);
                }

                div.appendChild(para);
            }
        }


        // Display links for the first 10 shopping offers.
        // TODO: Add 'more' link in case the user wants to see all of them.
        function addShopping(div, offers) {
            var length = (offers.length > 10) ? 10 : offers.length;

            for (var j = 0; j < length; j++) {
                var para = document.createElement('p');

                var offer = document.createElement('a');
                offer.text = offers[j].name;
                offer.setAttribute('href', offers[j].url);
                offer.setAttribute('style', 'margin: 20px 20px 0 0');
                offer.setAttribute('target', '_blank')
                para.appendChild(offer);

                var span = document.createElement('span');
                span.textContent = 'by ' + offers[j].seller.name + ' | ' + offers[j].price + ' ' + offers[j].priceCurrency;
                para.appendChild(span);

                div.appendChild(para);
            }
        }


        // Display the first 10 related products. Display a clickable image of the 
        // product that takes the user to Bing.com search results for the product.
        // If there are any offers associated with the product, provide links to the offers.
        // TODO: Add 'more' link in case the user wants to see all of them.
        function addProducts(div, products) {
            var length = (products.length > 10) ? 10 : products.length;

            for (var j = 0; j < length; j++) {
                var childDiv = document.createElement('div');
                childDiv.setAttribute('class', 'stackLink');
                div.appendChild(childDiv);

                var img = document.createElement('img');
                img.setAttribute('src', products[j].thumbnailUrl + '&w=120&h=120');
                img.setAttribute('title', products[j].name);
                img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
                img.setAttribute('data-webSearchUrl', products[j].webSearchUrl)
                img.addEventListener('click', function(e) {
                    var url = e.target.getAttribute('data-webSearchUrl');
                    window.open(url, 'foo');
                })
                childDiv.appendChild(img);

                if (products[j].insightsMetadata.hasOwnProperty('aggregateOffer')) {
                    if (products[j].insightsMetadata.aggregateOffer.offerCount > 0) {
                        var offers = products[j].insightsMetadata.aggregateOffer.offers;

                        // Show all the offers. Not all markets provide links to offers.
                        for (var i = 0; i < offers.length; i++) {  
                            var para = document.createElement('p');

                            var offer = document.createElement('a');
                            offer.text = offers[i].name;
                            offer.setAttribute('href', offers[i].url);
                            offer.setAttribute('style', 'margin: 20px 20px 0 0');
                            offer.setAttribute('target', '_blank')
                            para.appendChild(offer);

                            var span = document.createElement('span');
                            span.textContent = 'by ' + offers[i].seller.name + ' | ' + offers[i].price + ' ' + offers[i].priceCurrency;
                            para.appendChild(span);

                            childDiv.appendChild(para);
                        }
                    }
                    else {  // Otherwise, just show the lowest price that Bing found.
                        var offer = products[j].insightsMetadata.aggregateOffer;

                        var para = document.createElement('p');
                        para.textContent = `${offer.name} | ${offer.lowPrice} ${offer.priceCurrency}`; 

                        childDiv.appendChild(para);
                    }
                }
            }
        }


        // If Bing recognized any text in the image, display the text.
        function addTextResult(div, action) {
            var text = document.createElement('p');
            text.textContent = action.displayName;
            div.appendChild(text);
        }


        // If the image is of a person, the tag might include an entity
        // action type. Display a link that takes the user to Bing.com
        // where they can get details about the entity.
        function addEntity(div, action) {
            var entity = document.createElement('a');
            entity.text = action.displayName;
            entity.setAttribute('href', action.webSearchUrl);
            entity.setAttribute('style', 'margin: 20px 20px 0 0');
            entity.setAttribute('target', '_blank');
            div.appendChild(entity);
        }


        // Adds a clickable image to the div that takes the user to
        // Bing.com search results.
        function addImageWithWebSearchUrl(div, image, action) {
            var img = document.createElement('img');
            img.setAttribute('src', image.thumbnailUrl + '&w=120&h=120');
            img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
            img.setAttribute('data-webSearchUrl', action.webSearchUrl);
            img.addEventListener('click', function(e) {
                var url = e.target.getAttribute('data-webSearchUrl');
                window.open(url, 'foo');
            })
            div.appendChild(img);
        }

```



## <a name="adding-styles-to-make-the-page-display-correctly"></a>Adicionar estilos para apresentar a página corretamente

Adicione a seguinte secção \<style\> à tag \<head\>.

```html
        <style>
            
            .thumb {
                height: 75px;
                border: 1px solid #000;
            }

            .stackLink {
                width:180px;
                min-height:210px;
                display:inline-block;
            }
            .stackLink a {
                float:left;
                clear:left;
            }

            .section {
                float:left;
                display:none;
            }

            .subSection {
                clear:left;
                float:left;
            }

        </style>
```



## <a name="complete-code"></a>Código completo

Segue-se o exemplo completo do HTML e do JavaScript.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Bing Visual Search Demo</title>
        
        <style>
            
            .thumb {
                height: 75px;
                border: 1px solid #000;
            }

            .stackLink {
                width:180px;
                min-height:210px;
                display:inline-block;
            }
            .stackLink a {
                float:left;
                clear:left;
            }

            .section {
                float:left;
                display:none;
            }

            .subSection {
                clear:left;
                float:left;
            }

        </style>

        <script>

            // Handles the user's file selection. The file input button specifies this handler.
            function handleFileSelect(selector) {
                var files = document.getElementById(selector).files; // FileList object

                // The list will contain only one file.
                for (var i = 0, f; f = files[i]; i++) {
        
                    // Only process image files.
                    if (!f.type.match('image.*')) {
                        alert("Selected file must be an image file.");
                        document.getElementById("uploadImage").value = null;
                        continue;
                    }
        
                    // Image must be <= 1 MB and should be about 1500px.
                    if (f.size > 1000000) {
                        alert("Image must be less than 1 MB.");
                        document.getElementById("uploadImage").value = null;
                        continue;
                    }


                    var reader = new FileReader();
        
                    // Capture the file information.
                    reader.onload = (function(theFile) {
                        return function(e) {
                            var fileOutput = document.getElementById('thumbnail');

                            if (fileOutput.childElementCount > 0) {
                                fileOutput.removeChild(fileOutput.lastChild);  // Remove the current pic, if it exists
                            }

                            // Render thumbnail.
                            var span = document.createElement('span');
                            span.innerHTML = ['<img class="thumb" src="', e.target.result,
                                            '" title="', escape(theFile.name), '"/>'].join('');
                            fileOutput.insertBefore(span, null);
                        };
                    })(f);
        
                    // Read in the image file as a data URL.
                    reader.readAsDataURL(f);
                }
            }


            // Contains the toggle state of divs.
            var divToggleMap = {};  // divToggleMap['foo'] = 0;  // 1 = show, 0 = hide


            // Toggles between showing and hiding the specified div.
            function expandCollapse(divToToggle) {
                var div = document.getElementById(divToToggle);

                if (divToggleMap[divToToggle] == 1) {   // if div is expanded
                    div.style.display = "none";
                    divToggleMap[divToToggle] = 0;
                }
                else {                                  // if div is collapsed
                    div.style.display = "inline-block";
                    divToggleMap[divToToggle] = 1;
                }
            }


            // Called when the user clicks the Query insights button.
            function handleQuery() {
                var subscriptionKey = document.getElementById('key').value;

                // Make sure user provided a subscription key and image.
                // For this demo, the user provides the key but typically you'd 
                // get it from secured storage.
                if (subscriptionKey.length !== 32) {
                    alert("Subscription key length is not valid. Enter a valid key.");
                    document.getElementById('key').focus();
                    return;
                }

                var imagePath = document.getElementById('uploadImage').value;

                if (imagePath.length === 0)
                {
                    alert("Please select an image to upload.");
                    document.getElementById('uploadImage').focus();
                    return;
                }

                var responseDiv = document.getElementById('responseSection');

                // Clear out the response from the last query.
                while (responseDiv.childElementCount > 0) {
                    responseDiv.removeChild(responseDiv.lastChild);
                }

                // Send the request to Bing to get insights about the image.
                var f = document.getElementById('uploadImage').files[0];
                sendRequest(f, subscriptionKey);
            }


            // Format the request and send it.
            function sendRequest(file, key) {
                var market = document.getElementById('mkt').value;
                var safeSearch = document.getElementById('safesearch').value;
                var baseUri = `https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=${market}&safesearch=${safeSearch}`;

                var form = new FormData();
                form.append("image", file);

                var request = new XMLHttpRequest();

                request.open("POST", baseUri);
                request.setRequestHeader('Ocp-Apim-Subscription-Key', key);
                request.addEventListener('load', handleResponse);
                request.send(form);
            }


            // Handles the response from Bing. Parses the response and 
            // the tag divs.
            function handleResponse() {
                if(this.status !== 200){
                    alert("Error calling Bing Visual Search. See console log for details.");
                    console.log(this.responseText);
                    return;
                }

                var tags = parseResponse(JSON.parse(this.responseText));
                var h4 = document.createElement('h4');
                h4.textContent = 'Bing internet search results';
                document.getElementById('responseSection').appendChild(h4);
                buildTagSections(tags);

                document.body.style.cursor = 'default'; // reset the wait curor set by query insights button
            }


            // Parses the json response by tags.
            function parseResponse(json) {
                var dict = {};

                for (var i =0; i < json.tags.length; i++) {
                    var tag = json.tags[i];
                
                    if (tag.displayName === '') {
                        dict['Default'] = JSON.stringify(tag);
                    }
                    else {
                        dict[tag.displayName] = JSON.stringify(tag);
                    }
                }

                return(dict);
            }


            // Builds divs fro each tag in the response.
            function buildTagSections(tags) {
                for (var tag in tags) {
                    if (tags.hasOwnProperty(tag)) {
                        var tagSection = buildDiv(tags, tag);
                        document.getElementById('responseSection').appendChild(tagSection);
                    }
                }  
            }


            // Builds the div for the specified tag. The div is shown as a
            // link that when clicked, expands or collapses. The divs are 
            // initially collapsed.
            function buildDiv(tags, tag) {
                var tagSection = document.createElement('div');
                tagSection.setAttribute('class', 'subSection');

                var link = document.createElement('a');
                link.setAttribute('href', '#');
                link.setAttribute('style', 'float: left;')
                link.text = tag;
                tagSection.appendChild(link);

                var contentDiv = document.createElement('div');
                contentDiv.setAttribute('id', tag);
                contentDiv.setAttribute('style', 'clear: left;')
                contentDiv.setAttribute('class', 'section');
                tagSection.appendChild(contentDiv);

                link.setAttribute('onclick', `expandCollapse("${tag}")`);
                divToggleMap[tag] = 0;  // 1 = show, 0 = hide

                addDivContent(contentDiv, tag, tags[tag]);

                return tagSection;
            }


            // Adds the tag's action types to the div.
            function addDivContent(div, tag, json) {

                // Adds the first 100 characters of the json that contains
                // the tag's data. The user can click the text to show the
                // full json. They can click it again to collapse the json.
                var para = document.createElement('p');
                para.textContent = String(json).substr(0, 100) + '...';
                para.setAttribute('title', 'click to expand');
                para.setAttribute('style', 'cursor: pointer;')
                para.setAttribute('data-json', json);
                para.addEventListener('click', function(e) {
                    var json = e.target.getAttribute('data-json');

                    if (e.target.textContent.length <= 103) {  // 100 + '...'
                        e.target.textContent = json;
                        para.setAttribute('title', 'click to collapse');
                    }
                    else {
                        para.textContent = String(json).substr(0, 100) + '...';
                        para.setAttribute('title', 'click to expand');
                    }
                });
                div.appendChild(para); 

                var parsedJson = JSON.parse(json);

                // Loop through all the actions in the tag and display them.
                for (var j = 0; j < parsedJson.actions.length; j++) {
                    var action = parsedJson.actions[j];

                    var subSectionDiv = document.createElement('div');
                    subSectionDiv.setAttribute('class', 'subSection');
                    div.appendChild(subSectionDiv);

                    var h4 = document.createElement('h4');
                    h4.innerHTML = action.actionType;
                    subSectionDiv.appendChild(h4);

                    if (action.actionType === 'ImageResults') {
                        addImageWithWebSearchUrl(subSectionDiv, parsedJson.image, action);
                    }
                    else if (action.actionType === 'DocumentLevelSuggestions') {
                        addRelatedSearches(subSectionDiv, action.data.value);
                    }
                    else if (action.actionType === 'RelatedSearches') {
                        addRelatedSearches(subSectionDiv, action.data.value);
                    }
                    else if (action.actionType === 'PagesIncluding') {
                        addPagesIncluding(subSectionDiv, action.data.value);
                    }
                    else if (action.actionType === 'VisualSearch') {
                        addRelatedImages(subSectionDiv, action.data.value);
                    }
                    else if (action.actionType === 'Recipes') {
                        addRecipes(subSectionDiv, action.data.value);
                    }
                    else if (action.actionType === 'ShoppingSources') {
                        addShopping(subSectionDiv, action.data.offers);
                    }
                    else if (action.actionType === 'ProductVisualSearch') {
                        addProducts(subSectionDiv, action.data.value);
                    }
                    else if (action.actionType === 'TextResults') {
                        addTextResult(subSectionDiv, action);
                    }
                    else if (action.actionType === 'Entity') {
                        addEntity(subSectionDiv, action);
                    }
                }
            }


            // Display the first 10 related images.
            // TODO: Add 'more' link in case the user wants to see all of them.
            function addRelatedImages(div, images) {
                var length = (images.length > 10) ? 10 : images.length;

                // Set the title to the website that hosts the image. The title displays 
                // when the user hovers over the image. 

                // Make the image clickable. If the user clicks the image, they're taken
                // to the image in Bing.com.

                for (var j = 0; j < length; j++) {
                    var img = document.createElement('img');
                    img.setAttribute('src', images[j].thumbnailUrl + '&w=120&h=120');
                    img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
                    img.setAttribute('title', images[j].hostPageDisplayUrl);
                    img.setAttribute('data-webSearchUrl', images[j].webSearchUrl)

                    img.addEventListener('click', function(e) {
                        var url = e.target.getAttribute('data-webSearchUrl');
                        window.open(url, 'foo');
                    })

                    div.appendChild(img);
                }
            }


            // Display links to the first 5 webpages that include the image.
            // TODO: Add 'more' link in case the user wants to see all of them.
            function addPagesIncluding(div, pages) {
                var length = (pages.length > 5) ? 5 : pages.length;

                for (var j = 0; j < length; j++) {
                    var page = document.createElement('a');
                    page.text = pages[j].name;
                    page.setAttribute('href', pages[j].hostPageUrl);
                    page.setAttribute('style', 'margin: 20px 20px 0 0');
                    page.setAttribute('target', '_blank')
                    div.appendChild(page);

                    div.appendChild(document.createElement('br'));
                }
            }


            // Display the first 10 related searches. Include a link with the image
            // that when clicked, takes the user to Bing.com and displays the 
            // related search results.
            // TODO: Add 'more' link in case the user wants to see all of them.
            function addRelatedSearches(div, relatedSearches) {
                var length = (relatedSearches.length > 10) ? 10 : relatedSearches.length;

                for (var j = 0; j < length; j++) {
                    var childDiv = document.createElement('div');
                    childDiv.setAttribute('class', 'stackLink');
                    div.appendChild(childDiv);

                    var img = document.createElement('img');
                    img.setAttribute('src', relatedSearches[j].thumbnail.url + '&w=120&h=120');
                    img.setAttribute('style', 'margin: 20px 20px 0 0;');
                    childDiv.appendChild(img);

                    var relatedSearch = document.createElement('a');
                    relatedSearch.text = relatedSearches[j].displayText;
                    relatedSearch.setAttribute('href', relatedSearches[j].webSearchUrl);
                    relatedSearch.setAttribute('target', '_blank');
                    childDiv.appendChild(relatedSearch);

                }
            }


            // Display links to the first 10 recipes. Include the recipe's rating,
            // if available. 
            // TODO: Add 'more' link in case the user wants to see all of them.
            function addRecipes(div, recipes) {
                var length = (recipes.length > 10) ? 10 : recipes.length;

                for (var j = 0; j < length; j++) {
                    var para = document.createElement('p');

                    var recipe = document.createElement('a');
                    recipe.text = recipes[j].name;
                    recipe.setAttribute('href', recipes[j].url);
                    recipe.setAttribute('style', 'margin: 20px 20px 0 0');
                    recipe.setAttribute('target', '_blank')
                    para.appendChild(recipe);

                    if (recipes[j].hasOwnProperty('aggregateRating')) {
                        var span = document.createElement('span');
                        span.textContent = 'rating: ' + recipes[j].aggregateRating.text;
                        para.appendChild(span);
                    }

                    div.appendChild(para);
                }
            }


            // Display links for the first 10 shopping offers.
            // TODO: Add 'more' link in case the user wants to see all of them.
            function addShopping(div, offers) {
                var length = (offers.length > 10) ? 10 : offers.length;

                for (var j = 0; j < length; j++) {
                    var para = document.createElement('p');

                    var offer = document.createElement('a');
                    offer.text = offers[j].name;
                    offer.setAttribute('href', offers[j].url);
                    offer.setAttribute('style', 'margin: 20px 20px 0 0');
                    offer.setAttribute('target', '_blank')
                    para.appendChild(offer);

                    var span = document.createElement('span');
                    span.textContent = 'by ' + offers[j].seller.name + ' | ' + offers[j].price + ' ' + offers[j].priceCurrency;
                    para.appendChild(span);

                    div.appendChild(para);
                }
            }


            // Display the first 10 related products. Display a clickable image of the 
            // product that takes the user to Bing.com search results for the product.
            // If there are any offers associated with the product, provide links to the offers.
            // TODO: Add 'more' link in case the user wants to see all of them.
            function addProducts(div, products) {
                var length = (products.length > 10) ? 10 : products.length;

                for (var j = 0; j < length; j++) {
                    var childDiv = document.createElement('div');
                    childDiv.setAttribute('class', 'stackLink');
                    div.appendChild(childDiv);

                    var img = document.createElement('img');
                    img.setAttribute('src', products[j].thumbnailUrl + '&w=120&h=120');
                    img.setAttribute('title', products[j].name);
                    img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
                    img.setAttribute('data-webSearchUrl', products[j].webSearchUrl)
                    img.addEventListener('click', function(e) {
                        var url = e.target.getAttribute('data-webSearchUrl');
                        window.open(url, 'foo');
                    })
                    childDiv.appendChild(img);

                    if (products[j].insightsMetadata.hasOwnProperty('aggregateOffer')) {
                        if (products[j].insightsMetadata.aggregateOffer.offerCount > 0) {
                            var offers = products[j].insightsMetadata.aggregateOffer.offers;

                            // Show all the offers. Not all markets provide links to offers.
                            for (var i = 0; i < offers.length; i++) {  
                                var para = document.createElement('p');

                                var offer = document.createElement('a');
                                offer.text = offers[i].name;
                                offer.setAttribute('href', offers[i].url);
                                offer.setAttribute('style', 'margin: 20px 20px 0 0');
                                offer.setAttribute('target', '_blank')
                                para.appendChild(offer);

                                var span = document.createElement('span');
                                span.textContent = 'by ' + offers[i].seller.name + ' | ' + offers[i].price + ' ' + offers[i].priceCurrency;
                                para.appendChild(span);

                                childDiv.appendChild(para);
                            }
                        }
                        else {  // Otherwise, just show the lowest price that Bing found.
                            var offer = products[j].insightsMetadata.aggregateOffer;

                            var para = document.createElement('p');
                            para.textContent = `${offer.name} | ${offer.lowPrice} ${offer.priceCurrency}`; 

                            childDiv.appendChild(para);
                        }
                    }
                }
            }


            // If Bing recognized any text in the image, display the text.
            function addTextResult(div, action) {
                var text = document.createElement('p');
                text.textContent = action.displayName;
                div.appendChild(text);
            }


            // If the image is of a person, the tag might include an entity
            // action type. Display a link that takes the user to Bing.com
            // where they can get details about the entity.
            function addEntity(div, action) {
                var entity = document.createElement('a');
                entity.text = action.displayName;
                entity.setAttribute('href', action.webSearchUrl);
                entity.setAttribute('style', 'margin: 20px 20px 0 0');
                entity.setAttribute('target', '_blank');
                div.appendChild(entity);
            }


            // Adds a clickable image to the div that takes the user to
            // Bing.com search results.
            function addImageWithWebSearchUrl(div, image, action) {
                var img = document.createElement('img');
                img.setAttribute('src', image.thumbnailUrl + '&w=120&h=120');
                img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
                img.setAttribute('data-webSearchUrl', action.webSearchUrl);
                img.addEventListener('click', function(e) {
                    var url = e.target.getAttribute('data-webSearchUrl');
                    window.open(url, 'foo');
                })
                div.appendChild(img);
            }

        </script>

    </head>

    <body>

        <div id="requestSection">
            <div>
                <p>Select image to get insights from Bing:
                    <input type="file" accept="image/*" id="uploadImage" name="files[]" size=40 onchange="handleFileSelect('uploadImage')" />
                </p>

                <output id="thumbnail"></output>
            </div>

            <div>
                <p>Subscription key: 
                    <input type="text" id="key" name="subscription" size=40 maxlength="32" />
                </p>
            </div>

            <div>
                <p><a href="#" onclick="expandCollapse(options.id)">Query options</a></p>

                <div id="options" style="display:none">
                    <p style="margin-left: 20px">Market: 
                        <select id="mkt">
                            <option value="es-AR">Argentina (Spanish)</option>
                            <option value="en-AU">Australia (English)</option>
                            <option value="de-AT">Austria (German)</option>
                            <option value="nl-BE">Belgium (Dutch)</option>
                            <option value="fr-BE">Belgium (French)</option>
                            <option value="pt-BR">Brazil (Portuguese)</option>
                            <option value="en-CA">Canada (English)</option>
                            <option value="fr-CA">Canada (French)</option>
                            <option value="es-CL">Chile (Spanish)</option>
                            <option value="da-DK">Denmark (Danish)</option>
                            <option value="fi-FI">Finland (Finnish)</option>
                            <option value="fr-FR">France (French)</option>
                            <option value="de-DE">Germany (German)</option>
                            <option value="zh-HK">Hong Kong (Traditional Chinese)</option>
                            <option value="en-IN">India (English)</option>
                            <option value="en-ID">Indonesia (English)</option>
                            <option value="it-IT">Italy (Italian)</option>
                            <option value="ja-JP">Japan (Japanese)</option>
                            <option value="ko-KR">Korea (Korean)</option>
                            <option value="en-MY">Malaysia (English)</option>
                            <option value="es-MX">Mexico (Spanish)</option>
                            <option value="nl-NL">Netherlands (Dutch)</option>
                            <option value="en-NZ">New Zealand (English)</option>
                            <option value="no-NO">Norway (Norwegian)</option>
                            <option value="zh-CN">People's Republic of China (Chinese)</option>
                            <option value="pl-PL">Poland (Polish)</option>
                            <option value="pt-PT">Portugal (Portuguese)</option>
                            <option value="en-PH">Philippines (English)</option>
                            <option value="ru-RU">Russia (Russian)</option>
                            <option value="ar-SA">Saudi Arabia (Arabic)</option>
                            <option value="en-ZA">South Africa (English)</option>
                            <option value="es-ES">Spain (Spanish)</option>
                            <option value="sv-SE">Sweden (Swedish)</option>
                            <option value="fr-CH">Switzerland (French)</option>
                            <option value="de-CH">Switzerland (German)</option>
                            <option value="zh-TW">Taiwan (Traditional Chinese)</option>
                            <option value="tr-TR">Turkey (Turkish)</option>
                            <option value="en-GB">United Kingdom (English)</option>
                            <option value="en-US" selected>United States (English)</option>
                            <option value="es-US">United States (Spanish)</option>
                        </select>
                    </p>
                    <p style="margin-left: 20px">Safe search: 
                        <select id="safesearch">
                            <option value="moderate" selected>Moderate</option>
                            <option value="strict">Strict</option>
                            <option value="off">off</option>
                        </select>
                    </p>
                </div>
            </div>
        </div>
        
        <p><input type="button" id="query" value="Get insights" onclick="document.body.style.cursor='wait'; handleQuery()" /></p>

        <hr />
        
        <div id="responseSection"></div>
    </body>
</html>      
```

## <a name="next-steps"></a>Passos seguintes

Para saber como utilizar um token de informações para as obter veja [Bing Visual Search SDK ImageInsightsToken tutorial](.\tutorial-visual-search-insights-token.md) (Tutorial de ImageInsightsToken do SDK da Pesquisa Visual do Bing).