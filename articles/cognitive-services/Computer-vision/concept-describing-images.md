---
title: Descrevendo imagens - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados com a funcionalidade de descrição de imagem da API de visão do computador.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7919a84ffe948c9b6a8f68fc1372f1976c09bc79
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864286"
---
# <a name="describe-images-with-human-readable-language"></a>Descrever as imagens com linguagem legível por humanos

Algoritmos do imagem digitalizada analisam o conteúdo numa imagem. Esta análise é a base de uma "descrição" apresentada como um idioma legível por humanos em frases completas. A descrição resume o que é encontrado na imagem. Algoritmos do imagem digitalizada geram vários descrições com base nos recursos visual identificados na imagem. Descrição de cada é avaliada e uma pontuação de confiança gerado. Em seguida, é devolvida uma lista ordenada da pontuação de confiança mais alta para a mais baixa.

## <a name="image-description-example"></a>Exemplo de descrição de imagem

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve ao descrever a imagem de exemplo com base nos seus recursos do visual.

![Uma imagem simples de edifícios em Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [categorizar imagens](concept-categorizing-images.md).
