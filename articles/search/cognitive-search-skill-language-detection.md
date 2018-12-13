---
title: Habilidade de pesquisa cognitiva de deteção de idioma - Azure Search
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
ms.custom: seodec2018
ms.openlocfilehash: 741710a9f2a9e505681401183f5f41be0695633b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308571"
---
#   <a name="language-detection-cognitive-skill"></a>Habilidade de cognitiva de deteção de idioma

Para até 120 idiomas, o **deteção de idioma** habilidade Deteta o idioma de texto de entrada e comunica um código de idioma único para cada documento enviado no pedido. O código de idioma é emparelhado com uma pontuação indicando a força da análise.

Esta capacidade é especialmente útil quando tem de indicar o idioma do texto como entrada para outras habilidades (por exemplo, o [habilidade de análise de sentimentos](cognitive-search-skill-sentiment.md) ou [habilidade de divisão de texto](cognitive-search-skill-textsplit.md)).

> [!NOTE]
> A partir de 21 de Dezembro de 2018, poderá associar um recurso dos serviços cognitivos com um conjunto de capacidades do Azure Search. Isso permitirá que comecem a cobrança de consumo para a execução do conjunto de capacidades. Esta data, podemos também começar a cobrar extração de imagem como parte da fase de aberturas de documentos. Continuaremos a oferecer a extração de texto de documentos sem custos adicionais.
>
> A execução das habilidades internas será cobrada existente [dos serviços cognitivos pay as you go preço](https://azure.microsoft.com/pricing/details/cognitive-services/) . Preços de extração de imagem são cobrados ao preço de pré-visualização e está descrito na [página de preços do Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400). Saiba mais [mais](cognitive-search-attach-cognitive-services.md).

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
