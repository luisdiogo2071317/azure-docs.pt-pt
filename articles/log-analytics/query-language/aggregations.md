---
title: As agregações em consultas do Log Analytics do Azure | Documentos da Microsoft
description: Descreve as funções de agregação em consultas do Log Analytics que oferecem formas úteis de analisar os seus dados.
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
ms.openlocfilehash: f72fb6f654b4699214a22a7f96431c605af52f2d
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603678"
---
# <a name="aggregations-in-log-analytics-queries"></a>Agregações em consultas do Log Analytics

> [!NOTE]
> Deve efetuar [começar com o portal do Analytics](get-started-analytics-portal.md) e [introdução às consultas](get-started-queries.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Este artigo descreve as funções de agregação em consultas do Log Analytics que oferecem formas úteis de analisar os seus dados. Estas funções todos os funcionam com o `summarize` operador que produz uma tabela com os resultados agregados da tabela de entrada.

## <a name="counts"></a>Contagens

### <a name="count"></a>count
Conte o número de linhas no resultado definida depois de todos os filtros são aplicados. O exemplo seguinte devolve o número total de linhas na _Perf_ tabela dos últimos 30 minutos. O resultado é devolvido numa coluna chamada *count_* a menos que atribua um nome específico:


```KQL
Perf
| where TimeGenerated > ago(30m) 
| summarize count()
```

```KQL
Perf
| where TimeGenerated > ago(30m) 
| summarize num_of_records=count() 
```

Uma visualização de Pego pode ser útil para ver uma tendência ao longo do tempo:

```KQL
Perf 
| where TimeGenerated > ago(30m) 
| summarize count() by bin(TimeGenerated, 5m)
| render timechart
```

O resultado deste exemplo mostra o trendline de contagem de registos de desempenho em intervalos de 5 minutos:

![Tendência de contagem](media/aggregations/count-trend.png)


### <a name="dcount-dcountif"></a>DCount, dcountif
Uso `dcount` e `dcountif` para contar valores distintos numa coluna específica. A consulta seguinte avalia como quantos computadores distintos enviam heartbeats na última hora:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcount(Computer)
```

Para contar apenas os computadores Linux que enviam heartbeats, utilize `dcountif`:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcountif(Computer, OSType=="Linux")
```

### <a name="evaluating-subgroups"></a>Avaliar subgrupos
Para executar uma contagem ou outras agregações em subgrupos nos seus dados, utilize o `by` palavra-chave. Por exemplo, para contar o número de computadores Linux distintos que enviam heartbeats a cada país:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry
```

|RemoteIPCountry  | distinct_computers  |
------------------|---------------------|
|Estados Unidos    | 19                  |
|Canadá           | 3                   |
|Irlanda          | 0                   |
|Reino Unido   | 0                   |
|Países Baixos      | 2                   |


Para analisar ainda mais pequenos subgrupos dos seus dados, adicionar nomes de coluna adicional para o `by` secção. Por exemplo, pode querer contar os computadores distintos de cada país por OSType:

```KQL
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry, OSType
```

## <a name="percentiles-and-variance"></a>Percentis e desvio
Ao avaliar os valores numéricos, uma prática comum é a média-los usando `summarize avg(expression)`. Médias são afetadas por valores extreme que caracterizam apenas alguns casos. Para resolver esse problema, pode utilizar as funções menos sensíveis como `median` ou `variance`.

### <a name="percentile"></a>Percentil
Para localizar o valor mediano, utilize o `percentile` função com um valor a especificar o percentil:

```KQL
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 50) by Computer
```

Também pode especificar diferentes percentis para obter um resultado agregado para cada:

```KQL
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 25, 50, 75, 90) by Computer
```

Isto pode mostrar que algum computador CPUs têm valores de mediano semelhante, mas algumas são constante em todo o valor mediano, outros computadores que comunicaram valores de CPU muito inferior e superiores, que significa que ocorreu picos.

### <a name="variance"></a>Variância
Para avaliar diretamente a variação de um valor, utilize os métodos de variância e o desvio padrão:

```KQL
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), variance(CounterValue) by Computer
```

Uma boa maneira para analisar a estabilidade da utilização da CPU é combinar stdev com o cálculo mediano:

```KQL
Perf
| where TimeGenerated > ago(130m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), percentiles(CounterValue, 50) by Computer
```

Consulte outras lições para utilizar a linguagem de consulta do Log Analytics:

- [Operações de cadeia de caracteres](string-operations.md)
- [Operações de data e hora](datetime-operations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [Estruturas de dados e JSON](json-data-structures.md)
- [Escrita de consultas avançado](advanced-query-writing.md)
- [Associações](joins.md)
- [Gráficos](charts.md)
