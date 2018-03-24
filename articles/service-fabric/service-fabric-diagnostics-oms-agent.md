---
title: Azure Service Fabric - configurar a monitorização com o agente do OMS | Microsoft Docs
description: Saiba como configurar o agente do OMS para monitorização contentores e contadores de desempenho para os clusters de Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: 4c4095071235dac7e8be3c16b614bdfa5b706a1c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="add-the-oms-agent-to-a-cluster"></a>Adicionar o agente do OMS a um cluster

Este artigo aborda os passos para adicionar o agente do OMS como um máquina virtual conjunto de dimensionamento de extensão para o cluster e ligá-lo à sua área de trabalho do Log Analytics do Azure existente. Isto permite recolher dados de diagnóstico sobre contentores, aplicações e a monitorização de desempenho. Adicionando-o como uma extensão, do Azure Resource Manager garante que é instalado em cada nó, mesmo quando a dimensionar o cluster.

> [!NOTE]
> Este artigo pressupõe que tem uma área de trabalho do Log Analytics do Azure já definida. Se não o fizer, aceda a [configurar Log Analytics do Azure](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>Adicionar a extensão de agente, através da CLI do Azure

Através do dimensionamento da máquina virtual, a melhor forma de adicionar o agente do OMS para o cluster está definida APIs disponíveis com a CLI do Azure. Se não tiver a CLI do Azure ainda configurar, aceda ao portal do Azure e abrir um [nuvem Shell](../cloud-shell/overview.md) instância, ou [instalar o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Assim que for solicitada a Shell de nuvem, certifique-se de que está a trabalhar na mesma subscrição, como o recurso. Verificar isto com `az account show` e certifique-se de que o valor "name" corresponde da subscrição do cluster.

2. No Portal, navegue para o grupo de recursos onde está localizada a sua área de trabalho de análise de registos. Clique para o análise de registos recursos (o tipo de recurso será Log Analytics), no painel de navegação à direita, desloque para baixo e clique em **propriedades**.

    ![Página de propriedades de análise do registo](media/service-fabric-diagnostics-oms-agent/oms-properties.png)

    Tome nota do seu `workspaceId`. 

3. Também terá do `workspaceKey` para implementar o agente. Para obter a chave, clique em **definições avançadas**, sob o *definições* secção do painel de navegação esquerdo. Clique em **servidores Windows** se que é colocado cópias de segurança num cluster do Windows, e **servidores Linux** se estiver a criar um cluster do Linux. Terá do *chave primária* que aparece para implementar os agentes, como o `workspaceKey`.

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
 
5. Execute o comando para aplicar esta configuração para as instâncias de VM que já existem:  


    ```sh
    az vmss update-instances
    ```

Isto deve demorar menos de 15 min adicionar com êxito o agente para os nós. Pode verificar se os agentes foram adicionados utilizando a `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Adicionar o agente através do modelo do Resource Manager

Modelos de Gestor de recursos de exemplo que implementar uma área de trabalho do Log Analytics do Azure e adicione um agente para cada um dos seus nós está disponível para [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) ou [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Pode transferir e modificar este modelo para implementar um cluster que melhor se adapta às suas necessidades.

## <a name="next-steps"></a>Próximos Passos

* Recolher relevante [contadores de desempenho](service-fabric-diagnostics-event-generation-perf.md). Para configurar o agente do OMS para recolher contadores de desempenho específicos, consulte [configurar origens de dados](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Configurar a análise de registos para configurar [automatizada alertas](../log-analytics/log-analytics-alerts.md) para ajudar a detetar e diagnóstico
