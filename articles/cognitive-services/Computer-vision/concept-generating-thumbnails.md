---
title: Gerar miniaturas - imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a geração de miniaturas de imagens usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 8881a0c5e809dc75d1b9f4b6f6bd3af7142333cb
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884635"
---
# <a name="generating-smart-cropped-thumbnails-with-computer-vision"></a>Gerar miniaturas de smart-recortadas com imagem digitalizada

Uma miniatura é uma representação de tamanho reduzido de uma imagem. Elas são usadas para representar as imagens e outros dados de uma forma compatível com esquema mais econômica. A API de imagem digitalizada utiliza o corte inteligente, juntamente com a redimensionar a imagem, para criar miniaturas intuitivas para uma determinada imagem.

O algoritmo de geração de miniaturas de imagem digitalizada funciona da seguinte forma:
1. Remover elementos distração da imagem e identificar os _área de interesse_&mdash;a área da imagem em que o objeto principal (s) aparece.
1. Recortar a imagem com base no identificados _área de interesse_.
1. Altere a proporção de acordo com as dimensões de miniatura de destino.

## <a name="area-of-interest"></a>Área de interesse

Ao carregar uma imagem, a API de imagem digitalizada analisa para determinar a *área de interesse*. Em seguida, pode utilizar esta região para determinar como recortar a imagem. A operação de recorte, no entanto, sempre corresponderá a proporção pretendida se for especificado.

Também pode obter as coordenadas de caixa delimitadora não processados disso mesmos *área de interesse* chamando o **areaOfInterest** API em vez disso. Em seguida, pode utilizar estas informações para modificar a imagem original, no entanto, o que desejar.

## <a name="examples"></a>Exemplos

A miniatura gerada pode variar bastante, dependendo do que especificou para a altura, largura e corte inteligente, conforme mostrado na imagem seguinte.

![Miniaturas](./Images/thumbnail-demo.png)

A tabela seguinte ilustra as miniaturas típicas geradas de imagem digitalizada para as imagens de exemplo. As miniaturas geradas para uma altura de destino especificado e a largura de 50 pixels, com o corte inteligente ativado.

| Imagem | Miniatura |
|-------|-----------|
|![Uma pessoa seguindo um uma pedra das regiões montanhosas em sunset](./Images/mountain_vista.png) | ![Miniatura de montanha de equipamentos esportivos](./Images/mountain_vista_thumbnail.png) |
|![Uma flor branca com um plano de fundo verde](./Images/flower.png) | ![Miniatura de analisar flor de imagem digitalizada](./Images/flower_thumbnail.png) |
|![Uma mulher no teto de um apartamento](./Images/woman_roof.png) | ![Miniatura de teto de mulher](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [marcação de imagens](concept-tagging-images.md) e [categorizar imagens](concept-categorizing-images.md).
