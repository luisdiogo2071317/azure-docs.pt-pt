---
title: Definição de competências personalizadas num pipeline pesquisa cognitivos (Azure Search) de interface | Microsoft Docs
description: Interface de extração de dados personalizados para skill personalizada da web api no pipeline de cognitivos pesquisa na Azure Search.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 8f21a56982189aa13745f27f0fae49310ae55aa0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640324"
---
# <a name="how-to-add-a-custom-skill-to-a-cognitive-search-pipeline"></a>Como adicionar um skill personalizado para um pipeline de pesquisa cognitivos

Neste artigo, irá aprender a adicionar um skill personalizado para um pipeline de pesquisa cognitivos. A [pipeline de indexação da pesquisa cognitivos](cognitive-search-concept-intro.md) na pesquisa do Azure pode ser assembled de [predefinidas competências](cognitive-search-predefined-skills.md) e competências personalizadas que criar e adicionar ao pipeline pessoal.

Criar um skill personalizado dá-lhe uma forma de inserir transformações exclusivas para o conteúdo. Um skill personalizado executa independentemente, aplicar qualquer passo sem causa precisa. Por exemplo, pode definir entidades personalizadas específicas do campo, criar modelos de classificação personalizada para diferenciar o negócio e contratos financeiros e os documentos ou adicionar um skill de reconhecimento de voz para chegar a mais profundo áudio ficheiros para o conteúdo relevante. Para obter um exemplo passo a passo, consulte [exemplo: criar um skill personalizado](cognitive-search-create-custom-skill-example.md).

 Qualquer precisa, não há uma interface simple e desmarque para ligar um skill personalizado para o resto do pipeline sem causa de capacidade personalizado. O único requisito para inclusão num [skillset](cognitive-search-defining-skillset.md) é a capacidade de aceitar as entradas e saídas de tal forma que é consumíveis dentro skillset como um todo de emissão. O foco deste artigo é os formatos de entrada e de saída que requer que o pipeline sem causa.

## <a name="web-api-custom-skill-interface"></a>Interface de skill personalizada de API Web

Atualmente, o mecanismo de apenas para interagir com um skill personalizado é através de uma interface de Web API. A API Web necessidades têm de cumprir os requisitos descritos nesta secção.

### <a name="1--web-api-input-format"></a>1.  Formato de entrada de API Web

A API Web tem de aceitar uma matriz de registos a serem processados. Cada registo tem de conter um "conjunto de propriedades" que é a entrada fornecida para a API Web. 

Suponha que pretende criar um enricher simple que identifica a data da primeira mencionada no texto de um contrato. Neste exemplo, o skill aceita uma única entrada *contractText* como o texto de contrato. O skill também tem um resultado único, que é a data de contrato. Para tornar o enricher mais interessante, devolver *contractDate* na forma de um tipo complexo com várias parte.

A API Web deve estar pronta para receber um lote de registos de entrada. Cada membro de *valores* matriz representa a entrada de um registo específico. Cada registo é necessário ter os seguintes elementos:

+ A *recordId* membro que é o identificador único para um determinado registo. Quando o enricher devolve os resultados, deve fornecer esta *recordId* para permitir que o autor da chamada fazer corresponder os resultados de registo para a sua entrada.

+ A *dados* membro, o que é, essencialmente, uma matriz de campos de entrada para cada registo.

Para ser mais concreta, por exemplo acima, a API Web deve esperar pedidos que este aspeto:

```json
{
    "values": [
      {
        "recordId": "a1",
        "data":
           {
             "contractText": 
                "This is a contract that was issues on November 3, 2017 and that involves... "
           }
      },
      {
        "recordId": "b5",
        "data":
           {
             "contractText": 
                "In the City of Seattle, WA on February 5, 2018 there was a decision made..."
           }
      },
      {
        "recordId": "c3",
        "data":
           {
             "contractText": null
           }
      }
    ]
}
```
Na realidade, o serviço pode obter chamado com centenas ou milhares de registos em vez de apenas três mostrados aqui.

### <a name="2-web-api-output-format"></a>2. Formato de saída de API Web

O formato de saída é um conjunto de registos que contêm um *recordId*e uma matriz de propriedades 

```json
{
  "values": 
  [
      {
        "recordId": "b5",
        "data" : 
        {
            "contractDate":  { "day" : 5, "month": 2, "year" : 2018 }
        }
      },
      {
        "recordId": "a1",
        "data" : {
            "contractDate": { "day" : 3, "month": 11, "year" : 2017 }                    
        }
      },
      {
        "recordId": "c3",
        "data" : 
        {
        },
        "errors": [ { "message": "contractText field required "}   ],  
        "warnings": [ {"message": "Date not found" }  ]
      }
    ]
}
```

Este exemplo em concreto tem apenas uma saída, mas foi saída mais do que uma propriedade. 

### <a name="errors-and-warning"></a>Erros e de aviso

Como é mostrado no exemplo anterior, pode devolver o erro e mensagens de aviso para cada registo.

## <a name="consuming-custom-skills-from-skillset"></a>Consumir personalizadas competências de skillset

Quando cria um enricher Web API, pode descrevem os cabeçalhos de HTTP e os parâmetros como parte do pedido. O fragmento abaixo mostra como os parâmetros do pedido e cabeçalhos de HTTP podem ser descritos como parte da definição de skillset.

```json
{
    "skills": [
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill calls an Azure function, which in turn calls TA sentiment",
        "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/DateExtractor?language=en",
        "context": "/document",
        "httpHeaders": {
            "DateExtractor-Api-Key": "foo"
        },
        "inputs": [
          {
            "name": "contractText",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "contractDate",
            "targetName": "date"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

+ [Exemplo: Criar um skill personalizado para a API para traduzir texto](cognitive-search-create-custom-skill-example.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Criar Skillset (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Como mapear os campos avançados](cognitive-search-output-field-mapping.md)
