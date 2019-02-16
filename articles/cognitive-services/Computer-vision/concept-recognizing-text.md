---
title: Reconhecer texto impresso, manuscritos - de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados com o reconhecimento de manuscrito e impresso texto em imagens usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 48ce15a11c3e3282535420f3e1bb1915276d70f5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313182"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Reconhecer impressos e texto manuscrito

Imagem digitalizada pode detetar e extrair texto impresso ou manuscrito de imagens de vários objetos com diferentes superfícies e fundos, como recibos, cartazes, cartões de visita, letras e quadros de comunicações.

O recurso de reconhecimento de texto é muito semelhante à [reconhecimento ótico de carateres (OCR)](concept-extracting-text-ocr.md), mas, ao contrário de OCR é executado de forma assíncrona e utiliza modelos de reconhecimento de atualizado.

> [!NOTE]
> Nota: esta tecnologia está atualmente em pré-visualização e só está disponível para textos em inglês.

## <a name="text-recognition-requirements"></a>Requisitos de reconhecimento de texto

Imagem digitalizada pode reconhecer texto impresso e manuscritos em imagens que cumprem os requisitos seguintes:

- A imagem deve ser apresentada no formato JPEG, PNG ou BMP
- O tamanho do ficheiro da imagem tem de ser inferior a 4 megabytes (MB)
- As dimensões da imagem tem de estar entre 50 x 50 e 4200 x 4200 pixels

## <a name="next-steps"></a>Passos Seguintes

Consulte a [reconhecer a documentação de referência de texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para saber mais.