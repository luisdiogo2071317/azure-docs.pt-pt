---
title: Aplicação de Web de página única de pesquisa de imagem do Bing | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como utilizar a API de pesquisa do Bing imagem numa aplicação Web de página única.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-brapel
ms.openlocfilehash: 303d7745167d2ea25fda083ed99881ac4e0a7ec7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354139"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Tutorial: Página única pesquisa Visual de aplicação Web

API de pesquisa do Bing Visual fornece uma experiência semelhante para os detalhes da imagem apresentados na Bing.com/images. Com o Visual pesquisa, pode especificar uma imagem e voltar a informações sobre a imagem, como imagens visualmente semelhantes, compras origens, páginas Web que incluem a imagem e muito mais. 

Este tutorial expande a aplicação web de página única o tutorial de pesquisa de imagem do Bing (consulte [aplicação Web de página única](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Código de origem completo iniciar este tutorial, consulte [aplicação Web de página única (código de origem)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Para o código de origem final deste tutorial, consulte [aplicação Web de página única pesquisa Visual](tutorial-bing-visual-search-single-page-app-source.md).

As tarefas abrangidas são:

> [!div class="checklist"]
> * Chamar a API do Bing Visual pesquisa com um token de informações de imagem
> * Apresentar imagens semelhantes

## <a name="call-bing-visual-search"></a>Chamada pesquisa Visual do Bing
Edite o tutorial de pesquisa do Bing imagem e adicione o seguinte código no fim do elemento de script na linha 409. Este código chama a API do Bing Visual pesquisa e apresenta os resultados.

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

## <a name="capture-insights-token"></a>Captura de token de insights
Adicione o seguinte código para o `searchItemsRenderer` objeto na linha 151. Este código adiciona uma **encontrar semelhantes** ligação que chama o `bingVisualSearch` funcionar quando clicado. A função recebe o imageInsightsToken como um argumento.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Apresentar imagens semelhantes
Adicione o seguinte código HTML na linha 601. Este código de marcação adiciona um elemento utilizado para apresentar os resultados da chamada de API de pesquisa do Bing Visual.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Com todas as o novo código JavaScript e HTML elementos no local, os resultados da pesquisa são apresentados com um **encontrar semelhantes** ligação. Clique na ligação para preencher o **semelhante** secção com imagens semelhantes às que selecionado. Poderá ter de expandir o **semelhante** secção para mostrar as imagens.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Origem de aplicação Web de página única pesquisa Visual](tutorial-bing-visual-search-single-page-app-source.md)
> [referência da API de pesquisa do Bing Visual](https://aka.ms/bingvisualsearchreferencedoc)