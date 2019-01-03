---
title: Como remotamente um nó escreva no Azure Service Fabric | Documentos da Microsoft
description: Saiba como remover um tipo de nó no Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: v-steg
manager: JeanPaul.Connick
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/26/2018
ms.author: v-steg
ms.openlocfilehash: 3704a356763b16a30285baee1aabffdd3aa3f8aa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53980859"
---
# <a name="remove-a-service-fabric-node-type"></a>Remover um tipo de nó do Service Fabric

Uso [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) para remover um tipo de nó do Service Fabric.

As duas operações que ocorrem quando é chamado Remove-AzureRmServiceFabricNodeType são:
1.  A Máquina Virtual de dimensionamento definido (VMSS) por trás do tipo de nó é eliminado.
2.  Para todos os nós dentro desse tipo de nó, todo o estado para esse nó é removido do sistema. Se existirem serviços nesse nó, em seguida, os serviços são primeiro movidos para outro nó. Se o Gestor de clusters não é possível encontrar um nó para o serviço/réplica, em seguida, a operação é atrasado/bloqueado.

> [!NOTE]
> Utilizar Remove-AzureRmServiceFabricNodeType remover um tipo de nó de um cluster de produção não é recomendada a ser utilizada com frequência. Neste caso, é um comando muito perigoso como basicamente elimina o recurso de conjunto de dimensionamento de Máquina Virtual por trás do tipo de nó. Quando chama Remove-AzureRmServiceFabricNodeType, o sistema não tem como saber se se preocupa a remoção, garantindo a segurança. 

## <a name="durability-characteristics"></a>Características de durabilidade
Segurança é priorizada em relação a velocidade, ao utilizar Remove-AzureRmServiceFabricNodeType o limite de tempo. Gold ou Silver [características de durabilidade](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) são recomendadas, porque:
- Bronze não dão-lhe oferece garantias sobre ao guardar as informações de estado.
- Silver e Gold durabilidade interceptar todas as alterações para o VMSS.
- Ouro também lhe dá controle sobre o Azure atualiza por baixo do VMSS.

Service Fabric "orquestra" subjacentes de alterações e atualizações para que os dados não são perdidos. No entanto, ao remover um nó com durabilidade de Bronze, pode perder as informações de estado. Se está removendo a um tipo de nó primário e a sua aplicação está sem monitoração de estado, Bronze é aceitável. Ao executar cargas de trabalho com monitorização de estado em produção, a configuração mínima deve ser Silver. Da mesma forma, para cenários de produção do tipo de nó principal deve ser sempre Gold ou Silver.

### <a name="more-about-bronze-durability"></a>Mais informações sobre a durabilidade de Bronze

Quando remover um tipo de nó é Bronze, todos os nós num tipo de nó descer imediatamente. Service Fabric não intercetar quaisquer atualizações VMSS de nós de Bronze, portanto, todas as VMs descer imediatamente. Se tiver qualquer coisa com monitoração de estado em nós, os dados são perdidos. Agora, mesmo se fosse sem monitoração de estado, todos os nós nos recursos de infraestrutura do serviço de participam no anel, portanto, uma vizinhança inteira pode ser perdida, que poderão afetar o próprio cluster.

Ao contrário de remoção de um único nó, porque, em teoria, pode remover um nó ao mesmo tempo, aguarde que as réplicas e os serviços para mova sobre, aguarde que o sistema estabilizar, antes de remover o outro nó e assim por diante.  No entanto, se remover vários nós ao mesmo tempo em simultâneo, o cluster pode descer (uma vez que o Service Fabric não interceptar todas as atualizações VMSS com durabilidade de Bronze).

## <a name="recommended-node-type-removal-process"></a>Recomendado o processo de remoção de tipo de nó

Para remover o tipo de nó de forma mais segura e rápida:
1.  Se estiver a utilizar durabilidade de Bronze ou não pretender que o sistema se mova as aplicações que contêm informações de estado que serão perdidas como parte da eliminação de tipo de nó, primeiros dados de com monitoração de estado vazios a partir de nós que serão afetados pela remoção de tipo de nó.
2.  Execute [Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) em todos os nós que pretende que sejam removidos.
3.  Execute [Remove-AzureRmVmss](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#remove-vms-from-a-scale-set) para as VMs que serão afetadas pela remoção de tipo de nó.
4. Execute [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) para remover o tipo de nó.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre o cluster [características de durabilidade](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).
- Saiba mais sobre [tipos de nós e conjuntos de dimensionamento de máquinas virtuais](service-fabric-cluster-nodetypes.md).
- Saiba mais sobre [dimensionamento de clusters do Service Fabric](service-fabric-cluster-scaling.md).