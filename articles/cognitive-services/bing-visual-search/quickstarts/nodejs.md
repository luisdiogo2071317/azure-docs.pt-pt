---
title: 'Início rápido: Obtenha informações de imagem usando a API de REST de pesquisa Visual do Bing e o node. js'
titleSuffix: Azure Cognitive Services
description: Aprenda a carregar uma imagem para a API de pesquisa Visual do Bing e obter conhecimentos sobre ele.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 7a0103e21b4c287526e53b9f886e98027f49c392
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863997"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Início rápido: Obtenha informações de imagem usando a API de REST de pesquisa Visual do Bing e o node. js

Utilize este guia de introdução para efetuar a primeira chamada à API de pesquisa Visual do Bing e ver os resultados da pesquisa. Esta aplicação JavaScript simple carrega uma imagem para a API e apresenta as informações devolvidas sobre ele. Embora esse aplicativo é escrito em JavaScript, a API é um serviço RESTful Web compatível com a maioria das linguagens de programação.

Ao carregar uma imagem do local, os dados do formulário tem de incluir o cabeçalho Content-Disposition. O respetivo parâmetro `name` tem de ser definido como “image” e o parâmetro `filename` pode ser definido como qualquer cadeia. Os conteúdos do formulário são o binário da imagem. O tamanho máximo de imagem que pode carregar é 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Pré-requisitos

* [Node.js](https://nodejs.org/en/download/)
* O módulo de pedido para JavaScript
    * Pode instalar este módulo com `npm install request`
* O módulo de dados do formulário
    * Pode instalar este módulo com `npm install form-data`


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="initialize-the-application"></a>Inicializar o aplicativo

1. Criar um novo ficheiro JavaScript no seu IDE ou editor favorito e defina os seguintes requisitos:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Crie variáveis para o ponto final de API, a chave de subscrição e o caminho para a sua imagem.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Criar uma função chamada `requestCallback()` para imprimir a resposta da API.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Construir e enviar o pedido de pesquisa

1. Crie um novo com dados de formulário `FormData()`e de acréscimo seu caminho de imagem, usando `fs.createReadStream()`.
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Utilize a biblioteca de pedido para carregar a imagem, chamar `requestCallback()` para imprimir a resposta. Certifique-se de que adicionar a chave de subscrição para o cabeçalho do pedido. 

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar uma aplicação web de pesquisa personalizada](../tutorial-bing-visual-search-single-page-app.md)
