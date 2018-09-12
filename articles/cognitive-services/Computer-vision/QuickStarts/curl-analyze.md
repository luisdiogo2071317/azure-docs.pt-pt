---
title: Início rápido análise de uma imagem remota da API de Imagem Digitalizada com cURL | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai analisar uma imagem remota através da Imagem Digitalizada com cURL nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: c092432adbcba9cf7a131584131fd05991d1edef
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772278"
---
# <a name="quickstart-analyze-a-remote-image---rest-curl"></a>Início Rápido: Analisar uma imagem remota – REST, cURL

Neste início rápido, vai analisar uma imagem remota para extrair funcionalidades visuais através da Imagem Digitalizada. Para analisar uma imagem local, consulte [Analisar uma imagem local com o cURL](curl-disk.md).

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-a-remote-image"></a>Analisar uma imagem remota

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
1. Altere o URL de Pedido (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) para utilizar a localização onde obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere a imagem (`{\"url\":\"...`) para analisar.
1. Opcionalmente, altere o idioma de resposta (`language=en`).
1. Abra uma janela de comando num computador com o cURL instalado.
1. Cole o código na janela e execute o comando.

>[!NOTE]
>Tem de utilizar na sua chamada REST a mesma localização que utilizou para obter as chaves de subscrição. Por exemplo, se tiver obtido as chaves de subscrição a partir de westus, substitua "westcentralus" no URL abaixo por "westus".

## <a name="analyze-image-request"></a>Pedido de Análise de Imagem

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg\"}"
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
  "requestId": "b6e33879-abb2-43a0-a96e-02cb5ae0b795",
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
