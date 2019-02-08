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
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0793f572e043248af409e65cca4fd854f1371900
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880891"
---
# <a name="detect-color-schemes-in-images"></a>Detetar os esquemas de cores em imagens

Imagem digitalizada extrai as cores de uma imagem. As cores, em seguida, são analisadas em três contextos diferentes: a cor de primeiro plano dominante, a cor de fundo dominante e as cores dominantes para a imagem como um todo. Eles são agrupados em 12 cores de destaque dominantes. Estas cores de destaque são o preto, azul, castanho, cinzento, verde, laranja, rosa, roxo, vermelho, verde-cinza, branco e amarelo. Imagem digitalizada analisa as cores extraídas a partir de uma imagem para retornar uma que representa a cor mais vibrante para a imagem para visualizadores, através de uma combinação de cores dominantes e saturação de cor de destaque. Consoante as cores numa imagem, poderão ser devolvidos o preto e branco simples ou cores de destaque em códigos de cor hexadecimais. Imagem digitalizada também devolve um valor booleano que indica se uma imagem é preta e branco.

## <a name="color-scheme-detection-examples"></a>Exemplos de deteção do esquema de cor

O exemplo seguinte ilustra a resposta JSON devolvida de imagem digitalizada quando detetar o esquema de cores da imagem de exemplo. Neste caso, a imagem de exemplo não é uma imagem de preta e branca, mas as cores de primeiro plano e fundo dominantes são pretas e as cores dominantes para a imagem como um todo são preto e branco.

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

A tabela seguinte descreve o primeiro plano dominante, em segundo plano e cores de imagem para cada imagem de exemplo devolvido pela visão do computador.

| Imagem | Cores dominantes |
|-------|-----------------|
|![Uma flor branca com um plano de fundo verde](./Images/flower.png)| Em primeiro plano: Preto<br/>Em segundo plano: Branco<br/>Cores: Preto, White, verde|
![Comboio em execução por meio de uma estação](./Images/train_station.png) | Em primeiro plano: Preto<br/>Em segundo plano: Preto<br/>Cores: Preto |

### <a name="accent-color-examples"></a>Exemplos de cor de destaque

 A tabela seguinte descreve a cor de destaque, como um valor hexadecimal de cor do HTML, para cada imagem de exemplo devolvido pela visão do computador.

| Imagem | Cor de destaque |
|-------|--------------|
|![Uma pessoa seguindo um uma pedra das regiões montanhosas em sunset](./Images/mountain_vista.png) | #BB6D10 |
|![Uma flor branca com um plano de fundo verde](./Images/flower.png) | #C6A205 |
|![Comboio em execução por meio de uma estação](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Exemplos de deteção de preto e branco

A tabela seguinte indica se cada imagem de exemplo está a preta e branco, conforme devolvido pela visão do computador.

| Imagem | Preto e branco? |
|-------|----------------|
|![Uma imagem simples de edifícios em Manhattan](./Images/bw_buildings.png) | true |
|![Uma casa azul e o front-yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [detetar tipos de imagem](concept-detecting-image-types.md).
