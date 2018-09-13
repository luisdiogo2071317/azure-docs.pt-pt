---
title: Habilidade de pesquisa cognitiva de sentimento (Azure Search) | Documentos da Microsoft
description: Extrair sentimentos de texto num pipeline de enriquecimento de Azure Search.
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
ms.openlocfilehash: f6a0214ce9bc1cdf6aefbd9cde86e72b7cf7b0f2
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35947653"
---
#   <a name="sentiment-cognitive-skill"></a>Competências cognitivas de sentimentos

O **sentimentos** habilidade avalia o texto não estruturado ao longo de um continuum positivo negativo e para cada registo, devolve uma pontuação numérica entre 0 e 1. As pontuações próximas de 1 indicam um sentimento positivo, enquanto as pontuações próximas de 0 indicam um sentimento negativo.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registo deve ser 5000 carateres conforme medido pela `String.Length`. Se tiver de dividir os dados antes de os enviar para o analisador de sentimentos, utilize o [habilidade de divisão de texto](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Parâmetros de habilidades

Parâmetros diferenciam maiúsculas de minúsculas.

| Nome do Parâmetro |                      |
|----------------|----------------------|
| defaultLanguageCode | (opcional) O código de idioma para aplicar a documentos que não especificar explicitamente o idioma. <br/> Consulte [lista completa das linguagens suportadas](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Entradas de habilidades 

| Nome de entrada | Descrição |
|--------------------|-------------|
| texto | O texto a ser analisados.|
| languageCode  |  (Opcional) Uma cadeia que indica o idioma dos registos. Se este parâmetro não for especificado, o valor predefinido é "en". <br/>Ver [uma lista completa das linguagens suportadas](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Saídas de habilidades

| Nome de saída | Descrição |
|--------------------|-------------|
| pontuação | Um valor entre 0 e 1 que representa o sentimento do texto analisado. Valores próximas de 0 têm um sentimento negativo, próximo 0,5 têm neutro sentimentos e valores próximas de 1 tem um sentimento positivo.|


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
Se estiver vazio, uma pontuação de sentimento não é devolvida para esses registos.

## <a name="error-cases"></a>Casos de erro
Se um idioma não for suportado, é gerado um erro e nenhuma classificação de sentimento é retornada.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)