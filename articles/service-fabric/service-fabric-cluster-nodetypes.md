---
title: Conjuntos de dimensionamento de tipos de nó de Service Fabric do Azure e a máquina virtual | Microsoft Docs
description: Saiba como define o nó de Azure Service Fabric tipos relacionados com o dimensionamento de máquina virtual e como ligar remotamente a uma escala definir instância ou nó de cluster.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: 84d7f407781f09fed4667a22f0a46bc72c6e02a9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212369"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Conjuntos de dimensionamento de tipos de nó de Service Fabric do Azure e a máquina virtual
[Conjuntos de dimensionamento de máquina virtual](/azure/virtual-machine-scale-sets) são recursos de computação de um Azure. Pode utilizar conjuntos de dimensionamento para implementar e gerir uma coleção de máquinas virtuais como um conjunto. Cada tipo de nó que definem num cluster do Azure Service Fabric configura um dimensionamento independente.  Definir o tempo de execução do Service Fabric instalado em cada máquina virtual na escala. Independentemente pode dimensionar cada tipo de nó ou reduzir verticalmente, alterar o SKU de SO em execução em cada nó de cluster, têm conjuntos diferentes de portas abertas e utilizar as métricas de capacidade diferentes.

A figura seguinte mostra um cluster que tem dois tipos de nó, com o nome de front-end e back-end. Cada tipo de nó tem cinco nós.

![Um cluster que tem dois tipos de nó][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Mapear instâncias de conjunto de dimensionamento de máquina virtual para nós
Conforme mostrado na imagem anterior, as instâncias do conjunto de dimensionamento começam em 0 de instância e, em seguida, aumentam, 1. A numeração é refletida nos nomes de nó. Por exemplo, o nó BackEnd_0 é 0 uma instância do conjunto de dimensionamento de back-end. Este conjunto de dimensionamento específica tem cinco instâncias, com o nome BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 e BackEnd_4.

Quando dimensionar um conjunto de dimensionamento, é criada uma nova instância. O novo nome de instância do conjunto de dimensionamento é, normalmente, que a escala definir nome e o seguinte número de instância. No nosso exemplo, é BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Mapear balanceadores de carga do conjunto de dimensionamento para tipos de nó e conjuntos de dimensionamento
Se implementar o seu cluster no portal do Azure ou utilizar o modelo Azure Resource Manager de exemplo, são listados todos os recursos de um grupo de recursos. Pode ver os balanceadores de carga para cada tipo de conjunto ou nó de escala. O nome do Balanceador de carga utiliza o seguinte formato: **LB -&lt;nome do tipo de nó&gt;**. Um exemplo é LB-sfcluster4doc-0, conforme mostrado na figura seguinte:

![Recursos][Resources]


## <a name="next-steps"></a>Passos Seguintes
* Consulte o [descrição geral da funcionalidade "Implementar em qualquer local" e uma comparação com clusters geridos pelo Azure](service-fabric-deploy-anywhere.md).
* Saiba mais sobre [cluster segurança](service-fabric-cluster-security.md).
* [Ligar remoto](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) a uma instância de conjunto de dimensionamento específico
* [Atualize os valores de intervalo de portas RDP](./scripts/service-fabric-powershell-change-rdp-port-range.md) no cluster VMs após a implementação
* [Alterar o nome de utilizador de administrador e a palavra-passe](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) para as VMs do cluster

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
