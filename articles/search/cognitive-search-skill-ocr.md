---
title: Skill de pesquisa cognitivos OCR (Azure Search) | Microsoft Docs
description: Extraia o texto a partir dos ficheiros de imagem no pipeline de sem causa pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 478afe81ed739b98487973eb092ee9cad0aa17fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059193"
---
# <a name="ocr-cognitive-skill"></a>Skill cognitivos OCR

O **OCR** skill extrai o texto de ficheiros de imagem. Formatos de ficheiro suportados incluem:

+ . JPEG
+ . JPG
+ . PNG
+ . BMP
+ . GIF


## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| detectOrientation | Permite autodetection de orientação de imagem. <br/> Os valores válidos: Verdadeiro / Falso.|
|defaultLanguageCode | <p>  Código de idioma texto de entrada. As linguagens suportadas incluem: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>CS (Checo) <br/>da (dinamarquês) <br/>NL (Neerlandês) <br/>en (inglês) <br/>Fi (Finlandês)  <br/>FR (francês) <br/>  Alemanha (alemão) <br/>EL (Grego) <br/> hu (Húngaro) <br/> -lo (italiano) <br/>  ja (japonês) <br/> ko (coreano) <br/> nb (Norueguês) <br/>   LP (Polaco) <br/> PT (Português) <br/>  RU (Russo) <br/>  es (espanhol) <br/>  SV (Sueco) <br/>  TR (Turco) <br/> ar (Árabe) <br/> ro (Romeno) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  SK (Eslovaco). <br/>  UNK (desconhecido) <br/><br/> Se o código de idioma não especificado ou é nulo, o idioma é autodetected. </p> |
| textExtractionAlgorithm | "impressas" ou "handwritten". O algoritmo de OCR de reconhecimento de texto "handwritten" está atualmente em pré-visualização e só é suportado em inglês. |

## <a name="skill-inputs"></a>Entradas de skill

| Nome de entrada      | Descrição                                          |
|---------------|------------------------------------------------------|
| image         | Tipo complexo. Atualmente, apenas funciona com o campo de "documento/normalized_images", produzido pelo indexador de Blob do Azure quando ```imageAction``` está definido como ```generateNormalizedImages```. Consulte o [exemplo](#sample-output) para obter mais informações.|


## <a name="skill-outputs"></a>Saídas skill
| Nome de saída     | Descrição                   |
|---------------|-------------------------------|
| texto          | Texto simples extraído a partir da imagem.   |
| layoutText    | Tipo de complexo que descreve o texto extraído, bem como a localização onde o texto foi encontrado.|


## <a name="sample-definition"></a>Definição de exemplo

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Saída de texto e layoutText de exemplo

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Exemplo: Intercalação extraído de imagens incorporadas com o conteúdo do documento de texto.

Um caso de utilização comuns para texto fusão é a capacidade para intercalar a representação textual de imagens (texto de um skill OCR ou a legenda de uma imagem) para o campo de conteúdo de um documento. 

Skillset de exemplo seguinte cria um *merged_text* campo para conter o conteúdo textual do seu documento, bem como o texto de OCRed de cada uma das imagens incorporado esse documento. 

#### <a name="request-body-syntax"></a>Sintaxe do Corpo do Pedido
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
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
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
O exemplo de skillset acima assume que existe um campo de imagens normalizado. Para gerar este campo, defina o *imageAction* configuração na sua definição de indexador *generateNormalizedImages* conforme mostrado abaixo:

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
+ [TextMerger skill](cognitive-search-skill-textmerger.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Criar indexador (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
