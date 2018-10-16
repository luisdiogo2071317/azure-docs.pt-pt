---
title: Gerar miniaturas - imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a geração de miniaturas de imagens usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 9cb82b40d1fbec513b0219f26d1959fbd7f64570
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343967"
---
# <a name="generating-thumbnails"></a>Gerar miniaturas

Uma miniatura é uma representação pequenas de uma imagem em tamanho normal. Variados dispositivos como telemóveis, tablets e PCs criam a necessidade de layouts de experiência (UX) de utilizador diferente e tamanhos de miniatura. Utilizar o corte inteligente, esse recurso de imagem digitalizada ajuda a resolver o problema.

Depois de carregar uma imagem, de imagem digitalizada gera uma miniatura de alta qualidade e, em seguida, analisa os objetos na imagem para identificar os *região de interesse* (ROI). Opcionalmente, ele pode recortar a imagem de acordo com os requisitos do ROI. A miniatura gerada pode ser apresentada com uma taxa de proporção diferente da proporção da imagem original, para acomodar suas necessidades.

O algoritmo de miniatura funciona da seguinte forma:

1. Remove a distração de elementos da imagem e identifica o objeto principal, a região de interesse.
2. Recorta a imagem com base na região de interesse identificado.
3. Altera a taxa de proporção de acordo com as dimensões de miniatura de destino.

A miniatura gerada pode variar bastante, dependendo do que especificou para a altura, largura e corte inteligente, conforme mostrado na imagem seguinte.

![Miniaturas](./Images/thumbnail-demo.png)

## <a name="thumbnail-generation-examples"></a>Exemplos de geração de miniaturas

A tabela seguinte ilustra as miniaturas típicas geradas de imagem digitalizada para as imagens de exemplo. As miniaturas geradas para uma altura de destino especificado e a largura de 50 pixels, com o corte inteligente ativado.

| Imagem | Miniatura |
|-------|-----------|
|![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png) | ![Miniatura de montanha de equipamentos esportivos](./Images/mountain_vista_thumbnail.png) |
|![Visão analisar flor](./Images/flower.png) | ![Miniatura de analisar flor de imagem digitalizada](./Images/flower_thumbnail.png) |
|![Teto de mulher](./Images/woman_roof.png) | ![Miniatura de teto de mulher](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [categorizar imagens](concept-categorizing-images.md).