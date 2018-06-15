---
title: Chave skill de pesquisa cognitivos de extração de expressão (Azure Search) | Microsoft Docs
description: Avalia o texto não estruturado e para cada registo, devolve uma lista de expressões chaves no pipeline de sem causa pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a12efaa020e626e4a10a0708c9b84d8fe125588c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791036"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Skill cognitivos de extração de expressão de chave

O **extração da expressão de chave** skill avalia o texto não estruturado e para cada registo, devolve uma lista de expressões de chaves.

Esta capacidade é útil se precisar de identificar rapidamente os pontos principais da talking no registo. Por exemplo, texto de entrada especificado "o prato foi delicious e ocorreram wonderful pessoal", o serviço devolve "prato" e "wonderful pessoal".

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registo deve ser 50.000 carateres, medido pela `String.Length`. Se precisar de dividir os dados antes de a enviar para o extrator de expressão de chave, considere utilizar o [skill de texto divididos](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.
| Entradas                | Descrição |
|---------------------|-------------|
| defaultLanguageCode | (Opcional) O código de idioma para aplicar a documentos que não especificar o idioma explicitamente.  Se o código de idioma predefinido não for especificado, inglês (en) será utilizado como o código de idioma predefinido. <br/> Consulte [lista completa dos idiomas suportados](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). |
| maxKeyPhraseCount   | (Opcional) O número máximo de chaves expressões para produzir. |

## <a name="skill-inputs"></a>Entradas de skill
| Entradas     | Descrição |
|--------------------|-------------|
| Texto | O texto a ser analisada.|
| languageCode  |  Uma cadeia que indica o idioma dos registos. Se este parâmetro não for especificado, será utilizado o código de idioma predefinido para analisar os registos. <br/>Consulte [lista completa dos idiomas suportados](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)|

##  <a name="sample-definition"></a>Definição de exemplo

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      },
      {
        "name": "languageCode",
        "source": "/document/languagecode" 
      }
    ],
    "outputs": [
      {
        "name": "keyPhrases",
        "targetName": "myKeyPhrases"
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
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. They accumulate ice from snowfall and lose it through melting. As global temperatures have risen, many of the world’s glaciers have already started to shrink and retreat. Continued warming could see many iconic landscapes – from the Canadian Rockies to the Mount Everest region of the Himalayas – lose almost all their glaciers by the end of the century.",
             "language": "en"
           }
      }
    ]
```


##  <a name="sample-output"></a>Resultado da amostra

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "keyPhrases": 
            [
              "world’s glaciers", 
              "huge rivers of ice", 
              "Canadian Rockies", 
              "iconic landscapes",
              "Mount Everest region",
              "Continued warming"
            ]
           }
      }
    ]
}
```


## <a name="errors-and-warnings"></a>Erros e avisos
Se fornecer um código de idioma não suportados, é gerado um erro e expressões de chaves não são extraídos.
Se o texto estiver vazio, irá ser produziu um aviso.
Se o texto é maior do que 50.000 carateres, apenas os primeiro 50.000 carateres irão ser analisados e será emitido um aviso.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)