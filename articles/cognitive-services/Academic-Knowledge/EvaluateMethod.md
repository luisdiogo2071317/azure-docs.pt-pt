---
title: Avalie o método - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Utilize o método Evaluate para retornar um conjunto de entidades académicas com base numa expressão de consulta.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 262beeefbbafefc95da51e9f4afcbc1bc143f952
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902336"
---
# <a name="evaluate-method"></a>Avalie o método

O **avaliar** REST API é utilizada para devolver um conjunto de entidades académicas com base numa expressão de consulta.
<br>

**Ponto final REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>
## <a name="request-parameters"></a>Parâmetros do Pedido  
Nome     | Valor | Necessário?  | Descrição
-----------|-----------|---------|--------
**expr**       | Cadeia de texto | Sim | Uma expressão de consulta que especifica quais entidades devem ser devolvidas.
**Modelo**      | Cadeia de texto | Não  | Nome do modelo que pretende consultar.  Atualmente, o valor é predefinido para *mais recente*.        
**Atributos** | Cadeia de texto | Não<br>predefinição: Id | Uma lista delimitada por vírgulas que especifica os valores de atributos que estão incluídos na resposta. Nomes de atributo diferenciam maiúsculas de minúsculas.
**Contagem**        | Number | Não<br>Predefinição: 10 | Número de resultados a devolver.
**deslocamento**     | Number |   Não<br>Predefinição: 0    | Índice do primeiro resultado a devolver.
**OrderBy** |   Cadeia de texto | Não<br>Predefinição:, diminuindo prob | Nome de um atributo que é utilizado para ordenação as entidades. Opcionalmente, ascendente/descendente pode ser especificado. O formato é: *name: asc* ou *nome: desc*.
  
 <br>
## <a name="response-json"></a>Resposta (JSON)
Nome | Descrição
-------|-----   
**expr** |  O *expr* parâmetro no pedido.
**Entidades** |  Uma matriz de 0 ou mais entidades que correspondem a expressão de consulta. Cada entidade contém um valor de probabilidade de registo natural e os valores de outros atributos solicitados.
**abortada** | VERDADEIRO se o pedido excedeu o limite de tempo.

<br>
#### <a name="example"></a>Exemplo:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Normalmente, uma expressão será obtida a partir de uma resposta para o **interpretar** método.  Mas também é possível compor expressões de consulta por conta própria (consulte [sintaxe de expressão de consulta](QueryExpressionSyntax.md)).  
  
Utilizar o *contagem* e *deslocamento* parâmetros, um grande número de resultados pode ser obtido sem enviar um único pedido que a resposta resultados numa enorme (e potencialmente lenta).  Neste exemplo, o pedido de utilizado a expressão para a interpretação primeiro do **interpretar** resposta de API, como o *expr* valor. O *contagem = 2* parâmetro especifica que estão a ser requeridos 2 resultados de entidade. E o *atributos = te, Y, CC, AA. AuN, AA. AuId* parâmetro indica que o título, ano, contagem de citação, nome do autor e ID do autor são solicitados para cada resultado.  Ver [atributos de entidade](EntityAttributes.md) para obter uma lista de atributos.
  
```JSON
{
  "expr": "Composite(AA.AuN=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -15.08,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "CC": 372,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1968481722
        },
        {
          "AuN": "susan t dumais",
          "AuId": 676500258
        },
        {
          "AuN": "eric horvitz",
          "AuId": 1470530979
        }
      ]
    },
    {
      "logprob": -15.389,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "CC": 237,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1982462162
        },
        {
          "AuN": "christine alvarado",
          "AuId": 2163512453
        },
        {
          "AuN": "mark s ackerman",
          "AuId": 2055132526
        },
        {
          "AuN": "david r karger",
          "AuId": 2012534293
        }
      ]
    }
  ]
}
 ```
 
