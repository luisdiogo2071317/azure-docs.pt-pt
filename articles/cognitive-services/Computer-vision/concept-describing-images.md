---
title: Descrevendo imagens - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a descrever as imagens usando a API de imagem digitalizada.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: dc472a0a3f76dce8f3172689567e77c0b73ce8d9
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238753"
---
# <a name="describing-images"></a>Descrever imagens

Algoritmos do imagem digitalizada analisam o conteúdo numa imagem. Esta análise serve de alicerce para uma 'description' apresentada como uma linguagem legível em sentenças completas. A descrição resume o que for encontrado na imagem. Algoritmos do imagem digitalizada geram vários descrições com base nos recursos visual identificados na imagem. Descrição de cada é avaliada e uma pontuação de confiança gerado. Em seguida, é devolvida uma lista ordenada da pontuação de confiança mais alta para a mais baixa.

## <a name="image-description-example"></a>Exemplo de descrição de imagem

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve ao descrever a imagem de exemplo com base nos seus recursos do visual.

![B & edifícios W](./Images/bw_buildings.png)

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