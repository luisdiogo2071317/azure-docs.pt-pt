---
title: "Monitorização e Diagnósticos para Contentores do Windows no Azure Service Fabric | Microsoft Docs"
description: "Neste tutorial, vai configurar a monitorização e os diagnósticos para Contentores do Windows no Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: de77d10e4875173c7a067e945e473887d3cc7422
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="tutorial-monitor-windows-containers-on-service-fabric-using-oms"></a>Tutorial: Monitorizar contentores do Windows no Service Fabric com o OMS

Esta é a terceira parte de um tutorial e orienta-o ao longo da configuração do OMS para monitorizar os seus contentores do Windows orquestrados no Service Fabric.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar o OMS para o cluster do Service Fabric
> * Utilizar uma área de trabalho do OMS para ver e consultar os registos de contentores e nós
> * Configurar o agente do OMS para recolher métricas de contentores e nós

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, tem de:
- Tem um cluster no Azure ou [criar um com este tutorial](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Implementar uma aplicação contentorizada no mesmo](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-oms-with-your-cluster-in-the-resource-manager-template"></a>Configurar o OMS com o seu cluster no modelo do Resource Manager

Caso tenha utilizado o [modelo disponibilizado](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) na primeira parte deste tutorial, o mesmo deveria incluir as adições seguintes aos modelos do Azure Resource Manager genéricos do Service Fabric. Se quiser configurar o seu próprio cluster para monitorizar contentores com o OMS:
* Faça as alterações seguintes ao modelo do Resource Manager.
* Implemente-o com o PowerShell para atualizar o cluster mediante a [implementação do modelo](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm). O Azure Resource Manager apercebe-se de que o recurso existe, pelo que o implementa como uma atualização.

### <a name="adding-oms-to-your-cluster-template"></a>Adicionar o OMS ao modelo do cluster

Faça as alterações seguintes ao *template.json*:

1. Adicione a localização e o nome da área de trabalho do OMS à secção *parameters*:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your OMS Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your OMS workspace"
      }
    }
    ```

    Para alterar um dos dois valores utilizados, adicione os mesmos parâmetros ao *template.parameters.json* e altere os valores utilizados aí.

2. Adicione o nome da solução e a solução a *variables*: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. Adicione o Microsoft Monitoring Agent do OMS como uma extensão da máquina virtual. Localize o recurso de conjuntos de dimensionamento de máquinas virtuais: *resources* > *"apiVersion": "[variables('vmssApiVersion')]"*. Em *properties* > *virtualMachineProfile* > *extensionProfile* > *extensions*, adicione a descrição de extensão seguinte na extensão *ServiceFabricNode*: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Adicione a área de trabalho do OMS como recurso individual. Em *resources*, a seguir ao recurso de conjuntos de dimensionamento de máquinas virtuais, adicione o seguinte:
    
    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

Está disponível [aqui](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) um modelo de exemplo (utilizado na primeira parte do tutorial) que tem todas estas alterações e que pode consultar sempre que necessário. Estas alterações adicionarão uma área de trabalho do Log Analytics do OMS ao seu grupo de recursos. A área de trabalho será configurada para recolher eventos da plataforma do Service Fabric a partir das tabelas de armazenamento configuradas com o agente [Diagnóstico do Microsoft Azure](service-fabric-diagnostics-event-aggregation-wad.md). O agente do OMS (Microsoft Monitoring Agent) também foi adicionado a cada nó do seu cluster como extensão de máquina virtual, o que significa que, à medida que dimensiona o cluster, o agente é configurado automaticamente em cada máquina e ligado à mesma área de trabalho.

Implemente o modelo com as alterações novas para atualizar o seu cluster atual. Deverá ver os recursos do OMS no seu grupo de recursos quando a implementação estiver concluída. Quando o cluster estiver pronto, implemente a aplicação contentorizada no mesmo. No próximo passo, vamos configurar a monitorização dos contentores.

## <a name="add-the-container-monitoring-solution-to-your-oms-workspace"></a>Adicionar a solução de Monitorização de Contentores à sua área de trabalho do OMS

Para configurar a solução de Contentores na sua área de trabalho, procure *Solução de Monitorização de Contentores* e crie um recurso Contentores (na categoria Monitorização + Gestão).

![Adicionar a solução de Contentores](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

Quando lhe for pedida a *Área de Trabalho do OMS*, selecione a área que foi criada no seu grupo de recursos e clique em **Criar**. Esta ação adiciona uma *Solução de Monitorização de Contentores* à sua área de trabalho e faz com que o agente do OMS implementado pelo modelo comece a recolher automaticamente registos e estatísticas. 

Navegue de volta para o *grupo de recursos*, onde deverá ver agora a solução de monitorização acabada de adicionar. Se clicar na mesma, a página de destino deverá mostrar o número de imagens de contentor que tem em execução. 

*Repare que executei cinco instâncias do contentor fabrikam da [segunda parte](service-fabric-host-app-in-a-container.md) do tutorial*

![Página de destino da solução de contentor](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

Clicar na **Solução de Monitorização de Contentores** leva-o para um dashboard mais detalhado, que lhe permite navegar por vários painéis, bem como executar consultas no Log Analytics. 

*Tenha em conta que a solução vai sofrer algumas atualizações a partir de setembro de 2017; se receber erros sobre eventos do Kubernetes, ignore-os, pois estamos a trabalhar no sentido de integrar vários orquestradores na mesma solução.*

Uma vez que o agente recolhe registos do Docker, mostra *stdout* e *stderr* como predefinição. Se se deslocar para a direita, verá o inventário de imagens de contentor, o estado, métricas e consultas de exemplo que pode executar para obter dados mais úteis. 

![Dashboard da solução de Contentores](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Clicar em qualquer um destes painéis leva-o para a consulta do Log Analytics que está a gerar o valor apresentado. Altere a consulta para *\** de modo a ver todos os diferentes tipos de registos que estão a ser recolhidos. Aqui, pode consultar ou filtrar por desempenho do contentor ou por registos ou ver eventos da plataforma do Service Fabric. Os agentes também estão constantemente a emitir um heartbeat de cada nó e que pode ver para confirmar que ainda estão a ser recolhidos dados das suas máquinas, caso a configuração do seu cluster se altere.   

![Consulta do contentor](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>Configurar o agente do OMS para recolher contadores de desempenho

Outra vantagem da utilização do agente do OMS é a capacidade de alterar os contadores de desempenho que quer recolher através da experiência de IU do OMS em vez de ter de configurar o agente de diagnósticos do Azure e de fazer uma atualização baseada num modelo do Resource Manager de cada vez. Para tal, clique em **Portal do OMS** na página de destino da sua Solução de Monitorização de Contentores (ou do Service Fabric).

![Portal do OMS](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

Desta forma, é encaminhado para a sua área de trabalho no portal do OMS, onde pode ver as suas soluções, criar dashboards personalizados e configurar o agente do OMS. 
* Clique na **roda dentada**, no canto superior direito do ecrã, para abrir o menu *Definições*.
* Clique em **Connected Sources** (Origens Ligadas)  > **Windows Servers** (Servidores Windows) *5 Windows Computers Connected* (5 Computadores Windows Ligados).
* Clique em **Data** (Dados) > **Windows Performance Counters** (Contadores de Desempenho do Windows) para procurar e adicionar contadores de desempenho novos. Aqui, verá uma lista de recomendações do OMS relativamente aos contadores de desempenho que pode recolher, bem como a opção para procurar outros contadores. Clique em **Add the selected performance counters** (Adicionar os contadores de desempenho selecionados) para começar a recolher as métricas sugeridas.

    ![Contadores de desempenho](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

Novamente no portal do Azure, **atualize** a Solução de Monitorização de Contadores passados alguns minutos e, depois, deverá começar a receber dados de *Desempenho do Computador*. Isto ajuda a compreender de que forma é que os seus recursos estão a ser atualizados. Também pode utilizar estas métricas para tomar decisões adequadas relativamente ao dimensionamento do seu cluster ou para confirmar se um cluster está a balancear a sua carga conforme esperado.

*Nota: certifique-se de que os seus filtros de hora estão definidos corretamente para poder consumir estas métricas.* 

![Contadores de desempenho 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar o OMS para o cluster do Service Fabric
> * Utilizar uma área de trabalho do OMS para ver e consultar os registos de contentores e nós
> * Configurar o agente do OMS para recolher métricas de contentores e nós

Agora que configurou a monitorização para a sua aplicação contentorizada, experimente o seguinte:

* Configurar o OMS para um cluster do Linux, através de passos semelhantes aos anteriores. Veja [este modelo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) para fazer alterações ao seu modelo do Resource Manager.
* Configurar o OMS para definir [alertas automáticos](../log-analytics/log-analytics-alerts.md) para ajudar na deteção e nos diagnósticos.
* Explorar a lista de [contadores de desempenho recomendados](service-fabric-diagnostics-event-generation-perf.md) do Service Fabric a configurar para os seus clusters.
* Familiarizar-se com as funcionalidades de [registos de pesquisas e consultas](../log-analytics/log-analytics-log-searches.md) que são oferecidas como parte do Log Analytics.