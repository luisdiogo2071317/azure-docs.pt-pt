---
title: Com o nome habilidade de pesquisa cognitiva de reconhecimento de entidades - Azure Search
description: Extrair entidades nomeadas para pessoa, a localização e a organização de texto num pipeline do Azure Search pesquisa cognitiva.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: e63354152f8821c5ce975563639c8b87fb332bd4
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313994"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Habilidade de cognitiva reconhecimento de entidades nomeada

O **reconhecimento de entidades com o nome** habilidade extrai entidades nomeadas de texto. Entidades disponíveis incluem os tipos `person`, `location` e `organization`.

> [!NOTE]
> <ul>
> <li>
> A partir de 21 de Dezembro de 2018, poderá associar um recurso dos serviços cognitivos com um conjunto de capacidades do Azure Search. Isso permitirá que comecem a cobrança de consumo para a execução do conjunto de capacidades. Esta data, podemos também começar a cobrar extração de imagem como parte da fase de aberturas de documentos. Continuaremos a oferecer a extração de texto de documentos sem custos adicionais.>
> A execução das habilidades internas será cobrada existente [dos serviços cognitivos pay as you go preço](https://azure.microsoft.com/pricing/details/cognitive-services/) . Preços de extração de imagem são cobrados ao preço de pré-visualização e está descrito na [página de preços do Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400). Saiba mais [mais](cognitive-search-attach-cognitive-services.md).</li>
> <li> Habilidade de reconhecimento de entidades nomeadas é considerada "preterido" e não será oficialmente suportada a partir de 15 de Fevereiro, 2019. Siga as recomendações listadas na <a href="cognitive-search-skill-deprecated.md">preterido capacidades de pesquisa cognitivas</a> página para migrar para uma habilidade suportada</li>

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registo deve ser 50.000 carateres conforme medido pela `String.Length`. Se tiver de dividir os dados antes de os enviar para o extrator de expressões-chave, considere utilizar o [habilidade de divisão de texto](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parâmetros de habilidades

Parâmetros diferenciam maiúsculas de minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| categories    | Matriz de categorias que deve ser extraído.  Tipos de categoria possíveis: `"Person"`, `"Location"`, `"Organization"`. Se não for fornecida nenhum categoria, todos os tipos são devolvidos.|
|defaultLanguageCode |  Código de idioma do texto de entrada. São suportados os seguintes idiomas: `de, en, es, fr, it`|
| minimumPrecision  | Um número entre 0 e 1. Se a precisão é inferior este valor, a entidade não é devolvida. A predefinição é 0.|

## <a name="skill-inputs"></a>Entradas de habilidades

| Nome de entrada      | Descrição                   |
|---------------|-------------------------------|
| languageCode  | Opcional. A predefinição é `"en"`.  |
| texto          | O texto a analisar.          |

## <a name="skill-outputs"></a>Saídas de habilidades

| Nome de saída     | Descrição                   |
|---------------|-------------------------------|
| pessoas      | Uma matriz de cadeias de caracteres em que cada cadeia de caracteres representa o nome de uma pessoa. |
| localizações  | Uma matriz de cadeias de caracteres em que cada cadeia de caracteres representa uma localização. |
| organizações  | Uma matriz de cadeias de caracteres em que cada cadeia de caracteres representa uma organização. |
| entidades | Uma matriz de tipos complexos. Cada tipo complexo inclui os seguintes campos: <ul><li>categoria (`"person"`, `"organization"`, ou `"location"`)</li> <li>valor (o nome de entidade real)</li><li>deslocamento (localização onde foi encontrado no texto)</li><li>confiança (um valor entre 0 e 1 que representa essa confiança que o valor é uma entidade real)</li></ul> |

##  <a name="sample-definition"></a>Definição de exemplo

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
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
             "text": "This is the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
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
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Casos de erro
Se o código de idioma para o documento não é suportado, é devolvido um erro e não existem entidades são extraídas.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)
+ [Habilidade de reconhecimento de entidades](cognitive-search-skill-entity-recognition.md)
