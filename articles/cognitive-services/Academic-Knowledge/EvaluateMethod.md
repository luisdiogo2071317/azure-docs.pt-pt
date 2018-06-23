---
title: Avaliar o método API de conhecimento académico | Microsoft Docs
description: Utilize o método de Evaluate para devolver um conjunto de entidades académicas baseado numa expressão de consulta em serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 3005ae1f6df042a49db086de4982d8206f6938a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351385"
---
# <a name="evaluate-method"></a>avaliar o método

O **avaliar** REST API é utilizada para devolver um conjunto de entidades académicas baseado numa expressão de consulta.
<br>

**Ponto final REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>
## <a name="request-parameters"></a>Parâmetros do Pedido  
Nome     | Valor | Necessário?  | Descrição
-----------|-----------|---------|--------
**expr**       | Cadeia de texto | Sim | Uma expressão de consulta que especifica que as entidades devem ser devolvidas.
**modelo**      | Cadeia de texto | Não  | Nome do modelo que pretende consultar.  Atualmente, o valor será assumida a *mais recente*.        
**atributos** | Cadeia de texto | Não<br>predefinição: Id | Uma lista delimitada por vírgulas que especifica os valores de atributo que estão incluídos na resposta. Os nomes de atributos são sensíveis a maiúsculas.
**Contagem**        | Number | Não<br>Predefinição: 10 | Número de resultados para devolver.
**deslocamento**     | Number |   Não<br>Predefinição: 0    | Índice do primeiro resultado a devolver.
**OrderBy** |   Cadeia de texto | Não<br>Predefinição:, diminuindo prob | Nome de um atributo que é utilizado para ordenar as entidades. Opcionalmente, ascendente/descendente pode ser especificado. O formato é: *name: asc* ou *name: desc*.
  
 <br>
## <a name="response-json"></a>Resposta (JSON)
Nome | Descrição
-------|-----   
**expr** |  O *expr* parâmetro do pedido.
**Entidades** |  Uma matriz de 0 ou mais entidades que corresponde à expressão de consulta. Cada entidade contém um valor de probabilidade do registo natural e os valores de outros atributos pedidos.
**abortada** | TRUE se o pedido excedeu o tempo limite.

<br>
#### <a name="example"></a>Exemplo:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Normalmente, uma expressão irá obter uma resposta para o **interpretar** método.  Mas, pode também compor expressões de consulta por si (consulte [sintaxe de expressão de consulta](QueryExpressionSyntax.md)).  
  
Utilizar o *contagem* e *desvio* parâmetros, pode ser obtido um grande número de resultados sem enviar um pedido único essa resposta de resultados uma enorme (e potencialmente lenta).  Neste exemplo, o pedido utilizado a expressão para a interpretação do primeiro o **interpretar** resposta da API como o *expr* valor. O *contagem = 2* parâmetro especifica que estão a ser pedidos a 2 resultados de entidade. E o *atributos = Ti, Y, CC, AA. AuN, AA. AuId* parâmetro indica que o título, ano, contagem de citação, nome do autor e ID do autor são pedidos para cada resultado.  Consulte [entidade atributos](EntityAttributes.md) para obter uma lista de atributos.
  
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
 
