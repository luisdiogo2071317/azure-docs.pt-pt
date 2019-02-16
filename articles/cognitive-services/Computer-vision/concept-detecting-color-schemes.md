---
title: Detecção de esquemas de cores - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a detetar o esquema de cores em imagens usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6b25da9b2569b0185d41684c45a22a3eb3377511
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313080"
---
# <a name="detect-color-schemes-in-images"></a>Detetar os esquemas de cores em imagens

Imagem digitalizada analisa as cores numa imagem para fornecer três atributos diferentes: a cor de primeiro plano dominante, a cor dominante do fundo e o conjunto de cores dominantes para a imagem como um todo. Devolvido cores pertencem ao conjunto: preto, azul, brown, cinzento, verde, laranja, rosa, Roxo, vermelho, verde-cinza, branco e amarelo. 

Imagem digitalizada extrai também uma cor de destaque, que representa a cor mais vibrante na imagem, com base numa combinação de cores dominantes e saturação. A cor de destaque é retornada como um código de cor hexadecimais HTML. 

Imagem digitalizada também devolve um valor booleano que indica se uma imagem está em preto e branco.

## <a name="color-scheme-detection-examples"></a>Exemplos de deteção do esquema de cor

O exemplo seguinte ilustra a resposta JSON devolvida de imagem digitalizada quando detetar o esquema de cores da imagem de exemplo. Neste caso, a imagem de exemplo não é uma imagem simples, mas o primeiro plano dominante e cores de fundo são pretas e as cores dominantes para a imagem como um todo são preto e branco.

![Exterior/Montanha](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Exemplos de cor dominante

A tabela seguinte mostra o primeiro plano retornado, em segundo plano e cores de imagem para cada imagem de exemplo.

| Imagem | Cores dominantes |
|-------|-----------------|
|![Uma flor branca com um plano de fundo verde](./Images/flower.png)| Em primeiro plano: Preto<br/>Em segundo plano: Branco<br/>Cores: Preto, White, verde|
![Comboio em execução por meio de uma estação](./Images/train_station.png) | Em primeiro plano: Preto<br/>Em segundo plano: Preto<br/>Cores: Preto |

### <a name="accent-color-examples"></a>Exemplos de cor de destaque

 A tabela seguinte mostra a cor de destaque retornado, como um valor hexadecimal de cor do HTML, para cada imagem de exemplo.

| Imagem | Cor de destaque |
|-------|--------------|
|![Uma pessoa seguindo um uma pedra das regiões montanhosas em sunset](./Images/mountain_vista.png) | #BB6D10 |
|![Uma flor branca com um plano de fundo verde](./Images/flower.png) | #C6A205 |
|![Comboio em execução por meio de uma estação](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Exemplos de deteção de preto e branco

A tabela seguinte mostra a avaliação do imagem digitalizada preto e branca em imagens de exemplo.

| Imagem | Preto e branco? |
|-------|----------------|
|![Uma imagem simples de edifícios em Manhattan](./Images/bw_buildings.png) | true |
|![Uma casa azul e o front-yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [detetar tipos de imagem](concept-detecting-image-types.md).
