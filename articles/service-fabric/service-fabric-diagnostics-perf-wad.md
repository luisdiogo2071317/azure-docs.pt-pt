---
title: Azure Service Fabric - com a extensão do Windows Azure Diagnostics de monitorização de desempenho | Microsoft Docs
description: Utilize o Windows Azure Diagnostics para recolher os contadores de desempenho para os clusters de Service Fabric do Azure.
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
ms.date: 03/26/2018
ms.author: srrengar
ms.openlocfilehash: 5c8c1f107e9e70e72c48ea93ef211b2a760ffb5c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213236"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Monitorização de desempenho com a extensão de diagnóstico do Windows Azure

Este documento aborda os passos necessários para configurar a recolha de contadores de desempenho através da extensão do Windows Azure Diagnostics (WAD) para os clusters do Windows. Para os clusters do Linux, configurar o [agente do OMS](service-fabric-diagnostics-oms-agent.md) para recolher contadores de desempenho para os nós. 

 > [!NOTE]
> A extensão WAD deve ser implementada no seu cluster para estes passos para resolver o problema. Se este não está configurado, aceda a [agregação de eventos e coleção utilizando o Windows Azure Diagnostics](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters).

## <a name="collect-performance-counters-via-the-wadcfg"></a>Recolher contadores de desempenho através de WadCfg

Para recolher os contadores de desempenho através de WAD, terá de modificar a configuração correta no modelo de Gestor de recursos do cluster. Siga estes passos para adicionar um contador de desempenho que pretende recolher ao seu modelo e executar uma atualização de recursos do Resource Manager.

1. Localizar a configuração de WAD no modelo do seu cluster - localizar `WadCfg`. Irá adicionar os contadores de desempenho para recolher sob o `DiagnosticMonitorConfiguration`.

2. Definir a configuração para recolher contadores de desempenho adicionando a secção seguinte para sua `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    O `scheduledTransferPeriod` define como frquently os valores dos contadores que são recolhidos são transferidos para a tabela de armazenamento do Azure e qualquer configuradas sink. 

3. Adicionar os contadores de desempenho que pretende recolher o `PerformanceCounterConfiguration` que foi declarada no passo anterior. Cada contador que pretende recolher é definida com um `counterSpecifier`, `sampleRate`, `unit`, `annotation`e quaisquer relevantes `sinks`.

Eis um exemplo de uma configuração com o contador para a *Total de tempo do processador* (o período de tempo de CPU estava a ser utilizado para operações de processamento) e *invocações de método do serviço de recursos de infraestrutura Ator por segundo*, uma dos contadores de desempenho personalizado do Service Fabric. Consulte [fiável contadores de desempenho de Ator](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) e [fiável contadores de desempenho do serviço](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) para uma lista completa dos contadores de desempenho personalizado do Service Fabric.

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
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
           "PerformanceCounters": {
                 "scheduledTransferPeriod": "PT1M",
                 "PerformanceCounterConfiguration": [
                     {
                         "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                         "sampleRate": "PT1M",
                         "unit": "Percent",
                         "annotation": [
                         ],
                         "sinks": ""
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 Pode ser modificada a frequência de amostragem para o contador de acordo com as suas necessidades. O formato para o mesmo é `PT<time><unit>`, por isso, se pretender que o contador recolhido a cada segundo, em seguida, deverá definir o `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Embora pode utilizar `*` para especificar os grupos de contadores de desempenho com o nome da mesma forma, enviar contadores através de um receptor (para o Application Insights) necessita que são declarados individualmente. 

4. Depois de adicionar os contadores de desempenho adequadas que têm de ser recolhidos, terá de atualizar o recurso de cluster para que estas alterações são refletidas no seu cluster em execução. Guardar o modificado `template.json` e abra do PowerShell. Pode atualizar o seu cluster utilizando `New-AzureRmResourceGroupDeployment`. A chamada requer que o nome do grupo de recursos, o ficheiro de modelo atualizado e o ficheiro de parâmetros e pede ao Gestor de recursos para fazer alterações adequadas para os recursos que tenha atualizado. Assim que iniciou a sessão na sua conta e está a subscrição correta, utilize o seguinte comando para executar a atualização:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Após a conclusão da atualização disponibilizando (demora entre 15 45 minutos), WAD deve ser recolher os contadores de desempenho e enviar para a tabela com o nome WADPerformanceCountersTable na conta de armazenamento associados ao seu cluster. Consulte os contadores de desempenho no Application Insights por [adicionar o Sink de AI para o modelo do Resource Manager](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>Passos Seguintes
* Recolha mais contadores de desempenho para o cluster. Consulte [métricas de desempenho](service-fabric-diagnostics-event-generation-perf.md) para obter uma lista dos contadores deve a recolher.
* [Monitorização de utilização e diagnóstico com modelos de VM do Windows e o Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md) mais efetuar modificações ao seu `WadCfg`, incluindo a configurar contas de armazenamento adicional para enviar dados de diagnóstico para.
