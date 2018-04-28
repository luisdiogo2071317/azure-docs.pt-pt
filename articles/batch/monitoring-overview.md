---
title: Monitorizar o Azure Batch | Microsoft Docs
description: Saiba mais sobre os serviços de monitorização do Azure, métricas, os registos de diagnóstico e outras funcionalidades de monitorização para o Azure Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: 29ac86ed5c744d37150b0f1b2db17f60306fe77e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="monitor-batch-solutions"></a>Monitorizar soluções do Batch

Azure e o serviço Batch proporcionam uma variedade de serviços, ferramentas e APIs para monitorizar as soluções do Batch. Este artigo de descrição geral ajuda-o a escolha de uma abordagem de monitorização que se adeque às suas necessidades.

Para obter uma descrição geral do Azure componentes e serviços disponíveis para monitorizar os recursos do Azure, consulte [recursos e aplicações de monitorização do Azure](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Monitorização de nível de subscrição

Ao nível da subscrição, incluindo contas do Batch, o [registo de atividade do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) recolhe dados de eventos operacionais [várias categorias](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#categories-in-the-activity-log).

Especificamente, para contas do Batch o registo de atividade recolhe os eventos relacionados com a gestão de criação e eliminação e a chave de conta.

É uma forma de obter eventos a partir do seu registo de atividade para utilizar o portal do Azure. Clique em **todos os serviços** > **registo de atividade**. Em alternativa, consulta para eventos com a CLI do Azure, os cmdlets do PowerShell ou a API de REST de Monitor do Azure. Também pode exportar o registo de atividade ou configurar [alertas de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Monitorização de nível de conta do batch

Monitorizar a cada conta do Batch utilizar funcionalidades do [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Monitor do Azure recolhe [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) e, opcionalmente, [os registos de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) para recursos com âmbito no nível de uma conta do Batch, como conjuntos, trabalhos e tarefas. Recolher e consumir estes dados manualmente ou através de programação para monitorizar atividades na sua conta do Batch e diagnosticar problemas. Para obter mais informações, consulte [Batch métricas, alertas e registos para a monitorização e diagnóstico avaliação](batch-diagnostics.md).
 
> [!NOTE]
> Métricas estão disponíveis por predefinição na sua conta do Batch sem configuração adicional e têm um histórico de graduais de 30 dias. Tem de ativar o registo de diagnóstico para uma conta do Batch e pode implicar custos adicionais para armazenar ou processar os dados de registo de diagnóstico. 

## <a name="batch-resource-monitoring"></a>Monitorização de recursos do batch

Nas suas aplicações de Batch, utilize as APIs do Batch para monitorizar ou consultar o estado dos seus recursos, incluindo tarefas, tarefas, nós e agrupamentos. Por exemplo:

* [Contagem de tarefas por estado](batch-get-task-counts.md)
* [Criar consultas para recursos do Batch de lista de forma eficiente](batch-efficient-list-queries.md)
* [Criar as dependências de tarefas](batch-task-dependencies.md)
* Utilize um [tarefa do Gestor de tarefas](/rest/api/batchservice/job/add#jobmanagertask)
* Monitor de [estado de tarefas](/rest/api/batchservice/task/list#taskstate)
* Monitor de [estado do nó](/rest/api/batchservice/computenode/list#computenodestate)
* Monitor de [agrupamento Estado](/rest/api/batchservice/pool/get#poolstate)
* Monitor [utilização na conta do conjunto](/rest/api/batchservice/pool/listusagemetrics)
* [Contagem de nós de conjunto pelo Estado](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>Contadores de desempenho da VM e monitorização de aplicações

* [Application Insights](../application-insights/app-insights-overview.md) é um serviço Azure pode utilizar para monitorizar programaticamente a disponibilidade, desempenho e utilização das suas tarefas e tarefas do Batch. Facilmente os contadores de desempenho do get de nós de computação (VMs) e informações personalizadas para tarefas retire as VMs. 

  Por exemplo, consulte [Monitor e depuração uma aplicação .NET do Batch com o Application Insights](monitor-application-insights.md) e o que acompanha [exemplo de código](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights).

  > [!NOTE]
  > Pode implicar custos adicionais para utilizar o Application Insights. Consulte o [preços opções](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* O [BatchLabs](https://github.com/Azure/BatchLabs) é uma ferramenta de cliente autónoma, gratuita e rica em funcionalidades para ajudar a criar, depurar e monitorizar aplicações do Azure Batch. Transfira um [pacote de instalação](https://azure.github.io/BatchLabs/) para Mac, Linux ou Windows. Opcionalmente, configurar a sua solução do Batch para [apresentar dados do Application Insights](https://github.com/Azure/batch-insights) , tais como os contadores de desempenho VM BatchLabs.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o [Ferramentas e APIs do Batch](batch-apis-tools.md) disponíveis para criação de soluções para o Batch.
* Saiba mais sobre [registo diagnóstico](batch-diagnostics.md) com o Batch.