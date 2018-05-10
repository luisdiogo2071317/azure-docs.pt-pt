---
title: Skill de pesquisa cognitivos shaper (Azure Search) | Microsoft Docs
description: Extrair metadados e estruturadas informações de dados não estruturados e formam-lo como um tipo complexo no pipeline de sem causa pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 311f4bd67081de567763783a9d86540eda36d9f8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
#   <a name="shaper-cognitive-skill"></a>Skill cognitivos shaper

O **Shaper** skill cria um tipo complexo para suportar campos compostos (também conhecido como com várias partes campos). Um campo de tipo complexo tem várias partes, mas é tratado como um único item num índice da Azure Search. Campos consolidados útil em cenários de pesquisa exemplos de combinar um nome próprio e apelido num único campo, cidade e estado para um único campo, ou o nome e a data de nascimento para um único campo para estabelecer a identidade exclusiva.

O skill Shaper permite-lhe criar uma estrutura essencialmente, definir o nome dos membros essa estrutura e atribuir valores para cada membro.

Por predefinição, esta técnica suporta objetos que estão um nível profundo. Para os objetos mais complexos, pode encadeiam vários passos Shaper.

Na resposta, o nome de saída é sempre "saído". Internamente, o pipeline pode mapear um nome diferente, tal como "analyzedText" nos exemplos abaixo de "saída", mas o Shaper skill próprio devolve "saída" na resposta. Pode ser importante se está a depurar documentos avançados e repare a discrepância de nomenclatura ou se criar um skill personalizado e são structuring a resposta por si.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Exemplo 1: tipos complexos

Considere um cenário onde pretende criar uma estrutura chamada *analyzedText* que tem dois membros: *texto* e *sentimento*, respetivamente. Na Azure Search, um campo pesquisável com várias parte é chamado um *tipo complexo*, e ainda não é suportada a box. Nesta pré-visualização, um skill Shaper pode ser utilizado para gerar os campos de um tipo complexo no seu índice. 

O exemplo seguinte fornece os nomes de membro como entrada. A estrutura de saída (o campo complexa na Azure Search) é especificada através de *targetName*. 


```json
{
  "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
  "context": "/document/content/phrases/*",
  "inputs": [
    {
      "name": "text",
      "source": "/document/content/phrases/*"
    },
    {
      "name": "sentiment",
      "source": "/document/content/phrases/*/sentiment"
    }
  ],
  "outputs": [
    {
      "name": "output",
      "targetName": analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Entrada de exemplo
Pode ser um documento JSON fornecer utilizável entrada para este skill Shaper:

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "this movie is awesome",
                "sentiment": 0.9
            }
        }
    ]
}
```


### <a name="sample-output"></a>Resultado da amostra
O skill Shaper gera um novo elemento chamado *analyzedText* com os elementos combinados de *texto* e *sentimento*. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "analyzedText": 
              {
                "text": "this movie is awesome" ,
                "sentiment": 0.9
              }
           }
      }
    ]
}
```

## <a name="sample-2-input-consolidation"></a>Exemplo 2: consolidação de entrada

Noutro exemplo, imagine que em diferentes fases de processamento de pipeline, a extrair o título de um livro e títulos de capítulo em diferentes páginas no livro. Pode agora criar uma estrutura simples é composta por estas várias entradas.

A definição de skill Shaper para este cenário poderá ter um aspeto semelhante ao seguinte exemplo:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "source": "/document/content/pages/*/chapterTitles/*"
        }
    ],
    "outputs": [
        {
            "output": "titlesAndChapters",
            "targetName": "analyzedText"
        }
    ]
}
```

### <a name="sample-output"></a>Resultado da amostra
Neste caso, o Shaper flattens todos os títulos de capítulo para criar uma matriz única. 

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                        "Start young",
                        "Laugh often",
                        "Eat, sleep and exercise"
                    ]
                }
            }
        }
    ]
}
```

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)

