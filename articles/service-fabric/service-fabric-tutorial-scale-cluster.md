---
title: Dimensionar um cluster do Azure Service Fabric | Microsoft Docs
description: Neste tutorial, saiba como dimensionar rapidamente um cluster do Service Fabric.
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/06/2018
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 678ca45d12fd10a02d967cd32743b4d7b6ea26af
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642704"
---
# <a name="tutorial-scale-a-service-fabric-cluster"></a>Tutorial: dimensionar um cluster do Service Fabric

Este tutorial é a segunda parte de uma série e mostra-lhe como aumentar e reduzir o cluster horizontalmente. Quando tiver terminado, saberá como dimensionar o seu cluster e como limpar quaisquer recursos restantes.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ler a contagem de nós de cluster
> * Adicionar nós de cluster (aumentar horizontalmente)
> * Remover nós de cluster (reduzir horizontalmente)

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * Criar um [cluster do Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) ou um [cluster do Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) protegidos no Azure utilizando um modelo
> * Reduzir ou aumentar horizontalmente um cluster
> * [Atualizar o tempo de execução de um cluster](service-fabric-tutorial-upgrade-cluster.md)
> * [Implementar a Gestão de API com o Service Fabric](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial:
- Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Instale o [módulo do Azure PowerShell versão 4.1 ou superior](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ou a [CLI do Azure 2.0](/cli/azure/install-azure-cli).
- Crie um [cluster do Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) ou um [cluster do Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) protegidos no Azure.
- Se implementar um cluster do Windows, configure um ambiente de desenvolvimento do Windows. Instale o [Visual Studio 2017](http://www.visualstudio.com) e as cargas de trabalho **desenvolvimento no Azure**, **desenvolvimento em ASP.NET e na Web** e **desenvolvimento em várias plataformas .NET Core**.  Em seguida, configure um [ambiente de desenvolvimento .NET](service-fabric-get-started.md).
- Se implementar um cluster do Linux, configure um ambiente de desenvolvimento Java no [Linux](service-fabric-get-started-linux.md) ou no [MacOS](service-fabric-get-started-mac.md).  Instale a [CLI do Service Fabric](service-fabric-cli.md). 

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão na sua conta do Azure, selecione a sua subscrição antes de executar os comandos do Azure.

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Ligar ao cluster

Para concluir com êxito esta parte do tutorial, precisa de ligar ao cluster do Service Fabric e ao conjunto de dimensionamento de máquinas virtuais (que aloja o cluster). O conjunto de dimensionamento de máquinas virtuais é o recurso do Azure que aloja o Service Fabric no Azure.

Quando liga a um cluster, pode consultá-lo para obter informações. Pode utilizar o cluster para saber mais sobre os nós detetados pelo cluster. A ligação ao cluster no código seguinte utiliza o mesmo certificado que foi criado na primeira parte desta série. Certifique-se de que define as variáveis `$endpoint` e `$thumbprint` para os seus valores.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint $thumbprint `
          -FindType FindByThumbprint -FindValue $thumbprint `
          -StoreLocation CurrentUser -StoreName My

Get-ServiceFabricClusterHealth
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Agora que está ligado, pode utilizar um comando para obter o estado de cada nó do cluster. Para **PowerShell**, utilize o comando `Get-ServiceFabricClusterHealth` e para **sfctl**, utilize o comando `sfctl cluster select`.

## <a name="scale-out"></a>Aumentar horizontalmente

Ao aumentar horizontalmente, adiciona mais instâncias de máquina virtual ao conjunto de dimensionamento. Estas instâncias tornam-se os nós que o Service Fabric utiliza. O Service Fabric sabe quando são adicionadas mais instâncias ao conjunto de dimensionamento (ao aumentar horizontalmente) e reage automaticamente. O código seguinte obtém um conjunto de dimensionamento por nome e aumenta a **capacidade** do conjunto de dimensionamento em 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Este código define a capacidade para 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Reduzir horizontalmente

Reduzir horizontalmente é o mesmo que aumentar horizontalmente, exceto que utiliza um valor de **capacidade** inferior. Ao reduzir horizontalmente o conjunto de dimensionamento, remove as instâncias de máquina virtual do conjunto de dimensionamento. Normalmente, o Service Fabric desconhece o que ocorreu e pensa que desapareceu um nó. O Service Fabric comunica então o mau estado de funcionamento do cluster. Para impedir esse mau estado, deve informar os recursos de infraestrutura do serviço de que espera que o nó desapareça.

### <a name="remove-the-service-fabric-node"></a>Remover o nó dos recursos de infraestrutura do serviço

> [!NOTE]
> Esta parte aplica-se apenas ao escalão de durabilidade *Bronze*. Para obter mais informações sobre a durabilidade, veja [Planeamento da capacidade de cluster do Service Fabric][durability].

Ao reduzir horizontalmente um conjunto de dimensionamento de máquinas virtuais, o conjunto de dimensionamento (na maioria dos casos) remove a última instância de máquina virtual a ser criada. Por conseguinte, tem de encontrar o nó correspondente dos recursos de infraestrutura do serviço, que foi o último a ser criado. Pode encontrar este nó verificando o valor de propriedade `NodeInstanceId` maior nos nós dos recursos de infraestrutura do serviço. Os exemplos de código abaixo estão ordenados por instância de nó e devolvem os detalhes da instância com o maior valor de id. 

```powershell
Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1
```

```azurecli
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

O cluster dos recursos de infraestrutura do serviço tem de saber que este nó vai ser removido. Tem de seguir três passos:

1. Desative o nó para que deixe de ser uma replicação dos dados.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Pare o nó, para que o runtime dos recursos de infraestrutura do serviço encerre corretamente e que a aplicação obtenha um pedido para terminar.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Remova o nó do cluster.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Depois de estes três passos serem aplicados ao nó, este pode ser removido do conjunto de dimensionamento. Se estiver a utilizar qualquer escalão de durabilidade além de [bronze][durability], estes passos serão efetuados quando a instância do conjunto de dimensionamento for removida.

O bloco de código seguinte obtém o último nó criado, desativa, interrompe e remove o nó do cluster.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300
    
    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }
    
    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

No código **sfctl** abaixo, o comando seguinte é utilizado para aceder ao valor **node-name** do último nó criado: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Utilize as seguintes consultas **sfctl** para verificar o estado de cada passo
>
> **Verificar o estado de desativação**  
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Verificar o estado de paragem**  
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>


### <a name="scale-in-the-scale-set"></a>Reduzir horizontalmente o conjunto de dimensionamento

Agora que o nó dos recursos de infraestrutura do serviço foi removido do cluster, o conjunto de dimensionamento de máquinas virtuais pode ser reduzido horizontalmente. No exemplo abaixo, a capacidade do conjunto de dimensionamento foi reduzida em 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Este código define a capacidade para 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Ler a contagem de nós de cluster
> * Adicionar nós de cluster (aumentar horizontalmente)
> * Remover nós de cluster (reduzir horizontalmente)


Em seguida, avance para o tutorial seguinte para saber como atualizar o runtime de um cluster.
> [!div class="nextstepaction"]
> [Atualizar o tempo de execução de um cluster](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
