---
title: Ver registos de atividade para alterações RBAC no Azure | Microsoft Docs
description: Ver registos de atividade para alterações de controlo de acesso baseado em funções nos últimos 90 dias.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 03961de233861baf923402cc96ab8174b3233bd0
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266663"
---
# <a name="view-activity-logs-for-role-based-access-control-changes"></a>Ver registos de atividade para alterações de controlo de acesso baseado em funções

Por vezes, precisa de informações sobre as alterações (RBAC) do controlo de acesso baseado em funções, tal como para auditoria ou a resolução de problemas. Sempre que alguém fizer alterações a atribuições de funções ou as definições de função na suas subscrições, as alterações são registadas [registo de atividade do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). Pode ver os registos de atividade para ver todas as alterações RBAC nos últimos 90 dias.

## <a name="operations-that-are-logged"></a>Operações que são registadas

Seguem-se as operações relacionadas com o RBAC que são registadas no registo de atividade:

- Criar atribuição de função
- Eliminar atribuição de função
- Criar ou atualizar uma definição de função personalizada
- Eliminar definição de função personalizada

## <a name="azure-portal"></a>Portal do Azure

É a forma mais fácil começar a utilizar para ver os registos de atividade com o portal do Azure. A seguinte captura de ecrã mostra um exemplo de um registo de atividade que foi filtrado para apresentar a atribuição de função e as operações de definição de função. Também inclui uma hiperligação para transferir os registos como um ficheiro CSV.

![Registos de atividade com o portal - captura de ecrã](./media/change-history-report/activity-log-portal.png)

O registo de atividade no portal tiver vários filtros. Seguem-se os filtros de RBAC:

|Filtro  |Valor  |
|---------|---------|
|Categoria de eventos     | <ul><li>Administrativa</li></ul>         |
|Operação     | <ul><li>Criar atribuição de função</li> <li>Eliminar atribuição de função</li> <li>Criar ou atualizar uma definição de função personalizada</li> <li>Eliminar definição de função personalizada</li></ul>      |


Para obter mais informações sobre os registos de atividade, consulte [ver eventos no registo de atividade](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

Para ver registos de atividade com o Azure PowerShell, utilize o [Get-AzureRmLog](/powershell/module/azurerm.insights/get-azurermlog) comando.

Este comando apresenta uma lista de todas as alterações de atribuição de função numa subscrição nos últimos sete dias:

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Este comando apresenta uma lista de todas as alterações de definição de função num grupo de recursos nos últimos sete dias:

```azurepowershell
Get-AzureRmLog -ResourceGroupName pharma-sales-projectforecast -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Este comando apresenta uma lista de todas as alterações de definição de função numa subscrição nos últimos sete dias e atribuição de função e apresenta os resultados numa lista:

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast"}}

```

## <a name="azure-cli"></a>CLI do Azure

Para ver registos de atividade com a CLI do Azure, utilize o [lista de registo de atividade do monitor az](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) comando.

Este comando apresenta os registos de atividade num grupo de recursos, desde a hora de início:

```azurecli
az monitor activity-log list --resource-group pharma-sales-projectforecast --start-time 2018-04-20T00:00:00Z
```

Este comando apresenta os registos de atividade para o fornecedor de recursos de autorização desde a hora de início:

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-log-analytics"></a>Azure Log Analytics

[Análise de registos do Azure](../log-analytics/log-analytics-overview.md) é outra ferramenta, pode utilizar para recolher e analisar as alterações do RBAC para todos os seus recursos do Azure. Análise de registos tem as seguintes vantagens:

- Escrever consultas complexas e lógica
- Integrar com alertas, o Power BI e outras ferramentas
- Guardar os dados por períodos mais longos de retenção
- Referência cruzada com outros registos como segurança, a máquina virtual e personalizada

Eis os passos básicos para começar a utilizar:

1. [Criar uma área de trabalho de análise de registos](../log-analytics/log-analytics-quick-create-workspace.md).

1. [Configurar a solução de análise de registos de atividade](../log-analytics/log-analytics-activity.md#configuration) para a sua área de trabalho.

1. [Consulte os registos de atividade](../log-analytics/log-analytics-activity.md#using-the-solution). Uma forma rápida de navegar para a página de descrição geral da análise de registo de atividade consiste em clicar no **Log Analytics** opção.

   ![Opção de análise do registo no portal](./media/change-history-report/azure-log-analytics-option.png)

1. Opcionalmente, utilize o [pesquisa registo](../log-analytics/log-analytics-log-search.md) página ou o [portal da análise avançada](https://docs.loganalytics.io/docs/Learn) para consultar e ver os registos. Para obter mais informações sobre estas duas opções, consulte [página de pesquisa de registo ou no portal de análise avançadas](../log-analytics/log-analytics-log-search-portals.md).

Segue-se uma consulta que devolva novo atribuições de funções organizadas pelo fornecedor de recursos de destino:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Segue-se uma consulta que devolva alterações à atribuição de função apresentadas um gráfico:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Registos de atividade com o portal de análise avançadas - captura de ecrã](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Passos Seguintes
* [Ver eventos no registo de atividades](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Monitorizar a atividade de subscrição com o registo de atividade do Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
