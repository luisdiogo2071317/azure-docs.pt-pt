---
title: 'Início rápido: Reconhecer emoções no rostos numa imagem - API de emoções, cURL'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para o ajudar a começar a utilizar rapidamente a API de Emoções com cURL.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 8ecaf5e6de958a0f0af0ccccf9c712a227b504b9
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857737"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Início rápido: Crie uma aplicação para reconhecer emoções no rostos numa imagem.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/).

Este artigo disponibiliza informações e exemplos de código para ajudá-lo a começar a utilizar rapidamente o [método de Reconhecimento da API de Emoções](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com cURL para reconhecer emoções expressas por uma ou mais pessoas numa imagem.

## <a name="prerequisite"></a>Pré-requisito
* Obtenha a sua Chave de Subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-curl-example-request"></a>Pedido de Exemplo em cURL de Reconhecimento de Emoções

> [!NOTE]
> Tem de utilizar na sua chamada REST a mesma localização que utilizou para obter as chaves de subscrição. Por exemplo, se tiver obtido as chaves de subscrição na região westcentralus, substitua “westus” no URL abaixo por “westcentralus”.

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

## <a name="recognize-emotions-sample-response"></a>Resposta de Exemplo para Reconhecimento de Emoções
Uma chamada bem-sucedida devolve uma matriz de entradas de rostos e as pontuações das emoções associadas, ordenadas pelo tamanho dos retângulos de rostos por ordem descendente. Uma resposta vazia indica que não foram detetados rostos. Uma entrada de emoção contém os seguintes campos:
* faceRectangle – Localização do retângulo de rosto na imagem.
* pontuações – Pontuações das emoções de cada rosto na imagem.

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
