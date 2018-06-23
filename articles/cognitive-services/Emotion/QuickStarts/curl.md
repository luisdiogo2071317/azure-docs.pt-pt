---
title: Início rápido do cURL emoções API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de emoções com cURL nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: d36a191d3589702d676e694715f34ea1c924e2d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352387"
---
# <a name="emotion-api-curl-quick-start"></a>CURL emoções API início rápido

> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar o [emoções API reconhece o método](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com cURL para reconhecer os emotions expressados por um ou mais pessoas numa imagem. 

## <a name="prerequisite"></a>Pré-requisito
* Obter a chave de subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-curl-example-request"></a>Reconhecer que emotions cURL exemplo de pedido

> [!NOTE]
> Tem de utilizar a mesma localização a chamada REST como é utilizado para obter as chaves de subscrição. Por exemplo, se tiver adquirido as chaves de subscrição de westcentralus, substitua "westus" no URL abaixo "westcentralus".

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}" 
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

