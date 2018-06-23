---
title: Início rápido do API PHP emoções | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de emoções com o PHP nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: bb3f3bc1421e7c65a1fa6095f22f499415c50890
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352393"
---
# <a name="emotion-api-php-quick-start"></a>Início rápido do emoções PHP de API

> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar o PHP e o [emoções API reconhece o método](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) para reconhecer os emotions expressados por um ou mais pessoas numa imagem. 

## <a name="prerequisite"></a>Pré-requisito
* Obter a chave de subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-php-example-request"></a>Reconhece o pedido de exemplo do Emotions PHP

Altere o URL de REST para utilizar a localização onde obteve as chaves de subscrição, altere o corpo para um URL de uma imagem que pretende testar e substitua o valor de "Ocp-Apim-Subscription-Key" a sua chave de subscrição válido.

```php
<?php
// This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

// NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
//   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
//   URL below with "westcentralus".
$request = new Http_Request2('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',

    // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '13hc77781f7e4b19b5fcdd72a8df7156',
);

$request->setHeader($headers);

$parameters = array(
    // Request parameters
);

$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body
$request->setBody('{"url": "http://www.example.com/images/image.jpg"}');

try
{
    $response = $request->send();
    echo $response->getBody();
}
catch (HttpException $ex)
{
    echo $ex;
}

?>
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

