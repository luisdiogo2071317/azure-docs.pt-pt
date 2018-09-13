---
title: Identificação de imagens
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Conceitos relacionados a marcação de imagens através de imagem digitalizada nos serviços cognitivos do Azure.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: b06265bbdd5ba642c5395823e98a6a76171baff4
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725535"
---
# <a name="tagging-images"></a>Identificação de imagens

Imagem digitalizada devolve etiquetas com base em mais de 2000 objetos reconhecíveis, seres vivos, paisagens e ações. Quando as etiquetas são ambíguas ou dados de conhecimento não comuns, a resposta de API fornece "sugestões" para clarificar o significado da etiqueta no contexto de uma configuração conhecida. As etiquetas não são organizadas como uma taxonomia e não existem hierarquias de herança existem. Uma coleção de etiquetas de conteúdos serve de alicerce para uma imagem 'description', apresentado como linguagem legível humana formatada em sentenças completas. Tenha em atenção que neste momento inglês é o único idioma suportado para a descrição da imagem.

Depois de carregar uma imagem ou especificar um URL de imagem, algoritmos de imagem digitalizada saída etiquetas com base em objetos, seres vivos e ações identificadas na imagem. Marcação não se limita ao assunto principal, por exemplo, uma pessoa em primeiro plano, mas também inclui o mobiliário (fechado ou equipamentos esportivos), de definição, ferramentas, plantas, animais, acessórios, gadgets etc.

## <a name="image-tagging-example"></a>Exemplo de marcação de imagem

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve quando as funcionalidades visual detetadas na imagem de exemplo de identificação.

![House_Yard](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [categorizar imagens](concept-categorizing-images.md) e [descrevendo imagens](concept-describing-images.md).