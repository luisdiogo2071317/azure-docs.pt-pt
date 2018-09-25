---
title: Criação de gráficos e diagramas de consultas do Log Analytics do Azure | Documentos da Microsoft
description: Descreve várias visualizações no Azure Log Analytics para apresentar os dados de formas diferentes.
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
ms.openlocfilehash: e63345c0265d52fdd80fe4542efb7f13324926cf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989623"
---
# <a name="creating-charts-and-diagrams-from-log-analytics-queries"></a>Criação de gráficos e diagramas de consultas do Log Analytics

> [!NOTE]
> Deve efetuar [Advanced agregações em consultas do Log Analytics](advanced-aggregations.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Este artigo descreve várias visualizações no Azure Log Analytics para apresentar os dados de formas diferentes.

## <a name="charting-the-results"></a>Os resultados de criação de gráficos
Comece por rever como quantos computadores existem por sistema operativo, durante a última hora:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

Por predefinição, os resultados apresentam como uma tabela:

![Tabela](media/charts/table-display.png)

Para obter uma visão melhor, selecione **gráfico**e escolha o **circular** opção para visualizar os resultados:

![Gráfico circular](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Timecharts
Mostra a média, os percentis 50 º e 95 º percentis, de tempo do processador em contentores de 1 hora. A consulta gera várias séries e, em seguida, pode selecionar qual série para mostrar no gráfico de tempo:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

Selecione o **linha** opção de exibição de gráfico:

![Gráfico de linhas](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Linha de referência

Uma linha de referência pode ajudá-lo a identificar facilmente se a métrica exceder um limiar específico. Para adicionar uma linha num gráfico, expanda o conjunto de dados com uma coluna de constante:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Linha de referência](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Várias dimensões
Várias expressões no `by` cláusula de `summarize` criar várias linhas nos resultados, um para cada combinação de valores.

```Kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Quando exibir os resultados como um gráfico, ele usa a primeira coluna do `by` cláusula. O exemplo seguinte mostra um gráfico de colunas empilhadas usando o _EventID._ Tem de ser de dimensões `string` digitar, tão neste exemplo o _EventID_ é que está a ser convertido em cadeia de caracteres. 

![Gráfico de barras EventID](media/charts/charts-and-diagrams-multiDimension1.png)

Pode alternar entre ao selecionar a lista pendente com o nome da coluna. 

![Gráfico de barras AccountType](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Passos Seguintes
Consulte outras lições para utilizar a linguagem de consulta do Log Analytics:

- [Operações de cadeia de caracteres](string-operations.md)
- [Operações de data e hora](datetime-operations.md)
- [Funções de agregação](aggregations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [Estruturas de dados e JSON](json-data-structures.md)
- [Escrita de consultas avançado](advanced-query-writing.md)
- [Associações](joins.md)