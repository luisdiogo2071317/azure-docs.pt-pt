---
title: Skill de pesquisa cognitivos de intercalação de texto (Azure Search) | Microsoft Docs
description: Intercale texto de uma coleção de campos um campo consolidado. Utilize este skill cognitivos um pipeline de sem causa de pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: f349158873acca9d50d4d6e5fdfa3539f26207fe
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
#    <a name="text-merge-cognitive-skill"></a>Skill cognitivos de intercalação de texto

O **texto intercalar** skill consolida texto de uma coleção de campos para um único campo. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.TextMerger

## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| insertPreTag  | Cadeia a ser incluído antes de cada inserção. O valor predefinido é `" "`. Omitir o espaço, defina o valor como `""`.  |
| insertPostTag | Cadeia a incluir após cada inserção. O valor predefinido é `" "`. Omitir o espaço, defina o valor como `""`.  |


##  <a name="sample-input"></a>Entrada de exemplo
Pode ser um documento JSON fornecer utilizável entrada para este skill:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Resultado da amostra
Este exemplo mostra a saída da entrada anterior, partindo do princípio de que o *insertPreTag* está definido como `" "`, e *insertPostTag* está definido como `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>Exemplo expandida skillset definição

É um cenário comum para utilizar a intercalação de texto para intercalar a representação textual de imagens (texto de um skill OCR ou a legenda de uma imagem) para o campo de conteúdo de um documento. 

Skillset de exemplo seguinte utiliza o skill OCR para extraia o texto a partir de imagens incorporadas no documento. Em seguida, cria um *merged_text* campo a conter original e OCRed texto de cada imagem. 

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Util.TextMerger",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " "
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
O exemplo acima assume que existe um campo de imagens normalizado. Para obter o campo de imagens normalizado, defina o *imageAction* configuração na sua definição de indexador *generateNormalizedImages* conforme mostrado abaixo:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Criar indexador (REST)](ref-create-indexer.md)