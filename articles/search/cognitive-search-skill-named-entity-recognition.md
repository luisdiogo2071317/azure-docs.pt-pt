---
title: Com o nome skill de pesquisa cognitivos de reconhecimento de entidade (Azure Search) | Microsoft Docs
description: Extrair entidades nomeadas para a pessoa, a localização e a organização do texto no pipeline de pesquisa cognitivos pesquisa do Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 73ffcf5e2ced63fddaf0f5ef2ca7e72a7d94b966
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Nomeado skill cognitivos de reconhecimento de entidade

O **reconhecimento de entidade com o nome** skill extrai entidades com o nome de texto. Entidades disponíveis incluem os tipos de `person`, `location`, e `organization`.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="skill-parameters"></a>Parâmetros de skill

Os parâmetros são maiúsculas e minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| categorias    | Matriz de categorias deve ser extraído.  Tipos de categoria possíveis: `"Person"`, `"Location"`, `"Organization"`. Se não for fornecida nenhum categoria, são devolvidos todos os tipos.|
|defaultLanguageCode |  Código de idioma texto de entrada. São suportados os seguintes idiomas: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`|
| minimumPrecision  | Um número entre 0 e 1. Se for inferior a este valor a precisão, a entidade não é devolvida. A predefinição é 0.|

## <a name="skill-inputs"></a>Entradas de skill

| Nome de entrada      | Descrição                   |
|---------------|-------------------------------|
| languageCode  | Opcional. A predefinição é `"en"`.  |
| Texto          | O texto a analisar.          |

## <a name="skill-outputs"></a>Saídas skill

| Nome de saída     | Descrição                   |
|---------------|-------------------------------|
| pessoas      | Uma matriz de cadeias, onde cada cadeia representa o nome de uma pessoa. |
| localizações  | Uma matriz de cadeias, onde cada cadeia representa uma localização. |
| Organizações  | Uma matriz de cadeias, onde cada cadeia representa uma organização. |
| entidades | Uma matriz de tipos complexos. Cada tipo de complexo inclui os seguintes campos: <ul><li>categoria (`"person"`, `"organization"`, ou `"location"`)</li> <li>valor (o nome da entidade real)</li><li>deslocamento (localização onde foi encontrado no texto)</li><li>(um valor entre 0 e 1 que representa essa confiança de que o valor é uma entidade real) de confiança</li></ul> |

##  <a name="sample-definition"></a>Definição de exemplo

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
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
             "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
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
        "locations": ["Seattle"],
        "organizations":["Microsoft Corporation"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 45,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 59,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Seattle",
            "offset": 5,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft Corporation",
            "offset": 120,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Nos casos de erro
Se especificar um código de idioma não suportada ou se o conteúdo não corresponde ao idioma especificado, o erro é devolvido e não existem entidades são extraídas.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)