---
title: Método de CalcHistogram na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba como utilizar o método CalcHistogram no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 6ed694b0cc9cf41b815cc54b9f6d12adb2b7cd64
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351625"
---
# <a name="calchistogram-method"></a>calchistogram método
O *calchistogram* método calcula os objetos que correspondam uma expressão de consulta estruturada e calcula a distribuição os respetivos valores de atributo.

## <a name="request"></a>Pedir
`http://<host>/calchistogram?expr=<expr>[&options]` 

Nome|Valor|Descrição
----|-----|-----------
expr | Cadeia de texto | Expressão de consulta estruturada que especifica as entidades de índice através do qual calcular histogramas.
atributos | Cadeia de texto (predefinição = "") | Lista delimitada por vírgulas de atributo incluído na resposta.
count   | Número (predefinição = 10) | Número de resultados para devolver.
deslocamento  | Número (predefinição = 0) | Índice do primeiro resultado a devolver.

## <a name="response-json"></a>Resposta (JSON)
JSONPath | Descrição
----|----
$.expr | *expr* parâmetro do pedido.
$.num_entities | Número total de entidades correspondentes.
$.histograms |  Matriz de histogramas, um para cada atributo pedido.
$.histograms [\*] .attribute | Nome do atributo durante o qual a histograma foi calculada.
$.histograms [\*] .distinct_values | Número de valores distintos entre correspondente entidades para este atributo.
$.histograms [\*] .total_count | Número total de instâncias de valor entre correspondente entidades para este atributo.
$.histograms [\*] .histogram | Dados de histograma para este atributo.
$.histograms [\*] .histogram [\*] .value | Valor de atributo.
$.histograms [\*] .histogram [\*] .logprob  | Probabilidade de registo natural total da correspondência de entidades com este valor de atributo.
$.histograms [\*] .histogram [\*] .count    | Número de entidades correspondentes com este valor de atributo.
$.aborted | TRUE se o pedido excedeu o tempo limite.

### <a name="example"></a>Exemplo
No exemplo académico publicações, o seguinte calcula um histograma de contagens de publicação por ano e palavra-chave para um determinado autor desde 2013:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

A resposta indica que existem 37 papers correspondência de expressão de consulta.  Para o *ano* atributo, existem 3 valores distintos, um para cada ano desde 2013.  A contagem de documento total sobre os valores distintos 3 é 37.  Para cada *ano*, a histograma mostra o valor, a probabilidade de registo natural total e a contagem de correspondência de entidades.     

Histograma para *palavra-chave* mostra que existem 34 palavras-chave de distintas. Como um documento pode ser associado com várias palavras-chave, a contagem total (53) pode ser maior do que o número de entidades correspondentes.  Apesar de existirem 34 valores distintos, a resposta inclui apenas os primeiros 4 devido a "contagem = 4" parâmetro.

```json
{
  "expr": "And(Composite(Author.Name=='jaime teevan'),Y>=2013)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -6.894,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -6.927,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -7.082,
          "count": 10
        }
      ]
    },
    {
      "attribute": "Keyword",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -7.142,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -7.389,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -7.623,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -7.674,
          "count": 2
        }
      ]
    }
  ]
}
``` 
