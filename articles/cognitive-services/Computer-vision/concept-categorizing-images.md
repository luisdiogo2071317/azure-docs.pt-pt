---
title: Categorizar imagens - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a categorizar imagens usando a API de imagem digitalizada.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 345432cbc8a552ba7f6a4902cec72bcc1bf9feac
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983560"
---
# <a name="categorizing-images"></a>Categorizar imagens

Para além das etiquetas e descrições, imagem digitalizada devolve as categorias com base em taxonomia definidas nas versões anteriores. Estas categorias são organizadas como uma taxonomia com hierarquias principal/subordinado de hereditary. Todas as categorias estão em inglês. Eles podem ser usados isoladamente ou com a nossa nova marcação de modelos.

## <a name="the-86-category-concept"></a>O conceito de categoria 86

Com base numa lista de 86 conceitos ilustrado no diagrama seguinte, uma imagem pode ser categorizada desde o amplo até específico. Para a taxonomia no formato de texto completa, consulte [taxonomia da categoria](category-taxonomy.md).

![Analisar as categorias](./Images/analyze_categories.png)

## <a name="image-categorization-examples"></a>Exemplos de categorização de imagem

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve quando categorizar a imagem de exemplo com base nos seus recursos do visual.

![Teto de mulher](./Images/woman_roof.png)

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
| ![Fotos de família](./Images/family_photo.png) | people_group |
| ![Dog fofinhos](./Images/cute_dog.png) | animal_dog |
| ![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png) | outdoor_mountain |
| ![Visão analisar pão de comida](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [descrevendo imagens](concept-describing-images.md).