---
title: Como integrar o Azure Monitor para contentores (pré-visualização) | Documentos da Microsoft
description: Este artigo descreve como sua integração e configure o Azure Monitor para contentores, para que possa compreender o desempenho do seu contentor e quais problemas relacionados ao desempenho foram identificados.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: magoedte
ms.openlocfilehash: 2b7045f74a22732337ceb8dc9136da1c93ee7c2c
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037792"
---
# <a name="how-to-onboard-azure-monitor-for-containers-preview"></a>Como integrar o Azure Monitor para contentores (pré-visualização) 
Este artigo descreve como configurar o Azure Monitor para contentores para monitorizar o desempenho de cargas de trabalho que são implementadas nos ambientes do Kubernetes e alojada num [do Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

## <a name="prerequisites"></a>Pré-requisitos 
Antes de começar, certifique-se de que tem o seguinte:

- Um cluster do AKS novo ou existente.
- Um agente de Log Analytics em contentores de Linux versão microsoft / oms:ciprod04202018 ou posterior. O número de versão é representado por uma data no seguinte formato: *mmddyyyy*. O agente é instalado automaticamente durante a integração desse recurso. 
- Uma área de trabalho do Log Analytics. Pode criá-la quando ativar a monitorização do seu novo cluster do AKS ou permitir que a experiência de integração, criar uma área de trabalho padrão no grupo de recursos predefinido da subscrição de cluster do AKS. Se optar por criá-lo, pode criá-lo através de [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), da funcionalidade [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), ou no [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).
- A função de Contribuidor do Log Analytics, para ativar a monitorização do contentor. Para obter mais informações sobre como controlar o acesso a uma área de trabalho do Log Analytics, consulte [gerir áreas de trabalho](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Componentes 

Sua capacidade de monitorizar o desempenho se baseia num agente de Log Analytics em contentores de Linux, que recolhe dados de eventos de desempenho e de todos os nós do cluster. O agente automaticamente implementar e registar com a área de trabalho do Log Analytics especificada depois de ativar a monitorização de contentores. 

>[!NOTE] 
>Se já tiver implementado um cluster do AKS, ativa a monitorização com o CLI do Azure ou um modelo Azure Resource Manager fornecido, conforme demonstrado neste artigo. Não é possível utilizar `kubectl` para atualizar, eliminar, implemente novamente ou implementar o agente. O modelo precisa ser implantado no mesmo grupo de recursos do cluster."

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [portal do Azure](https://portal.azure.com). 

## <a name="enable-monitoring-for-a-new-cluster"></a>Ative a monitorização para um novo cluster
Durante a implementação, pode ativar a monitorização de um novo cluster do AKS no portal do Azure ou com a CLI do Azure. Siga os passos no artigo guia de introdução [implementar um cluster do Azure Kubernetes Service (AKS)](../aks/kubernetes-walkthrough-portal.md) se pretender ativar a partir do portal. Sobre o **monitorização** página, para o **Ativar monitorização** opção, selecione **Sim**e, em seguida, selecione uma área de trabalho do Log Analytics existente ou crie um novo. 

Para ativar a monitorização de um novo cluster do AKS criado com a CLI do Azure, siga o passo no artigo guia de introdução na seção [cluster do AKS criar](../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente. Tem de executar a CLI do Azure versão 2.0.43 ou posterior. Para identificar a versão, execute `az --version`. Se precisar de instalar ou atualizar a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

Depois de ativar a monitorização e todas as tarefas de configuração são concluídas com êxito, pode monitorizar o desempenho do seu cluster em qualquer uma das seguintes formas:

* Diretamente no cluster do AKS, selecionando **estado de funcionamento** no painel esquerdo.
* Ao selecionar o **informações de contentor do Monitor** mosaico na página de cluster do AKS para o cluster selecionado. No Azure Monitor, no painel esquerdo, selecione **estado de funcionamento**. 

  ![Opções para selecionar o Monitor do Azure para contentores no AKS](./media/monitoring-container-insights-onboard/kubernetes-select-monitoring-01.png)

Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar as métricas de estado de funcionamento para o cluster. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Ative a monitorização para os clusters geridos existentes
Pode ativar a monitorização de um cluster do AKS que já tenha sido implementado por meio da CLI do Azure, no portal ou com o modelo Azure Resource Manager fornecido com o cmdlet do PowerShell `New-AzureRmResourceGroupDeployment`. 

### <a name="enable-monitoring-using-azure-cli"></a>Ativar monitorização utilizando a CLI do Azure
O passo seguinte permite o monitoramento do cluster do AKS com a CLI do Azure. Neste exemplo, não tem de criar por ou especificar uma área de trabalho existente. Este comando simplifica o processo para si através da criação de uma área de trabalho padrão no grupo de recursos predefinido da subscrição de cluster do AKS, se já não existir na região.  O formato de é semelhante a área de trabalho predefinida criada *DefaultWorkspace -<GUID>-<Region>*.  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

A saída terá a seguinte aparência:

```azurecli
provisioningState       : Succeeded
```

Se teria, em vez disso, integrar com uma área de trabalho existente, utilize o seguinte comando para especificar essa área de trabalho.

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG --workspace-resource-id <ExistingWorkspaceResourceID> 
```

A saída terá a seguinte aparência:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-from-azure-monitor"></a>Ativar a monitorização do Azure Monitor
Para ativar a monitorização do seu cluster do AKS no portal do Azure do Azure Monitor, faça o seguinte:

1. No portal do Azure, selecione **Monitor**. 
2. Selecione **contentores (pré-visualização)** da lista.
3. Sobre o **Monitor - contentores (pré-visualização)** página, selecione **clusters não monitorizados**.
4. Na lista de clusters não monitorizado, encontrar o contentor na lista e clique em **ativar**.   
5. Na **inclusão de registos e de estado de funcionamento do contentor** página, se tiver um existentes do Log Analytics a área de trabalho na mesma subscrição que o cluster, selecione-o na lista pendente.  
    A lista preselects a área de trabalho predefinida e a localização que o contentor do AKS é implementado na subscrição. 

    ![Ativar a monitorização de informações de contentor do AKS](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Se quiser criar uma nova área de trabalho do Log Analytics para armazenar os dados de monitorização do cluster, siga as instruções em [criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md). Certifique-se de que criar a área de trabalho na mesma subscrição que está implementado o contentor do AKS. 
 
Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar as métricas de estado de funcionamento para o cluster. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>Ativar a monitorização a partir de um cluster do AKS no portal
Para ativar a monitorização do seu contentor do AKS no portal do Azure, efetue o seguinte:

1. No portal do Azure, selecione **Todos os serviços**. 
2. Na lista de recursos, comece a escrever **contentores**.  
    Os filtros de lista com base na sua entrada. 
3. Selecione **serviços de Kubernetes**.  

    ![A ligação de serviços do Kubernetes](./media/monitoring-container-insights-onboard/portal-search-containers-01.png)

4. Na lista de contentores, selecione um contentor.
5. Na página de descrição geral do contentor, selecione **monitorizar o estado de funcionamento do contentor**.  
6. Na **inclusão de registos e de estado de funcionamento do contentor** página, se tiver um existentes do Log Analytics a área de trabalho na mesma subscrição que o cluster, selecione-o na lista pendente.  
    A lista preselects a área de trabalho predefinida e a localização que o contentor do AKS é implementado na subscrição. 

    ![Ativar a monitorização de estado de funcionamento de contentor do AKS](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Se quiser criar uma nova área de trabalho do Log Analytics para armazenar os dados de monitorização do cluster, siga as instruções em [criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md). Certifique-se de que criar a área de trabalho na mesma subscrição que está implementado o contentor do AKS. 
 
Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar dados operacionais para o cluster. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Ativar a monitorização utilizando um modelo Azure Resource Manager
Esse método inclui dois modelos JSON. Um modelo especifica a configuração para ativar a monitorização e a outra contém valores de parâmetros que pode configurar para especificar o seguinte:

* O ID de recurso de contentor do AKS. 
* O grupo de recursos que o cluster é implementado no.
* A área de trabalho do Log Analytics e a região para criar a área de trabalho. 

>[!NOTE]
>O modelo precisa ser implantado no mesmo grupo de recursos do cluster.
>

A área de trabalho do Log Analytics tem de ser criadas manualmente. Para criar a área de trabalho, pode configurá-lo por meio [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), da funcionalidade [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), ou no [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).

Se não estiver familiarizado com o conceito de implementar recursos com um modelo, consulte:
* [Implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Implementar recursos com modelos do Resource Manager e a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md)

Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente. Tem de executar a CLI do Azure versão 2.0.27 ou posterior. Para identificar a versão, execute `az --version`. Se precisar de instalar ou atualizar a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Criar e executar um modelo

1. Copie e cole a seguinte sintaxe JSON no seu ficheiro:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. Guarde este ficheiro como **existingClusterOnboarding.json** para uma pasta local.
3. Cole a seguinte sintaxe JSON no seu ficheiro:

    ```json
    {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. Edite os valores dos **aksResourceId** e **aksResourceLocation** utilizando os valores no **descrição geral do AKS** página para o cluster do AKS. O valor para **workspaceResourceId** é a ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho. Especifique também a localização da área de trabalho para **workspaceRegion**. 
5. Guarde este ficheiro como **existingClusterParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Utilize os seguintes comandos do PowerShell na pasta que contém o modelo:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ClusterResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, será apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Para executar o seguinte comando com a CLI do Azure:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, será apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

        ```azurecli
        provisioningState       : Succeeded
        ```
Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar as métricas de estado de funcionamento para o cluster. 

## <a name="verify-agent-and-solution-deployment"></a>Verificar a implementação de agente e solução
Com a versão do agente *06072018* ou posterior, pode verificar que o agente e a solução foram implementados com êxito. Com as versões anteriores do agente, pode verificar apenas a implementação de agente.

### <a name="agent-version-06072018-or-later"></a>Versão do agente 06072018 ou posterior
Execute o seguinte comando para verificar que o agente é implementado com êxito. 

```
kubectl get ds omsagent --namespace=kube-system
```

A saída deve assemelhar-se o seguinte, que indica que foi implementado devidamente:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Para verificar a implementação da solução, execute o seguinte comando:

```
kubectl get deployment omsagent-rs -n=kube-system
```

A saída deve assemelhar-se o seguinte, que indica que foi implementado devidamente:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Versão de agente anteriores ao 06072018

Para verificar que a versão do agente do Log Analytics lançado antes *06072018* está implementado corretamente, execute o seguinte comando:  

```
kubectl get ds omsagent --namespace=kube-system
```

A saída deve assemelhar-se o seguinte, que indica que foi implementado devidamente:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Ver configuração com a CLI
Utilize o `aks show` comando para obter os detalhes desse tipo, tal como está a solução ativada ou não, o que é o resourceID de área de trabalho do Log Analytics e detalhes de resumo sobre o cluster.  

```azurecli
az aks show -g <resoourceGroupofAKSCluster> -n <nameofAksCluster>
```

Após alguns minutos, o comando é concluído e devolve o formato JSON informações sobre a solução.  Os resultados do comando deve mostrar o perfil de complemento de monitorização e é semelhante a saída de exemplo seguinte:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```
## <a name="next-steps"></a>Passos Seguintes

* Se ocorrerem problemas ao tentar carregar a solução, reveja o [guia de resolução de problemas](monitoring-container-insights-troubleshoot.md)

* Com a monitorização ativada para capturar métricas de estado de funcionamento para os nós de cluster do AKS e os pods, estas métricas de estado de funcionamento estão disponíveis no portal do Azure. Para saber como utilizar o Azure Monitor para contentores, veja [estado de funcionamento do serviço de Kubernetes do Azure de modo de exibição](monitoring-container-insights-analyze.md).