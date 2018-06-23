---
title: Início rápido do emoções API JavaScript | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de emoções com JavaScript nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 2578b0212f9b4a6483402074d7c9eff73e51ca6b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352400"
---
# <a name="emotion-api-javascript-quick-start"></a>Início rápido do emoções API JavaScript

> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar o [emoções API reconhece o método](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com JavaScript para reconhecer os emotions expressados por um ou mais pessoas numa imagem.

## <a name="prerequisite"></a>Pré-requisito
* Obter a chave de subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/), ou se tiver uma subscrição do Azure crie um recurso de emoções API e obter a chave de subscrição e o ponto final não existe.

![Criar o recurso de API de emoções](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Reconhece o pedido de exemplo de JavaScript Emotions

Copie o seguinte e guardá-lo como um ficheiro `test.html`. O pedido de alteração `url` para utilizar a localização onde obteve as chaves de subscrição e substitua o valor de "Ocp-Apim-Subscription-Key" a sua chave de subscrição válido. Pode encontrá-las no portal do Azure nas secções descrição geral e chaves do seu recurso emoções API, respetivamente. 

![Ponto final de API](../Images/api-url.png)

![Chave de subscrição de API](../Images/keys.png)

e corpo do pedido de alteração para a localização de uma imagem que pretende utilizar. Para executar o exemplo, arrastar e largar o ficheiro no seu browser.

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

## <a name="recognize-emotions-sample-response"></a>Reconhecer a resposta de amostra de Emotions
Uma chamada com êxito devolve uma matriz de entradas de letra e os respetivos pontuações de emoções associados, ordenadas pelo tamanho do retângulo de rostos em ordem descendente. Uma resposta vazia indica que não existem faces foram detetados. Uma entrada de emoções contém os seguintes campos:
* faceRectangle - localização do retângulo de rosto na imagem.
* pontuações - pontuações de emoções para cada enfrentam reside na imagem. 

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
