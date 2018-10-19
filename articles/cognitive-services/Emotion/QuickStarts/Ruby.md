---
title: 'Início Rápido: reconhecer emoções nos rostos numa imagem - API de Emoções, Ruby'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para o ajudar a começar a utilizar rapidamente a API de Emoções com o Ruby.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: bcab24334c1ee4e47061ce6ea28bd60039e17b3f
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239034"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Início Rápido: compilar uma aplicação para reconhecer emoções nos rostos duma imagem.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Este artigo disponibiliza informações e exemplos de código para ajudá-lo a começar a utilizar rapidamente o [método de Reconhecimento da API de Emoções](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com o Ruby para reconhecer emoções expressas por uma ou mais pessoas numa imagem.

## <a name="prerequisite"></a>Pré-requisito
* Obtenha a sua Chave de Subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-ruby-example-request"></a>Pedido de exemplo em Ruby de Reconhecer Emoções

Altere o URL REST para utilizar a localização onde obteve as chaves de subscrição, substitua o valor de “Ocp-Apim-Subscription-Key” pela sua chave de subscrição válida e adicione um URL a uma fotografia para a variável `body`.

```ruby
require 'net/http'

# NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
#   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
#   URL below with "westcentralus".
uri = URI('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize')
uri.query = URI.encode_www_form({
})

request = Net::HTTP::Post.new(uri.request_uri)
# Request headers
request['Content-Type'] = 'application/json'
# NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '13hc77781f7e4b19b5fcdd72a8df7156'
# Request body
request.body = "{\"url\":\"http://example.com/1.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts response.body

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
