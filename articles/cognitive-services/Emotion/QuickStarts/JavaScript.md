---
title: 'Início Rápido: reconhecer emoções nos rostos numa imagem - API de Emoções, JavaScript'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para o ajudar a começar a utilizar rapidamente a API de Emoções com o JavaScript.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: eeaf2ea080d8c0b604b9831532028e31b8306169
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239494"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Início Rápido: compilar uma aplicação para reconhecer emoções nos rostos duma imagem.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Este artigo disponibiliza informações e exemplos de código para ajudá-lo a começar a utilizar rapidamente o [método de Reconhecimento da API de Emoções](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com o JavaScript para reconhecer as emoções expressas por uma ou mais pessoas numa imagem.

## <a name="prerequisite"></a>Pré-requisito
* Obtenha a Chave de Subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/) ou, se tiver uma Subscrição do Azure, crie um recurso da API de Emoções e aceda aí à Chave e ao Ponto Final da Subscrição.

![Criar o Recurso da API de Emoções](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Pedido de exemplo em JavaScript para Reconhecer Emoções

Copie o seguinte e guarde-o num ficheiro como `test.html`. Altere o `url` do pedido para que utilize a localização onde obteve as chaves de subscrição e substitua o valor de “Ocp-Apim-Subscription-Key” pela sua chave de subscrição válida. Pode encontrá-las no portal do Azure nas secções Descrição Geral e Chaves do seu recurso da API de Emoções, respectivamente.

![Ponto Final de API](../Images/api-url.png)

![Chave de Subscrição de API](../Images/keys.png)

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

## <a name="recognize-emotions-sample-response"></a>Resposta de Exemplo para Reconhecer Emoções
Uma chamada bem-sucedida devolve uma matriz de entradas de rostos e as pontuações das respetivas emoções associadas, ordenadas pelo tamanho dos retângulos de rostos por ordem descendente. Uma resposta vazia indica que não foram detetados rostos. Uma entrada de emoção contém os seguintes campos:
* faceRectangle - localização do retângulo de rosto na imagem.
* pontuações - pontuações das emoções de cada rosto na imagem.

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
