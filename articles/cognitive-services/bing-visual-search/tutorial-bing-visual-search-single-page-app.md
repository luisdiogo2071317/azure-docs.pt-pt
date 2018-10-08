---
title: 'Tutorial: criar uma aplicação Web de página única - Pesquisa Visual do Bing'
titleSuffix: Azure Cognitive Services
description: Mostra como utilizar a API de Pesquisa Visual do Bing numa aplicação Web de página única.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 10/04/2017
ms.author: v-brapel
ms.openlocfilehash: e3cd36d799256406b3ae12f35303bd2406468b3c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227185"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Tutorial: aplicação Web de página única de Pesquisa Visual

A API de Pesquisa Visual do Bing proporciona uma experiência semelhante aos detalhes de imagens mostrados em Bing.com/images. Com a Pesquisa Visual, pode especificar uma imagem e receber informações sobre a mesma, como imagens visualmente parecidas, origens de compras, páginas Web que incluem a imagem, entre outras. 

Este tutorial expande a aplicação Web de página única do tutorial de Pesquisa de Imagens do Bing (veja [Aplicação Web de página única](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Para obter o código-fonte completo para iniciar este tutorial, veja [Aplicação Web de página única (código-fonte)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Para obter o código-fonte final deste tutorial, veja [Aplicação Web de página única de Pesquisa Visual](tutorial-bing-visual-search-single-page-app-source.md).

As tarefas abrangidas são:

> [!div class="checklist"]
> * Chamar a API de Pesquisa Visual do Bing com um token de informações de imagem
> * Apresentar imagens semelhantes

## <a name="call-bing-visual-search"></a>Chamar a Pesquisa Visual do Bing
Edite o tutorial de Pesquisa de Imagens do Bing e adicione o seguinte código ao final do elemento de script na linha 409. Este código chama a API de Pesquisa Visual do Bing e apresenta os resultados.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}

function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;       
    
    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>Recolha do token de informações
Adicione o seguinte código ao objeto `searchItemsRenderer` na linha 151. Este código adiciona uma ligação **encontrar semelhante** que chama a função `bingVisualSearch` ao clicar na ligação. A função recebe o imageInsightsToken como um argumento.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Apresentar imagens semelhantes
Adicione o seguinte código HTML na linha 601. Este código de marcação adiciona um elemento utilizado para apresentar os resultados da chamada à API de Pesquisa Visual do Bing.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Com todo o novo código JavaScript e elementos HTML implementados, os resultados da pesquisa são apresentados com uma ligação **encontrar semelhante**. Clique na ligação para preencher a secção **Semelhante** com as imagens semelhantes à que escolheu. Poderá ter de expandir a secção **Semelhante** para mostrar as imagens.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Origem de aplicação Web de página única de Pesquisa Visual](tutorial-bing-visual-search-single-page-app-source.md)
> [Referência da API de Pesquisa Visual do Bing](https://aka.ms/bingvisualsearchreferencedoc)