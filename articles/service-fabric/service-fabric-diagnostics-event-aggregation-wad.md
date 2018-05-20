---
title: Agregação de eventos de recursos de infraestrutura de serviço do Azure com o Windows Azure Diagnostics | Microsoft Docs
description: Saiba mais sobre agregar e recolha de eventos utilizando WAD para monitorização e diagnóstico de clusters de Service Fabric do Azure.
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
ms.date: 04/03/2018
ms.author: srrengar
ms.openlocfilehash: 38a026e8995bb7384c866dcd2f12588ca816009f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Agregação de eventos e coleção utilizando o Windows Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Quando estiver a executar um cluster do Service Fabric do Azure, é uma boa ideia para recolher os registos de todos os nós numa localização central. Com os registos numa localização central ajuda a analisar e resolver problemas no seu cluster, ou problemas em aplicações e serviços em execução nesse cluster.

Uma forma para carregar e recolher registos consiste em utilizar a extensão do Windows Azure Diagnostics (WAD), que carrega registos para o Storage do Azure e tem também a opção para enviar registos ao Application Insights do Azure ou Event Hubs. Também pode utilizar o processo externo para leitura dos eventos de armazenamento e colocá-los como um produto de plataforma de análise, [Log Analytics](../log-analytics/log-analytics-service-fabric.md) ou outra solução de análise do registo.

## <a name="prerequisites"></a>Pré-requisitos
As ferramentas seguintes são utilizadas neste artigo:

* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Modelo do Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-platform-events"></a>Eventos de plataforma do Service Fabric
Service Fabric configura, com algumas [canais de registo de out of box](service-fabric-diagnostics-event-generation-infra.md), que são previamente configuradas com a extensão para enviar a monitorização e os dados de diagnóstico para uma tabela de armazenamento ou noutro local as seguintes canais:
  * [Eventos operacionais](service-fabric-diagnostics-event-generation-operational.md): operações de nível mais elevadas que efetua a plataforma do Service Fabric. Os exemplos incluem a criação de aplicações e serviços, as alterações de estado do nó e informações de atualização. Estes são emitidos como registos de rastreio de eventos para o Windows (ETW)
  * [Eventos do modelo de programação de Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
  * [Serviços fiáveis de eventos do modelo de programação](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>Implementar a extensão de diagnóstico através do portal
É o primeiro passo na recolher registos para implementar a extensão de diagnóstico em nós de conjunto de dimensionamento de máquina virtual no cluster de Service Fabric. A extensão de diagnóstico recolhe registos de cada VM e carrega-os para a conta do storage que especificar. Os passos seguintes descrevem como efetuar este procedimento para clusters novas e existentes através do portal do Azure e os modelos Azure Resource Manager.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Implementar a extensão de diagnóstico como parte da criação do cluster através do portal do Azure
Ao criar o cluster, o passo de configuração de cluster, expanda as definições opcionais e certifique-se de que o diagnóstico está definido como **no** (a predefinição).

![Definições de diagnóstico do Azure no portal para a criação do cluster](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

É vivamente recomendado que transfira o modelo **antes de clicar em criar** no último passo. Para obter detalhes, consulte [configurar um cluster do Service Fabric utilizando um modelo Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). É necessário o modelo para efetuar alterações em que canais (listados acima) para recolher dados.

![Modelo de cluster](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

Agora que está a agregar eventos no Storage do Azure, [configurar a análise de registos](service-fabric-diagnostics-oms-setup.md) para obter informações e consultá-los no portal do Log Analytics

>[!NOTE]
>Atualmente, não há nenhuma forma de filtro ou tratar os eventos que são enviados para as tabelas. Se não implemente um processo para remover eventos da tabela, a tabela continua a crescer (o limite predefinido é 50 GB). Instruções sobre como alterar esta são [mais abaixo neste artigo](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota). Além disso, não há um exemplo de um serviço de tratamento de dados em execução [exemplo Watchdog](https://github.com/Azure-Samples/service-fabric-watchdog-service), e é recomendado que escreve uma por si, a menos que exista um bom motivo para armazenar os registos para além de um período de tempo de dia 30 ou 90.

## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>Implementar a extensão de diagnóstico através do Azure Resource Manager

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>Criar um cluster com a extensão de diagnóstico
Para criar um cluster utilizando o Gestor de recursos, terá de adicionar a configuração de diagnósticos JSON para o modelo do Resource Manager completa antes de criar o cluster. Podemos fornecer um modelo de Gestor de recursos de cluster de VM de cinco de exemplo com a configuração de diagnósticos adicionada como parte dos nossos exemplos de modelo do Resource Manager. Pode vê-lo nesta localização na galeria do Azure Samples: [cluster de cinco nós com o exemplo de modelo de Gestor de recursos de diagnóstico](https://azure.microsoft.com/en-in/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/).

Para ver a definição de diagnóstico no modelo do Resource Manager, abra o ficheiro azuredeploy. JSON e procure **IaaSDiagnostics**. Para criar um cluster utilizando este modelo, selecione o **implementar no Azure** botão disponível na hiperligação anterior.

Em alternativa, pode transferir o exemplo de Gestor de recursos, efetuar alterações e criar um cluster com o modelo modificado utilizando o `New-AzureRmResourceGroupDeployment` comando numa janela do PowerShell do Azure. Consulte o seguinte código para os parâmetros que passa para o comando. Para obter informações detalhadas sobre como implementar um grupo de recursos com o PowerShell, consulte o artigo [implementar um grupo de recursos com o modelo Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>Adicionar a extensão de diagnóstico para um cluster existente
Se tiver um cluster existente que não tenha implementado o diagnóstico, pode adicionar ou atualizá-lo através do modelo de cluster. Modificar o modelo do Resource Manager que é utilizado para criar o cluster existente ou transferir o modelo do portal conforme descrito anteriormente. Modificar o ficheiro Template efetuando as seguintes tarefas:

Adicione um novo recurso de armazenamento para o modelo, adicionando a secção de recursos.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Em seguida, adicione a secção de parâmetros apenas depois das definições de conta de armazenamento, entre `supportLogStorageAccountName`. Substitua o texto do marcador de posição *nome da conta de armazenamento aqui* com o nome da conta de armazenamento que pretende.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Em seguida, atualize o `VirtualMachineProfile` secção do ficheiro Template adicionando o seguinte código dentro da matriz de extensões. Lembre-se de que adicione uma vírgula no início ou fim, consoante onde é inserido.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Depois de modificar o ficheiro Template, tal como descrito, voltar a publicar o modelo do Resource Manager. Se o modelo foi exportado, executar o ficheiro deploy.ps1 republishes o modelo. Depois de implementar, certifique-se de que **ProvisioningState** é **com êxito**.

> [!TIP]
> Se pretender implementar contentores para o cluster, ativar WAD recolher estatísticas de docker adicionando esta opção para sua **WadCfg > DiagnosticMonitorConfiguration** secção.
>
>```json
>"DockerSources": {
>    "Stats": {
>        "enabled": true,
>        "sampleRate": "PT1M"
>    }
>},
>```

### <a name="update-storage-quota"></a>Atualizar a quota de armazenamento

Dado que as tabelas preenchidas pela extensão cresce até é atingiu a quota, poderá querer pondere diminuir o tamanho de quota. O valor predefinido é 50 GB e é configurável no modelo sob o `overallQuotainMB` campo em `DiagnosticMonitorConfiguration`

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>Configurações de coleção de registo
Também estão disponíveis para a recolha de registos de canais adicionais, seguem-se algumas das configurações mais comuns que pode efetuar no modelo de clusters em execução no Azure.

* Canal de operacional - Base: Ativado por predefinição, operações de alto nível efetuadas pelo serviço de recursos de infraestrutura e o cluster, incluindo eventos de um nó de futuras cópias de segurança, uma nova aplicação que está a ser implementada ou uma atualização reversão, etc. Para obter uma lista de eventos, consulte [eventos operacionais de canal](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-operational).
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* Canal de operacional - detalhadas: Isto inclui relatórios de estado de funcionamento e decisões plus todo o canal operacional base de balanceamento de carga. Estes eventos são gerados pelo sistema ou o código utilizando o estado de funcionamento ou carregar APIs de relatórios, tais como [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) ou [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Para ver estes eventos no Visualizador de eventos de diagnóstico do Visual Studio adicionar "Microsoft-ServiceFabric:4:0x4000000000000008" à lista de fornecedores ETW.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* Dados e o canal de mensagens - Base: registos críticos e os eventos gerados no serviço de mensagens (atualmente apenas o ReverseProxy) e o caminho de dados, além disso, para os registos de detalhado canal operacional. Estes eventos são falhas e outros problemas críticos no ReverseProxy o processamento de pedidos, bem como os pedidos processados. **Esta é a nossa recomendação para registo abrangente**. Para ver estes eventos no Visualizador de eventos de diagnóstico do Visual Studio, adicione "Microsoft-ServiceFabric:4:0x4000000000000010" à lista de fornecedores ETW.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* Canal de mensagens - detalhadas & dados: verboso canal que contém todos os registos de mensagens no cluster e o canal operacional detalhado e de dados não críticos. Para resolução de erros detalhada todos os eventos de proxy inverso, consulte o [guia de diagnóstico de proxy inverso](service-fabric-reverse-proxy-diagnostics.md).  Para ver estes eventos no Visualizador de eventos de diagnóstico do Visual Studio, adicione "Microsoft-ServiceFabric:4:0x4000000000000020" à lista de fornecedores ETW.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>Este canal tem um volume muito elevado de eventos, ativar recolha de eventos deste detalhadas resultados de canal em muitos rastreios a ser gerado rapidamente e pode consumir a capacidade de armazenamento. Apenas ative esta opção se for absolutamente necessário.


Para ativar o **Base de dados e o canal de mensagens** a nossa recomendação para registo abrangente, o `EtwManifestProviderConfiguration` no `WadCfg` do seu modelo seria o seguinte aspeto:

```json
  "WadCfg": {
        "DiagnosticMonitorConfiguration": {
          "overallQuotaInMB": "50000",
          "EtwProviders": {
            "EtwEventSourceProviderConfiguration": [
              {
                "provider": "Microsoft-ServiceFabric-Actors",
                "scheduledTransferKeywordFilter": "1",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableActorEventTable"
                }
              },
              {
                "provider": "Microsoft-ServiceFabric-Services",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableServiceEventTable"
                }
              }
            ],
            "EtwManifestProviderConfiguration": [
              {
                "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                "scheduledTransferLogLevelFilter": "Information",
                "scheduledTransferKeywordFilter": "4611686018427387928",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricSystemEventTable"
                }
              }
            ]
          }
        }
      },
```

## <a name="collect-from-new-eventsource-channels"></a>Recolher novos canais de EventSource

Para atualizar o diagnóstico para recolher registos de novos canais de EventSource que representam uma nova aplicação que está a sobre a implementação, efetue os mesmos passos conforme descritos anteriormente para o programa de configuração de diagnósticos para existente cluster.

Atualizar o `EtwEventSourceProviderConfiguration` secção no ficheiro de Template para adicionar entradas para os canais de EventSource novo antes de aplicar a configuração de atualização utilizando o `New-AzureRmResourceGroupDeployment` comando do PowerShell. O nome da origem de evento está definido como parte do seu código no ficheiro ServiceEventSource.cs gerado pelo Visual Studio.

Por exemplo, se a origem do evento com o nome My Eventsource, adicione o seguinte código para colocar os eventos a partir do meu Eventsource numa tabela com o nome MyDestinationTableName.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Para recolher contadores de desempenho ou os registos de eventos, modifique o modelo do Resource Manager utilizando os exemplos fornecidos no [criar máquina virtual do Windows com a monitorização e diagnóstico utilizando um modelo Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Em seguida, voltar a publicar o modelo do Resource Manager.

## <a name="collect-performance-counters"></a>Recolher contadores de desempenho

Para recolher métricas de desempenho do seu cluster, adicione os contadores de desempenho para o seu "WadCfg > DiagnosticMonitorConfiguration" no modelo do Resource Manager para o cluster. Consulte [monitorização de desempenho com WAD](service-fabric-diagnostics-perf-wad.md) para obter os passos sobre a modificação do `WadCfg` para recolher contadores de desempenho específicos. Referência [contadores de desempenho de recursos de infraestrutura de serviço](service-fabric-diagnostics-event-generation-perf.md) para contadores de uma lista de desempenho, recomendamos a recolher.
  
Se estiver a utilizar um receptor de Application Insights, conforme descrito na secção abaixo e pretender que estas métricas apareçam no Application Insights, em seguida, certifique-se de que adiciona o nome de receptores na secção "sinks", conforme mostrado acima. Isto irá enviar automaticamente os contadores de desempenho que são configurados individualmente para o recurso do Application Insights.


## <a name="send-logs-to-application-insights"></a>Enviar registos ao Application Insights

Enviar dados de monitorização e diagnóstico para o Application Insights (AI) pode ser feita como parte da configuração WAD. Se optar por utilizar AI para análise de eventos e visualização, leia [como configurar um receptor de AI](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template) como parte da sua "WadCfg".

## <a name="next-steps"></a>Passos Seguintes

Assim que tiver configurado corretamente o diagnóstico do Azure, verá dados as tabelas de armazenamento a partir de registos do ETW e EventSource. Se optar por utilizar a análise de registos, Kibana ou quaisquer outro dados e visualização de análise plataforma que não é diretamente configurada no modelo do Resource Manager, certifique-se configurar a plataforma à sua escolha para ler nos dados destas tabelas de armazenamento. Fazê-lo para análise de registos é relativamente trivial e é explicado no [análise de eventos e registo](service-fabric-diagnostics-event-analysis-oms.md). O Application Insights está pouco num caso especial neste sentido, uma vez que pode ser configurado como parte da configuração da extensão de diagnóstico, por isso, consulte o [artigo adequado](service-fabric-diagnostics-event-analysis-appinsights.md) se optar por utilizar AI.

>[!NOTE]
>Atualmente, não há nenhuma forma de filtro ou tratar os eventos que são enviados para a tabela. Se não implemente um processo para remover eventos da tabela, a tabela continua a crescer. Atualmente, não há um exemplo de um serviço de tratamento de dados em execução [exemplo Watchdog](https://github.com/Azure-Samples/service-fabric-watchdog-service), e é recomendado que escreve uma por si, a menos que exista um bom motivo para armazenar os registos para além de um período de tempo de dia 30 ou 90.

* [Saiba como recolher contadores de desempenho ou os registos ao utilizar a extensão de diagnóstico](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Análise de eventos e visualização com o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Análise de eventos e visualização de análise de registos](service-fabric-diagnostics-event-analysis-oms.md)
