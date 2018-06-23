---
title: Referência de datetimeV2 entidades LUIS Prebuilt - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo tem datetimeV2 informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 261f6f27c39c280efdcd070888d735374a473c85
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321891"
---
# <a name="datetimev2-entity"></a>Entidade DatetimeV2

O **datetimeV2** entidade prebuilt extrai os valores de data e hora. Resolva estes valores num formato padronizado para programas de cliente consumir. Quando um utterance tem uma data ou hora não está concluída, LUIS inclui _passado e valores futuros_ na resposta do ponto final. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém datetimeV2 para pendentes a aplicação. 

## <a name="types-of-datetimev2"></a>Tipos de datetimeV2
DatetimeV2 é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml) repositório do Github

## <a name="example-json"></a>JSON de exemplo 
O exemplo seguinte resposta JSON tem um `datetimeV2` entidade com um subtipo de `datetime`. Para obter exemplos de outros tipos de entidades datetimeV2, consulte [subtipos de datetimeV2](#subtypes-of-datetimev2)</a>.

```JSON
"entities": [
  {
    "entity": "8am on may 2nd 2017",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 15,
    "endIndex": 30,
    "resolution": {
      "values": [
        {
          "timex": "2017-05-02T08",
          "type": "datetime",
          "value": "2017-05-02 08:00:00"
        }
      ]
    }
  }
]
  ```

## <a name="json-property-descriptions"></a>Descrições das propriedades JSON

|Nome da propriedade |Tipo de propriedade e descrição|
|---|---|
|Entidade|**cadeia** -texto extraído de utterance com o tipo de data, hora, intervalo de datas ou um intervalo de tempo.|
|tipo|**cadeia** - um do [subtipos de datetimeV2](#subtypes-of-datetimev2)
|startIndex|**Int** -o índice do utterance que começa a entidade.|
|endIndex|**Int** -o índice do utterance que termina a entidade.|
|resolução|Tem um `values` matriz que tenha um, dois ou quatro [valores da resolução](#values-of-resolution).|
|end|O valor final de uma hora ou intervalo de datas no mesmo formato como `value`. Utilizado apenas se `type` é `daterange`, `timerange`, ou `datetimerange`|

## <a name="subtypes-of-datetimev2"></a>Subtipos de datetimeV2

O **datetimeV2** prebuilt entidade tem os seguintes subtipos e exemplos de cada são fornecidos na tabela que se segue:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Valores de resolução
* A matriz tem um elemento se a data ou hora no utterance é totalmente especificado e inequívoca.
* A matriz tem dois elementos, se o valor de datetimeV2 é ambíguo. Ambiguidade inclui falta de ano específico, hora ou intervalo de tempo. Consulte [datas ambíguas](#ambiguous-dates) para obter exemplos. Quando o tempo é ambíguo para A.M. ou 00h, ambos os valores estão incluídos.
* A matriz não tem quatro elementos se o utterance tem dois elementos com ambiguidade. Esta ambiguidade inclui elementos com:
  * Uma data ou um intervalo de datas que é ambíguo relativamente ano
  * Uma hora ou intervalo de tempo que é ambíguo relativamente A.M. ou 00h Por exemplo, 3rd de Abril 3:00.

Cada elemento do `values` matriz pode ter os seguintes campos: 

|Nome da propriedade|Descrição de propriedade|
|--|--|
|Timex|hora, data ou intervalo de datas expressado no formato TIMEX que se segue o [norma ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e os atributos de TIMEX3 utilizando a linguagem de TimeML de anotação. Este anotação está descrita com os [diretrizes TIMEX](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf).|
|tipo|O subtipo, que pode ser um dos seguintes itens: datetime, data, hora, daterange, timerange, datetimerange, duração, conjunto.|
|valor|**Opcional.** Um objeto de datetime yyyy:MM:dd formato (data), hh: mm: (hora) yyyy:MM:dd hh: mm: (datetime). Se `type` é `duration`, o valor é o número de segundos (duração) <br/> Utilizado apenas se `type` é `datetime` ou `date`, `time`, ou ' duração.|

## <a name="valid-date-values"></a>Valores de data válido

O **datetimeV2** suporta datas entre os seguintes intervalos de:

| Mín. | Máx. |
|----------|-------------|
| 1 de Janeiro de 1900   | 31 de Dezembro de 2099 |

## <a name="ambiguous-dates"></a>Datas ambíguas

Se a data pode pertencer ao passado ou futuras, LUIS fornece ambos os valores. Um exemplo é um utterance que inclui as datas sem o ano e mês.  

Por exemplo, dada utterance "2nd de Maio":
* Se a data de hoje for 3rd de Maio de 2017, LUIS fornece "2017-05-02" e "2018-05-02" como valores. 
* Quando a data de hoje é 1ª de Maio de 2017, LUIS fornece "2016-05-02" e "2017-05-02" como valores.

O exemplo seguinte mostra a resolução da entidade "2nd de Maio". Esta resolução assume que data de hoje uma data entre 2nd de Maio de 2017 e de 2018 1ª de pode.
Campos com `X` no `timex` campo são partes da data que não são especificadas explicitamente no utterance.

```JSON
  "entities": [
    {
      "entity": "may 2nd",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2017-05-02"
          },
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2018-05-02"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-numeric-date"></a>Exemplos de resolução de intervalo de datas para data numérico

O `datetimeV2` entidade extrai os intervalos de data e hora. O `start` e `end` fields, especifique o início e fim do intervalo. Para utterance "2nd de Maio para 5th de Maio", fornece LUIS **daterange** valores para o ano atual e o ano seguinte. No `timex` campo, o `XXXX` valores indicam a ambiguidade do ano. `P3D` indica o período de tempo muito de três dias.

```JSON
"entities": [
    {
      "entity": "may 2nd to may 5th",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 17,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2017-05-02",
            "end": "2017-05-05"
          },
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2018-05-02",
            "end": "2018-05-05"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-day-of-week"></a>Exemplos de resolução de intervalo de datas para o dia da semana

O exemplo seguinte mostra como o utiliza LUIS **datetimeV2** para resolver o utterance "Terça-feira a quinta-feira". Neste exemplo, a data atual for 19 de Junho. Inclui LUIS **daterange** valores para os dois intervalos de data que preceder e siga a data atual.

```JSON
  "entities": [
    {
      "entity": "tuesday to thursday",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 19,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-13",
            "end": "2017-06-15"
          },
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-20",
            "end": "2017-06-22"
          }
        ]
      }
    }
  ]
```
## <a name="ambiguous-time"></a>Hora ambígua
A matriz de valores tem dois elementos de tempo se a hora ou intervalo de tempo é ambíguo. Quando existe um período de tempo ambíguo, os valores têm ambas as A.M. e 00h vezes.

## <a name="time-range-resolution-example"></a>Exemplo de resolução de intervalo de tempo

O exemplo seguinte mostra como o utiliza LUIS **datetimeV2** para resolver o utterance que tem um intervalo de tempo.

```
  "entities": [
    {
      "entity": "6pm to 7pm",
      "type": "builtin.datetimeV2.timerange",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          {
            "timex": "(T18,T19,PT1H)",
            "type": "timerange",
            "start": "18:00:00",
            "end": "19:00:00"
          }
        ]
      }
    }
  ]
```

## <a name="deprecated-prebuilt-datetime"></a>Datetime prebuilt preterido

O `datetime` prebuilt entidade é preterida e substituída pelo [ `datetimeV2` ](#builtindatetimev2). 

Para substituir `datetime` com `datetimeV2` na sua aplicação LUIS, conclua os seguintes passos:

1. Abra o **entidades** painel da interface de web LUIS. 
2. Eliminar o **datetime** prebuilt entidade.
3. Clique em **adicionar prebuilt entidade**
4. Selecione **datetimeV2** e clique em **guardar**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [dimensão](luis-reference-prebuilt-dimension.md), [e-mail](luis-reference-prebuilt-email.md) entidades, e [número](luis-reference-prebuilt-number.md). 

