---
title: Método de CalcHistogram na API de conhecimento académico | Microsoft Docs
description: Utilize o método de CalcHistogram para calcular a distribuição de valores de atributo para um conjunto de entidades de papel numa serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: e0b773fb9791ee638c8cfdbbc9dca40543e50ec0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351427"
---
# <a name="calchistogram-method"></a>Método de CalcHistogram

O **calchistogram** REST API é utilizada para calcular a distribuição de valores de atributo para um conjunto de entidades do documento.          


**Ponto final REST:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>
  
## <a name="request-parameters"></a>Parâmetros do Pedido

Nome  |Valor | Necessário?  |Descrição
-----------|----------|--------|----------
**expr**    |Cadeia de texto | Sim  |Uma expressão de consulta que especifica as entidades durante o qual calcular histogramas.
**modelo** |Cadeia de texto | Não |Selecione o nome do modelo que pretende consultar.  Atualmente, o valor será assumida a *mais recente*.
**atributos** | Cadeia de texto | Não<br>predefinição: | Uma lista delimitada por vírgulas que especifica os valores de atributo que estão incluídos na resposta. Os nomes de atributos são sensíveis a maiúsculas.
**Contagem** |Number | Não<br>Predefinição: 10 |Número de resultados para devolver.
**deslocamento**  |Number | Não<br>Predefinição: 0 |Índice do primeiro resultado a devolver.
<br>
## <a name="response-json"></a>Resposta (JSON)
Nome | Descrição
--------|---------
**expr**  |O parâmetro expr do pedido.
**num_entities** | Número total de entidades correspondentes.
**histogramas** |  Uma matriz de histogramas, um para cada atributo especificado no pedido.
**.attribute histogramas [x]** | Nome do atributo durante o qual a histograma foi calculada.
**.distinct_values histogramas [x]** | Número de valores distintos entre correspondente entidades para este atributo.
**.total_count histogramas [x]** | Número total de instâncias de valor entre correspondente entidades para este atributo.
**.histogram histogramas [x]** | Dados de histograma para este atributo.
**histogramas [.value .histogram [y] x]** |  Um valor para o atributo.
**histogramas [.logprob .histogram [y] x]**  |Probabilidade de registo natural total da correspondência de entidades com este valor de atributo.
**histogramas [.count .histogram [y] x]**  |Número de entidades correspondentes com este valor de atributo.
**abortada** | TRUE se o pedido excedeu o tempo limite.

 <br>
#### <a name="example"></a>Exemplo:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Neste exemplo, para gerar um histograma da contagem de publicações anualmente para um determinado autor desde 2010, pode primeiro geramos a expressão de consulta utilizando o **interpretar** API com a cadeia de consulta: *papers por Jaime teevan após 2012*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>A expressão na interpretação primeiro que é devolvida a partir da API de interpret é *e (compostos (AA. AuN = = 'jaime teevan'), Y > 2012)*.
<br>Este valor da expressão, em seguida, é transmitido para o **calchistogram** API. O *attributes=Y,F.FN* parâmetro indica que as distribuições de contagens de documento devem ser por ano e o campo de prático, por ex.:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>A resposta a este pedido primeiro indica que existem 37 papers que corresponde à expressão de consulta.  Para o *ano* atributo, existem 3 valores distintos, um para cada ano depois de 2012 (ou seja, 2013, 2014 e 2015), conforme especificado na consulta.  A contagem de documento total sobre os valores distintos 3 é 37.  Para cada *ano*, a histograma mostra o valor, a probabilidade de registo natural total e a contagem de correspondência de entidades.     

Histograma para *campo de estudar* mostra que existem 34 campos distintos de prático. Como um documento pode ser associado a vários campos de prático, a contagem total (53) pode ser maior do que o número de entidades correspondentes.  Apesar de existirem 34 valores distintos, a resposta inclui apenas os primeiros 4 devido a *contagem = 4* parâmetro.

```JSON
{
  "expr": "And(Composite(AA.AuN=='jaime teevan'),Y>2012)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -15.753,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -15.805,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -16.035,
          "count": 10
        }
      ]
    },
    {
      "attribute": "F.FN",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -15.258,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -16.002,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -16.226,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -17.228,
          "count": 2
        }
      ]
    }
  ]
}
```
