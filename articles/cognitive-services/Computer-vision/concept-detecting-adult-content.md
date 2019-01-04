---
title: Descrever o conteúdo para adultos, imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a detecção de conteúdos para adultos em imagens usando a APi de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: e0cca054acb7d3d649105ecd45748a60a2aa9bbb
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582172"
---
# <a name="detecting-adult-and-racy-content"></a>Detetar conteúdos adultos e indecorosos

Entre as várias categorias visuais encontra-se o grupo para adultos e de profanidades, que permite a deteção de materiais para adultos e restringe a exibição de imagens com conteúdos sexuais. O filtro para a deteção de conteúdos para adultos ou profano pode ser definido numa escala móvel para se adaptar às preferências do utilizador.

## <a name="defining-adult-and-racy-content"></a>Definir o conteúdo para adultos

Entre as várias funcionalidades visual abrangidas pela [método de imagem de analisar](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), o recurso visual para adultos permite a deteção de imagens para adultos. Imagens "Para adultos" são definidas como aqueles que são pornográfico por natureza e, muitas vezes, retratar nudez e atos sexual. Imagens "Ousadas" são definidas como imagens que são sexualmente suggestive por natureza e geralmente contêm menos conteúdo sexualmente explícito que imagens etiquetadas como "Adulto." O tipo de recurso visual para adultos normalmente é usado para restringir a exibição de imagens que contenham sexualmente suggestive e conteúdo sexual explicitamente.

## <a name="identifying-adult-and-racy-content"></a>Identificação de conteúdo para adultos

O método de imagem de analisar retorna duas propriedades, `isAdultContent` e `isRacyContent`, na resposta JSON do método para indicar, respectivamente, o conteúdo para adultos. Ambas as propriedades devolvem um valor booleano, VERDADEIRO ou FALSO. O método também retorna duas propriedades, `adultScore` e `racyScore`, que representam, respectivamente, as pontuações de confiança para identificar conteúdos para adultos. Um filtro de confiança para a deteção de conteúdos para adultos pode ser definido numa escala móvel para acomodar sua preferência, com base no seu cenário específico.

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [detetar conteúdo específicas do domínio](concept-detecting-domain-content.md) e [detetar rostos](concept-detecting-faces.md).