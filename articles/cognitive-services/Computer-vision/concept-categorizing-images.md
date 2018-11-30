---
title: Categorizar imagens - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a categorizar imagens usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 7062d98d40c15f4e9e873038fc12fc1b104c996d
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333484"
---
# <a name="categorizing-images"></a>Categorizar imagens

Para além das etiquetas e descrições, imagem digitalizada devolve as categorias com base em taxonomia definidas nas versões anteriores. Estas categorias são organizadas como uma taxonomia com hierarquias hereditárias principais/subordinadas. Todas as categorias estão em inglês. Eles podem ser usados isoladamente ou com a nossa nova marcação de modelos.

## <a name="the-86-category-concept"></a>O conceito de 86 categorias

Com base numa lista de 86 conceitos ilustrado no diagrama seguinte, uma imagem pode ser categorizada desde o amplo até específico. Para obter a taxonomia completa no formato de texto, veja [Taxonomia de 86 Categorias](category-taxonomy.md).

![listas agrupadas de todas as categorias a taxonomia da categoria](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Exemplos de categorização de imagem

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve quando categorizar a imagem de exemplo com base nos seus recursos do visual.

![Mulher num Terraço](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

A tabela a seguir ilustra um conjunto típico de imagem e a categoria devolvido pela visão do computador para cada imagem.

| Imagem | Categoria |
|-------|----------|
| ![Fotografia de Família](./Images/family_photo.png) | people_group |
| ![Cão fofinho](./Images/cute_dog.png) | animal_dog |
| ![Exterior/Montanha](./Images/mountain_vista.png) | outdoor_mountain |
| ![Análise de imagem de pão](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [descrevendo imagens](concept-describing-images.md).