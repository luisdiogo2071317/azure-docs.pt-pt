---
title: Início rápido de API JavaScript de emoções | Documentos da Microsoft
description: Exemplos de código e informações de GET para ajudá-lo a rapidamente começar a utilizar a API de emoções com o JavaScript nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: fb9cc2335582c4ec75ec45635e519346d65d7e08
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072097"
---
# <a name="emotion-api-javascript-quick-start"></a>Início rápido de API JavaScript de emoções

> [!IMPORTANT]
> Pré-visualização da API de vídeo termina a 30 de Outubro de 2017. Experimente a nova [pré-visualização de API do indexador de vídeo](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente informações dos vídeos e para melhorar as experiências de deteção de conteúdos, como resultados de pesquisa, através da deteção falada, rostos, carateres e emoções. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este artigo fornece informações e exemplos de código para ajudá-lo a rapidamente começar a utilizar o [reconhecer da API de emoções método](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com JavaScript para reconhecer emoções expressadas por uma ou mais pessoas numa imagem.

## <a name="prerequisite"></a>Pré-requisito
* Obtenha a chave de subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/), ou se tiver uma subscrição do Azure crie um recurso de API de emoções e aceder aí a sua chave de subscrição e o ponto final.

![Criar o recurso de API de emoções](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Reconhecer emoções exemplo JavaScript a pedido

Copie o seguinte e guarde-o para um ficheiro como `test.html`. O pedido de alteração `url` para utilizar a localização onde obteve as chaves de subscrição e substitua o valor de "Ocp-Apim-Subscription-Key" com a sua chave de subscrição válido. Estes podem ser encontrados no portal do Azure nas secções descrição geral e chaves do seu recurso da API de emoções, respectivamente. 

![Ponto final de API](../Images/api-url.png)

![Chave de subscrição de API](../Images/keys.png)

Altere o corpo do pedido para a localização de uma imagem que pretende utilizar. Para executar o exemplo, arraste e largue o ficheiro no seu browser.

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<h2>Face Rectangle</h2>
<ul id="faceRectangle">
<!-- Will populate list with response content -->
</ul>

<h2>Emotions</h2>
<ul id="scores">
<!-- Will populate list with response content -->
</ul>

<body>

<script type="text/javascript">
    $(function() {
        // No query string parameters for this API call.
        var params = { };
      
        $.ajax({
            // NOTE: You must use the same location in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URL below with "westcentralus".
            url: "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers, also supports "application/octet-stream"
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","<your subscription key>");
            },
            type: "POST",
            // Request body
            data: '{"url": "<your image url>"}',
        }).done(function(data) {
            // Get face rectangle dimensions
            var faceRectangle = data[0].faceRectangle;
            var faceRectangleList = $('#faceRectangle');

            // Append to DOM
            for (var prop in faceRectangle) {
                faceRectangleList.append("<li> " + prop + ": " + faceRectangle[prop] + "</li>");
            }
            
            // Get emotion confidence scores
            var scores = data[0].scores;
            var scoresList = $('#scores');

            // Append to DOM
            for(var prop in scores) {
                scoresList.append("<li> " + prop + ": " + scores[prop] + "</li>")
            }
        }).fail(function(err) {
            alert("Error: " + JSON.stringify(err));
        });
    });
</script>
</body>
</html>
```

## <a name="recognize-emotions-sample-response"></a>Reconhecer emoções resposta de exemplo
Uma chamada bem-sucedida devolve uma matriz de entradas de rostos e suas pontuações de emoções associado, classificadas por tamanho do retângulo de rostos por ordem descendente. Uma resposta vazia indica que nenhuma rostos foram detetados. Uma entrada de emoções contém os seguintes campos:
* faceRectangle - localização do retângulo de rosto na imagem.
* pontuação - pontuações de emoções para cada rosto na imagem. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
