---
title: Estabelecer ligação remota para um nó de cluster do Service Fabric do Azure | Microsoft Docs
description: Saiba como ligar remotamente a uma instância de conjunto de dimensionamento (um nó de cluster do Service Fabric).
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo
ms.openlocfilehash: 3c7b3626db0e38d28513d4665a83dd7155663034
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Remoto ligar a uma instância de conjunto de dimensionamento de máquina virtual ou um nó de cluster
Um recurso de infraestrutura do serviço de cluster em execução no Azure, cada tipo de nó de cluster que definir [configura um dimensionamento independente da máquina virtual](service-fabric-cluster-nodetypes.md).  Pode remoto ligar-se a escala específica conjunto instâncias (ou nós de cluster).  Ao contrário das VMs de instância única, as instâncias do conjunto de dimensionamento não tem os seus próprios endereços IP virtuais. Isto pode ser um desafio quando está a procurar um endereço IP e a porta que pode utilizar para ligar remotamente a uma instância específica.

Para localizar um endereço IP e a porta que pode utilizar para ligar remotamente a uma instância específica, conclua os passos seguintes.

1. Localize o endereço IP virtual para o tipo de nó por obter regras NAT de entrada para o protocolo de ambiente de trabalho remoto (RDP).

    Em primeiro lugar, obter os valores de regras NAT entrados que foram definidos como parte da definição do recurso para `Microsoft.Network/loadBalancers`.
    
    No portal do Azure, na página de Balanceador de carga, selecione **definições** > **regras NAT de entrada**. Isto fornece o endereço IP e definir a porta que pode utilizar para ligar remotamente à escala primeira instância. 
    
    ![Load balancer][LBBlade]
    
    Na figura seguinte, o endereço IP e a porta são **104.42.106.156** e **3389**.
    
    ![Regras de NAT][NATRules]

2. Localize a porta que pode utilizar para ligar remotamente para a instância do conjunto de dimensionamento específico ou o nó.

    Conjunto de dimensionamento mapa de instâncias para nós. Utilize as informações de conjunto de dimensionamento para determinar a porta exata a utilizar.
    
    As portas são alocadas por uma ordem ascendente que corresponda a instância do conjunto de dimensionamento. Para obter o anterior exemplo o tipo de nó de front-end, a tabela seguinte lista as portas para cada uma das instâncias do cinco nó. Aplica o mesmo mapeamento à sua instância do conjunto de dimensionamento.
    
    | **Instância do conjunto de dimensionamento de máquina virtual** | **Porta** |
    | --- | --- |
    | FrontEnd_0 |3389 |
    | FrontEnd_1 |3390 |
    | FrontEnd_2 |3391 |
    | FrontEnd_3 |3392 |
    | FrontEnd_4 |3393 |
    | FrontEnd_5 |3394 |

3. Ligar de forma remota a instância do conjunto de dimensionamento específico.

    A figura seguinte demonstra utilizando a ligação ao ambiente de trabalho remoto para ligar à instância de conjunto de dimensionamento do FrontEnd_1:
    
    ![Ligação de ambiente de trabalho remota][RDP]


Para os passos seguintes, leia os artigos seguintes:
* Consulte o [descrição geral da funcionalidade "Implementar em qualquer local" e uma comparação com clusters geridos pelo Azure](service-fabric-deploy-anywhere.md).
* Saiba mais sobre [cluster segurança](service-fabric-cluster-security.md).
* [Atualize os valores de intervalo de portas RDP](./scripts/service-fabric-powershell-change-rdp-port-range.md) no cluster VMs após a implementação
* [Alterar o nome de utilizador de administrador e a palavra-passe](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) para as VMs do cluster

<!--Image references-->
[LBBlade]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/NATRules.png
[RDP]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/RDP.png
