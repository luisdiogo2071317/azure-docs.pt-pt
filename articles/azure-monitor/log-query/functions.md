---
title: Funções do Log Analytics do Azure | Documentos da Microsoft
description: Este artigo descreve como utilizar as funções para chamar uma consulta a partir de outra consulta do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: 8f2855ed56d298ec4c6abee02dd59ce9471f0d2e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882852"
---
# <a name="using-functions-in-azure-monitor-log-analytics"></a>Com as funções no Log Analytics do Azure Monitor

> [!NOTE]
> Deve efetuar [começar com o portal do Analytics](get-started-portal.md) e [introdução às consultas](get-started-queries.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]


Para utilizar uma consulta do Log Analytics com outra consulta pode guardá-lo como uma função. Isto permite-lhe simplificar consultas complexas, ao dividi-las em partes e permite-lhe reutilizar código comum com várias consultas.

## <a name="create-a-function"></a>Criar uma função

Criar uma função no portal do Azure ao clicar em **guardar** e, em seguida, fornecer as informações na tabela seguinte.

| Definição | Descrição |
|:---|:---|
| Nome           | Nome a apresentar para a consulta no **Explorador de consultas**. |
| Guardar como        | Função |
| Alias de Função | Nome abreviado para usar a função em outras consultas. Não pode conter espaços e tem de ser exclusivo. |
| Categoria       | Uma categoria para organizar consultas guardadas e funções no **Explorador de consultas**. |

> [!NOTE]
> Uma função no Log Analytics não pode conter outra função.

> [!NOTE]
> A guardar uma função é possível, em consultas do Log Analytics, mas, atualmente, não para consultas do Application Insights.



## <a name="use-a-function"></a>Utilizar uma função
Utilize uma função, incluindo o respetivo alias na outra consulta. Ele pode ser usado como qualquer outra tabela.

## <a name="example"></a>Exemplo
A consulta de exemplo seguinte devolve todas as atualizações de segurança em falta comunicadas no último dia. Guardar esta consulta como uma função com o alias _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Criar outra consulta e uma referência a _security_updates_last_day_ função para procurar atualizações de segurança necessário relacionados com SQL.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Passos Seguintes
Consulte outras lições para utilizar a linguagem de consulta do Log Analytics:

- [Operações de cadeia de caracteres](string-operations.md)
- [Operações de data e hora](datetime-operations.md)
- [Funções de agregação](aggregations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [Estruturas de dados e JSON](json-data-structures.md)
- [Associações](joins.md)
- [Gráficos](charts.md)
