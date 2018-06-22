---
title: Recursos de infraestrutura do serviço do Azure - com a análise de registos de monitorização de desempenho | Microsoft Docs
description: Saiba como configurar o agente de análise do registo para a monitorização de contentores e contadores de desempenho para os clusters de Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: a31fe62f2e81a0e39e4c314fc736e91e72bf7517
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301558"
---
# <a name="performance-monitoring-with-log-analytics"></a>Monitorização com a análise de registos de desempenho

Este artigo aborda os passos para adicionar o agente de análise do registo como um máquina virtual conjunto de dimensionamento de extensão para o cluster e ligá-lo à sua área de trabalho do Log Analytics do Azure existente. Isto permite recolher dados de diagnóstico sobre contentores, aplicações e a monitorização de desempenho. Adicionando-o como uma extensão para o recurso de conjunto de dimensionamento de máquina virtual, do Azure Resource Manager garante que é instalado em cada nó, mesmo quando a dimensionar o cluster.

> [!NOTE]
> Este artigo pressupõe que tem uma área de trabalho do Log Analytics do Azure já definida. Se não o fizer, aceda a [configurar Log Analytics do Azure](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>Adicionar a extensão de agente, através da CLI do Azure

Através do dimensionamento da máquina virtual, a melhor forma de adicionar o agente de análise do registo para o cluster está definida APIs disponíveis com a CLI do Azure. Se não tiver a CLI do Azure ainda configurar, aceda ao portal do Azure e abrir um [nuvem Shell](../cloud-shell/overview.md) instância, ou [instalar o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Assim que for solicitada a Shell de nuvem, certifique-se de que está a trabalhar na mesma subscrição, como o recurso. Verificar isto com `az account show` e certifique-se de que o valor "name" corresponde da subscrição do cluster.

2. No Portal, navegue para o grupo de recursos onde está localizada a sua área de trabalho de análise de registos. Clique para o recurso de análise de registos (o tipo de recurso será Log Analytics). Assim que estiver na página de descrição geral de recursos, clique em **definições avançadas** na secção definições no menu da esquerda.

    ![Página de propriedades de análise do registo](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)
 
3. Clique em **servidores Windows** se que é colocado cópias de segurança num cluster do Windows, e **servidores Linux** se estiver a criar um cluster do Linux. Esta página irá mostrar-lhe o `workspace ID` e `workspace key` (listados como chave primária no portal). É necessário para o passo seguinte.

4. Execute o comando para instalar o agente de análise de registos no seu cluster, utilizando o `vmss extension set` API na sua Shell de Cloud:

    Para um cluster do Windows:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Para um cluster do Linux:

    ```sh
    az vmss extension set --name Log AnalyticsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Eis um exemplo do agente de análise do registo a ser adicionado a um cluster do Windows.

    ![Comando de cli de agente de análise de registo](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Isto deve demorar menos de 15 min adicionar com êxito o agente para os nós. Pode verificar se os agentes foram adicionados utilizando a `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Adicionar o agente através do modelo do Resource Manager

Modelos de Gestor de recursos de exemplo que implementar uma área de trabalho do Log Analytics do Azure e adicione um agente para cada um dos seus nós está disponível para [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) ou [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Pode transferir e modificar este modelo para implementar um cluster que melhor se adapta às suas necessidades.

## <a name="view-performance-counters"></a>Contadores de desempenho de vista

Agora que adicionou o agente de análise de registos, head no ativação pós-falha para o portal de análise de registos para escolher que desempenho contadores pretende recolher. 

1. No portal do Azure, vá para o grupo de recursos no qual criou a solução de análise de recursos de infraestrutura de serviço. Selecione **ServiceFabric\<nameOfLog AnalyticsWorkspace\>**.

2. Clique em **Log Analytics**.

3. Clique em **definições avançadas**.

4. Clique em **dados**, em seguida, clique em **Windows ou Linux contadores de desempenho**. Existe uma lista de contadores predefinido, que pode optar por ativar e que pode definir o intervalo de coleção. Também pode adicionar [contadores de desempenho adicionais](service-fabric-diagnostics-event-generation-perf.md) para recolher. O formato correto é referenciado nesta [artigo](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

5. Clique em **guardar**, em seguida, clique em **OK**.

6. Feche o painel de definições avançadas.

7. Sob o cabeçalho geral, clique em **descrição geral**.

8. Verá os mosaicos sob a forma de um gráfico para cada uma das soluções ativadas, incluindo um para o Service Fabric. Clique em de **Service Fabric** gráfico para continuar para a solução de análise de recursos de infraestrutura de serviço.

9. Irá ver alguns mosaicos com gráficos no canal operacional e eventos de serviços fiável. A representação gráfica dos dados que circulam para os contadores selecionados será apresentado no nó de métricas. 

10. Clique num gráfico de métrica de contentor para ver detalhes adicionais. Pode também consultar dados de contador de desempenho da mesma forma para eventos de cluster e filtrar em nós, nome do contador de desempenho e valores utilizando a linguagem de consulta Kusto.

![Consulta de contador de desempenho de análise de registo](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Passos Seguintes

* Recolher relevante [contadores de desempenho](service-fabric-diagnostics-event-generation-perf.md). Para configurar o agente de análise de registos para recolher contadores de desempenho específicos, consulte [configurar origens de dados](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Configurar a análise de registos para configurar [automatizada alertas](../log-analytics/log-analytics-alerts.md) para ajudar a detetar e diagnóstico
* Como alternativa pode recolher contadores de desempenho através de [extensão Azure diagnósticos e enviá-los aos conhecimentos aprofundados de aplicação](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template)
