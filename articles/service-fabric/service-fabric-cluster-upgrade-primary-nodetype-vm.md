---
title: Atualizar um cluster do Azure Service Fabric SO ou VMs SKU | Microsoft Docs
description: Saiba como atualizar as máquinas virtuais no nodetype principal de um cluster de Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: ryanwi
ms.openlocfilehash: bf707bf4b1c001b5467dd9954e9c6feb55e65654
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655746"
---
# <a name="upgrade-the-primary-node-type-vms-of-a-service-fabric-cluster"></a>Atualizar o tipo de nó principal VMs de um cluster do Service Fabric
Este artigo descreve como atualizar as máquinas de virtuais do tipo de nó principal de um cluster de Service Fabric em execução no Azure.  Um cluster do Service Fabric é um conjunto de ligados à rede de máquinas virtuais ou físicos para o qual os micro-serviços são implementados e geridos. Um computador ou a VM que faz parte de um cluster é designado por um nó. Conjuntos de dimensionamento de máquina virtual são um recurso de computação do Azure que utilizar para implementar e gerir uma coleção de máquinas virtuais como um conjunto. Cada tipo de nó que está definido num cluster do Azure é [configurado como um conjunto de dimensionamento separado](service-fabric-cluster-nodetypes.md). Em seguida, pode ser gerido a cada tipo de nó em separado. Depois de criar um cluster do Service Fabric, pode dimensionar um tipo de nó de cluster verticalmente (alterar os recursos de nós) ou Atualize o sistema operativo do tipo nó a VMs.  Pode dimensionar o cluster em qualquer altura, mesmo quando as cargas de trabalho em execução no cluster.  Como o cluster dimensiona, as suas aplicações, dimensionar bem.

> [!WARNING]
> Recomendamos que altere o SKU de VM de um tipo de nó/conjunto de dimensionamento, a menos que está a ser executado em [Silver durabilidade ou superior](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Alterar tamanho da VM SKU é uma operação de infraestrutura do destrutivas de dados no local. Sem capacidade algum atraso ou monitorizar esta alteração, é possível que a operação pode causar a perda de dados para os serviços com monitorização de estado ou fazer com que outros problemas operacionais imprevistos, mesmo para cargas de trabalho sem monitorização de estado. 
>

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Atualizar o tamanho e o sistema operativo do tipo de nó principal VMs
Eis o processo para atualizar o tamanho da VM e o sistema operativo do tipo de nó principal VMs.  Após a atualização, o tipo de nó principal VMs são tamanho Standard D4_V2 e Datacenter de 2016 do Windows Server em execução com contentores.

> [!WARNING]
> Antes de repetir este procedimento num cluster de produção, recomendamos que Estude os modelos de exemplo e verifique se o processo de um cluster de teste. O cluster também está disponível durante um período de tempo.

1. Implementar o cluster inicial com dois tipos de nó e conjuntos de dimensionamento dois (um dimensionamento definido por tipo de nó) a utilizar estas exemplo [modelo](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) e [parâmetros](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) ficheiros.  Ambos os conjuntos de dimensionamento são tamanho D2_V2 padrão e em execução Datacenter do Windows Server 2012 R2.  Aguarde que o cluster concluir a atualização de linha de base.   
2. Opcional - implementar um exemplo de monitorização de estado para o cluster.
3. Depois de decidir atualizar o tipo de nó principal VMs, adicione um novo dimensionamento definido para o tipo de nó principal a utilizar estas exemplo [modelo](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) e [parâmetros](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) ficheiros para o tipo de nó principal tem agora dois conjuntos de dimensionamento.  Serviços do sistema e as aplicações de utilizador são capazes de migrar entre VMs em dois conjuntos de dimensionamento diferentes.  O conjunto de dimensionamento novas VMs são tamanho D4_V2 padrão e execute o Centro de dados do Windows Server 2016 com contentores.  Um novo Balanceador de carga e o endereço IP público também são adicionados com o novo conjunto de dimensionamento.  
    Para localizar o conjunto no modelo de dimensionamento nova, procure o recurso "Microsoft.Compute/virtualMachineScaleSets" com o nome de *vmNodeType2Name* parâmetro.  O novo conjunto de dimensionamento é adicionado para o tipo de nó principal utilizando as propriedades -> virtualMachineProfile - > extensionProfile -> extensões -> propriedades -> Definições -> nodeTypeRef definição.
4. Verifique o estado de funcionamento do cluster e certifique-se de que todos os nós estão em bom Estados.
5. Desative os nós no conjunto de dimensionamento antigo do tipo de nó principal com a tentativa de remover o nó. Pode desativar ao mesmo tempo e as operações são colocados em fila. Aguarde até que todos os nós estiverem desativados, o que pode demorar algum tempo.  À medida que os nós no tipo de nó mais antigos estão desativados, migrar os nós de seed e serviços do sistema para as VMs da escala novo definido no tipo de nó principal.
6. Remova o conjunto do tipo de nó principal de dimensionamento mais antigo.
7. Remova o Balanceador de carga associado o conjunto de dimensionamento antigo. O cluster não está disponível enquanto o novo público IP endereço e a carga balanceador estão configurados para o novo conjunto de dimensionamento.  
8. As definições de DNS do arquivo do endereço IP público associadas com o nó principal antigo escreva o conjunto de dimensionamento numa variável e remover esse endereço IP público.
9. Substitua as definições de DNS do endereço IP público associados a escala de tipo de nó principal novo conjunto com as definições de DNS do endereço IP público eliminado.  O cluster está agora acessível novamente.
10. Remove o estado do nó de nós do cluster.  Se o nível de durabilidade do conjunto de dimensionamento antigo foi silver ou ouro, este passo é feito automaticamente pelo sistema.
11. Se implementou a aplicação com monitorização de estado num passo anterior, certifique-se de que a aplicação está funcional.

```powershell
# Variables.
$groupname = "sfupgradetestgroup"
$clusterloc="southcentralus"  
$subscriptionID="<your subscription ID>"

# sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $subscriptionID 

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Deploy the two node type cluster.
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.json" -Verbose

# Connect to the cluster and check the cluster health.
$ClusterName= "sfupgradetest.southcentralus.cloudapp.azure.com:19000"
$thumb="F361720F4BD5449F6F083DDE99DC51A86985B25B"

Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Get-ServiceFabricClusterHealth

# Deploy a new scale set into the primary node type.  Create a new load balancer and public IP address for the new scale set.
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.json" -Verbose

# Check the cluster health again. All 15 nodes should be healthy.
Get-ServiceFabricClusterHealth

# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}

Write-Host "Checking node status..."
foreach($name in $nodeNames){
 
    $state = Get-ServiceFabricNode -NodeName $name 

    $loopTimeout = 50

    do{
        Start-Sleep 5
        $loopTimeout -= 1
        $state = Get-ServiceFabricNode -NodeName $name
        Write-Host "$name state: " $state.NodeDeactivationInfo.Status
    }

    while (($state.NodeDeactivationInfo.Status -ne "Completed") -and ($loopTimeout -ne 0))
    

    if ($state.NodeStatus -ne [System.Fabric.Query.NodeStatus]::Disabled)
    {
        Write-Error "$name node deactivation failed with state" $state.NodeStatus
        exit
    }
}

# Remove the scale set
$scaleSetName="NTvm1"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force
Write-Host "Removed scale set $scaleSetName"

$lbname="LB-sfupgradetest-NTvm1"
$oldPublicIpName="PublicIP-LB-FE-0"
$newPublicIpName="PublicIP-LB-FE-2"

# Store DNS settings of public IP address related to old Primary NodeType into variable 
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname

$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel

$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

# Remove Load Balancer related to old Primary NodeType. This will cause a brief period of downtime for the cluster
Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force

# Remove the old public IP
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

# Check the cluster health
Get-ServiceFabricClusterHealth

# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [escalabilidade de aplicação](service-fabric-concepts-scalability.md).
* [Dimensionar um cluster do Azure ou reduzir](service-fabric-tutorial-scale-cluster.md).
* [Dimensionar um cluster do Azure através de programação](service-fabric-cluster-programmatic-scaling.md) através do Azure fluent computação SDK.
* [Um cluster de standaone entrada ou saída de escala](service-fabric-cluster-windows-server-add-remove-nodes.md).

