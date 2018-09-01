---
title: Monitorizar estado de funcionamento do Azure Kubernetes Service (AKS) (pré-visualização) | Documentos da Microsoft
description: Este artigo descreve como pode ver facilmente o desempenho do seu contentor do AKS para compreender rapidamente a utilização de seu ambiente alojado do Kubernetes.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: magoedte
ms.openlocfilehash: caf290477a4fd4f03bb248cc89f91027dbe68f3e
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382452"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Monitorizar estado de funcionamento do Azure Kubernetes Service (AKS) contentor (pré-visualização)

Este artigo descreve como configurar e utilizar o estado de funcionamento do contentor do Azure Monitor para monitorizar o desempenho de cargas de trabalho que são implementadas nos ambientes do Kubernetes e alojada no Azure Kubernetes Service (AKS). Monitorizar o cluster de Kubernetes e contentores é fundamental, especialmente quando estiver a executar um cluster de produção em escala, com várias aplicações.

Estado de funcionamento do contentor-lhe capacidade por memória de coleta e métricas de processador de controladores, nós e contentores que estão disponíveis no Kubernetes por meio da API de métricas de monitorização do desempenho. Depois de ativar o estado de funcionamento do contentor, estas métricas são automaticamente recolhidas para por meio de uma versão em contentores do agente do Log Analytics para Linux e armazenadas no seu [do Log Analytics](../log-analytics/log-analytics-overview.md) área de trabalho. As vistas predefinidas incluídas apresentam as cargas de trabalho de contentor que residem e o que afeta o estado de funcionamento do desempenho do cluster de Kubernetes, de modo que pode:  

* Identifique os contentores em execução no nó e a utilização de processador e memória média. Esse conhecimento pode ajudá-lo a identificar afunilamentos de recursos.
* Identifique onde o contentor reside num controlador ou de um pod. Esse conhecimento pode ajudá-lo a ver do controlador ou do pod desempenho geral. 
* Reveja a utilização de recursos de cargas de trabalho em execução no anfitrião que não estão relacionadas com os processos padrão que suportam o pod.
* Compreenda o comportamento do cluster sob cargas mais pesadas e médios. Esse conhecimento pode ajudá-lo a identificar necessidades de capacidade e determinar a carga máxima que o cluster pode suportar. 

Se estiver interessado na monitorização e gestão de Docker e o Windows anfitriões de contentores para ver a configuração, auditoria e utilização de recursos, consulte a [solução de monitorização de contentores](../log-analytics/log-analytics-containers.md).

## <a name="prerequisites"></a>Pré-requisitos 
Antes de começar, certifique-se de que tem o seguinte:

- Um cluster do AKS novo ou existente.
- Um agente de Log Analytics em contentores de Linux versão microsoft / oms:ciprod04202018 ou posterior. O número de versão é representado por uma data no seguinte formato: *mmddyyyy*. O agente é instalado automaticamente durante a integração do Estado de funcionamento do contentor. 
- Uma área de trabalho do Log Analytics. Pode criá-la quando ativar a monitorização do seu novo cluster do AKS ou permitir que a experiência de integração, criar uma área de trabalho padrão no grupo de recursos predefinido da subscrição de cluster do AKS. Se optar por criá-lo, pode criá-lo através de [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), da funcionalidade [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), ou no [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).
- A função de Contribuidor do Log Analytics, para ativar a monitorização do contentor. Para obter mais informações sobre como controlar o acesso a uma área de trabalho do Log Analytics, consulte [gerir áreas de trabalho](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Componentes 

Sua capacidade de monitorizar o desempenho se baseia num agente de Log Analytics em contentores de Linux, que recolhe dados de eventos de desempenho e de todos os nós do cluster. O agente automaticamente implementar e registar com a área de trabalho do Log Analytics especificada depois de ativar a monitorização de contentores. 

>[!NOTE] 
>Se já tiver implementado um cluster do AKS, ativa a monitorização com o CLI do Azure ou um modelo Azure Resource Manager fornecido, conforme demonstrado neste artigo. Não é possível utilizar `kubectl` para atualizar, eliminar, implemente novamente ou implementar o agente. 
>

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [portal do Azure](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Ativar a monitorização de estado de funcionamento de contentor para um novo cluster
Durante a implementação, pode ativar a monitorização de um novo cluster do AKS no portal do Azure ou com a CLI do Azure. Siga os passos no artigo guia de introdução [implementar um cluster do Azure Kubernetes Service (AKS)](../aks/kubernetes-walkthrough-portal.md) se pretender ativar a partir do portal. Sobre o **monitorização** página, para o **Ativar monitorização** opção, selecione **Sim**e, em seguida, selecione uma área de trabalho do Log Analytics existente ou crie um novo. 

Para ativar a monitorização de um novo cluster do AKS criado com a CLI do Azure, siga o passo no artigo guia de introdução na seção [cluster do AKS criar](../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente. Tem de executar a CLI do Azure versão 2.0.43 ou posterior. Para identificar a versão, execute `az --version`. Se precisar de instalar ou atualizar a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

Depois de ativar a monitorização e todas as tarefas de configuração são concluídas com êxito, pode monitorizar o desempenho do seu cluster em qualquer uma das seguintes formas:

* Diretamente no cluster do AKS, selecionando **estado de funcionamento** no painel esquerdo.
* Ao selecionar o **monitorizar o estado de funcionamento do contentor** mosaico na página de cluster do AKS para o cluster selecionado. No Azure Monitor, no painel esquerdo, selecione **estado de funcionamento**. 

  ![Opções para selecionar o estado de funcionamento do contentor no AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar dados operacionais para o cluster. 

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Ativar a monitorização de estado de funcionamento de contentor para os clusters geridos existentes
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

### <a name="enable-monitoring-in-the-azure-portal"></a>Ativar a monitorização no portal do Azure
Para ativar a monitorização do seu contentor do AKS no portal do Azure, efetue o seguinte:

1. No portal do Azure, selecione **todos os serviços**. 
2. Na lista de recursos, comece a escrever **contentores**.  
    Os filtros de lista com base na sua entrada. 
3. Selecione **serviços de Kubernetes**.  

    ![A ligação de serviços do Kubernetes](./media/monitoring-container-health/azure-portal-01.png)

4. Na lista de contentores, selecione um contentor.
5. Na página de descrição geral do contentor, selecione **monitorizar o estado de funcionamento do contentor**.  
6. Na **inclusão de registos e de estado de funcionamento do contentor** página, se tiver um existentes do Log Analytics a área de trabalho na mesma subscrição que o cluster, selecione-o na lista pendente.  
    A lista preselects a área de trabalho predefinida e a localização que o contentor do AKS é implementado na subscrição. 

    ![Ativar a monitorização de estado de funcionamento de contentor do AKS](./media/monitoring-container-health/container-health-enable-brownfield-02.png)

>[!NOTE]
>Se quiser criar uma nova área de trabalho do Log Analytics para armazenar os dados de monitorização do cluster, siga as instruções em [criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md). Certifique-se de que criar a área de trabalho na mesma subscrição que está implementado o contentor do AKS. 
 
Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar dados operacionais para o cluster. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Ativar a monitorização utilizando um modelo Azure Resource Manager
Esse método inclui dois modelos JSON. Um modelo especifica a configuração para ativar a monitorização e a outra contém valores de parâmetros que pode configurar para especificar o seguinte:

* O ID de recurso de contentor do AKS. 
* O grupo de recursos que o cluster é implementado no.
* A área de trabalho do Log Analytics e a região para criar a área de trabalho. 

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
        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
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
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "East US"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "eastus"
       }
     }
    }
    ```

4. Edite os valores dos **aksResourceId** e **aksResourceLocation** utilizando os valores no **descrição geral do AKS** página para o cluster do AKS. O valor para **workspaceResourceId** é a ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho. Especifique também a localização da área de trabalho para **workspaceRegion**. 
5. Guarde este ficheiro como **existingClusterParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Utilize os seguintes comandos do PowerShell na pasta que contém o modelo:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, será apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Para executar o comando seguinte ao utilizar a CLI do Azure no Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, será apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

        ```azurecli
        provisioningState       : Succeeded
        ```
Depois de ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar dados operacionais para o cluster. 

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

## <a name="view-performance-utilization"></a>Utilização de desempenho do Vista
Quando abre o estado de funcionamento do contentor, a página apresenta imediatamente a utilização de desempenho do seu cluster inteiro. Ver informações sobre o seu cluster do AKS está organizado em quatro perspectivas:

- Cluster
- Nós 
- Controladores  
- Contentores

Sobre o **Cluster** separador, os quatro gráficos de desempenho de linhas apresentam métricas chave de desempenho do seu cluster. 

![Exemplos de gráficos de desempenho no separador de Cluster](./media/monitoring-container-health/container-health-cluster-perfview.png)

Gráfico de desempenho apresenta quatro métricas de desempenho:

- **Utilização da CPU de nó&nbsp;%**: um ponto de vista agregado da utilização da CPU para todo o cluster. Pode filtrar os resultados para o intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**, **90**, e **95** no Seletor de percentis acima do gráfico, seja individualmente ou combinado. 
- **Utilização de memória do nó&nbsp;%**: um ponto de vista agregado de utilização da memória para todo o cluster. Pode filtrar os resultados para o intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**, **90**, e **95** no Seletor de percentis acima do gráfico, seja individualmente ou combinado. 
- **Contagem de nós**: uma contagem de nós e o estado do Kubernetes. São Estados de nós do cluster representados *todos os*, *pronto*, e *não está pronto* e podem ser filtrados individualmente ou combinado no Seletor de acima do gráfico. 
- **Contagem de pod de atividade**: uma contagem de pod e o estado a partir do Kubernetes. Estados de pods representados são *todos os*, *pendente*, *em execução*, e *desconhecido* e podem ser filtrados individualmente ou combinado no Seletor de acima do gráfico. 

Quando muda para **nós**, **controladores**, e **contentores** separador apresentada automaticamente no lado direito da página é o painel de propriedades.  Mostra as propriedades do item selecionado, incluindo etiquetas definir para organizar os objetos de Kubernetes.  Clique nas **>>** ligação no painel para view\hide o painel.  

![Painel de propriedades de perspetivas do exemplo Kubernetes](./media/monitoring-container-health/perspectives-preview-pane-01.png)

À medida que expande os objetos da hierarquia, as atualizações do painel de propriedades com base no objeto selecionado. No painel também pode ver eventos de Kubernetes com pesquisas de registos predefinido ao clicar no **registos de eventos de Kubernetes do modo de exibição** link na parte superior do painel. Para obter mais informações sobre a visualização de dados de registo do Kubernetes, consulte [pesquisar registos para analisar dados](#search-logs-to-analyze-data).

Mude para o **nós** separador e a hierarquia de linha segue o modelo de objeto do Kubernetes, começando com um nó do cluster. Expanda o nó e pode ver um ou mais pods em execução no nó. Se mais de um contêiner é agrupado para um pod, eles são exibidos como a última linha na hierarquia. Também pode ver a cargas de trabalho relacionadas não pod quantas estão em execução no anfitrião se o anfitrião tiver pressão de memória ou processador.

![Hierarquia de nós do Kubernetes de exemplo na vista de desempenho](./media/monitoring-container-health/container-health-nodes-view.png)

Pode selecionar os controladores ou contentores na parte superior da página e reveja a utilização de estado e de recursos para esses objetos. Utilize as caixas de lista pendente na parte superior para filtrar por espaço de nomes, o serviço e o nó. Se em vez disso, pretende rever a utilização da memória, na **métrica** na lista pendente, selecione **memória RSS** ou **conjunto de trabalho de memória**. **Memória RSS** só é suportada para Kubernetes versão 1.8 e posterior. Caso contrário, exibir os valores para **Min&nbsp; %**  como *NaN&nbsp;%*, que é um valor de tipo de dados numéricos que representa um indefinido ou valor não representável. 

![Vista de desempenho de nós de contentor](./media/monitoring-container-health/container-health-node-metric-dropdown.png)

Por predefinição, os dados de desempenho se baseia em seis últimas horas, mas pode alterar a janela utilizando o **intervalo de tempo** na lista pendente no canto superior direito. Neste momento, a página não atualização automática, por isso terá de atualizá-lo manualmente. Também pode filtrar os resultados dentro do intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**, **90**, e **95** no Seletor de percentil. 

![Seleção de percentil para filtragem de dados](./media/monitoring-container-health/container-health-metric-percentile-filter.png)

No exemplo seguinte, tenha em atenção para o nó *aks-nodepool-3977305*, o valor de **contentores** é 5, que é um rollup do número total de contentores implementados.

![Rollup de contentores, por exemplo de nó](./media/monitoring-container-health/container-health-nodes-containerstotal.png)

Ele pode ajudar a identificar rapidamente se tiver um equilíbrio correto de contentores entre nós do cluster. 

As informações que são apresentadas quando exibir nós são descritas na tabela a seguir:

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do anfitrião. |
| Estado | Vista de Kubernetes do Estado de nó. |
| Média&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Média de percentagem de nó com base no percentil durante o período selecionado. |
| AVG, Min, Max, 50th, 90th | Valor real de média de nós com base no percentil de, durante esse período de tempo selecionado. O valor médio é medido desde o limite de CPU/memória definido para um nó; para pods e contentores é o valor de média comunicado pelo host. |
| Contentores | Número de contentores. |
| Tempo de atividade | Representa o tempo, uma vez que um nó iniciado ou foi reiniciado. |
| Controladores | Apenas para os contentores e pods. Mostra qual controlador está residindo no. Nem todos os pods estão num controlador, para alguns podem apresentar **n/d**. | 
| Média de tendência&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Tendência de gráfico de barras apresenta a percentagem de métrica de percentil do controlador. |


No Seletor de, selecione **controladores**.

![Selecione controladores de exibição](./media/monitoring-container-health/container-health-controllers-tab.png)

Aqui pode ver o estado de funcionamento do desempenho dos seus controladores.

![Vista de desempenho de controladores do < nome >](./media/monitoring-container-health/container-health-controllers-view.png)

A hierarquia de linha começa com um controlador e expande o controlador. Ver um ou mais contentores. Expanda um pod e a última linha apresenta o contentor agrupado para o pod. 

As informações que são apresentadas ao ver controladores são descritas na tabela a seguir:

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador.|
| Estado | O estado de rollup dos contentores se foi concluída em execução com o estado, tal como *OK*, *Terminated*, *falha* *parado*, ou *Colocada em pausa*. Se o contentor está em execução, mas o estado foi um não está corretamente apresentados ou não foi capturado pelo agente e não tenha respondido a mais de 30 minutos, o estado é *desconhecido*. São fornecidos detalhes adicionais do ícone de estado na tabela abaixo.|
| Média&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Média de rollup da percentagem média de cada entidade para a métrica selecionada e percentil. |
| AVG, Min, Max, 50th, 90th  | Rollup da média da CPU millicore ou memória desempenho do contentor para o percentil selecionado. O valor médio é medido desde o limite de CPU/memória definido para um pod. |
| Contentores | Número total de contentores para o controlador ou pod. |
| Reinicia | Rollup da contagem de reinício de contentores. |
| Tempo de atividade | Representa o tempo desde o início de um contentor. |
| Nó | Apenas para os contentores e pods. Mostra qual controlador está residindo. | 
| Média de tendência&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;%| Tendência de gráfico de barras que representa a métrica de percentil do controlador. |

Os ícones no campo status indicam o estado online dos contentores:
 
| Ícone | Estado | 
|--------|-------------|
| ![Ícone de estado em execução pronto](./media/monitoring-container-health/container-health-ready-icon.png) | Em execução (pronto)|
| ![Ícone de estado aguardando ou em pausa](./media/monitoring-container-health/container-health-waiting-icon.png) | Aguardando ou em pausa|
| ![Reportou pela última vez com o ícone de estado](./media/monitoring-container-health/container-health-grey-icon.png) | Por fim comunicados em execução, mas ainda não responderam a mais de 30 minutos|
| ![Ícone de estado com êxito](./media/monitoring-container-health/container-health-green-icon.png) | Parado ou falha ao parar com êxito|

O ícone de estado apresenta uma contagem com base no que fornece o pod. Mostra os pior dois Estados, e quando paira o rato sobre o status, apresenta um Estado de rollup de todos os pods no contentor. Se não existir um estado estável, o valor de estado apresenta **(0)**. 

No Seletor de, selecione **contentores**.

![Ver os contentores selecionadas](./media/monitoring-container-health/container-health-containers-tab.png)

Aqui pode ver o estado de funcionamento do desempenho de seus contêineres.

![Vista de desempenho de controladores do < nome >](./media/monitoring-container-health/container-health-containers-view.png)

As informações que são apresentadas quando exibir contêineres são descritas na tabela a seguir:

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador.|
| Estado | Estado dos contentores, se aplicável. São fornecidos detalhes adicionais do ícone de estado na tabela seguinte.|
| Média&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | O rollup da percentagem média de cada entidade para a métrica selecionada e percentil. |
| AVG, Min, Max, 50th, 90th  | O rollup do média da CPU millicore ou memória desempenho do contentor para o percentil selecionado. O valor médio é medido desde o limite de CPU/memória definido para um pod. |
| Pod | Contentor onde reside o pod.| 
| Nó |  Nó onde reside o contentor. | 
| Reinicia | Representa o tempo desde o início de um contentor. |
| Tempo de atividade | Representa o tempo, uma vez que um contentor foi iniciado ou reiniciado. |
| Média de tendência&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Uma tendência de gráfico de barras que representa a percentagem média de métrica do contentor. |

Os ícones no campo status indicam os Estados online de pods, conforme descrito na tabela a seguir:
 
| Ícone | Estado | 
|--------|-------------|
| ![Ícone de estado em execução pronto](./media/monitoring-container-health/container-health-ready-icon.png) | Em execução (pronto)|
| ![Ícone de estado aguardando ou em pausa](./media/monitoring-container-health/container-health-waiting-icon.png) | Aguardando ou em pausa|
| ![Reportou pela última vez com o ícone de estado](./media/monitoring-container-health/container-health-grey-icon.png) | Por fim comunicados em execução, mas ainda não responderam em mais de 30 minutos|
| ![Ícone de estado terminada](./media/monitoring-container-health/container-health-terminated-icon.png) | Parado ou falha ao parar com êxito|
| ![Ícone de estado com falhas](./media/monitoring-container-health/container-health-failed-icon.png) | Estado de falha |

## <a name="container-data-collection-details"></a>Detalhes de recolha de dados de contentor
Estado de funcionamento do contentor recolhe diversos dados de registo e métricas de desempenho a partir de anfitriões de contentor e contentores. Dados são recolhidos a cada três minutos.

### <a name="container-records"></a>Registos de contentor

Exemplos de registos que são recolhidos por Estado de funcionamento do contentor e os tipos de dados que aparecem nos resultados de pesquisa de registos são apresentados na tabela a seguir:

| Tipo de dados | Tipo de dados na pesquisa de registos | Campos |
| --- | --- | --- |
| Desempenho para anfitriões e contentores | `Perf` | CounterName de computador, ObjectName, &#40;% de tempo do processador, o disco lê MB, disco escreve MB, MB de utilização de memória, rede receba Bytes, rede enviar Bytes, o processador de seg de utilização, rede&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventário de contentor | `ContainerInventory` | TimeGenerated, computador, nome do contentor, ContainerHostname, imagem, ImageTag, ContainerState, ExitCode, EnvironmentVar, comando, CreatedTime, StartedTime, FinishedTime, SourceSystem, ID do contentor, ImageID |
| Inventário de imagens de contentor | `ContainerImageInventory` | TimeGenerated, computador, imagem, ImageTag, ImageSize, VirtualSize, em execução, em pausa, parado, falha, SourceSystem, ImageID, TotalContainer |
| Registo de contentor | `ContainerLog` | TimeGenerated, o computador, o ID de imagem, o nome do contentor, LogEntrySource, LogEntry, SourceSystem, ID do contentor |
| Registo do serviço de contentor | `ContainerServiceLog`  | TimeGenerated, computador, TimeOfCommand, imagem, comando, SourceSystem, ID do contentor |
| Inventário de nó do contentor | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Processo de contentor | `ContainerProcess_CL` | TimeGenerated, computador, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventário de pods num cluster do Kubernetes | `KubePodInventory` | TimeGenerated, computador, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ID do contentor, ContainerName, nome, PodLabel, espaço de nomes, PodStatus, ClusterName PodIp, SourceSystem |
| Inventário de parte de nós de um cluster de Kubernetes | `KubeNodeInventory` | TimeGenerated, computador, ClusterName, ClusterId, LastTransitionTimeReady, etiquetas, estado, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents_CL` | TimeGenerated, computador, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, mensagem, SourceSystem | 
| Serviços no cluster do Kubernetes | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s ServiceType_s, SourceSystem | 
| Métricas de desempenho para a parte de nós de cluster do Kubernetes | Desempenho &#124; onde ObjectName = = "K8SNode" | CounterName de computador, ObjectName, &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de desempenho para a parte de contentores de cluster do Kubernetes | Desempenho &#124; onde ObjectName = = "K8SContainer" | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Registos de pesquisa para analisar dados
O log Analytics pode ajudá-lo a analisar as tendências, diagnosticar afunilamentos, previsão, ou correlacionar dados que podem ajudar a determinam se a configuração de cluster atual é satisfatória. Pesquisas de registos predefinidas são fornecidas por si para começar imediatamente a utilizar ou personalizar para retornar as informações da maneira como desejar. 

Pode efetuar análises interativas de dados na área de trabalho ao selecionar o **registos de eventos de Kubernetes do modo de exibição** ou **ver registos de contentor** opção no painel de pré-visualização. O **pesquisa de registos** é apresentada a página à direita da página do portal do Azure que estavam no.

![Analisar dados no Log Analytics](./media/monitoring-container-health/container-health-log-search-example.png)   

A saída de registos de contentor é reencaminhada para o Log Analytics são STDOUT e STDERR. Uma vez que o estado de funcionamento do contentor está a monitorizar o Kubernetes gerido pelo Azure (AKS), o sistema do Kube não é coletado hoje devido ao grande volume de dados gerados. 

### <a name="example-log-search-queries"></a>Consultas de pesquisa de registo de exemplo
Muitas vezes é útil criar consultas que começam com um ou dois exemplos em, em seguida, modificá-las para satisfazer as suas necessidades. Para ajudar a criar consultas mais avançadas, pode experimentar com as seguintes consultas de exemplo:

| Consulta | Descrição | 
|-------|-------------|
| ContainerInventory<br> &#124;Computador, nome, imagem, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime do projeto<br> &#124;tabela de composição | Listar todas as informações de ciclo de vida de um contentor| 
| KubeEvents_CL<br> &#124;onde not(isempty(Namespace_s))<br> &#124;Ordenar por TimeGenerated desc<br> &#124;tabela de composição | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124;resumir AggregatedValue = count () by imagem, ImageTag, está em execução | Inventário de imagens | 
| **No Advanced Analytics, selecione os gráficos de linhas**:<br> Desempenho<br> &#124;onde ObjectName = = "Contentor" e CounterName = = "% tempo do processador"<br> &#124;resumir AvgCPUPercent = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Contentor da CPU | 
| **No Advanced Analytics, selecione os gráficos de linhas**:<br> Desempenho &#124; onde ObjectName = = "Contentor" e CounterName = = "MB de utilização de memória"<br> &#124;resumir AvgUsedMemory = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Memória de contentor |

## <a name="how-to-stop-monitoring-with-container-health"></a>Como parar a monitorização com o estado de funcionamento do contentor
Se, depois de ativar a monitorização do seu contentor do AKS, decida que já não pretende monitorizá-lo, pode *para anular* utilizando modelos do Azure Resource Manager fornecidos com o cmdlet do PowerShell  **Novo-AzureRmResourceGroupDeployment** ou a CLI do Azure. Um modelo JSON Especifica a configuração para *para anular*. A outra contém valores de parâmetros que pode configurar para especificar o grupo de recurso e o ID de recurso de cluster para AKS que o cluster é implementado no. 

Se não estiver familiarizado com o conceito de implementar recursos com um modelo, consulte:
* [Implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Implementar recursos com modelos do Resource Manager e a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md)

Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente. Tem de executar a CLI do Azure versão 2.0.27 ou posterior. Para identificar a versão, execute `az --version`. Se precisar de instalar ou atualizar a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-template"></a>Criar e executar o modelo

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
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Guarde este ficheiro como **OptOutTemplate.json** para uma pasta local.
3. Cole a seguinte sintaxe JSON no seu ficheiro:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Edite os valores dos **aksResourceId** e **aksResourceLocation** utilizando os valores do cluster do AKS, que pode ser encontrado no **propriedades** página para o cluster selecionado .

    ![Página de propriedades do contentor](./media/monitoring-container-health/container-properties-page.png)

    Enquanto estiver a utilizar o **propriedades** página, também copiar a **ID de recurso da área de trabalho**. Este valor é necessário se decidir que pretende eliminar a área de trabalho do Log Analytics mais tarde. A eliminar a área de trabalho do Log Analytics não é efetuada como parte deste processo. 

5. Guarde este ficheiro como **OptOutParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Para utilizar os seguintes comandos do PowerShell na pasta que contém o modelo:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, é devolvida uma mensagem semelhante ao seguinte, que inclui o resultado:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Para executar seguinte comando com a CLI do Azure no Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, é devolvida uma mensagem semelhante ao seguinte, que inclui o resultado:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Se a área de trabalho foi criada apenas para suportar a monitorização do cluster e já não for necessário, terá de eliminar manualmente. Se não estiver familiarizado com como eliminar uma área de trabalho, consulte [eliminar uma área de trabalho do Log Analytics do Azure com o portal do Azure](../log-analytics/log-analytics-manage-del-workspace.md). Não se esqueça da **ID de recurso da área de trabalho** copiou anteriormente no passo 4, vai precisar que. 

## <a name="troubleshooting"></a>Resolução de problemas
Esta seção fornece informações para ajudar a resolver problemas com o estado de funcionamento do contentor.

Se o estado de funcionamento do contentor foi ativado e configurado com êxito, mas não pode ver informações de estado ou resultados no Log Analytics ao efetuar uma pesquisa de registos, pode ajudar a diagnosticar o problema efetuando o seguinte procedimento: 

1. Verifique o estado do agente executando o seguinte comando: 

    `kubectl get ds omsagent --namespace=kube-system`

    A saída deve assemelhar-se o seguinte, que indica que foi implementado devidamente:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Verifique o estado de implementação de solução com a versão do agente *06072018* ou posterior, executando o seguinte comando:

    `kubectl get deployment omsagent-rs -n=kube-system`

    A saída deve assemelhar-se o seguinte, que indica que foi implementado devidamente:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Verifique o estado de pod para verificar que está em execução ao executar o seguinte comando: `kubectl get pods --namespace=kube-system`

    A saída deve assemelhar-se o seguinte com o estado *em execução* para o omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Verifique os registos do agente. Quando o agente em contentores é implementado, ele executa uma verificação rápida ao executar comandos OMI e apresenta a versão do agente e fornecedor. 

5. Para verificar se o agente foram carregadas com êxito, execute o seguinte comando: `kubectl logs omsagent-484hw --namespace=kube-system`

    O estado deve assemelhar-se ao seguinte:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Passos Seguintes

Para ver informações de desempenho de estado de funcionamento e a aplicação de contentor detalhadas, consulte [pesquisar registos](../log-analytics/log-analytics-log-search.md). 
