---
title: Referência de cmdlets do PowerShell - Azure Scheduler
description: Saiba mais sobre cmdlets do PowerShell do agendador do Azure
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: a06439150ac255e7b436082ecc88702bf0c1dec1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991101"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Referência de cmdlets do PowerShell para o Azure Scheduler

> [!IMPORTANT]
> [O Azure Logic Apps](../logic-apps/logic-apps-overview.md) está a substituir o Azure Scheduler, que está a ser descontinuado. Para agendar tarefas, [Experimente o Azure Logic Apps antes](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Para criar scripts para criar e gerir tarefas do Scheduler e coleções de tarefas, pode utilizar os cmdlets do PowerShell. Este artigo lista as principais [cmdlets do PowerShell para o Azure Scheduler](/powershell/module/azurerm.scheduler) com links para os artigos de referência. Para instalar o Azure PowerShell para a sua subscrição do Azure, veja [como instalar e configurar o Azure PowerShell](/powershell/azure/overview). Para obter mais informações sobre [cmdlets do Azure Resource Manager](/powershell/azure/overview), consulte [utilizar o Azure PowerShell com o Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | Descrição |
|--------|-------------|
| [Disable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Desativa uma coleção de tarefas. |
| [Ativar AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Permite que uma coleção de tarefas. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Obtém as tarefas do Scheduler. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Obtém coleções de tarefas. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Obtém o histórico de tarefas. |
| [Novo AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Cria uma tarefa HTTP. |
| [Novo AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Cria uma coleção de tarefas. |
| [Novo AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Cria uma tarefa de fila do Service Bus. |
| [Novo AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Cria uma tarefa de tópico do Service Bus. |
| [Novo AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Cria uma tarefa da fila de armazenamento. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Remove uma tarefa do agendador. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Remove uma coleção de tarefas. |
| [Conjunto AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Modifica uma tarefa do Scheduler HTTP. |
| [Conjunto AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Modifica uma coleção de tarefas. |
| [Conjunto AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Modifica uma tarefa de fila do Service Bus. |
| [Conjunto AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Modifica uma tarefa de tópico do Service Bus. |
| [Conjunto AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Modifica uma tarefa da fila de armazenamento. |
||| 

Para obter mais detalhes, pode executar qualquer um destes cmdlets: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Consulte também

* [O que é o agendador do Azure?](scheduler-intro.md)
* [Conceitos, terminologia e hierarquia de entidades](scheduler-concepts-terms.md)
* [Criar e agendar a sua primeira tarefa - portal do Azure](scheduler-get-started-portal.md)
* [Referência da API REST do Azure Scheduler](https://msdn.microsoft.com/library/mt629143)
