---
title: Recursos de infraestrutura de serviço do Azure Auto dimensionamento serviços e contentores | Microsoft Docs
description: Recursos de infraestrutura de serviço do Azure permite-lhe definir automaticamente as políticas de serviços e contentores de dimensionamento.
services: service-fabric
documentationcenter: .net
author: radicmilos
manager: ''
editor: nipuzovi
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2018
ms.author: miradic
ms.openlocfilehash: cd19c0e51ca1ac7863058d7c3944400719508f9b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213202"
---
# <a name="introduction-to-auto-scaling"></a>Introdução à escala automática
Escala automática é uma capacidade adicional do Service Fabric dinamicamente dimensionar os serviços com base na carga que os serviços estiverem a comunicar ou com base na respetiva utilização de recursos. Escala automática proporciona excelente elasticidade e permite o aprovisionamento de instâncias adicionais ou partições do seu serviço a pedido. A automática toda dimensionamento processo é automática e transparente e, depois de configurar as políticas num serviço é necessário para operações de dimensionamento manuais a nível de serviço. Escala automática pode ser ativada no momento de criação do serviço, ou em qualquer altura por atualizar o serviço.

É um cenário comum, onde o dimensionamento automático é útil quando a carga sobre um determinado serviço varia ao longo do tempo. Por exemplo, um serviço, tal como um gateway pode dimensionar com base na quantidade de recursos necessários para processar os pedidos recebidos. Vamos ver um exemplo dessas regras de dimensionamento foi aspeto:
* Se a todas as instâncias do meu gateway estiver a utilizar mais do que dois núcleos em média, em seguida, dimensione o serviço de gateway horizontalmente adicionando uma instância mais. Fazê-lo a cada hora, mas nunca ter mais de sete instâncias no total.
* Se a todas as instâncias do meu gateway estiver a utilizar o menor 0.5 núcleos em média, em seguida, dimensione o serviço no através da remoção de uma instância. Fazê-lo a cada hora, mas nunca ter menos de três instâncias no total.

Escala automática é suportada para contentores e serviços do Service Fabric regulares. O resto deste artigo descreve as políticas de dimensionamento, formas para ativar ou desativar o dimensionamento automático e fornece exemplos sobre como utilizar esta funcionalidade.

## <a name="describing-auto-scaling"></a>Descrever a escala automática
Automática dimensionamento políticas pode ser definida para cada serviço de um cluster do Service Fabric. Cada política de dimensionamento é composto por duas partes:
* **Dimensionamento acionador** descreve quando será executada dimensionamento do serviço. As condições definidas no acionador são verificadas periodicamente para determinar se um serviço deve ser ampliado ou não.

* **Dimensionamento mecanismo** descreve como dimensionar irá ser executada quando é acionada. Mecanismo só se aplica quando são satisfeitas as condições de Acionador.

Todos os acionadores são atualmente suportados trabalham com [métricas de carga lógica](service-fabric-cluster-resource-manager-metrics.md), ou com a métrica física como a utilização da CPU ou memória. Qualquer forma, Service Fabric irá monitorizar a carga comunicada para a métrica e irão avaliar o acionador periodicamente para determinar se o dimensionamento é necessária.

Existem dois mecanismos são atualmente suportados para o dimensionamento automático. Primeiro destina-se de serviços sem monitorização de estado ou de contentores onde dimensionamento automático é realizado adicionando ou removendo [instâncias](service-fabric-concepts-replica-lifecycle.md). Para os serviços com monitorização de estado e sem monitorização de estado, dimensionamento automático pode também ser executada adicionando ou removendo denominado [partições](service-fabric-concepts-partitioning.md) do serviço.

> [!NOTE]
> Atualmente não há suporte para apenas uma política de dimensionamento por serviço.

## <a name="average-partition-load-trigger-with-instance-based-scaling"></a>Acionador de carga da partição média com dimensionamento de instâncias, com base
O primeiro tipo de accionador baseia-se a carga de instâncias de uma partição de serviço sem estado. Métricas cargas são smoothed primeiro para obter a carga de todas as instâncias de uma partição e, em seguida, estes valores são Considerando todas as instâncias da partição. Existem três fatores que determinam quando o serviço irá ser escalado:

* _Limiar de carregamento inferior_ é um valor que determina quando o serviço será **ampliada no**. Se a carga média de todas as instâncias das partições for inferior a este valor, o serviço irá ser ampliado no.
* _Limiar de carregamento superior_ é um valor que determina quando o serviço será **ampliar**. Se a carga média de todas as instâncias da partição for inferior a este valor, em seguida, o serviço irá ser ampliado.
* _Intervalo de dimensionamento_ determina a frequência com que o acionador será verificado. Depois do acionador estiver selecionado, se necessitar de dimensionamento o mecanismo de será aplicado. Se não for necessário para o dimensionamento, irá ser efetuada nenhuma ação. Em ambos os casos, acionador será não verificado novamente antes de expira de intervalo de dimensionamento novamente.

Este acionador pode ser utilizado apenas com os serviços sem monitorização de estado (contentores sem monitorização de estado ou serviços do Service Fabric). No caso de quando um serviço tem várias partições, o acionador é avaliado separadamente para cada partição e cada partição terão o mecanismo de especificado aplicado ao mesmo de forma independente. Por conseguinte, neste caso, é possível que algumas das partições do serviço irão ser ampliadas, algumas tiverem serão dimensionadas no, e algumas não irão ser ampliadas em todos os ao mesmo tempo, com base na respetiva carga.

O mecanismo de só pode ser utilizado com este acionador é PartitionInstanceCountScaleMechanism. Existem três fatores que determinam a forma como este mecanismo é aplicado:
* _Dimensionar o incremento_ determina quantas instâncias irão ser adicionadas ou removidas quando o mecanismo que é acionado.
* _Número máximo de instâncias_ define o limite superior para o dimensionamento. Se o número de instâncias da partição atingir este limite, em seguida, o serviço será não ser ampliado, independentemente da carga. É possível omita este limite especificando um valor de -1 e, nesse caso, o serviço irá ser ampliado out quanto possível (o limite é o número de nós que estão disponíveis no cluster).
* _Número mínimo de instâncias_ define o limite inferior para o dimensionamento. Se o número de instâncias da partição atingir este limite, em seguida, serviço será não ser ampliado no independentemente da carga.

## <a name="setting-auto-scaling-policy"></a>A definição de política de dimensionamento de automática

### <a name="using-application-manifest"></a>Utilizar o manifesto da aplicação
``` xml
<LoadMetrics>
<LoadMetric Name="MetricB" Weight="High"/>
</LoadMetrics>
<ServiceScalingPolicies>
<ScalingPolicy>
    <AveragePartitionLoadScalingTrigger MetricName="MetricB" LowerLoadThreshold="1" UpperLoadThreshold="2" ScaleIntervalInSeconds="100"/>
    <InstanceCountScalingMechanism MinInstanceCount="3" MaxInstanceCount="4" ScaleIncrement="1"/>
</ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>Utilizar APIs de c#
```csharp
FabricClient fabricClient = new FabricClient();
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//set up the rest of the ServiceDescription
AveragePartitionLoadScalingTrigger trigger = new AveragePartitionLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new PartitionInstanceCountScaleMechanism();
mechanism.MaxInstanceCount = 3;
mechanism.MinInstanceCount = 1;
mechanism.ScaleIncrement = 1;
trigger.MetricName = "servicefabric:/_CpuCores";
trigger.ScaleInterval = TimeSpan.FromMinutes(20);
trigger.LowerLoadThreshold = 1.0;
trigger.UpperLoadThreshold = 2.0;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceDescription.ScalingPolicies.Add(policy);
//as we are using scaling on a resource this must be exclusive service
//also resource monitor service needs to be enabled
serviceDescription.ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```
### <a name="using-powershell"></a>Com o Powershell
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.PartitionInstanceCountScaleMechanism
$mechanism.MinInstanceCount = 1
$mechanism.MaxInstanceCount = 6
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AveragePartitionLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_CpuCores"
$trigger.LowerLoadThreshold = 0.3
$trigger.UpperLoadThreshold = 0.8
$trigger.ScaleInterval = New-TimeSpan -Minutes 10
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
Update-ServiceFabricService -Stateless -ServiceName "fabric:/AppName/ServiceName" -ScalingPolicies $scalingpolicies
```

## <a name="average-service-load-trigger-with-partition-based-scaling"></a>Acionador de carga média de serviço com partições com base em dimensionamento
O segundo acionador baseia-se a carga de todas as partições de um serviço. Métricas cargas são smoothed primeiro para obter a carga de cada réplica ou de uma instância de uma partição. Para os serviços com monitorização de estado, a carga da partição é considerada a carga da réplica primária, enquanto para serviços sem monitorização de estado a carga da partição se a carga média de todas as instâncias da partição. Estes valores são Considerando todas as partições do serviço e este valor é utilizado para acionar o dimensionamento automático. Mesmo do mecanismo anterior, existem três fatores que determinam quando o serviço irá ser escalado:

* _Limiar de carregamento inferior_ é um valor que determina quando o serviço será **ampliada no**. Se a carga média de todas as partições do serviço for inferior a este valor, o serviço irá ser ampliado no.
* _Limiar de carregamento superior_ é um valor que determina quando o serviço será **ampliar**. Se a carga média de todas as partições do serviço for inferior a este valor, em seguida, o serviço irá ser ampliado.
* _Intervalo de dimensionamento_ determina a frequência com que o acionador será verificado. Depois do acionador estiver selecionado, se necessitar de dimensionamento o mecanismo de será aplicado. Se não for necessário para o dimensionamento, irá ser efetuada nenhuma ação. Em ambos os casos, acionador será não verificado novamente antes de expira de intervalo de dimensionamento novamente.

Este acionador pode ser utilizadas com os serviços de monitorização de estado e sem monitorização de estado. O mecanismo de só pode ser utilizado com este acionador é AddRemoveIncrementalNamedParitionScalingMechanism. Quando o serviço é ampliado, em seguida, é adicionada uma nova partição, e quando o serviço é dimensionado de uma das partições existentes são removidos. Existem as restrições que serão verificadas quando o serviço é criado ou atualizado e serviço criação/atualização falhará se estas condições não forem cumpridas:
* Esquema de partição com o nome tem de ser utilizada para o serviço.
* Os nomes de partição tem de ser números de número inteiro consecutivos, como "0", "1",...
* Nome da primeira partição tem de ser "0".

Por exemplo, se um serviço for criado inicialmente com três partições, a possibilidade de só é válida para nomes de partição é "0", "1" e "2".

A automática real dimensionamento operação é efetuada serão Respeitamos a este esquema de nomenclatura:
* Se partições atuais do serviço são denominadas "0", "1" e "2", em seguida, a partição que será adicionada para aumentar horizontalmente irá ser denominada "3".
* Partições atuais do serviço são denominadas "0", "1" e "2", em seguida, a partição que será removida para dimensionamento em estiver partição com o nome "2".

Mesmo, tal como acontece com o mecanismo que utiliza o dimensionamento adicionando ou removendo instâncias, existem três parâmetros que determinam a forma como este mecanismo é aplicado:
* _Dimensionar o incremento_ determina quantos partições irão ser adicionadas ou removidas quando o mecanismo que é acionado.
* _Contagem da partição máximo_ define o limite superior para o dimensionamento. Se o número de partições do serviço atingir este limite, em seguida, o serviço será não ser ampliado, independentemente da carga. É possível omita este limite especificando um valor de -1 e, nesse caso, o serviço irá ser ampliado out quanto possível (o limite é a capacidade real do cluster).
* _Número mínimo de instâncias_ define o limite inferior para o dimensionamento. Se o número de partições do serviço atingir este limite, em seguida, serviço será não ser ampliado no independentemente da carga.

## <a name="setting-auto-scaling-policy"></a>A definição de política de dimensionamento de automática

### <a name="using-application-manifest"></a>Utilizar o manifesto da aplicação
``` xml
<ServiceScalingPolicies>
    <ScalingPolicy>
        <AverageServiceLoadScalingTrigger MetricName="servicefabric:/_MemoryInMB" LowerLoadThreshold="300" UpperLoadThreshold="500" ScaleIntervalInSeconds="600"/>
        <AddRemoveIncrementalNamedParitionScalingMechanism MinPartitionCount="1" MaxPartitionCount="3" ScaleIncrement="1"/>
    </ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>Utilizar APIs de c#
```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
AveragePartitionLoadScalingTrigger trigger = new AverageServiceLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new AddRemoveIncrementalNamedParitionScalingMechanism();
mechanism.MaxPartitionCount = 4;
mechanism.MinPartitionCount = 1;
mechanism.ScaleIncrement = 1;
//expecting that the service already has metric NumberOfConnections
trigger.MetricName = "NumberOfConnections";
trigger.ScaleInterval = TimeSpan.FromMinutes(15);
trigger.LowerLoadThreshold = 10000;
trigger.UpperLoadThreshold = 20000;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceUpdate.ScalingPolicies = new List<ScalingPolicyDescription>;
serviceUpdate.ScalingPolicies.Add(policy);
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), serviceUpdate);
```
### <a name="using-powershell"></a>Com o Powershell
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.AddRemoveIncrementalNamedParitionScalingMechanism
$mechanism.MinPartitionCount = 1
$mechanism.MaxPartitionCount = 3
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AverageServiceLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_MemoryInMB"
$trigger.LowerLoadThreshold = 5000
$trigger.UpperLoadThreshold = 10000
$trigger.ScaleInterval = New-TimeSpan -Minutes 25
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -TargetReplicaSetSize 3 -MinReplicaSetSize 2 -HasPersistedState true -PartitionNames @("0","1") -ServicePackageActivationMode ExclusiveProcess -ScalingPolicies $scalingpolicies
```

## <a name="auto-scaling-based-on-resources"></a>Escala automática com base nos recursos

Para ativar o serviço de monitor de recursos de dimensionamento com base nos recursos reais

``` json
"fabricSettings": [
...      
],
"addonFeatures": [
    "ResourceMonitorService"
],
```
Existem duas métricas que representam os recursos físicos reais. Uma delas é servicefabric: / _CpuCores que representam a utilização de cpu real (para 0,5 representa meio um núcleo) e outra a ser servicefabric: / _MemoryInMB que representa a utilização de memória em MB.
ResourceMonitorService é responsável pelo controlo da utilização de cpu e memória dos serviços de utilizador. Este serviço será aplicada a média móvel ponderada para uma conta para potenciais picos de curta duração. Monitorização de recursos é suportada para aplicações de e não no Windows e de relações no Linux. Auto dimensionamento em recursos só está ativada para serviços ativados no [modelo de processo exclusivo](service-fabric-hosting-model.md#exclusive-process-model).

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre [escalabilidade de aplicação](service-fabric-concepts-scalability.md).