---
title: Detetar rostos - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a deteção de rostos com a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: bf358d1e8f60f989ced8db966bbf0a5179fab25b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342318"
---
# <a name="detecting-faces"></a>Detetar rostos

Imagem digitalizada Deteta rostos humanos em imagens e gera a idade, sexo e retângulo para cada rosto detetado. A Imagem Digitalizada fornece um subconjunto das funcionalidades que podem ser encontradas na [Face](/azure/cognitive-services/face/) e pode utilizar o serviço Face para uma análise mais detalhada, como identificação facial e deteção de poses.  

## <a name="face-detection-examples"></a>Exemplos de deteção de rostos

O primeiro exemplo demonstra a resposta JSON devolvida pela visão do computador para uma imagem que contém um único face humana.

![Visão analisar mulher teto Face](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

O segundo exemplo demonstra a resposta JSON devolvida para uma imagem que contém vários rostos humanos.

![Visão analisar Face de fotos de família](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [detetar conteúdo específicas do domínio](concept-detecting-domain-content.md).