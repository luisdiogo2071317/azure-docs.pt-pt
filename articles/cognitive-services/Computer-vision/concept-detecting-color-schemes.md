---
title: Detecção de esquemas de cores - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a detetar o esquema de cores em imagens usando a API de imagem digitalizada.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: c01433ed3ef1279bf3db9bdcf76e57aad2208895
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983602"
---
# <a name="detecting-color-schemes"></a>Detetar esquemas de cores

Imagem digitalizada extrai as cores de uma imagem. As cores, em seguida, são analisadas em três contextos diferentes: a cor de primeiro plano dominante, a cor de fundo dominante e as cores dominantes para a imagem como um todo. Eles são agrupados em 12 cores de destaque dominantes. Essas cores de destaque são pretas, azul, brown, cinzento, verde, laranja, rosa, Roxo, vermelho, verde-cinza, branco e amarelo. Imagem digitalizada analisa as cores extraídas a partir de uma imagem para retornar uma que representa a cor mais vibrante para a imagem para visualizadores, através de uma combinação de cores dominantes e saturação de cor de destaque. Consoante as cores numa imagem, podem ser devolvidas simple preto e branco ou cores de destaque em códigos de cor hexadecimal. Imagem digitalizada também devolve um valor booleano que indica se uma imagem é preta e branco.

## <a name="color-scheme-detection-examples"></a>Exemplos de deteção do esquema de cor

O exemplo seguinte ilustra a resposta JSON devolvida de imagem digitalizada quando detetar o esquema de cores da imagem de exemplo. Neste caso, a imagem de exemplo não é uma imagem de preta e branca, mas as cores de primeiro plano e fundo dominantes são pretas e as cores dominantes para a imagem como um todo são preto e branco.

![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png)

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
|![Visão analisar flor](./Images/flower.png)| Em primeiro plano: preto<br/>Em segundo plano: branco<br/>Cores: Preto, White, verde|
![Visão analisar Train estação](./Images/train_station.png) | Em primeiro plano: preto<br/>Em segundo plano: preto<br/>Cores: preto |

### <a name="accent-color-examples"></a>Exemplos de cor de destaque

 A tabela seguinte descreve a cor de destaque, como um valor hexadecimal de cor do HTML, para cada imagem de exemplo devolvido pela visão do computador.

| Imagem | Cor de destaque |
|-------|--------------|
|![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png) | N. º BB6D10 |
|![Visão analisar flor](./Images/flower.png) | N. º C6A205 |
|![Visão analisar Train estação](./Images/train_station.png) | N. º 474A84 |

### <a name="black--white-detection-examples"></a>Exemplos de deteção de preto e branco

A tabela seguinte indica se cada imagem de exemplo está a preta e branco, conforme devolvido pela visão do computador.

| Imagem | Preto e branco? |
|-------|----------------|
|![Construção de analisar de imagem digitalizada](./Images/bw_buildings.png) | true |
|![Visão analisar Yard de casa](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [detetar tipos de imagem](concept-detecting-image-types.md).