---
title: expressão de App() na consulta do Log Analytics do Azure | Documentos da Microsoft
description: A expressão de aplicação é utilizada numa consulta do Log Analytics para recuperar dados de uma aplicação específica do Application Insights no mesmo grupo de recursos, outro grupo de recursos ou outra subscrição.
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
ms.topic: article
ms.date: 09/10/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: dda137ecba91e3ac59be635eed22d18e5bcadcce
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277312"
---
# <a name="app-expression-in-log-analytics-query"></a>expressão de App() na consulta do Log Analytics

O `app` expressão é utilizada numa consulta do Log Analytics para recuperar dados de uma aplicação específica do Application Insights no mesmo grupo de recursos, outro grupo de recursos ou outra subscrição. Isto é útil para incluir dados de aplicativos numa consulta do Log Analytics e para consultar dados com os vários aplicativos numa consulta do Application Insights.



## <a name="syntax"></a>Sintaxe

`app(`*Identificador*`)`


## <a name="arguments"></a>Argumentos

- *Identificador*: identifica a aplicação utilizando um dos formatos da tabela abaixo.

| Identificador | Descrição | Exemplo
|:---|:---|:---|
| Nome do Recurso | Nome legível por humano da aplicação (também conhecidas como "nome do componente") | App("fabrikamapp") |
| Nome qualificado | Nome completo da aplicação no formato: "subscriptionName/resourceGroup/componentName" | App('AI-Prototype/Fabrikam/fabrikamapp') |
| ID | GUID da aplicação | App("988ba129-363e-4415-8fe7-8cbab5447518") |
| ID de recurso do Azure | Identificador para o recurso do Azure |App("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/Providers/Microsoft.insights/Components/fabrikamapp") |


## <a name="notes"></a>Notas

* Tem de ter acesso de leitura para a aplicação.
* Identificar um aplicativo pelo respetivo nome de parte do princípio de que é exclusivo em todas as subscrições acessíveis. Se tiver várias aplicações com o nome especificado, a consulta irá falhar devido a ambiguidade. Neste caso tem de utilizar um dos outros identificadores.
* Utilizar a expressão relacionada [área de trabalho](workspace-expression.md) a consulta em áreas de trabalho do Log Analytics.

## <a name="examples"></a>Exemplos

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Passos Seguintes

- Consulte a [expressão de área de trabalho](workspace-expression.md) para se referir à área de trabalho do Log Analytics.
- Saiba mais sobre como [dados do Log Analytics](../../log-analytics/log-analytics-queries.md) são armazenados.