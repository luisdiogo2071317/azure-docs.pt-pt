---
title: Propriedades padrão nos registos do Log Analytics do Azure Monitor | Documentos da Microsoft
description: Descreve as propriedades que são comuns a vários tipos de dados no Log Analytics do Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: c43f8acf7f9278ca412bee9ad15f2d51541cd27b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044336"
---
# <a name="standard-properties-in-log-analytics-records"></a>Propriedades padrão nos registos do Log Analytics
Dados no [do Log Analytics](../log-analytics/log-analytics-queries.md) é armazenado como um conjunto de registos, cada um com um tipo de dados específico que tenha um conjunto exclusivo de propriedades. Vários tipos de dados terá propriedades padrão que são comuns em vários tipos. Este artigo descreve essas propriedades e fornece exemplos de como pode usá-los em consultas.

Algumas dessas propriedades são ainda estiver no processo que está a ser implementado, pelo que poderá ver em alguns tipos de dados, mas ainda não em outros.

## <a name="timegenerated"></a>TimeGenerated
O **TimeGenerated** propriedade contém a data e hora em que o registo foi criado. Ele fornece uma propriedade comuns a utilizar para filtragem ou resumir por hora. Quando seleciona um intervalo de tempo para uma vista ou dashboard no portal do Azure, ele usa TimeGenerated para filtrar os resultados.

### <a name="examples"></a>Exemplos

A seguinte consulta devolve o número de eventos de erro criadas para cada dia da semana anterior.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

## <a name="type"></a>Tipo
O **tipo** propriedade contém o nome da tabela que o registo foi obtido a partir da qual pode também ser considerado como o tipo de registo. Esta propriedade é útil para consultas que combinam registos de múltiplas tabelas, tais como os que utilizam o `search` operador, para distinguir entre os registros de diferentes tipos. **$table** pode ser utilizado em vez de **tipo** em alguns locais.

### <a name="examples"></a>Exemplos
A seguinte consulta devolve a contagem de registos por tipo recolhido durante a última hora.

```Kusto
search * 
| where TimeGenerated > ago(1h) 
| summarize count() by Type 
```

## <a name="resourceid"></a>\_ResourceId
O  **\_ResourceId** propriedade contém um identificador exclusivo para o recurso que o registo está associado. Isto dá-lhe uma propriedade padrão para utilizar para definir o âmbito sua consulta para apenas os registos de um recurso específico ou para associar dados relacionados em várias tabelas.

Para obter recursos do Azure, o valor de **_ResourceId** é o [URL de ID de recurso do Azure](../azure-resource-manager/resource-group-template-functions-resource.md). A propriedade está limitada aos recursos do Azure, mas ele será expandido para recursos fora do Azure, tais como computadores no local.

> [!NOTE]
> Alguns tipos de dados já tem campos que contêm o ID de recurso do Azure ou, pelo menos, partes do mesmo, como o ID de subscrição. Embora estes campos são mantidos para compatibilidade com versões anteriores, recomenda-se para utilizar o _ResourceId para efetuar a correlação cruzada, uma vez que é mais consistente.

### <a name="examples"></a>Exemplos
A consulta seguinte associa os dados de desempenho e eventos para cada computador. Mostra todos os eventos com o ID _101_ e mais de 50% de utilização do processador.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

A seguinte consulta associações _AzureActivity_ regista com _SecurityEvent_ registos. Mostra todas as operações de atividade com utilizadores que foram registadas para essas máquinas.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre como [dados do Log Analytics são armazenados](../log-analytics/log-analytics-queries.md).
- Obtenha uma lição sobre [escrevem consultas no Log Analytics](../log-analytics/query-language/get-started-queries.md).
- Obtenha uma lição sobre [associar tabelas em consultas do Log Analytics](../log-analytics/query-language/joins.md).
