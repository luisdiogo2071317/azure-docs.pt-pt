---
title: Detetar tipos de imagem - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados com a funcionalidade de deteção do tipo de imagem da API de visão do computador.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: da7cce0cd69823f254cd6b666dc468f155b9e097
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156280"
---
# <a name="detecting-image-types-with-computer-vision"></a>Detetar tipos de imagem com imagem digitalizada

Imagem digitalizada pode analisar o tipo de conteúdo de imagens ao indicar se uma imagem é clip-art, a probabilidade numa escala ou um desenho de linha de classificação.

## <a name="detecting-clip-art"></a>Clip-art de detecção

Imagem digitalizada analisa uma imagem e classifica a probabilidade da imagem a ser clip-art numa escala de 0 a 3, conforme descrito na tabela seguinte.

| Valor | Significado |
|-------|---------|
| 0 | Não é ClipArt |
| 1 | Ambígua |
| 2 | Normal – clip-arts |
| 3 | Bom-clip-arts |

### <a name="clip-art-detection-examples"></a>Exemplos de deteção de arte do Clip

As respostas JSON seguintes ilustra o que o de imagem digitalizada devolve quando a probabilidade das imagens de exemplo que está a ser clip-art de classificação.

![Uma imagem de arte do clip de um setor de queijo](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Uma casa azul e o front-yard](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Detetar desenhos de linha

Imagem digitalizada analisa uma imagem e retorna um valor booleano que indica se a imagem é um desenho de linha.

### <a name="line-drawing-detection-examples"></a>Exemplos de deteção de desenho de linha

As respostas JSON seguintes ilustra o que o de imagem digitalizada devolve quando que indica se as imagens de exemplo são desenhos de linha.

![Uma imagem de desenho de linha de uma lion](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Uma flor branca com um plano de fundo verde](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [categorizar imagens](concept-categorizing-images.md).
