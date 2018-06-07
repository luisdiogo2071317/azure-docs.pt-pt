---
title: Imagem skill de pesquisa cognitivos Analysis (Azure Search) | Microsoft Docs
description: Extraia o texto semântico através da análise de imagem com o skill cognitivos ImageAnalysis no pipeline de sem causa pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: dd26dbe34cd04d1ad3184e2cd62afae5166ac914
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640511"
---
#   <a name="image-analysis-cognitive-skill"></a>Imagem skill cognitivos Analysis Services

O **imagem Analysis** skill extrai um conjunto avançado de funcionalidades visual com base no conteúdo de imagem. Por exemplo, pode gerar uma legenda a partir de uma imagem, gerar etiquetas ou identificar celebridades para e landmarks.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| defaultLanguageCode   |  Uma cadeia que indica o idioma para devolver. O serviço devolve resultados de reconhecimento num idioma especificado. Se este parâmetro não for especificado, o valor predefinido é "pt". <br/><br/>Idiomas suportados são: <br/>*EN* -inglês (predefinição) <br/> *zh* -chinês simplificado|
|visualFeatures |   Uma matriz de cadeias que indica os tipos de funcionalidade visual para devolver. Os tipos de funcionalidade visual válidos incluem:  <ul><li> *categorias* -categoriza conteúdo da imagem de acordo com uma taxonomia definido nos serviços cognitivos [documentação](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).</li><li> *etiquetas* -etiquetas a imagem com uma lista detalhada das palavras relacionados com o conteúdo da imagem.</li><li>*Descrição* -descreve a imagem de conteúdo com uma frase inglês concluída.</li><li>*Faces* -Deteta se faces estão presentes. Se estiver presente, gera as coordenadas, sexo e idade.</li><li> *ImageType* -Deteta se imagem clipart ou um linha de desenho.</li><li>   *Cor* -determina a cor a acentos, dominante cor, e se uma imagem é preta & branco.</li><li>*Para adultos* -Deteta se a imagem pornographic natureza (ilustra nudity ou um ATO sex). Conteúdo sexually suggestive também é detetado.</li></ul> Os nomes de funcionalidades do visual são maiúsculas e minúsculas.|
| detalhes   | Uma matriz de cadeias que indica que específicas do domínio detalhes para devolver. Os tipos de funcionalidade visual válidos incluem: <ul><li>*Celebridades para* -identifica celebridades para detetada na imagem.</li><li>*Landmarks* -identifica landmarks detetada na imagem.</li></ul>
 |

## <a name="skill-inputs"></a>Entradas de skill

| Nome de entrada      | Descrição                                          |
|---------------|------------------------------------------------------|
| Imagem         | Tipo complexo. Atualmente, apenas funciona com o campo de "documento/normalized_images", produzido pelo indexador de Blob do Azure quando ```imageAction``` está definido como ```generateNormalizedImages```. Consulte o [exemplo](#sample-output) para obter mais informações.|



##  <a name="sample-definition"></a>Definição de exemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "visualFeatures": [
        "Tags",
        "Faces",
        "Categories",
        "Adult",
        "Description",
        "ImageType",
        "Color"
    ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "image",
            "source": "/document/normalized_images/*"
        }
    ],
    "outputs": [
        {
            "name": "categories",
            "targetName": "myCategories"
        },
        {
            "name": "tags",
            "targetName": "myTags"
        },
        {
            "name": "description",
            "targetName": "myDescription"
        },
        {
            "name": "faces",
            "targetName": "myFaces"
        },
        {
            "name": "imageType",
            "targetName": "myImageType"
        },
        {
            "name": "color",
            "targetName": "myColor"
        },
        {
            "name": "adult",
            "targetName": "myAdultCategory"
        }
    ]
}
```

##  <a name="sample-input"></a>Entrada de exemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "url": "https://storagesample.blob.core.windows.net/sample-container/image.jpg"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Resultado da amostra

```json
{
    "values": [
      {
        "recordId": "1",
            "data": {
                "categories": [
           {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                "name": "people_",
                        "score": 0.83984375,
                "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceRectangle": {
                                        "left": 597,
                                        "top": 162,
                                        "width": 248,
                                        "height": 248
                                    },
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                    "faceRectangle": {
                            "left": 593,
                            "top": 160,
                            "width": 250,
                            "height": 250
                        }
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBWImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
           }
      }
    ]
}
```


## <a name="error-cases"></a>Nos casos de erro
Nos seguintes casos de erro, não existem elementos são extraídos.

| Código de Erro | Descrição |
|------------|-------------|
| NotSupportedLanguage | O idioma fornecido não é suportado. |
| InvalidImageUrl | O URL da imagem é incorretamente formatado ou não está acessível.|
| InvalidImageFormat | Dados de entrada não são uma imagem válida. |
| InvalidImageSize | Imagem de entrada é demasiado grande. |
| NotSupportedVisualFeature  | Tipo de função especificada não é válido. |
| NotSupportedImage | Imagem não suportada, por exemplo, pornografia infantil. |
| InvalidDetails | Modelo de domínio específico não suportado. |

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Criar indexador (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
