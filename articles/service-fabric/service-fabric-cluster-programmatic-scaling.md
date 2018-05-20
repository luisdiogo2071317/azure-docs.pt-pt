---
title: Azure Service Fabric programático dimensionamento | Microsoft Docs
description: Um cluster do Azure Service Fabric entrada ou saída de escala através de programação, de acordo com os acionadores personalizados
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: mikerou
ms.openlocfilehash: dcf4721012fb8ec39bcd1de02c294747357b3539
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Dimensionar um cluster do Service Fabric através de programação 

Clusters de Service Fabric em execução no Azure assentes em conjuntos de dimensionamento de máquina virtual.  [O dimensionamento de clusters](./service-fabric-cluster-scale-up-down.md) descreve como podem ser escalados para clusters de Service Fabric manualmente ou com regras de dimensionamento automático. Este artigo descreve como gerir as credenciais e dimensionar um cluster na ou utilizando o Azure fluent computação SDK, que é o cenário mais avançado. Para obter uma descrição geral, leia o artigo [programáticos métodos para coordenar as operações de dimensionamento de Azure](service-fabric-cluster-scaling.md#programmatic-scaling). 

## <a name="manage-credentials"></a>Gerir credenciais
Um desafio de escrever um serviço para processar o dimensionamento é que o serviço tem de ser capaz de aceder a recursos de conjunto de dimensionamento de máquina virtual sem um início de sessão interativo. Aceder ao cluster do Service Fabric é fácil se o serviço de dimensionamento é modificar as suas próprias aplicações de Service Fabric, mas as credenciais são necessárias para aceder ao conjunto de dimensionamento. Para iniciar sessão, pode utilizar um [principal de serviço](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) criado com o [Azure CLI 2.0](https://github.com/azure/azure-cli).

Um principal de serviço pode ser criado com os seguintes passos:

1. Inicie sessão na CLI do Azure (`az login`) como um utilizador com acesso para o dimensionamento da máquina virtual definido
2. Criar o serviço principal com o `az ad sp create-for-rbac`
    1. Anote o appId (denominado 'ID de cliente' noutro local), o nome, a palavra-passe e o tenant para utilização posterior.
    2. Também terá do ID de subscrição, que pode ser visualizado com `az account list`

A biblioteca de computação fluent pode iniciar sessão utilizando estas credenciais da seguinte forma (tenha em atenção que, como tipos de Azure fluent core `IAzure` estão a [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) pacote):

```csharp
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

Assim que a sessão iniciada, a contagem de instâncias do conjunto de dimensionamento pode ser consultada através de `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Aumentar horizontalmente
Utilizar o Azure fluent computação SDK, podem ser adicionadas a instâncias para o conjunto com as chamadas alguns - de dimensionamento de máquina virtual

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Em alternativa, o tamanho do conjunto de dimensionamento de máquina virtual também pode ser gerido com os cmdlets do PowerShell. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) Pode obter o objeto de conjunto de dimensionamento de máquina virtual. A capacidade atual está disponível através de `.sku.capacity` propriedade. Depois de alterar a capacidade para o valor pretendido, o conjunto no Azure de dimensionamento de máquina virtual pode ser atualizado com o [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) comando.

Como adicionar uma escala definir instância deve ser quando adicionar um nó manualmente, tudo o que precisa para iniciar um novo nó de Service Fabric, uma vez que a escala definir modelo inclui extensões para associar automaticamente novas instâncias para o cluster do Service Fabric. 

## <a name="scaling-in"></a>Dimensionamento no

Dimensionamento no é semelhante ao aumentar horizontalmente. O máquina de virtual real do conjunto de dimensionamento alterações são praticamente os mesmos. No entanto, tal como foi mencionado anteriormente, Service Fabric só limpa automaticamente removidas nós com uma durabilidade de ouro ou Silver. Por isso, as características de durabilidade Bronze escala em maiúsculas e minúsculas, é necessário interagir com o cluster do Service Fabric para encerrar o nó a remover e, em seguida, para remover o seu estado.

Preparar o nó de encerramento envolve ao localizar o nó ser removido (a mais recentemente adicionada escala conjunto instância de máquina virtual) e desativá-lo. Instâncias de conjunto de dimensionamento de máquina virtual estão numeradas pela ordem que forem adicionadas, pelo que podem ser encontrados nós mais recentes ao comparar o sufixo numérico nos nomes de nós (que correspondem a escala de máquina virtual subjacente definido nomes de instância). 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Assim que for encontrado o nó ser removido, podem ser desativado e removida com o mesmo `FabricClient` instância e o `IAzure` instância de versões anteriores.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Como aumentar horizontalmente, cmdlets do PowerShell para modificar o dimensionamento da máquina virtual com o conjunto de capacidade também pode ser utilizada aqui se uma abordagem de script é preferível. Assim que a instância de máquina virtual é removida, pode remover o estado de nó de Service Fabric.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="next-steps"></a>Passos Seguintes

Para começar a implementar a sua própria lógica de dimensionamento automático, familiarize-se com os seguintes conceitos e APIs úteis:

- [Dimensionar manualmente ou com regras de dimensionamento automático](./service-fabric-cluster-scale-up-down.md)
- [Fluent bibliotecas de gestão do Azure para .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (útil para interagir com conjuntos de dimensionamento de máquina virtual do cluster do Service Fabric subjacente)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (útil para interagir com um cluster do Service Fabric e os respetivos nós)
