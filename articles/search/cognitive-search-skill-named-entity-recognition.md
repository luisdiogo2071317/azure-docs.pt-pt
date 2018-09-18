---
title: Com o nome habilidade de pesquisa cognitiva de reconhecimento de entidades (Azure Search) | Documentos da Microsoft
description: Extrair entidades nomeadas para pessoa, a localização e a organização de texto num pipeline do Azure Search pesquisa cognitiva.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 653a4675d546432eea8478ba6203be1df71ec4f4
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731398"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Habilidade de cognitiva reconhecimento de entidades nomeada

O **reconhecimento de entidades com o nome** habilidade extrai entidades nomeadas de texto. Entidades disponíveis incluem os tipos `person`, `location`, e `organization`.

> [!NOTE]
> A Pesquisa Cognitiva está em pré-visualização pública. Conjunto de capacidades execução e a extração de imagem e a normalização atualmente são oferecidos gratuitamente. Posteriormente, os preços para estas capacidades serão anunciado. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="skill-parameters"></a>Parâmetros de habilidades

Parâmetros diferenciam maiúsculas de minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| categorias    | Matriz de categorias que deve ser extraído.  Tipos de categoria possíveis: `"Person"`, `"Location"`, `"Organization"`. Se não for fornecida nenhum categoria, todos os tipos são devolvidos.|
|defaultLanguageCode |  Código de idioma do texto de entrada. São suportados os seguintes idiomas: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`|
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


## <a name="error-cases"></a>Casos de erro
Se especificar um código de idioma não suportado ou se o conteúdo não corresponde ao idioma especificado, um erro é devolvido e não existem entidades são extraídas.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)