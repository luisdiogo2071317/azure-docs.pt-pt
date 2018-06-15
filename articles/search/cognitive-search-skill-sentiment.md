---
title: Skill de pesquisa cognitivos sentimento (Azure Search) | Microsoft Docs
description: Extrair dados de sentimento do texto no pipeline de sem causa pesquisa do Azure.
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
ms.openlocfilehash: 1ddbba5b881cd05a997cd24a9396d5b722376e6f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791001"
---
#   <a name="sentiment-cognitive-skill"></a>Skill cognitivos sentiment

O **sentimento** skill avalia o texto não estruturado ao longo de um continuum positivos negativo e para cada registo, devolve uma pontuação numérico entre 0 e 1. As pontuações próximas de 1 indicam um sentimento positivo, enquanto as pontuações próximas de 0 indicam um sentimento negativo.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registo deve ser 5000 carateres, medido pela `String.Length`. Se precisar de dividir os dados antes de a enviar para o analisador de sentimento, utilize o [skill de texto divididos](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.

| Nome do Parâmetro |                      |
|----------------|----------------------|
| defaultLanguageCode | (opcional) O código de idioma para aplicar a documentos que não especificar o idioma explicitamente. <br/> Consulte [lista completa dos idiomas suportados](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Entradas de skill 

| Nome de entrada | Descrição |
|--------------------|-------------|
| Texto | O texto a ser analisada.|
| languageCode  |  (Opcional) Uma cadeia que indica o idioma dos registos. Se este parâmetro não for especificado, o valor predefinido é "pt". <br/>Consulte [lista completa dos idiomas suportados](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Saídas skill

| Nome de saída | Descrição |
|--------------------|-------------|
| Modelo de pontuação | Um valor entre 0 e 1 que representa o sentimento do texto analisado. Valores próximo de 0 tem sentimento negativo, próximo 0,5 ter dados de sentimento independente e valores do próximo de 1 têm sentimento positivo.|


##  <a name="sample-definition"></a>Definição de exemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
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
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
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
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notas
Se estiver vazio, uma classificação de dados de sentimento não é devolvida para esses registos.

## <a name="error-cases"></a>Nos casos de erro
Se um idioma não for suportado, é gerado um erro e não pontuação de sentimento é devolvida.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)