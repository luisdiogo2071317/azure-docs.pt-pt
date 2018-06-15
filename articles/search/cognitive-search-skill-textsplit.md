---
title: Texto dividida skill pesquisa cognitivos (Azure Search) | Microsoft Docs
description: Quebra texto segmentos ou páginas de texto com base no comprimento no pipeline de sem causa pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: dbb9261cfce0a8437cfe76121fa16aa87c4b3393
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791029"
---
#   <a name="text-split-cognitive-skill"></a>Texto dividida skill cognitivos

O **texto divididos** skill quebras de texto em segmentos de texto. Pode especificar se pretende quebrar o texto num frases ou num páginas com um comprimento específico. Este skill é especialmente útil se não existirem texto máximo requisitos de comprimento nas outras competências downstream. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| textSplitMode      | "Páginas" ou "frases" | 
| maximumPageLength | Se textSplitMode estiver definido como "páginas", refere-se para o comprimento de máximo de página, medido pela `String.Length`. O valor mínimo é 100. | 
| defaultLanguageCode   | (opcional) Um dos códigos de idioma seguintes: `da, de, en, es, fi, fr, it, ko, pt`. Predefinição é inglês (en). Alguns aspetos a considerar:<ul><li>Se obtiver um formato de languagecode-indicativo do país, é utilizada apenas a parte languagecode do formato.</li><li>Quando o idioma não estiver na lista anterior, a divisão skill interrompe o texto em limites de caráter.</li><li>Fornecer um código de idioma é útil para evitar cortando uma palavra na metade para idiomas não espaço como chinês, japonês e coreano.</li></ul>  |


## <a name="skill-inputs"></a>Entradas de skill

| Nome do parâmetro       | Descrição      |
|----------------------|------------------|
| Texto  | O texto a ser dividida em substring. |
| languageCode  | (Opcional) Código de idioma para o documento.  |

## <a name="skill-outputs"></a>Saídas skill 

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| textItems | Uma matriz de subcadeias foram extraídos. |


##  <a name="sample-definition"></a>Definição de exemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
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
                "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Saída de exemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Nos casos de erro
Se um idioma não for suportado, é gerado um aviso e o texto é dividido em limites de caráter.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
