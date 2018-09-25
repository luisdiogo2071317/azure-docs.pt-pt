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
ms.date: 08/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 663f0b04c528c180e4130c1c157441cbc0ceb98b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955872"
---
# <a name="standard-properties-in-log-analytics-records"></a>Propriedades padrão nos registos do Log Analytics
Dados no [do Log Analytics](../log-analytics/log-analytics-queries.md) é armazenado como um conjunto de registos, cada um com um tipo de dados específico que tenha um conjunto exclusivo de propriedades. Vários tipos de dados terá propriedades padrão que são comuns em vários tipos. Este artigo descreve essas propriedades e fornece exemplos de como pode usá-los em consultas.

Algumas dessas propriedades são ainda estiver no processo que está a ser implementado, pelo que poderá ver em alguns tipos de dados, mas ainda não em outros.


## <a name="resourceid"></a>_ResourceId
O **_ResourceId** propriedade contém um identificador exclusivo para o recurso que o registo está associado. Isto dá-lhe uma propriedade padrão para utilizar para definir o âmbito sua consulta para apenas os registos de um recurso específico ou para associar dados relacionados em várias tabelas.

Para obter recursos do Azure, o valor de **_ResourceId** é o [URL de ID de recurso do Azure](../azure-resource-manager/resource-group-template-functions-resource.md). A propriedade está limitada aos recursos do Azure, mas ele será expandido para recursos fora do Azure, tais como computadores no local. 

### <a name="examples"></a>Exemplos
O exemplo seguinte mostra uma consulta que associa o desempenho e dados de eventos para cada computador. Mostra todos os eventos com o ID _101_ e mais de 50% de utilização do processador.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

O exemplo seguinte mostra uma consulta que associada _AzureActivity_ regista com _SecurityEvent_ registos. Mostra todas as operações de atividade com utilizadores que foram registadas para essas máquinas.

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