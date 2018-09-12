---
title: Início rápido criar miniatura da API de Imagem Digitalizada com Node.js | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, irá gerar uma miniatura de uma imagem através da Imagem Digitalizada com Node.js nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: c7038a294cbb273e21e892956a0b9c9e5fbfc38a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772243"
---
# <a name="quickstart-generate-a-thumbnail---rest-nodejs"></a>Início Rápido: Gerar uma miniatura – REST, Node.js

Neste início rápido, irá gerar uma miniatura de uma imagem através da Imagem Digitalizada.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Pedido Obter Miniatura

Com o [método Obter Miniatura](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), pode gerar uma miniatura de uma imagem. Especifica a altura e a largura, que podem ser diferentes da proporção da imagem de entrada. A Imagem Digitalizada utiliza o recorte inteligente para identificar de forma inteligente a região de interesse e gerar coordenadas de recorte com base nessa região.

Para executar o exemplo, siga os passos seguintes:

1. Copie o código seguinte para um editor.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `uriBase` para a localização na qual obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere o valor `imageUrl` para a imagem que pretende analisar.
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
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
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
});
```

## <a name="get-thumbnail-response"></a>Resposta Obter Miniatura

Uma resposta de êxito contém o binário da imagem em miniatura. Se o pedido falhar, a resposta contém um código de erro e uma mensagem para ajudar a determinar o que correu mal.

## <a name="next-steps"></a>Passos seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito. Para experimentar rapidamente as API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
