---
title: Deteção de domínios específicos conteúdo
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Conceitos relacionados a descrever de imagens através de imagem digitalizada nos serviços cognitivos do Azure.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 699698d8235d8fa53050a67491ee4f0e1c336d0e
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725519"
---
# <a name="detecting-domain-specific-content"></a>Deteção de domínios específicos conteúdo

Além disso categorização a etiquetagem e de nível superior, imagem digitalizada também suporta informações especializadas (ou específicas de domínio). Informações especializadas podem ser implementadas como método autónomo ou com a categorização de alto nível. Ele funciona como um meio para refinar ainda mais a taxonomia da categoria de 86 por meio da adição de modelos de domínios específicos.

Existem duas opções para utilizar os modelos de domínios específicos:

* Análise de âmbito  
  Analise apenas um modelo escolhido ao invocar uma chamada de POST de HTTP. Se souber qual modelo de que pretende utilizar, especifique o nome do modelo. Apenas obter informações relevantes para esse modelo. Por exemplo, pode utilizar esta opção apenas serve para reconhecimento de celebridade. A resposta contém uma lista de potencial de correspondência de celebridades, acompanhadas por suas pontuações de confiança.
* Recursos de análise  
  Analise para fornecer detalhes adicionais relacionados com categorias de taxonomia 86 categoria. Esta opção está disponível para uso em aplicativos em que os usuários querem obter análises de imagem genérica além dos detalhes de um ou mais modelos de domínios específicos. Quando esse método é invocado, o classificador de taxonomia da categoria 86 é chamado pela primeira vez. Se nenhuma das categorias fazer a correspondência dos modelos de conhecidos ou correspondentes, uma segunda passagem de invocações de classificador segue. Por exemplo, se o `details` parâmetro da chamada de POST de HTTP ou está definido para "todos" ou inclua "celebridades", o método chama o classificador de celebridade depois que o classificador de categoria 86 é chamado. Se a imagem é classificada como `people_` ou numa subcategoria dessa categoria, em seguida, o classificador de celebridade é chamada.

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