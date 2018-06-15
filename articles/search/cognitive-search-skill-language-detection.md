---
title: Skill de cognitivos pesquisa de deteção de idioma (Azure Search) | Microsoft Docs
description: Avalia o texto não estruturado e para cada registo devolve um identificador de linguagem com uma pontuação que indica a força da análise no pipeline de sem causa pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 338d89b47ea451efcf8300d4ac016a6946a95259
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791057"
---
#   <a name="language-detection-cognitive-skill"></a>Skill cognitivos de deteção de idioma

Para até 120 idiomas, o **idioma deteção** skill Deteta o idioma de texto de entrada e reporta um código de idioma individual para cada documento submetido no pedido. O código de idioma se encontra emparelhado com uma pontuação que indica a força da análise.

Esta capacidade é especialmente útil quando tem de fornecer o idioma do texto como entrada para outras competências (por exemplo, o [sentimento Snalysis skill](cognitive-search-skill-sentiment.md) ou [skill de texto divididos](cognitive-search-skill-textsplit.md)).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registo deve ser 50.000 carateres, medido pela `String.Length`. Se precisar de dividir os dados antes de a enviar para o analisador de sentimento, pode utilizar o [skill de texto divididos](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Entradas de skill

Os parâmetros são maiúsculas e minúsculas.

| Entradas     | Descrição |
|--------------------|-------------|
| Texto | O texto a ser analisada.|

## <a name="skill-outputs"></a>Saídas skill

| Nome de saída    | Descrição |
|--------------------|-------------|
| languageCode | O código de idioma ISO 6391 para o idioma identificado. Por exemplo, "pt". |
| LanguageName | O nome de idioma. Por exemplo, "inglês". |
| Modelo de pontuação | Um valor entre 0 e 1. A probabilidade de que o idioma corretamente é identificado. A classificação pode ser inferior a 1 se o frase tem misto idiomas.  |

##  <a name="sample-definition"></a>Definição de exemplo

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill ",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
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
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Nos casos de erro
Se o texto é expresso um idioma não suportados, é gerado um erro e não é devolvido nenhum identificador de linguagem.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)