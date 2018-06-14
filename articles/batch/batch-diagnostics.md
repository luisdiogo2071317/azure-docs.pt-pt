---
title: Métricas, alertas e registos de diagnóstico para o Azure Batch | Microsoft Docs
description: Registar e analisar eventos de registo de diagnóstico para recursos da conta do Azure Batch, como conjuntos e tarefas.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 04/05/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: e64d272695c4e47c972df040d1c1c2a63bf3dddd
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31788199"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Métricas de batch, alertas e registos para a avaliação de diagnóstico e monitorização

Este artigo explica como monitorizar uma conta do Batch utilizar funcionalidades do [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Monitor do Azure recolhe [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) e [os registos de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) para recursos na sua conta do Batch. Recolher e consumir estes dados de diversas formas de monitorizar a sua conta do Batch e diagnosticar problemas. Também pode configurar [alertas métricas](../monitoring-and-diagnostics/monitoring-overview-alerts.md#alerts-on-azure-monitor-data) para receber notificações quando uma métrica de atinge um valor especificado. 

## <a name="batch-metrics"></a>Métricas de batch

As métricas são dados de telemetria do Azure (também denominados de contadores de desempenho) emitidos pelos recursos do Azure que são consumidos pelo serviço de monitorização do Azure. As métricas de exemplo numa conta do Batch incluem: eventos de criar conjunto, número de nós de prioridade baixa e eventos de conclusão de tarefas. 

Consulte o [lista das métricas de Batch suportadas](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftbatchbatchaccounts).

As métricas são:

* Ativado por predefinição em cada conta do Batch sem configuração adicional
* Gerados a cada 1 minuto
* Não são persistidos automaticamente, mas tem um histórico de graduais de 30 dias. Pode manter as métricas de atividade como parte da [registo diagnóstico](#work-with-diagnostic-logs).

### <a name="view-metrics"></a>Veja as métricas

Veja as métricas para a sua conta do Batch no portal do Azure. O **descrição geral** página para a conta por predefinição apresenta nó chave, núcleos e métricas de tarefas. 

Para ver as métricas de conta do Batch: 

1. No portal, clique em **todos os serviços** > **contas do Batch**e, em seguida, clique no nome da sua conta do Batch.
2. Em **monitorização**, clique em **métricas**.
3. Selecione um ou mais das métricas. Se quiser, selecionar métricas de recurso adicionais utilizando o **subscrições**, **grupo de recursos**, **tipo de recurso**, e **recursos** DropDowns.

    ![Métricas de batch](media/batch-diagnostics/metrics-portal.png)

Para obter métricas programaticamente, utilize as APIs de Monitor do Azure. Por exemplo, consulte [métricas de obter Monitor do Azure com o .NET](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

## <a name="batch-metric-alerts"></a>Alertas de métricas de batch

Opcionalmente, configure praticamente em tempo real *alertas métricas* que acionar quando o valor de uma métrica especificado atravesse um limiar que atribuir. Gera o alerta um [notificação](../monitoring-and-diagnostics/insights-alerts-portal.md) escolha quando o alerta é "ativado" (quando o limiar for ultrapassado e a condição de alerta for cumprida), bem como quando for "resolvida" (quando o limiar é cruzado novamente e a condição é nenhuma já está preenchido). 

Por exemplo, pode querer configurar um alerta de métrico quando a contagem de núcleos de baixa prioridade atingir um para um determinado nível, pelo que pode ajustar a composição dos seus conjuntos.

Para configurar um alerta de métrico no portal:

1. Clique em **Todos os serviços** > **Contas do Batch**e, em seguida, clique no nome da sua conta do Batch.
2. Em **monitorização**, clique em **regras de alerta** > **Adicionar alerta métrica**.
3. Selecione uma métrica, uma condição de alerta (por exemplo, quando uma métrica excede um valor específico durante um período) e uma ou mais notificações.

Também pode configurar um breve em tempo real alertas utilizando o [REST API](). Para obter mais informações, consulte [utilizar os alertas de métricos mais recentes para serviços do Azure no portal do Azure](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md)
## <a name="batch-diagnostics"></a>Diagnóstico de batch

Os registos de diagnóstico contêm informações emitidas pelo recursos do Azure que descrevem a operação de cada recurso. Lote, é possível recolher os seguintes registos:

* **Registos do serviço** eventos emitidos pelo serviço Azure Batch durante a duração de um recurso de Batch individuais como um conjunto ou tarefas. 

* **Métricas** registos ao nível da conta. 

As definições para ativar a recolha de registos de diagnóstico não são ativadas por predefinição. Ative explicitamente a definições de diagnóstico para cada conta de Batch que pretende monitorizar.

### <a name="log-destinations"></a>Destinos de registo

Um cenário comum consiste em selecionar uma conta de armazenamento do Azure como o destino de registo. Para armazenar os registos no armazenamento do Azure, crie a conta antes de ativar a recolha de registos. Se associado a uma conta de armazenamento com a sua conta do Batch, pode escolher essa conta como o destino de registo. 

Outros destinos opcionais para os registos de diagnóstico:

* Fluxo de eventos de registo de diagnóstico de Batch para uma [Hub de eventos do Azure](../event-hubs/event-hubs-what-is-event-hubs.md). Hubs de eventos podem ingerir milhões de eventos por segundo, que pode, em seguida, transformar e armazenar utilizando um fornecedor de análise em tempo real. 

* Enviar registos de diagnóstico para [Log Analytics do Azure](../log-analytics/log-analytics-overview.md), onde pode analisá-las no portal do Operations Management Suite (OMS), ou exportá-las para análise no Power BI ou no Excel.

> [!NOTE]
> Pode implicar custos adicionais para armazenar ou processar os dados de registo de diagnóstico com serviços do Azure. 
>

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Ativar a recolha de registos de diagnóstico do Batch

1. No portal, clique em **todos os serviços** > **contas do Batch**e, em seguida, clique no nome da sua conta do Batch.
2. Em **monitorização**, clique em **registos de diagnóstico** > **ative os diagnósticos**.
3. No **definições de diagnóstico**, introduza um nome para a definição e escolha um destino de registo (existente armazenamento conta, Hub de eventos ou Log Analytics). Selecione um ou ambos **ServiceLog** e **AllMetrics**.

    Ao selecionar uma conta de armazenamento, opcionalmente, defina uma política de retenção. Se não especificar um número de dias para a retenção, os dados são mantidos durante a vigência da conta de armazenamento.

4. Clique em **Guardar**.

    ![Diagnóstico de batch](media/batch-diagnostics/diagnostics-portal.png)

Outras opções para ativar a recolha de registos incluem: utilizar o Monitor do Azure no portal para configurar as definições de diagnóstico, utilize um [modelo do Resource Manager](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md), ou utilizar o Azure PowerShell ou a CLI do Azure. consulte [recolher e consumir dados de registo dos seus recursos do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs).


### <a name="access-diagnostics-logs-in-storage"></a>Os registos de diagnóstico de acesso no armazenamento

Se arquivar registos de diagnóstico do Batch numa conta de armazenamento, um contentor de armazenamento é criado na conta de armazenamento, assim que ocorre um evento relacionado. Os BLOBs são criados de acordo com o padrão de nomenclatura seguinte:

```
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
Exemplo:

```
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```
Cada ficheiro de blob PT1H.json contém eventos formatada em JSON que ocorreram dentro da hora especificada no URL do blob (por exemplo, h = 12). Durante a hora presente, os eventos são acrescentados ao ficheiro PT1H.json à medida que ocorrem. O valor de minuto (m = 00) é sempre 00, uma vez que os eventos de registo de diagnóstico são divididos em blobs individuais por hora. (Todas as horas são em UTC).


Para mais informações sobre o esquema de registos de diagnóstico na conta de armazenamento, consulte [os registos de diagnóstico do arquivo Azure](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

Para aceder os registos na sua conta de armazenamento através de programação, utilize as APIs de armazenamento. 

### <a name="service-log-events"></a>Eventos de registo do serviço
Azure Batch registos do serviço, caso recolhidos, contém eventos emitidos pelo serviço Azure Batch durante a duração de um recurso de Batch individuais como um conjunto ou tarefas. Cada evento emitido pelo Batch é registado no formato JSON. Por exemplo, este é o corpo de uma amostra **conjunto criar eventos**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "5",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

O serviço Batch emite atualmente os seguintes eventos de registo do serviço. Esta lista não pode ser exaustiva, uma vez que os eventos adicionais poderão ter sido adicionados uma vez que este artigo foi atualizado pela última vez.

| **Eventos de registo do serviço** |
| --- |
| [Criar conjunto](batch-pool-create-event.md) |
| [Início de eliminação do conjunto](batch-pool-delete-start-event.md) |
| [Eliminar conjunto concluída](batch-pool-delete-complete-event.md) |
| [Início de redimensionamento de conjunto](batch-pool-resize-start-event.md) |
| [Redimensionamento de agrupamento completo](batch-pool-resize-complete-event.md) |
| [Início da tarefa](batch-task-start-event.md) |
| [Tarefa concluída](batch-task-complete-event.md) |
| [Falha de tarefa](batch-task-fail-event.md) |



## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o [Ferramentas e APIs do Batch](batch-apis-tools.md) disponíveis para criação de soluções para o Batch.
* Saiba mais sobre [monitorização soluções do Batch](monitoring-overview.md).
