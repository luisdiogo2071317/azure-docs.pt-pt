---
title: Detetar conteúdos específicos de domínio - de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Saiba como especificar um domínio de categorização de imagem para devolver informações mais detalhadas sobre uma imagem.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 50942634bd50974453c242d1980db9fc589bd47e
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53579214"
---
# <a name="detecting-domain-specific-content"></a>Detetar conteúdo específico de um domínio

Além disso categorização a etiquetagem e de nível superior, imagem digitalizada também suporta informações especializadas (ou específicas de domínio). As informações especializadas podem ser implementadas como um método autónomo ou com a categorização geral. Funcionam como um meio para aperfeiçoar a taxonomia de 86 categorias ao adicionar modelos específicos de um domínio.

Existem duas opções para utilizar os modelos específicos de um domínio:

* Análise de âmbito  
  Analise apenas um modelo escolhido ao invocar uma chamada de POST de HTTP. Se souber qual modelo de que pretende utilizar, especifique o nome do modelo. Apenas obter informações relevantes para esse modelo. Por exemplo, pode utilizar esta opção apenas para o reconhecimento de celebridades. A resposta contém uma lista de potenciais correspondências de celebridades, juntamente com as respetivas pontuações de confiança.
* Recursos de análise  
  Analise para obter detalhes adicionais relacionados com as categorias da taxonomia de 86 categorias. Esta opção está disponível para utilização em aplicações nas quais os utilizadores querem obter uma análise de imagem genérica, além dos detalhes de um ou mais modelos específicos de um domínio. Ao invocar este método, é chamado primeiro o classificador da taxonomia de 86 categorias. Se nenhuma das categorias fazer a correspondência dos modelos de conhecidos ou correspondentes, uma segunda passagem de invocações de classificador segue. Por exemplo, se o `details` parâmetro da chamada de POST de HTTP ou está definido para "todos" ou inclua "celebridades", o método chama o classificador de celebridade depois que o classificador de categoria 86 é chamado. Se a imagem é classificada como `people_` ou numa subcategoria dessa categoria, em seguida, o classificador de celebridade é chamada.

## <a name="listing-domain-specific-models"></a>Modelos de domínios específicos de lista

Pode listar os modelos de domínios específicos suportados de imagem digitalizada. Atualmente, o de imagem digitalizada suporta os seguintes modelos de domínios específicos para a detecção de conteúdo específicos de domínio:

| Nome | Descrição |
|------|-------------|
| celebridades | Reconhecimento de celebridades, suportado para imagens classificada no `people_` categoria |
| Pontos de referência | Reconhecimento de marcos, suportado para imagens classificada no `outdoor_` ou `building_` categorias |

### <a name="domain-model-list-example"></a>Exemplo de lista do modelo de domínio

A resposta JSON seguinte lista os modelos de domínios específicos suportados de imagem digitalizada.

```json
{
    "models": [
        {
            "name": "celebrities",
            "categories": ["people_", "人_", "pessoas_", "gente_"]
        },
        {
            "name": "landmarks",
            "categories": ["outdoor_", "户外_", "屋外_", "aoarlivre_", "alairelibre_",
                "building_", "建筑_", "建物_", "edifício_"]
        }
    ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [categorizar imagens](concept-categorizing-images.md).