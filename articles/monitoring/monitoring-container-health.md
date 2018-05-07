---
title: Monitorizar estado de funcionamento do serviço de Kubernetes do Azure (AKS) (pré-visualização) | Microsoft Docs
description: Este artigo descreve como facilmente pode rever o desempenho do seu contentor AKS rapidamente compreender a utilização do seu ambiente alojado do Kubernetes.
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
ms.date: 04/30/2018
ms.author: magoedte
ms.openlocfilehash: f0501d4404375ee44b96ae4514c15e69b616d38a
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Monitorizar estado de funcionamento do serviço de Kubernetes do Azure (AKS) contentor (pré-visualização)

Este artigo descreve como configurar e utilizar o estado de funcionamento do Monitor de Azure contentor para monitorizar o desempenho das cargas de trabalho implementadas para ambientes de Kubernetes alojados no serviço do Azure Kubernetes (AKS).  É fundamental monitorizar o cluster e contentores do Kubernetes, especialmente ao executar um cluster de produção em escala, com várias aplicações.

Estado de funcionamento do contentor dá-lhe capacidade por memória de recolher e métricas de processador de controladores, nós e contentores disponíveis no Kubernetes através da API de métricas de monitorização de desempenho.  Depois de ativar o estado de funcionamento do contentor, estas métricas são recolhidas para a utilizar uma versão do agente do OMS para Linux e automaticamente armazenadas no seu [Log Analytics](../log-analytics/log-analytics-overview.md) área de trabalho.  As vistas predefinidas incluídas mostram as cargas de trabalho do contentor residing e para que possa compreender o que está a afetar o estado de funcionamento do desempenho do Kubernetes cluster:  

* Os contentores estão em execução no nó e a utilização de processador e memória média para identificar estrangulamentos de recursos
* Identificar onde reside o contentor no controlador de e/ou pods para ver o desempenho global para um controlador ou pod 
* Reveja a utilização de recursos das cargas de trabalho em execução no anfitrião não relacionados para os processos padrão que suportam o pod
* Compreender o comportamento do cluster em carga média e mais pesado para ajudar a identificar as necessidades de capacidade e determinar a carga máxima que pode suportar 

Se estiver interessado na monitorização e gestão dos seus Docker e Windows anfitriões de contentor para a configuração de vista, auditoria e utilização de recursos, consulte o [solução de monitorização do contentor](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Requisitos 
Antes de começar, reveja os detalhes seguintes para que possa compreender os pré-requisitos suportados.

- São suportadas as seguintes versões do AKS cluster: 1.7.7 para 1.9.6.
- Um agente OMS de para a versão do Linux microsoft / oms:ciprod04202018 e mais tarde. Este agente é instalado automaticamente durante a integração do Estado de funcionamento do contentor.  
- Uma área de trabalho de análise de registos.  Podem ser criado quando ativar a monitorização do novo cluster AKS ou pode criar um através de [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), ou a partir de [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).


## <a name="components"></a>Componentes 

Esta funcionalidade baseia-se num agente OMS de para Linux recolher dados de eventos de desempenho e de todos os nós do cluster.  O agente é automaticamente implementado e registado com a área de trabalho de análise de registos especificada depois de ativar a monitorização do contentor. 

>[!NOTE] 
>Se já tiver implementado um cluster AKS, que ativa a monitorização utilizando um modelo Azure Resource Manager fornecido como demonstrados mais à frente neste artigo. Não é possível utilizar `kubectl` para atualizar, eliminar, implemente novamente ou implementar o agente.  
>

## <a name="log-in-to-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Ativar a monitorização de estado de funcionamento de contentor para um novo cluster
Só pode ativar a monitorização do seu cluster AKS ao implementar a partir do portal do Azure.  Siga os passos no artigo do início rápido [implementar um cluster do serviço de Kubernetes do Azure (AKS)](../aks/kubernetes-walkthrough-portal.md).  Quando estiver no **monitorização** página, selecione **Sim** para a opção **Ativar monitorização** para ativar e, em seguida, selecione um existente ou criar uma nova área de trabalho de análise de registos.  

Depois de ativar a monitorização de todas as tarefas de configuração são concluídas com êxito, pode monitorizar o desempenho do seu cluster de uma das seguintes formas:

1. Diretamente do cluster AKS selecionando **estado de funcionamento** no painel esquerdo.<br><br> 
2. Ao clicar no **monitorizar estado de funcionamento do contentor** mosaico na página de cluster AKS para o cluster selecionado.  No Monitor do Azure, selecione **estado de funcionamento** no painel esquerdo.  

![Opções para selecionar o estado de funcionamento do contentor no AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Depois de monitorização estiver ativada, pode demorar cerca de 15 minutos antes de conseguir ver dados operacionais para o cluster.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Ativar a monitorização de estado de funcionamento de contentor para clusters geridos existentes
Ativar a monitorização do seu contentor AKS já implementado não pode ser efetuada no portal, só pode ser efetuado utilizando o modelo Azure Resource Manager fornecido com o cmdlet do PowerShell **New-AzureRmResourceGroupDeployment** ou a CLI do Azure.  Um modelo JSON Especifica a configuração para ativar a monitorização e o modelo JSON contém valores de parâmetro que configurar para especificar o seguinte:

* ID de recurso de contentor AKS 
* Grupo de recursos do cluster é implementado no 
* Área de trabalho de análise de registo e a região para criar a área de trabalho 

A área de trabalho de análise de registos tem de ser criadas manualmente.  Para criar a área de trabalho, pode configurar um através de [do Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), do [portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).

Se não estiver familiarizado com conceitos de implementar recursos através de um modelo com o PowerShell, consulte [implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)ou para ver a CLI do Azure, [implementar recursos com o Modelos do Resource Manager e a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

Se optar por utilizar a CLI do Azure, terá primeiro de instalar e utilizar a CLI localmente.  É necessário que está a executar a CLI do Azure versão 2.0.27 ou posterior. Executar `az --version` para identificar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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
           "description": "AKS Cluster resource id"
        }
    },
    "aksResourceLocation": {
      "type": "string",
      "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
        }
      },
      "workspaceId": {
        "type": "string",
        "metadata": {
          "description": "Azure Monitor Log Analytics resource id"
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
                "logAnalyticsWorkspaceResourceID": "[parameters('workspaceId')]"
              }
            }
          }
        }
      },
      {
            "type": "Microsoft.Resources/deployments",
            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceId'),'/')[8], ')')]",
            "apiVersion": "2017-05-10",
            "subscriptionId": "[split(parameters('workspaceId'),'/')[2]]",
            "resourceGroup": "[split(parameters('workspaceId'),'/')[4]]",
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
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceId'),'/')[8], ')')]",
                            "properties": {
                                "workspaceResourceId": "[parameters('workspaceId')]"
                            },
                            "plan": {
                                "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceId'),'/')[8], ')')]",
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
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
        },
        "aksResourceLocation": {
          "value": "East US"
        },
        "workspaceId": {
          "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
        },
        "workspaceRegion": {
          "value": "eastus"
        }
      }
    }
    ```

4. Edite o valor para **aksResourceId**, **aksResourceLocation** com os valores que pode encontrar no **descrição geral de AKS** página para o cluster AKS.  O valor para **workspaceId** deve ser o nome de uma área de trabalho de análise de registos e especifique a localização de área de trabalho é criada para **workspaceRegion**.    
5. Guarde este ficheiro como **existingClusterParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Utilize os seguintes comandos do PowerShell a partir da pasta que contém o modelo:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        A alteração de configuração pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem semelhante ao seguinte que inclui o resultado:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Para executar os seguintes comandos com a CLI do Azure no Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem semelhante ao seguinte que inclui o resultado:

        ```azurecli
        provisioningState       : Succeeded
        ```
Depois de monitorização estiver ativada, pode demorar cerca de 15 minutos antes de conseguir ver dados operacionais para o cluster.  

## <a name="verify-agent-deployed-successfully"></a>Verifique se o agente foi implementado com êxito
Para verificar o agente do OMS implementado corretamente, execute o seguinte comando: ` kubectl get ds omsagent -—namespace=kube-system`.

A saída deve assemelhar-se a seguinte que indica que foi implementada corretamente:

```
User@aksuser:~$ kubectl get ds omsagent -—namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Ver utilização de desempenho
Quando abrir o estado de funcionamento do contentor, a página apresenta imediatamente a utilização de desempenho dos seus nós de cluster.  Ver informações sobre o cluster AKS está organizada nas três perspetivas:

- Nós 
- Controladores  
- Contentores

A hierarquia de linha segue o modelo de objeto de Kubernetes começados por um nó do cluster.  Expanda o nó e vir um ou mais pods em execução no nó e, se existir mais do que um contentor agrupado para um pod, são apresentadas como a última linha na hierarquia.<br><br> ![Exemplo de hierarquia de nó Kubernetes na vista de desempenho](./media/monitoring-container-health/container-performance-and-health-view-03.png)

Pode selecionar os controladores ou de contentores da parte superior da página e reveja a utilização de estado e de recursos para esses objetos.  Utilize as caixas de lista pendente, na parte superior do ecrã, para filtrar por espaço de nomes, o serviço e o nó. Se em vez disso, pretende rever utilização da memória, do **métrica** selecione na lista pendente **memória RSS** ou **conjunto de trabalho de memória**.  **Memória RSS** só é suportada para Kubernetes versão 1.8 e posterior. Caso contrário, verá os valores para **médio %** que mostra como *NaN %*, que é um valor de tipo de dados numéricos que representa um valor indefinido ou unrepresentable. 

![Vista de desempenho de nós de desempenho de contentor](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Por predefinição, dados de desempenho baseia-se em seis últimas horas, mas pode alterar a janela com o **intervalo de tempo** encontrado na lista pendente no canto superior direito da página. Neste momento, a página não atualização automática, por isso terá de atualizá-lo manualmente. 

No exemplo seguinte, repare para o nó *aks-agentpool-3402399-0*, o valor para **contentores** é 10, que é um rollup do número total de contentores implementado.<br><br> ![Rollup de contentores, por exemplo de nó](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Isto pode ajudar a identificar rapidamente se não tiver equilíbrio adequado de contentores entre os nós do cluster.  

A tabela seguinte descreve as informações apresentadas ao ver nós.

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do anfitrião |
| Estado | Kubernetes vista do Estado do nó |
| % DE MÉDIA | Percentagem média nó com base numa métrica selecionada para a duração de tempo selecionado. |
| MÉDIA | Valor de real médio nós com base numa métrica selecionada para a duração de tempo selecionado.  O valor médio é medido desde o limite de memória e CPU definido para um nó; para pods e contentores é o valor médio comunicado pelo anfitrião. |
| Contentores | Número de contentores. |
| Tempo de atividade | Representa o tempo, uma vez que um nó foi iniciado ou foi reiniciado. |
| Pod | Apenas para contentores. Mostra que pods-que reside. |
| Controladores | Apenas para contentores e pods. Mostra o controlador é a que reside. Pods de nem todas as estará num controlador, pelo que alguns podem mostrar n/d. | 
| % De média de tendência | Tendência de gráfico de barras com base no contentor e o nó % de métrica de Méd. |


No Seletor de escolha **controladores**.<br><br> ![Selecione controladores vista](./media/monitoring-container-health/container-performance-and-health-view-08.png)

Aqui pode ver o estado de funcionamento do desempenho dos seus controladores.<br><br> ![< nome > Vista de desempenho de controladores](./media/monitoring-container-health/container-performance-and-health-view-05.png)

A hierarquia de linha começa com um controlador e expande o controlador e vir um ou mais pods ou um ou mais contentores.  Expanda um pod e a última linha Mostrar o contentor agrupado para o pod.  

A tabela seguinte descreve as informações apresentadas ao ver controladores.

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador de|
| Estado | Estado dos contentores quando foi concluída em execução com o estado, tal como *Terminated*, *falha* *parado*, ou *em pausa*. Se o contentor está em execução, mas o estado era: não está corretamente apresentados ou não foi captado pelo agente e não respondeu mais de 30 minutos, o estado será *desconhecido*. |
| % DE MÉDIA | Rollup de média de média de % de cada entidade para a métrica selecionada. |
| MÉDIA | Agregação do média da CPU millicore ou memória desempenho do contentor.  O valor médio é medido desde o limite de memória e CPU definido para um pod. |
| Contentores | Número total de contentores para o controlador ou pod. |
| Reinicia | Agregação da contagem de reinício de contentores. |
| Tempo de atividade | Representa o tempo desde o início de um contentor. |
| Pod | Apenas para contentores. Mostra que pods-que reside. |
| Nó | Apenas para contentores e pods. Mostra o controlador é a que reside. | 
| % De média de tendência | Tendência de gráfico de barras apresentar média % métrica do contentor. |

No Seletor de escolha **contentores**.<br><br> ![Vista de selecionar contentores](./media/monitoring-container-health/container-performance-and-health-view-09.png)

Aqui, pode ver o estado de funcionamento do desempenho dos seus contentores.<br><br> ![< nome > Vista de desempenho de controladores](./media/monitoring-container-health/container-performance-and-health-view-06.png)

A tabela seguinte descreve as informações apresentadas ao ver contentores.

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador de|
| Estado | Agregação do estado dos contentores, se existirem. |
| % DE MÉDIA | Rollup de média de média de % de cada entidade para a métrica selecionada. |
| MÉDIA | Agregação do média da CPU millicore ou memória desempenho do contentor. O valor médio é medido desde o limite de memória e CPU definido para um pod. |
| Contentores | Número total de contentores para o controlador.|
| Reinicia | Representa o tempo desde o início de um contentor. |
| Tempo de atividade | Representa o tempo, uma vez que um contentor foi iniciado ou reiniciado. |
| Pod | Informações de pod em que reside. |
| Nó |  Nó onde reside o contentor.  | 
| % De média de tendência | Tendência de gráfico de barras apresentar média % métrica do contentor. |

## <a name="container-data-collection-details"></a>Detalhes de recolha de dados do contentor
Estado de funcionamento do contentor recolhe diversos dados de métricas e registo de desempenho dos anfitriões de contentor e contentores. Os dados são recolhidos a cada três minutos.

### <a name="container-records"></a>Registos do contentor

A tabela seguinte mostra exemplos de registos por Estado de funcionamento do contentor e os tipos de dados que aparecem nos resultados de pesquisa de registo recolhidos.

| Tipo de dados | Tipo de dados na pesquisa de registo | Campos |
| --- | --- | --- |
| Desempenho para os anfitriões e contentores | `Perf` | Computador, ObjectName, CounterName &#40;% tempo do processador, o disco lê MB, disco escreve MB, MB de utilização de memória, rede receber Bytes, rede enviar Bytes, o processador de seg de utilização, rede&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventário de contentor | `ContainerInventory` | TimeGenerated, computador, nome do contentor, ContainerHostname, imagem, ImageTag, ContainerState, ExitCode, EnvironmentVar, comando, CreatedTime, StartedTime, FinishedTime, SourceSystem, ID do contentor, ImageID |
| Inventário de imagem do contentor | `ContainerImageInventory` | TimeGenerated, computador, imagem, ImageTag, ImageSize, VirtualSize, em execução, em pausa, parar, falha, SourceSystem, ImageID, TotalContainer |
| Registo de contentor | `ContainerLog` | TimeGenerated, o computador, o ID de imagem, o nome do contentor, LogEntrySource, LogEntry, SourceSystem, ID do contentor |
| Registo do serviço de contentor | `ContainerServiceLog`  | TimeGenerated, computador, TimeOfCommand, imagem, comando, SourceSystem, ID do contentor |
| Inventário de nó do contentor | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Processo de contentor | `ContainerProcess_CL` | TimeGenerated, computador, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventário de pods num Kubernetes cluster | `KubePodInventory` | TimeGenerated, computador, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ID do contentor, ContainerName, nome, PodLabel, espaço de nomes, PodStatus, ClusterName PodIp, SourceSystem |
| Inventário da parte de nós de um cluster de Kubernetes | `KubeNodeInventory` | TimeGenerated, computador, ClusterName, ClusterId, LastTransitionTimeReady, etiquetas, estado, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents_CL` | TimeGenerated, computador, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, mensagem, SourceSystem | 
| Serviços em Kubernetes cluster | `KubeServices_CL` | TimeGenerated ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Métricas de desempenho para a parte de nós do Kubernetes cluster | Desempenho &#124; onde ObjectName = = "K8SNode" | cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes | 
| Métricas de desempenho para a parte de contentores do Kubernetes cluster | Desempenho &#124; onde ObjectName = = "K8SContainer" | cpuUsageNanoCores memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes | 

## <a name="search-logs-to-analyze-data"></a>Registos de pesquisa para analisar dados
Análise de registos pode ajudar a procurar tendências, diagnosticar congestionamentos, previsão ou correlacionar os dados que podem ajudar a determinam se a configuração de cluster atual for satisfatória.  Pesquisas de registo predefinidos são fornecidas para iniciar imediatamente a utilizar ou personalizar para devolver as informações da forma que pretende. 

Pode efetuar análises interativas dos dados na área de trabalho, selecionando o **ver registo** opção, disponível na extremidade direita quando expande um contentor.  **Registo de pesquisa** direita superior da página onde estava no portal é apresentada a página.<br><br> ![Analisar dados de análise de registos](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

A saída de registos do contentor reencaminhada para a análise de registos são STDOUT e STDERR. Porque o estado de funcionamento do contentor está a monitorizar Kubernetes gerida do Azure (AKS), o sistema Kube não é recolhido hoje devido ao grande volume de dados gerados.     

### <a name="example-log-search-queries"></a>Consultas de pesquisa de registo de exemplo
Muitas vezes, é útil criar consultas de exemplo ou dois carateres começadas e, em seguida, modificando-los para satisfazer as suas necessidades. Pode experimentar com as seguintes consultas de exemplo para ajudar a criar consultas mais avançadas.

| Consulta | Descrição | 
|-------|-------------|
| ContainerInventory<br> &#124;Computador, nome, imagem, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime do projeto<br> &#124;tabela de composição | Lista de ciclo de vida do contentor de todas as informações| 
| KubeEvents_CL<br> &#124;onde not(isempty(Namespace_s))<br> &#124;Ordenar por TimeGenerated desc<br> &#124;tabela de composição | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124;resumir AggregatedValue = existente pela imagem, ImageTag, está em execução | Inventário de imagem | 
| **Na análise avançadas, selecione gráficos**:<br> Desempenho<br> &#124;onde ObjectName = = "Contentor" e CounterName = = "% tempo do processador"<br> &#124;resumir AvgCPUPercent = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Contentor da CPU | 
| **Na análise avançadas, selecione gráficos**:<br> Desempenho &#124; onde ObjectName = = "Contentor" e CounterName = = "MB de utilização de memória"<br> &#124;resumir AvgUsedMemory = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Contentor de memória |

## <a name="how-to-stop-monitoring-with-container-health"></a>Como parar a monitorização de estado de funcionamento do contentor
Depois de ativar a monitorização do seu contentor AKS, decidir que já não pretenda monitorizá-lo, pode *ativamente* utilizando os modelos Azure Resource Manager fornecido com o cmdlet do PowerShell  **Novo-AzureRmResourceGroupDeployment** ou a CLI do Azure.  Um modelo JSON Especifica a configuração para *ativamente* e o modelo JSON contém valores de parâmetro que configurar para especificar o AKS cluster recursos e ID de grupo de recursos do cluster é implementado no.  Se não estiver familiarizado com conceitos de implementar recursos através de um modelo com o PowerShell, consulte [implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) ou para ver a CLI do Azure, [implementar recursos com o Modelos do Resource Manager e a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

Se optar por utilizar a CLI do Azure, terá primeiro de instalar e utilizar a CLI localmente.  É necessário que está a executar a CLI do Azure versão 2.0.27 ou posterior. Executar `az --version` para identificar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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
             "description": "AKS Cluster resource id"
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

4. Edite o valor para **aksResourceId** e **aksResourceLocation** com os valores do cluster AKS, que pode encontrar no **propriedades** página para o cluster selecionado.<br><br> ![Página de propriedades do contentor](./media/monitoring-container-health/container-properties-page.png)<br>

    Enquanto estiver a **propriedades** página, também copia o **Id de recurso da área de trabalho**.  Este valor é necessário se decidir que pretende eliminar a análise de registos área de trabalho mais tarde, que não é efetuada como parte deste processo.  

5. Guarde este ficheiro como **OptOutParam.json** para uma pasta local.
6. Está pronto para implementar este modelo. 

    * Para utilizar os seguintes comandos do PowerShell a partir da pasta que contém o modelo:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem semelhante ao seguinte que inclui o resultado:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Para executar os seguintes comandos com a CLI do Azure no Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        A alteração de configuração pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem semelhante ao seguinte que inclui o resultado:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Se a área de trabalho foi criada apenas para suportar a monitorização de cluster e já não é necessário, terá de eliminar manualmente. Se não estiver familiarizado com como eliminar uma área de trabalho, consulte [eliminar uma área de trabalho do Log Analytics do Azure com o portal do Azure](../log-analytics/log-analytics-manage-del-workspace.md).  Não se esqueça sobre o **Id de recurso da área de trabalho** copiou anteriormente no passo 4, que vai necessitar que.  

## <a name="troubleshooting"></a>Resolução de problemas
Esta secção fornece informações para ajudar a resolver problemas com o estado de funcionamento do contentor.

Se o estado de funcionamento do contentor foi ativado e configurado com êxito, mas são não ver quaisquer informações de estado ou resulta na análise de registos quando efetua uma pesquisa de registo, pode realizar os passos seguintes para ajudar a diagnosticar o problema.   

1. Verifique o estado do agente executando o seguinte comando: `kubectl get ds omsagent --namespace=kube-system`

    A saída deve assemelhar-se a seguinte que indica que o agente está em execução em todos os nós do cluster.  Por exemplo, este cluster tem dois nós e deve esperar o valor para um número igual de nós.  

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```
2. Verifique o estado de pod para verificar se encontra em execução ou não, executando o seguinte comando: `kubectl get pods --namespace=kube-system`

    A saída deve assemelhar-se o seguinte com o estado *executar* para o omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

3. Verifique os registos do agente. Quando o agente de é implementado, este executa uma verificação rápida ao executar comandos OMI e mostra a versão do agente e o fornecedor de Docker. Para ver que o agente foi integrado com êxito, execute o seguinte comando: `kubectl logs omsagent-484hw --namespace=kube-system`

    O estado deve assemelhar-se do seguinte:

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

[Pesquisar registos](../log-analytics/log-analytics-log-search.md) para ver informações de desempenho de aplicações e estado de funcionamento do contentor de detalhado.  