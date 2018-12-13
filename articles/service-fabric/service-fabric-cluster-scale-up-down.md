---
title: Dimensionar um cluster do Service Fabric in ou out | Documentos da Microsoft
description: Dimensione um cluster do Service Fabric dentro ou para fora de acordo com a pedido através da definição de regras de dimensionamento automático para cada conjunto de dimensionamento de máquina de Virtual/tipo de nó. Adicionar ou remover nós do cluster do Service Fabric
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: aljo
ms.openlocfilehash: 91516e3284ebf3588c2dba31b67cc583e4d395db
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309427"
---
# <a name="read-before-you-scale"></a>Leia antes de aumentar
Dimensionar recursos de computação para a origem a carga de trabalho de aplicação requer um planejamento intencional, quase sempre irá demorar mais de uma hora para concluir para um ambiente de produção e exige que compreender a sua carga de trabalho e o contexto de negócios; na verdade se nunca tiver feito essa atividade antes, recomenda-se começar por ler e entender [considerações de planeamento de capacidade do cluster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity), antes de continuar o restante deste documento. Esta recomendação é evitar problemas de LiveSite indesejados, e também é recomendável que testar com êxito as operações que decidir executar em relação a um ambiente de não produção. Em qualquer altura, pode [comunicar problemas de produção ou pedido de suporte pago para o Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-production-issues-or-request-paid-support-for-azure). Para os engenheiros alocados para executar estas operações que possuem o contexto apropriado, este artigo descreverá as operações de dimensionamento, mas tem de decidir e compreender as operações são adequadas para seu caso de utilização; Por exemplo, quais recursos para dimensionamento (CPU, armazenamento, memória), que direção de dimensionamento (vertical ou horizontalmente) e quais operações a serem executadas (modelo do Resource a implementação, do Portal, o PowerShell/CLI).

## <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Dimensionar um cluster do Service Fabric dentro ou para fora usando regras de dimensionamento automático ou manual
Os conjuntos de dimensionamento de máquinas virtuais são um recurso de computação do Azure que pode utilizar para implementar e gerir uma coleção de máquinas virtuais como um conjunto. Cada tipo de nó que está definido no cluster do Service Fabric é configurado como um conjunto de dimensionamento de máquina virtual separada. Cada tipo de nó, em seguida, pode ser reduzido horizontalmente ou horizontalmente de forma independente, têm conjuntos diferentes de portas abertas e pode ter métricas de capacidade diferente. Saiba mais sobre ele na [nodetypes do Service Fabric](service-fabric-cluster-nodetypes.md) documento. Uma vez que os tipos de nós no cluster do Service Fabric são constituídos por conjuntos de dimensionamento de máquinas virtuais no back-end, terá de configurar regras de dimensionamento automático para cada conjunto de dimensionamento de máquina de Virtual/tipo de nó.

> [!NOTE]
> A sua subscrição tem de ter núcleos suficientes para adicionar as VMs novas que compõem este cluster. Não existe nenhuma validação de modelo atualmente, para que tenha uma falha de tempo de implementação, se qualquer um dos limites são atingidos. Também um tipo de nó único não pode simplesmente exceder 100 nós por VMSS. Poderá ter de adicionar o VMSS alcançar a dimensão de destino, e o dimensionamento automático não pode automagicamente adicionar do VMSS. Adição direta do VMSS a um cluster ativo é uma tarefa desafiadora e normalmente isso resulta em novos clusters de aprovisionamento com os tipos de nó adequado aprovisionados no momento de criação de utilizadores [planear a capacidade de cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) em conformidade. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Escolha o nó tipo/Virtual conjunto de dimensionamento de dimensionamento de máquina
Atualmente, não é possível especificar as regras de dimensionamento automático para conjuntos de dimensionamento de máquina virtual com o portal para criar um Cluster do Service Fabric, então, vamos utilizar o Azure PowerShell (1.0 +) para listar os tipos de nó e, em seguida, adicionar regras de dimensionamento automático aos mesmos.

Para obter a lista de conjunto de dimensionamento de máquinas virtuais que compõem o cluster, execute os seguintes cmdlets:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Definir regras de dimensionamento automático para o nó tipo/Virtual conjunto de dimensionamento de máquina
Se o cluster tem vários tipos de nó, em seguida, repetir que isso para cada nó tipos/Virtual de dimensionamento de máquinas define de que pretende dimensionar (entrada ou saída). Antes de configurar o dimensionamento automático, tenha em conta o número de nós que tem de ter. O número mínimo de nós que tem de ter para o tipo de nó principal é condicionado pelo nível de fiabilidade que escolheu. Leia mais sobre [níveis de confiabilidade](service-fabric-cluster-capacity.md).

> [!NOTE]
> Reduzir verticalmente o nó principal, escreva para o menor do que a marca de número mínimo, o cluster instável ou colocá-la para baixo. Isto pode resultar em perda de dados para as suas aplicações e para os serviços do sistema.
> 
> 

Atualmente a funcionalidade de dimensionamento automático não é orientada por potenciais cargas que os seus aplicativos podem reportar ao Service Fabric. Então, neste momento, o dimensionamento automático, que obtém é puramente orientado pelos contadores de desempenho que são emitidos por cada da máquina virtual conjunto de dimensionamento de instâncias.  

Siga estas instruções [para configurar o dimensionamento automático para cada conjunto de dimensionamento de Máquina Virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> Numa escala para baixo do cenário, a menos que o seu tipo de nó tem um nível de durabilidade de Gold ou Silver precisa chamar o [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) com o nome de nó adequado. Para a durabilidade de Bronze, não é recomendado para reduzir verticalmente mais de um nó por vez.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Adicionar VMs manualmente para um nó tipo/Virtual conjunto de dimensionamento de máquina
Siga as instruções/exemplo no [Galeria de modelos de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) para alterar o número de VMs em cada Nodetype. 

> [!NOTE]
> Adição de VMs demora tempo, pelo que não conta as adições ser instantâneo. Então, planeie adicionar capacidade bem no tempo, para permitir mais de 10 minutos antes da capacidade VM está disponível para as réplicas / instâncias para obter colocado de serviço.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>Remova manualmente as VMs do nó primário tipo/Virtual conjunto de dimensionamento de máquina
> [!NOTE]
> Executam os serviços de sistema do service fabric no tipo de nó principal no seu cluster. Portanto, nunca deve encerrar ou reduzir verticalmente o número de instâncias em que tipos de nó menor do que o escalão de fiabilidade que justifica. Consulte a [os detalhes sobre as camadas de confiabilidade aqui](service-fabric-cluster-capacity.md). 
> 
> 

Terá de executar a seguinte passos uma instância de VM ao mesmo tempo. Isso permite que os serviços do sistema (e seus serviços com estado) ao encerrar corretamente na instância de VM que está a remover e réplicas novo, criadas nos outros nós.

1. Execute [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) com a intenção "RemoveNode" para desativar o nó pretende remover o (a instância desse tipo de nó mais alta).
2. Execute [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) para se certificar de que o nó tem de fato transitado para desativado. Caso contrário, aguarde até que o nó está desabilitado. Não é possível hurry este passo.
3. Siga as instruções/exemplo no [Galeria de modelos de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) para alterar o número de VMs por um nesse tipo de nó. A instância removida é a instância VM mais alta. 
4. Repita os passos 1 a 3 conforme necessário, mas nunca reduzir verticalmente o número de instâncias, os tipos de nó primário, de menos do que o que justifica o escalão de fiabilidade. Consulte a [os detalhes sobre as camadas de confiabilidade aqui](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>Remova manualmente as VMs do nó não primário tipo/Virtual conjunto de dimensionamento de máquina
> [!NOTE]
> Para um serviço com estado, terá de um determinado número de nós sempre até ser manter a disponibilidade e preservar o estado do seu serviço. No mínimo, terá do número de nós igual à contagem de conjunto de réplicas de destino do serviço/partição. 
> 
> 

Terá da executar a seguinte passos uma instância de VM ao mesmo tempo. Este procedimento permite que os serviços do sistema (e seus serviços com estado) ao encerrar corretamente na instância de VM que está a remover e réplicas novo criado onde mais.

1. Execute [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) com a intenção "RemoveNode" para desativar o nó pretende remover o (a instância desse tipo de nó mais alta).
2. Execute [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) para se certificar de que o nó tem de fato transitado para desativado. Caso contrário, aguarde até que o nó está desabilitado. Não é possível hurry este passo.
3. Siga as instruções/exemplo no [Galeria de modelos de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) para alterar o número de VMs por um nesse tipo de nó. Agora, esta ação irá remover a instância VM mais alta. 
4. Repita os passos 1 a 3 conforme necessário, mas nunca reduzir verticalmente o número de instâncias, os tipos de nó primário, de menos do que o que justifica o escalão de fiabilidade. Consulte a [os detalhes sobre as camadas de confiabilidade aqui](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Comportamentos, que pode observar no Service Fabric Explorer
Ao aumentar verticalmente um cluster do Service Fabric Explorer irá refletir o número de nós (instâncias de conjunto de dimensionamento de máquinas virtuais), que fazem parte do cluster.  No entanto, quando dimensiona um cluster para baixo, verá a instância VM/nó removido apresentada em mau estado de funcionamento, a menos que chamar [Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) com o nome de nó adequado.   

Aqui está a explicação para esse comportamento.

Os nós listados no Service Fabric Explorer são um reflexo do que os serviços de sistema do Service Fabric (FM especificamente) sabe sobre o número de nós de cluster tinha/tem. Quando dimensionar o conjunto de dimensionamento para baixo, a VM foi eliminada, mas serviço de sistema de FM ainda acha que o nó (que foi mapeado para a VM que foi eliminada) será retornar. Portanto, o Service Fabric Explorer continua exibir esse nó (embora o estado de funcionamento pode ser erro ou desconhecido).

Para certificar-se de que um nó é removido quando uma VM é removida, tem duas opções:

1. Escolha um nível de durabilidade de Gold ou Silver para os tipos de nó do cluster, o que lhe dá a integração de infraestrutura. Que, em seguida, removerá automaticamente os nós do nosso estado de serviços (FM) do sistema quando reduzir verticalmente.
Consulte [os detalhes sobre os níveis de durabilidade aqui](service-fabric-cluster-capacity.md)

2. Assim que a instância VM foi reduzida, precisa chamar o [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate).

> [!NOTE]
> Clusters do Service Fabric necessitam de um determinado número de nós para ficar operacional em todo o tempo para manter a disponibilidade e preservar o estado - referido como "manter o quórum." Assim, é normalmente não seguro para encerrar todas as máquinas no cluster, a menos que o utilizador tiver sido executado pela primeira vez um [cópia de segurança completa do seu estado](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Leia o seguinte para também saber mais sobre o planeamento de capacidade do cluster, atualização de um cluster e a criação de partições de serviços:

* [Planear a capacidade de cluster](service-fabric-cluster-capacity.md)
* [Atualizações de cluster](service-fabric-cluster-upgrade.md)
* [Serviços com estado de partição para dimensionamento máximo](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
