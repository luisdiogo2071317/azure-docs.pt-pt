---
title: Deteção de objetos - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a deteção de objetos com a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 3468f9341e0a8406733877a05798e427dd454fff
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167568"
---
# <a name="object-detection"></a>Deteção de objetos

Deteção de objetos é semelhante à [marcação](concept-tagging-images.md), mas a API devolve as coordenadas da caixa delimitadora (em pixéis) para cada objeto encontrado. Por exemplo, se uma imagem contiver um cachorro, gato e person, a operação de deteção irá listar esses objetos, juntamente com suas coordenadas na imagem. Pode utilizar esta funcionalidade para processar as relações entre os objetos numa imagem. Ele também permite determinar se existem várias instâncias da mesma marca numa imagem.

A API de detectar se aplica etiquetas com base nos objetos ou vivos identificados na imagem. Observe que, neste momento, não existe nenhuma relação formal entre taxonomia utilizado para etiquetagem e a taxonomia utilizados para deteção de objeto. Num nível conceitual, a API de detetar apenas localiza objetos e vivos, enquanto a API de marca também pode incluir contextuais termos como "fechado", que não podem ser localizados com delimitação de caixas.

## <a name="object-detection-example"></a>Exemplo de deteção de objeto

A resposta JSON seguinte ilustra o que o de imagem digitalizada devolve quando detetar objetos na imagem de exemplo.

![Uma mulher com um dispositivo Microsoft Surface numa cozinha](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [categorizar imagens](concept-categorizing-images.md) e [descrevendo imagens](concept-describing-images.md).
