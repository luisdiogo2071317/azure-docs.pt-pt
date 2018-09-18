---
title: Habilidade de pesquisa cognitiva de deteção de idioma (Azure Search) | Documentos da Microsoft
description: Avalia a texto não estruturado e para cada registo, devolve um identificador de idioma com uma pontuação indicando a força da análise num pipeline de enriquecimento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 2fd1c1ec0d2442afd6367e1d35af6f798dced2c7
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733283"
---
#   <a name="language-detection-cognitive-skill"></a>Habilidade de cognitiva de deteção de idioma

Para até 120 idiomas, o **deteção de idioma** habilidade Deteta o idioma de texto de entrada e comunica um código de idioma único para cada documento enviado no pedido. O código de idioma é emparelhado com uma pontuação indicando a força da análise.

Esta capacidade é especialmente útil quando tem de indicar o idioma do texto como entrada para outras habilidades (por exemplo, o [habilidade de análise de sentimentos](cognitive-search-skill-sentiment.md) ou [habilidade de divisão de texto](cognitive-search-skill-textsplit.md)).

> [!NOTE]
> A Pesquisa Cognitiva está em pré-visualização pública. Conjunto de capacidades execução e a extração de imagem e a normalização atualmente são oferecidos gratuitamente. Posteriormente, os preços para estas capacidades serão anunciado. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registo deve ser 50.000 carateres conforme medido pela `String.Length`. Se tiver de dividir os dados antes de os enviar para o analisador de sentimentos, pode utilizar o [habilidade de divisão de texto](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Entradas de habilidades

Parâmetros diferenciam maiúsculas de minúsculas.

| Entradas     | Descrição |
|--------------------|-------------|
| texto | O texto a ser analisados.|

## <a name="skill-outputs"></a>Saídas de habilidades

| Nome de saída    | Descrição |
|--------------------|-------------|
| languageCode | O código de idioma ISO 6391 para o idioma identificado. Por exemplo, "pt". |
| LanguageName | O nome de idioma. Por exemplo, "inglês". |
| pontuação | Um valor entre 0 e 1. A probabilidade de que o idioma é identificado corretamente. A classificação pode ser inferior a 1, se a frase tem misto de idiomas.  |

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


## <a name="error-cases"></a>Casos de erro
Se o texto é expressa num idioma não suportado, é gerado um erro e não é devolvido nenhum identificador de idioma.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)
