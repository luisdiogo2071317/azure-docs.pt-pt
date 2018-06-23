---
title: Avaliar o método API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba como utilizar o método de Evaluate no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: fc3d73b326b565cfe40d1b82cc419357b28a801a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351662"
---
# <a name="evaluate-method"></a>avaliar o método
O *avaliar* método avalia e devolve o resultado de uma expressão de consulta estruturada baseada nos dados de índice.

Normalmente, uma expressão será obtida a partir de uma resposta para o método interpret.  Mas, pode também compor expressões de consulta por si (consulte [expressão de consulta estruturada](Expressions.md)).  

## <a name="request"></a>Pedir 
`http://<host>/evaluate?expr=<expr>&attributes=<attrs>[&<options>]`   

Nome|Valor|Descrição
----|----|----
expr       | Cadeia de texto | Expressão de consulta estruturada que seleciona um subconjunto de entidades do índice.
atributos | Cadeia de texto | Lista delimitada por vírgulas de atributos para incluir na resposta.
count      | Número (predefinição = 10) | Número máximo de resultados para devolver.
deslocamento     | Número (predefinição = 0) | Índice do primeiro resultado a devolver.
OrderBy |   Cadeia de texto | Nome do atributo utilizado para ordenar os resultados, seguidos de sequência de ordenação opcional (predefinição = asc): "*attrname*[: (asc&#124;desc)]".  Se não for especificado, os resultados são devolvidos, diminuindo a probabilidade de registo natural.
tempo limite  | Número (predefinição = 1000) | Tempo limite em milissegundos. Apenas os resultados calculados antes do tempo limite decorreu são devolvidos.

Utilizar o *contagem* e *desvio* parâmetros, um grande número de resultados pode ser obtido a incremental através de vários pedidos.
  
## <a name="response-json"></a>Resposta (JSON)
JSONPath|Descrição
----|----
$.expr | *expr* parâmetro do pedido.
$.entities | Matriz de 0 ou mais entidades de objeto correspondente a expressão de consulta structured. 
$.aborted | TRUE se o pedido excedeu o tempo limite.

Cada entidade contém um *logprob* valor e os valores dos atributos pedidos.

## <a name="example"></a>Exemplo
No exemplo académico publicações, o seguinte pedido passa uma expressão de consulta estruturada (potencialmente da saída de um *interpretar* pedido) e obtém a alguns atributos para o superior 2 entidades de correspondência:

`http://<host>/evaluate?expr=Composite(Author.Name=='jaime teevan')&attributes=Title,Y,Author.Name,Author.Id&count=2`

A resposta contém a parte superior a 2 ("contagem = 2") correspondente provavelmente entidades.  Para cada entidade, são devolvidos o título, ano, nome do autor e atributos de ID do autor.  Tenha em atenção a forma como os valores de atributos a estrutura de compostos corresponde a forma que forem especificadas no ficheiro de dados. 

```json
{
  "expr": "Composite(Author.Name=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -6.645,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1968481722
        },
        {
          "Name": "susan t dumais",
          "Id": 676500258
        },
        {
          "Name": "eric horvitz",
          "Id": 1470530979
        }
      ]
    },
    {
      "logprob": -6.764,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1982462162
        },
        {
          "Name": "christine alvarado",
          "Id": 2163512453
        },
        {
          "Name": "mark s ackerman",
          "Id": 2055132526
        },
        {
          "Name": "david r karger",
          "Id": 2012534293
        }
      ]
    }
  ]
}
```
