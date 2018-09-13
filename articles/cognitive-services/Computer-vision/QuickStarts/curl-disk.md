---
title: Início rápido de análise de uma imagem local da API de Imagem Digitalizada com o cURL | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai analisar uma imagem local através da Imagem Digitalizada com o cURL nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 93ca3ea6eee3743dfd0c25c9514375ae63a531ee
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772411"
---
# <a name="quickstart-analyze-a-local-image---rest-curl"></a>Início Rápido: Analisar uma imagem local – REST, cURL

Neste início rápido, vai analisar uma imagem local para extrair capacidades visuais através da Imagem Digitalizada. Para analisar uma imagem remota, veja [Analyze a remote image with cURL](curl-analyze.md) (Analisar uma imagem remota com o cURL).

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, precisa de uma chave de subscrição; veja [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Obter Chaves de Subscrição).

## <a name="analyze-a-local-image"></a>Analisar uma imagem local

Este exemplo é semelhante a [Analyze a remote image with cURL](curl-analyze.md) (Analisar uma imagem remota com o cURL), exceto o facto de a imagem a analisar ser lida localmente a partir do disco. São necessárias três alterações:

- Altere o Content-Type para `"Content-Type: application/octet-stream"`.
- Altere o parâmetro `-d` para `--data-binary`.
- Especifique a imagem a analisar com a seguinte sintaxe: `@C:/Pictures/ImageToAnalyze.jpg`.

Para executar o exemplo, siga os seguintes passos:

1. Copie o código seguinte para um editor.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o URL do Pedido (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) para utilizar a localização onde obteve as suas chaves de subscrição, se assim for preciso.
1. Substitua `<Image To Analyze>` pela imagem local que pretende analisar.
1. Opcionalmente, altere o idioma de resposta (`language=en`).
1. Abra uma janela de comando num computador com o cURL instalado.
1. Cole o código na janela e execute o comando.

>[!NOTE]
>Tem de utilizar na sua chamada REST a mesma localização que utilizou para obter as chaves de subscrição. Por exemplo, se tiver obtido as chaves de subscrição a partir de westus, substitua "westcentralus" no URL abaixo por "westus".

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -H "Content-Type: application/octet-stream" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" --data-binary <Image To Analyze>
```

## <a name="analyze-image-response"></a>Analisar a resposta da Imagem

É devolvida uma resposta com êxito em JSON, por exemplo:

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

Explore as APIs de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e paisagens, criar uma miniatura e extrair texto manuscrito e impresso. Para experimentar rapidamente as APIs de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explorar APIs de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
