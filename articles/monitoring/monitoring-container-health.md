---
title: Monitorizar estado de funcionamento do Azure Kubernetes Service (AKS) (pré-visualização) | Documentos da Microsoft
description: Este artigo descreve como pode ver facilmente o desempenho do seu contentor do AKS para compreender rapidamente a utilização de seu ambiente alojado do Kubernetes.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/08/2018
ms.author: magoedte
ms.openlocfilehash: a94f7289c75a4f4d466542c608d81cf5b954f4b1
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37917348"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Monitorizar estado de funcionamento do Azure Kubernetes Service (AKS) contentor (pré-visualização)

Este artigo descreve como configurar e utilizar o estado de funcionamento do contentor do Azure Monitor para monitorizar o desempenho das cargas de trabalho implementadas nos ambientes do Kubernetes alojados no Azure Kubernetes Service (AKS).  É fundamental monitorizar o cluster e contentores do Kubernetes, especialmente ao executar um cluster de produção em escala, com várias aplicações.

Estado de funcionamento do contentor-lhe capacidade por memória de coleta e métricas de processador de controladores, nós e contentores disponíveis no Kubernetes por meio da API de métricas de monitorização do desempenho.  Depois de ativar o estado de funcionamento do contentor, estas métricas são automaticamente recolhidas para si com uma versão em contentores do agente do OMS para Linux e armazenadas no seu [do Log Analytics](../log-analytics/log-analytics-overview.md) área de trabalho.  As vistas predefinidas incluídas mostram as cargas de trabalho de contentor que residem e para que possa entender o que está a afetar o estado de funcionamento do desempenho do cluster de Kubernetes:  

* Que os contentores em execução no nó e a utilização de processador e memória média para identificar afunilamentos de recursos
* Identificar em que o contentor reside num controlador de e/ou os pods para ver o desempenho geral de um controlador ou de pod 
* Reveja a utilização de recursos de cargas de trabalho em execução no anfitrião não relacionadas com os processos padrão que suporta o pod
* Compreender o comportamento do cluster sob carga média e mais pesado para ajudar a identificar as necessidades de capacidade e determinar a carga máxima possa suportar 

Se estiver interessado na monitorização e gestão de Docker e o Windows anfitriões de contentores para ver a configuração, auditoria e utilização de recursos, consulte a [solução de monitorização de contentores](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Requisitos 
Antes de começar, reveja os seguintes detalhes para que possa entender os pré-requisitos suportados.

- Um cluster do AKS novo ou existente
- Um agente do OMS em contentores para a versão do Linux microsoft / oms:ciprod04202018 e mais tarde. O número de versão é representado por uma data seguindo o formato: *mmddyyyy*.  Ele é instalado automaticamente durante a integração do Estado de funcionamento do contentor.  
- Uma área de trabalho do Log Analytics.  Ele pode ser criado quando ativa a monitorização do seu novo cluster do AKS, ou pode criar a conta [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), ou a partir do [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).
- Membro da função de Contribuidor do Log Analytics para ativar a monitorização de contentores.  Para obter mais informações sobre como controlar o acesso a uma área de trabalho do Log Analytics, consulte [gerir áreas de trabalho](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Componentes 

Esse recurso depende de um agente em contentores do OMS para Linux recolher dados de eventos de desempenho e de todos os nós do cluster.  O agente automaticamente implementar e registar com a área de trabalho do Log Analytics especificada depois de ativar a monitorização de contentores. 

>[!NOTE] 
>Se já tiver implementado um cluster do AKS, ativa a monitorização utilizando um modelo Azure Resource Manager fornecido, conforme demonstrado neste artigo. Não é possível utilizar `kubectl` para atualizar, eliminar, implemente novamente ou implementar o agente.  
>

## <a name="sign-in-to-azure-portal"></a>Iniciar sessão no Portal do Azure
Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Ativar a monitorização de estado de funcionamento de contentor para um novo cluster
Pode ativar a monitorização de um novo cluster do AKS durante a implementação do portal do Azure.  Siga os passos no artigo guia de introdução [implementar um cluster do Azure Kubernetes Service (AKS)](../aks/kubernetes-walkthrough-portal.md).  Quando estiver a utilizar o **monitorização** página, selecione **Sim** para a opção **Ativar monitorização** para ativar e, em seguida, selecione um existente ou criar uma nova área de trabalho do Log Analytics.  

Depois de a monitorização está ativada, todas as tarefas de configuração são concluídas com êxito, pode monitorizar o desempenho do seu cluster a partir de uma das seguintes formas:

1. Diretamente a partir do cluster do AKS, selecionando **estado de funcionamento** no painel à esquerda.<br><br> 
2. Ao clicar no **monitorizar o estado de funcionamento do contentor** mosaico na página de cluster do AKS para o cluster selecionado.  No Azure Monitor, selecione **estado de funcionamento** no painel à esquerda.  

![Opções para selecionar o estado de funcionamento do contentor no AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Depois de monitorização estiver ativada, pode demorar cerca de 15 minutos antes de poder ver dados operacionais para o cluster.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Ativar a monitorização de estado de funcionamento de contentor para os clusters geridos existentes
Pode ativar a monitorização de um cluster do AKS já implementado a partir do portal do Azure ou com o modelo Azure Resource Manager fornecido com o cmdlet do PowerShell **New-AzureRmResourceGroupDeployment** ou da CLI do Azure.  


### <a name="enable-from-azure-portal"></a>Ativar a partir do portal do Azure
Execute os seguintes passos para ativar a monitorização do seu contentor do AKS no portal do Azure.

1. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **contentores**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **serviços de Kubernetes**.<br><br> ![Portal do Azure](./media/monitoring-container-health/azure-portal-01.png)<br><br>  
2. Na lista de contentores, selecione um contentor.
3. Na página de descrição geral do contentor, selecione **monitorizar o estado de funcionamento do contentor** e o **inclusão de registos e de estado de funcionamento do contentor** é apresentada a página.
4. Na **inclusão de registos e de estado de funcionamento do contentor** página, se tiver um existentes do Log Analytics a área de trabalho na mesma subscrição que o cluster, selecione-o na lista pendente.  A lista preselects área de trabalho predefinida e localização do contentor do AKS é implementada na subscrição.<br><br> ![Ativar a monitorização de estado de funcionamento de contentor do AKS](./media/monitoring-container-health/container-health-enable-brownfield-02.png) 

>[!NOTE]
>Se quiser criar uma nova área de trabalho do Log Analytics para armazenar os dados de monitorização do cluster, siga os passos em [Cretae uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md) e certifique-se de que criar a área de trabalho na mesma subscrição que é o contentor do AKS implementado.  
>
 
Depois de monitorização estiver ativada, pode demorar cerca de 15 minutos antes de poder ver dados operacionais para o cluster. 

### <a name="enable-using-azure-resource-manager-template"></a>Ativar a utilizar o modelo Azure Resource Manager
Esse método inclui dois modelos de JSON, um modelo especifica a configuração para ativar a monitorização e o modelo JSON contém valores de parâmetro que configurar para especificar o seguinte:

* ID de recurso de contentor do AKS 
* Grupo de recursos do cluster é implementado no 
* Área de trabalho do log Analytics e a região para criar a área de trabalho 

A área de trabalho do Log Analytics tem de ser criadas manualmente.  Para criar a área de trabalho, pode configurar um através de [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), das [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).

Se não estiver familiarizado com os conceitos de implementar recursos com um modelo com o PowerShell, veja [implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)ou para ver a CLI do Azure, [implementar recursos com Modelos do Resource Manager e a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente.  É necessário que está a executar a CLI do Azure versão 2.0.27 ou posterior. Executar `az --version` para identificar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-template"></a>Criar e executar o modelo

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
3. Copie e cole a seguinte sintaxe JSON no seu ficheiro:

    ```json
    {
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscroptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
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

4. Editar o valor para **aksResourceId**, **aksResourceLocation** pelos valores que pode ser encontrado no **descrição geral do AKS** página para o cluster do AKS.  O valor para **workspaceResourceId** é a ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho.  Também de especificar a localização da área de trabalho destina-se no **workspaceRegion**.    
5. Guarde este ficheiro como **existingClusterParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Utilize os seguintes comandos do PowerShell a partir da pasta que contém o modelo:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        A alteração de configuração pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem semelhante ao seguinte, que inclui o resultado:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Para executar seguinte comando com a CLI do Azure no Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem semelhante ao seguinte, que inclui o resultado:

        ```azurecli
        provisioningState       : Succeeded
        ```
Depois de monitorização estiver ativada, pode demorar cerca de 15 minutos antes de poder ver dados operacionais para o cluster.  

## <a name="verify-agent-and-solution-deployment"></a>Verificar a implementação de agente e solução
Com a versão do agente *06072018* e superior, é possível verificar que o agente e a solução foram implementados com êxito.  Com as versões anteriores do agente, apenas pode verificar a implementação do agente.

### <a name="agent-version-06072018-and-higher"></a>Versão do agente 06072018 e superior
Execute o seguinte comando para verificar se que o agente é implementado com êxito.   

```
kubectl get ds omsagent --namespace=kube-system
```

A saída deve assemelhar-se da indicar seguintes implantou corretamente:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Para verificar a implementação da solução, execute o seguinte comando:

```
kubectl get deployment omsagent-rs -n=kube-system
```

A saída deve assemelhar-se da indicar seguintes implantou corretamente:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Versão de agente anteriores ao 06072018

Para verificar a versão do agente OMS lançada antes *06072018* está implementado corretamente, execute o seguinte comando:  

```
kubectl get ds omsagent --namespace=kube-system
```

A saída deve assemelhar-se da indicar seguintes implantou corretamente:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Utilização de desempenho do Vista
Quando abre o estado de funcionamento do contentor, a página apresenta imediatamente a utilização de desempenho de seus nós de cluster.  Ver informações sobre o seu cluster do AKS é organizado em três perspectivas:

- Nós 
- Controladores  
- Contentores

A hierarquia de linha segue o modelo de objeto do Kubernetes a partir do nó do cluster.  Expanda o nó e vir um ou mais pods em execução no nó e, se houver mais de um contêiner agrupado para um pod, eles são apresentados como a última linha na hierarquia.<br><br> ![Hierarquia de nós do Kubernetes de exemplo na vista de desempenho](./media/monitoring-container-health/container-performance-and-health-view-03.png)

Pode selecionar os controladores ou contentores na parte superior da página e reveja a utilização de estado e de recursos para esses objetos.  Utilize as caixas de lista pendente na parte superior do ecrã para filtrar por espaço de nomes, o serviço e o nó. Se em vez disso, pretende rever a utilização da memória, do **métrica** selecione na lista pendente **memória RSS** ou **conjunto de trabalho de memória**.  **Memória RSS** só é suportado para Kubernetes versão 1.8 e posterior. Caso contrário, verá valores para **AVG %** que mostra como *NaN %*, que é um valor de tipo de dados numéricos que representa um valor indefinido ou não representável. 

![Vista de desempenho de nós de desempenho de contentor](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Por predefinição, os dados de desempenho se baseia em seis últimas horas, mas pode alterar a janela com o **intervalo de tempo** na lista pendente encontrado no canto superior direito da página. Neste momento, a página não atualização automática, por isso terá de atualizá-lo manualmente. 

No exemplo a seguir, tenha em atenção para o nó *aks-agentpool-3402399-0*, o valor de **contentores** é 10, que é um rollup do número total de contentores implementados.<br><br> ![Rollup de contentores, por exemplo de nó](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Ele pode ajudar a identificar rapidamente se não tiver um equilíbrio correto de contentores entre nós do cluster.  

A tabela seguinte descreve as informações apresentadas ao ver nós.

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do anfitrião |
| Estado | Vista de Kubernetes do Estado de nó |
| % DA MÉDIA | Média de percentagem de nó com base na métrica selecionada durante o período de tempo selecionado. |
| MÉDIA | Média de valor real de nós com base na métrica selecionada durante o período de tempo selecionado.  O valor médio é medido desde o limite de CPU/memória definido para um nó; para pods e contentores é o valor de média comunicado pelo host. |
| Contentores | Número de contentores. |
| Tempo de atividade | Representa o tempo, uma vez que um nó iniciado ou foi reiniciado. |
| Pod | Apenas para contentores. Ela mostra que pods-lo que residem. |
| Controladores | Apenas para os contentores e pods. Mostra qual controlador está residindo. Nem todos os pods estará num controlador, para que alguns podem mostrar n/d. | 
| % Da média de tendência | Tendência de gráfico de barras com base no contentor e o nó % de métrica de média. |


No Seletor, escolha **controladores**.<br><br> ![Selecione controladores de exibição](./media/monitoring-container-health/container-performance-and-health-view-08.png)

Aqui pode ver o estado de funcionamento do desempenho dos seus controladores.<br><br> ![Vista de desempenho de controladores do < nome >](./media/monitoring-container-health/container-performance-and-health-view-05.png)

A hierarquia de linha começa com um controlador e expande o controlador e ver os pods de um ou mais, ou um ou mais contentores.  Expanda um pod e a última linha mostram o contentor agrupado para o pod.  

A tabela seguinte descreve as informações apresentadas ao ver controladores.

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador|
| Estado | Estado dos contentores se foi concluída em execução com o estado, tal como *Terminated*, *com falhas* *parado*, ou *em pausa*. Se o contentor está em execução, mas o estado foi um não está corretamente apresentados ou não foi capturado pelo agente e não tenha respondido a mais de 30 minutos, o estado será *desconhecido*. |
| % DA MÉDIA | Agregar média de média de % de cada entidade para a métrica selecionada. |
| MÉDIA | Agregação do média da CPU millicore ou memória desempenho do contentor.  O valor médio é medido desde o limite de CPU/memória definido para um pod. |
| Contentores | Número total de contentores para o controlador ou pod. |
| Reinicia | Agregação da contagem de reinício de contentores. |
| Tempo de atividade | Representa o tempo desde o início de um contentor. |
| Pod | Apenas para contentores. Ela mostra que pods-lo que residem. |
| Nó | Apenas para os contentores e pods. Mostra qual controlador está residindo. | 
| % Da média de tendência | Tendência de gráfico de barras, apresentando a média de % métrica do contentor. |

No Seletor, escolha **contentores**.<br><br> ![Ver os contentores selecionadas](./media/monitoring-container-health/container-performance-and-health-view-09.png)

Aqui podemos ver o estado de funcionamento do desempenho de seus contêineres.<br><br> ![Vista de desempenho de controladores do < nome >](./media/monitoring-container-health/container-performance-and-health-view-06.png)

A tabela seguinte descreve as informações apresentadas ao ver contentores.

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador|
| Estado | Agregação do estado dos contentores, se houver. |
| % DA MÉDIA | Agregar média de média de % de cada entidade para a métrica selecionada. |
| MÉDIA | Agregação do média da CPU millicore ou memória desempenho do contentor. O valor médio é medido desde o limite de CPU/memória definido para um pod. |
| Contentores | Número total de contentores para o controlador.|
| Reinicia | Representa o tempo desde o início de um contentor. |
| Tempo de atividade | Representa o tempo, uma vez que um contentor foi iniciado ou reiniciado. |
| Pod | Informações de pod onde reside. |
| Nó |  Nó onde reside o contentor.  | 
| % Da média de tendência | Tendência de gráfico de barras, apresentando a média de % métrica do contentor. |

## <a name="container-data-collection-details"></a>Detalhes de recolha de dados de contentor
Estado de funcionamento do contentor recolhe diversos dados de registo e métricas de desempenho a partir de anfitriões de contentor e contentores. Dados são recolhidos a cada três minutos.

### <a name="container-records"></a>Registos de contentor

A tabela seguinte mostra exemplos de registos recolhidos pelo Estado de funcionamento do contentor e os tipos de dados que aparecem nos resultados de pesquisa de registo.

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
O log Analytics pode ajudá-lo a analisar as tendências, diagnosticar afunilamentos, previsão, ou correlacionar dados que podem ajudar a determinam se a configuração de cluster atual é satisfatória.  Pesquisas de registos predefinidas são fornecidas para começar imediatamente a utilizar ou personalizar para retornar as informações da maneira como desejar. 

Pode efetuar análises interativas de dados na área de trabalho ao selecionar o **ver registo** opção, disponível na extremidade direita quando expande um contentor.  **Pesquisa de registos** página é exibida bem acima a página onde estava no portal.<br><br> ![Analisar dados no Log Analytics](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

A saída de registos de contentor reencaminhada para o Log Analytics são STDOUT e STDERR. Uma vez que o estado de funcionamento do contentor está a monitorizar o Kubernetes gerido do Azure (AKS), o sistema do Kube não é coletado hoje devido ao grande volume de dados gerados.     

### <a name="example-log-search-queries"></a>Consultas de pesquisa de registo de exemplo
Muitas vezes é útil criar consultas, começando com um ou dois exemplos e, em seguida, modificá-los para satisfazer as suas necessidades. Pode experimentar com as seguintes consultas de exemplo para ajudar a criar consultas mais avançadas.

| Consulta | Descrição | 
|-------|-------------|
| ContainerInventory<br> &#124;Computador, nome, imagem, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime do projeto<br> &#124;tabela de composição | Listar o ciclo de vida do contentor de todas as informações| 
| KubeEvents_CL<br> &#124;onde not(isempty(Namespace_s))<br> &#124;Ordenar por TimeGenerated desc<br> &#124;tabela de composição | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124;resumir AggregatedValue = count () by imagem, ImageTag, está em execução | Inventário de imagens | 
| **No Advanced Analytics, selecione os gráficos de linhas**:<br> Desempenho<br> &#124;onde ObjectName = = "Contentor" e CounterName = = "% tempo do processador"<br> &#124;resumir AvgCPUPercent = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Contentor da CPU | 
| **No Advanced Analytics, selecione os gráficos de linhas**:<br> Desempenho &#124; onde ObjectName = = "Contentor" e CounterName = = "MB de utilização de memória"<br> &#124;resumir AvgUsedMemory = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Memória de contentor |

## <a name="how-to-stop-monitoring-with-container-health"></a>Como parar a monitorização com o estado de funcionamento do contentor
Depois de ativar a monitorização do seu contentor do AKS, decida que já não pretenda monitorizá-lo, pode *para anular* utilizar modelos do Azure Resource Manager fornecidos com o cmdlet do PowerShell  **Novo-AzureRmResourceGroupDeployment** ou a CLI do Azure.  Um modelo JSON Especifica a configuração para *para anular* e o modelo JSON contém valores de parâmetro que configurar para especificar o AKS cluster recurso ID de grupo de recursos e o cluster é implementado no.  Se não estiver familiarizado com os conceitos de implementar recursos com um modelo com o PowerShell, veja [implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) ou para ver a CLI do Azure, [implementar recursos com Modelos do Resource Manager e a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente.  É necessário que está a executar a CLI do Azure versão 2.0.27 ou posterior. Executar `az --version` para identificar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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
              "config": {
                "logAnalyticsWorkspaceResourceID": null
              }
            }
           }
         }
       }
      ]
    }
    ```

2. Guarde este ficheiro como **OptOutTemplate.json** para uma pasta local.
3. Copie e cole a seguinte sintaxe JSON no seu ficheiro:

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

4. Editar o valor para **aksResourceId** e **aksResourceLocation** pelos valores do cluster do AKS, que pode ser encontrado no **propriedades** página para o cluster selecionado.<br><br> ![Página de propriedades do contentor](./media/monitoring-container-health/container-properties-page.png)<br>

    Enquanto estiver a utilizar o **propriedades** página, também copiar a **ID de recurso da área de trabalho**.  Este valor é necessário se decidir que pretende eliminar a Log Analytics área de trabalho mais tarde, que não é executada como parte deste processo.  

5. Guarde este ficheiro como **OptOutParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Para utilizar os seguintes comandos do PowerShell a partir da pasta que contém o modelo:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando tiver concluído, é devolvida uma mensagem semelhante ao seguinte, que inclui o resultado:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Para executar seguinte comando com a CLI do Azure no Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando tiver concluído, é devolvida uma mensagem semelhante ao seguinte, que inclui o resultado:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Se a área de trabalho foi criada apenas para suportar a monitorização do cluster e já não for necessário, terá de eliminar manualmente. Se não estiver familiarizado com como eliminar uma área de trabalho, consulte [eliminar uma área de trabalho do Log Analytics do Azure com o portal do Azure](../log-analytics/log-analytics-manage-del-workspace.md).  Não se esqueça da **ID de recurso da área de trabalho** copiou anteriormente no passo 4, vai precisar que.  

## <a name="troubleshooting"></a>Resolução de problemas
Esta seção fornece informações para ajudar a resolver problemas com o estado de funcionamento do contentor.

Se o estado de funcionamento do contentor foi ativado e configurado com êxito, mas são não vê quaisquer informações de estado ou os resultados no Log Analytics ao efetuar uma pesquisa de registos, pode realizar os passos seguintes para ajudar a diagnosticar o problema.   

1. Verifique o estado do agente executando o seguinte comando: 

    `kubectl get ds omsagent --namespace=kube-system`

    A saída deve assemelhar-se da indicar seguintes implantou corretamente:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Verifique o estado de implementação de solução com a versão do agente *06072018* ou superior, executando o seguinte comando:

    `kubectl get deployment omsagent-rs -n=kube-system`

    A saída deve assemelhar-se da indicar seguintes implantou corretamente:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Verifique o estado de pod para verificar se que está a ser executado ou não executando o seguinte comando: `kubectl get pods --namespace=kube-system`

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

4. Verifique os registos do agente. Quando o agente em contentores é implementado, ele executa uma verificação rápida ao executar comandos OMI e mostra a versão do agente e fornecedor de Docker. Para ver que o agente foi carregada com êxito, execute o seguinte comando: `kubectl logs omsagent-484hw --namespace=kube-system`

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

[Pesquisar registos](../log-analytics/log-analytics-log-search.md) para ver informações de desempenho de estado de funcionamento e a aplicação de contentor detalhadas.  