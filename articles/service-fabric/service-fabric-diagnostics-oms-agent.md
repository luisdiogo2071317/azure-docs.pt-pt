---
title: Recursos de infraestrutura do serviço do Azure - com a análise de registos de monitorização de desempenho | Microsoft Docs
description: Saiba como configurar o agente do OMS para monitorização contentores e contadores de desempenho para os clusters de Service Fabric do Azure.
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
ms.openlocfilehash: a3ce72e51477c1eda99461b3910bfeeac207be55
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="performance-monitoring-with-log-analytics"></a>Monitorização com a análise de registos de desempenho

Este artigo aborda os passos para adicionar o agente de análise de registos, também conhecido como OMS, como um máquina virtual conjunto de dimensionamento de extensão para o cluster e ligá-lo à sua área de trabalho do Log Analytics do Azure existente. Isto permite recolher dados de diagnóstico sobre contentores, aplicações e a monitorização de desempenho. Adicionando-o como uma extensão para o recurso de conjunto de dimensionamento de máquina virtual, do Azure Resource Manager garante que é instalado em cada nó, mesmo quando a dimensionar o cluster.

> [!NOTE]
> Este artigo pressupõe que tem uma área de trabalho do Log Analytics do Azure já definida. Se não o fizer, aceda a [configurar Log Analytics do Azure](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>Adicionar a extensão de agente, através da CLI do Azure

Através do dimensionamento da máquina virtual, a melhor forma de adicionar o agente do OMS para o cluster está definida APIs disponíveis com a CLI do Azure. Se não tiver a CLI do Azure ainda configurar, aceda ao portal do Azure e abrir um [nuvem Shell](../cloud-shell/overview.md) instância, ou [instalar o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Assim que for solicitada a Shell de nuvem, certifique-se de que está a trabalhar na mesma subscrição, como o recurso. Verificar isto com `az account show` e certifique-se de que o valor "name" corresponde da subscrição do cluster.

2. No Portal, navegue para o grupo de recursos onde está localizada a sua área de trabalho de análise de registos. Clique para o recurso de análise de registos (o tipo de recurso será Log Analytics). Assim que estiver na página de descrição geral de recursos, clique em **definições avançadas** na secção definições no menu da esquerda.

    ![Página de propriedades de análise do registo](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)
 
3. Clique em **servidores Windows** se que é colocado cópias de segurança num cluster do Windows, e **servidores Linux** se estiver a criar um cluster do Linux. Esta página irá mostrar-lhe o `workspace ID` e `workspace key` (listados como chave primária no portal). É necessário para o passo seguinte.

4. Execute o comando para instalar o agente do OMS no seu cluster, utilizando o `vmss extension set` API na sua Shell de Cloud:

    Para um cluster do Windows:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Para um cluster do Linux:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Eis um exemplo do agente do OMS que está a ser adicionado a um cluster do Windows.

    ![Comando de cli de agente do OMS](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Isto deve demorar menos de 15 min adicionar com êxito o agente para os nós. Pode verificar se os agentes foram adicionados utilizando a `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Adicionar o agente através do modelo do Resource Manager

Modelos de Gestor de recursos de exemplo que implementar uma área de trabalho do Log Analytics do Azure e adicione um agente para cada um dos seus nós está disponível para [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) ou [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Pode transferir e modificar este modelo para implementar um cluster que melhor se adapta às suas necessidades.

## <a name="view-performance-counters-in-the-log-analytics-portal"></a>Ver contadores de desempenho no Portal de análise do registo

Agora que adicionou o agente do OMS, head no ativação pós-falha para o portal de análise de registos para escolher que desempenho contadores pretende recolher. 

1. No portal do Azure, vá para o grupo de recursos no qual criou a solução de análise de recursos de infraestrutura de serviço. Selecione **ServiceFabric\<nameOfOMSWorkspace\>**  e aceda à página de descrição geral. Na parte superior, clique na ligação para aceder ao Portal do OMS.

2. Quando estiver no portal, verá um mosaicos sob a forma de um gráfico para cada uma das soluções ativadas, incluindo um para o Service Fabric. Clique em continuar para a solução de análise de recursos de infraestrutura de serviço. 

3. Agora verá mosaicos alguns com gráficos no canal operacional e eventos de serviços fiável. No lado direito clique no ícone de equipamento de ir para a página de definições.

    ![Definições do OMS](media/service-fabric-diagnostics-oms-agent/oms-solutions-settings.png)

4. Na página Definições, clique em dados e escolha Windows ou Linux contadores de desempenho. Existem uma lista de predefinição aqueles que pode optar por ativar e que pode definir o intervalo de coleção. Também pode adicionar [contadores de desempenho adicionais](service-fabric-diagnostics-event-generation-perf.md) para recolher. O formato correto é referenciado nesta [artigo](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

Assim que estão configurados os contadores, head de volta para a página de soluções e verá logo que seja fluxo de dados em e apresentados nos gráficos em **métricas de nó**. Pode também consultar dados de contador de desempenho da mesma forma para eventos de cluster e filtrar em nós, nome do contador de desempenho e valores utilizando a linguagem de consulta Kusto. 

![Consulta de contador de desempenho do OMS](media/service-fabric-diagnostics-oms-agent/oms-perf-counter-query.png)

## <a name="next-steps"></a>Passos Seguintes

* Recolher relevante [contadores de desempenho](service-fabric-diagnostics-event-generation-perf.md). Para configurar o agente do OMS para recolher contadores de desempenho específicos, consulte [configurar origens de dados](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Configurar a análise de registos para configurar [automatizada alertas](../log-analytics/log-analytics-alerts.md) para ajudar a detetar e diagnóstico
* Como alternativa pode recolher contadores de desempenho através de [extensão Azure diagnósticos e enviá-los aos conhecimentos aprofundados de aplicação](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template)