---
title: Categorizar imagens - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Aprenda conceitos relacionados com a funcionalidade de categorização de imagem da API de visão do computador.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f182110d150583ee1c241c23a2e1924d9f3e3bd4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158048"
---
# <a name="image-categorization-with-computer-vision"></a>Categorização de imagem com imagem digitalizada

Para além das etiquetas e descrições, imagem digitalizada devolve as categorias com base em taxonomia definidas nas versões anteriores. Estas categorias são organizadas como uma taxonomia com hierarquias hereditárias principais/subordinadas. Todas as categorias estão em inglês. Eles podem ser usados isoladamente ou com a nossa nova marcação de modelos.

## <a name="the-86-category-concept"></a>O conceito de 86 categorias

Com base numa lista de 86 conceitos ilustrado no diagrama seguinte, uma imagem pode ser categorizada desde o amplo até específico. Para obter a taxonomia completa no formato de texto, veja [Taxonomia de 86 Categorias](category-taxonomy.md).

![listas agrupadas de todas as categorias a taxonomia da categoria](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Exemplos de categorização de imagem

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve quando categorizar a imagem de exemplo com base nos seus recursos do visual.

![Uma mulher no teto de um apartamento](./Images/woman_roof.png)

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
| ![Quatro pessoas colocadas em conjunto como uma família](./Images/family_photo.png) | people_group |
| ![Um lançamento será feito num campo grassy](./Images/cute_dog.png) | animal_dog |
| ![Uma pessoa seguindo um uma pedra das regiões montanhosas em sunset](./Images/mountain_vista.png) | outdoor_mountain |
| ![Uma pilha de funções de pão numa tabela](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [descrevendo imagens](concept-describing-images.md).
