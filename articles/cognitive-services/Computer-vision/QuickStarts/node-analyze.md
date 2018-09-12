---
title: Início rápido análise de imagem da API de Imagem Digitalizada com Node.js | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai analisar uma imagem através da Imagem Digitalizada com o Node.js nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: dab6547e08b1b01a9090a817d728c86359c680f2
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772166"
---
# <a name="quickstart-analyze-a-remote-image---rest-nodejs"></a>Início rápido: Analisar uma imagem remota - REST, Node.js

Neste início rápido, vai analisar uma imagem para extrair funcionalidades visuais através da Imagem Digitalizada.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-image-request"></a>Pedido de Análise de Imagem

Com o [método Análise de Imagem](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), pode extrair funcionalidades visuais com base no conteúdo da imagem. Pode carregar uma imagem ou especificar um URL da imagem e escolher as funcionalidades que deve devolver, incluindo:

* Uma lista detalhada das etiquetas relacionadas com o conteúdo da imagem.
* Uma descrição do conteúdo da imagem numa frase completa.
* As coordenadas, o sexo e a idade de qualquer rosto que a imagem contenha.
* O ImageType (ClipArt ou um desenho de linha).
* A cor dominante, a cor de destaque ou se uma imagem é a preto e branco.
* A categoria definida nesta [taxonomia](../Category-Taxonomy.md).
* A imagem contém conteúdo para adultos ou é sexualmente sugestiva?

Para executar o exemplo, siga os passos seguintes:

1. Copie o código seguinte para um editor.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `uriBase` para a localização na qual obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere o valor `imageUrl` para a imagem que pretende analisar.
1. Opcionalmente, altere o idioma de resposta (`'language': 'en'`).
1. Guarde o ficheiro com uma extensão `.js`.
1. Abra a linha de comandos do Node.js e execute o ficheiro, por exemplo: `node myfile.js`.

Este exemplo utiliza o pacote de [pedido](https://www.npmjs.com/package/request) npm.

```nodejs
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase =
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze';

const imageUrl =
    'http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

// Request parameters.
const params = {
    'visualFeatures': 'Categories,Description,Color',
    'details': '',
    'language': 'en'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="analyze-image-response"></a>Resposta de Análise de Imagem

O JSON devolve uma resposta de êxito, por exemplo:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBwImg": false
  },
  "requestId": "81b4e400-e3c1-41f1-9020-e6871ad9f0ed",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Passos seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito. Para experimentar rapidamente as API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
