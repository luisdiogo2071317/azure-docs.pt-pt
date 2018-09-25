---
title: Trabalhar com valores de hora da data em consultas do Log Analytics do Azure | Documentos da Microsoft
description: Descreve como trabalhar com dados de data e hora em consultas do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 9b0c58fdbfb0d55b3b8998f4edfc1222b9a3d4aa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988603"
---
# <a name="working-with-date-time-values-in-log-analytics-queries"></a>Trabalhar com valores de hora da data em consultas do Log Analytics

> [!NOTE]
> Deve efetuar [começar com o portal do Analytics](get-started-analytics-portal.md) e [introdução às consultas](get-started-queries.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Este artigo descreve como trabalhar com dados de data e hora em consultas do Log Analytics.


## <a name="date-time-basics"></a>Noções básicas de tempo de data
A linguagem de consulta do Log Analytics tem dois tipos de dados principal associados com datas e horas: datetime e período de tempo. Todas as datas são expressos em UTC. Embora vários formatos de datetime sejam suportados, o formato de ISO8601 é preferencial. 

Períodos de tempo são expressos como um valor decimal seguido de uma unidade de tempo:

|forma abreviada   | unidade de tempo    |
|:---|:---|
|d           | dia          |
|h           | hora         |
|m           | minuto       |
|s           | segundo       |
|ms          | milissegundo  |
|microssegundo | microssegundo  |
|escala        | nanossegundos   |

Datetimes podem ser criadas por lançando uma cadeia, utilizando o `todatetime` operador. Por exemplo, para rever os heartbeats VM enviados num período de tempo específico, pode fazer uso do [entre operador](https://docs.loganalytics.io/docs/Language-Reference/Scalar-operators/between-operator) que é útil especificar um intervalo de tempo....

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Outro cenário comum é comparar um datetime ao presente. Por exemplo, para ver todos os heartbeats durante os últimos dois minutos, pode usar o `now` operador em conjunto com um intervalo de tempo que representa os dois minutos:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

Um atalho também está disponível para esta função:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

O método mais curto e mais legível que está a utilizar o `ago` operador:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Suponha que, em vez de saber o tempo de início e de fim, sabe que a hora de início e a duração. Pode reescrever a consulta da seguinte forma:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Conversão de unidades de tempo
Pode ser útil expressar um datetime ou timespan numa unidade de tempo diferente da predefinição um. Por exemplo, suponha que está revisando eventos de erro dos últimos 30 minutos e precisar de uma coluna calculada que mostra que há quanto tempo o evento ter acontecido:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

Pode ver o _timeAgo_ coluna contém valores, tais como: "00:09:31.5118992", que significa que eles são formatados como hh:mm:ss.fffffff. Se pretender formatar estes valores para o _numver_ de minutos desde a hora de início, basta dividir esse valor por "1 minuto":

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>As agregações e bucketing por intervalos de tempo
Outro cenário muito comum é a necessidade de obter as estatísticas sobre um determinado período de tempo num grão de tempo específico. Para isso, um `bin` operador pode ser usado como parte de uma cláusula de summarize.

Utilize a seguinte consulta para obter o número de eventos que ocorreram durante a última meia hora a cada 5 minutos:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Isso produz a seguinte tabela:  
|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Outra forma de criar registos de resultados é usar as funções, tais como `startofday`:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Isso produz os seguintes resultados:

|carimbo de data/hora|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416  |


## <a name="time-zones"></a>Fusos horários
Uma vez que todos os valores de datetime são expressos em UTC, muitas vezes é útil para convertê-los para o fuso horário local. Por exemplo, utilize este cálculo para converter UTC PST vezes:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>Funções relacionadas

| Categoria | Função |
|:---|:---|
| Converter tipos de dados | [ToDateTime](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/todatetime())[totimespan](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/totimespan())  |
| Valor arredonda para o tamanho de discretização | [Bin](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/bin()) |
| Obtenha uma data específica ou a hora | [há](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago()) [agora](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/now())   |
| Obtenha a parte do valor | [datetime_part](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/datetime_part()) [getmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getmonth()) [monthofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/monthofyear()) [getyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getyear()) [dayofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofmonth()) [dayofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofweek()) [dayofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofyear()) [weekofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/weekofyear()) |
| Obtenha uma data em relação ao valor  | [endofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofday()) [endofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofweek()) [endofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofmonth()) [endofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofyear()) [startofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofday()) [startofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofweek()) [startofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofmonth()) [startofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofyear()) |

## <a name="next-steps"></a>Passos Seguintes
Consulte outras lições para utilizar a linguagem de consulta do Log Analytics:

- [Operações de cadeia de caracteres](string-operations.md)
- [Funções de agregação](aggregations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [Estruturas de dados e JSON](json-data-structures.md)
- [Escrita de consultas avançado](advanced-query-writing.md)
- [Associações](joins.md)
- [Gráficos](charts.md)