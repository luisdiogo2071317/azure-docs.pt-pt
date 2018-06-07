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
ms.openlocfilehash: 28424f9a7a0f77882ee3360c5599549303075c18
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642578"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Remoto ligar a uma instância de conjunto de dimensionamento de máquina virtual ou um nó de cluster
Um recurso de infraestrutura do serviço de cluster em execução no Azure, cada tipo de nó de cluster que definir [configura um dimensionamento independente da máquina virtual](service-fabric-cluster-nodetypes.md).  Remoto pode ligar a instâncias de conjunto de dimensionamento específico (nós de cluster).  Ao contrário das VMs de instância única, as instâncias do conjunto de dimensionamento não tem os seus próprios endereços IP virtuais. Isto pode ser um desafio quando está a procurar um endereço IP e a porta que pode utilizar para ligar remotamente a uma instância específica.

Para localizar um endereço IP e a porta que pode utilizar para ligar remotamente a uma instância específica, conclua os passos seguintes.

1. Obter as regras NAT de entrada para o protocolo de ambiente de trabalho remoto (RDP).

    Normalmente, cada tipo de nó definido no seu cluster tem o seu próprio endereço IP virtual e um balanceador de carga dedicado. Por predefinição, o Balanceador de carga para um tipo de nó com o nome com o seguinte formato: *LB-{nome do cluster}-{tipo de nó}*; por exemplo, *LB-mycluster-front-end*. 
    
    Na página do seu Balanceador de carga no portal do Azure, selecione **definições** > **regras NAT de entrada**: 

    ![As regras de NAT de entrada do Balanceador de carga](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    A seguinte captura de ecrã mostra as regras NAT de entrada para um tipo de nó com o nome de front-end: 

    ![As regras de NAT de entrada do Balanceador de carga](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    Para cada nó, o endereço IP é apresentado no **destino** coluna, a **destino** coluna fornece a instância de conjunto de dimensionamento e o **serviço** coluna fornece o número de porta. Para uma ligação remota, portas são atribuídas a cada nó no início de ordem com a porta 3389 de ascendente.

    Também pode encontrar as regras de NAT de entrada no `Microsoft.Network/loadBalancers` secção do modelo do Resource Manager para o cluster.
    
2. Para confirmar a porta de entrada para mapeamento de portas de destino para um nó, pode clicar a regra e observe o **porta de destino** valor. A seguinte captura de ecrã mostra a regra NAT de entrada para o **front-end (1 instância)** nó no passo anterior. Tenha em atenção que, apesar do número da porta (entrada) 3390, a porta de destino está mapeada para a porta 3389, a porta para o serviço RDP no destino.  

    ![Mapeamento de portas de destino](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    Por predefinição, para os clusters do Windows, a porta de destino é a porta 3389, que mapeia para o serviço RDP no nó de destino. Para os clusters do Linux, a porta de destino é a porta 22, que mapeia para o serviço de Secure Shell (SSH).

3. Ligar remotamente ao nó específico (instância de conjunto de dimensionamento). Pode utilizar o nome de utilizador e palavra-passe que definiu quando criou o cluster ou quaisquer outras credenciais que configurou. 

    A seguinte captura de ecrã mostra utilizando a ligação ao ambiente de trabalho remoto para ligar ao **front-end (1 instância)** nós num cluster do Windows:
    
    ![Ligação de ambiente de trabalho remota](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Em nós do Linux, pode ligar com SSH (o exemplo seguinte reutiliza o mesmo endereço IP e a porta de uma forma abreviada):

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Para os passos seguintes, leia os artigos seguintes:
* Consulte o [descrição geral da funcionalidade "Implementar em qualquer local" e uma comparação com clusters geridos pelo Azure](service-fabric-deploy-anywhere.md).
* Saiba mais sobre [cluster segurança](service-fabric-cluster-security.md).
* [Atualize os valores de intervalo de portas RDP](./scripts/service-fabric-powershell-change-rdp-port-range.md) no cluster VMs após a implementação
* [Alterar o nome de utilizador de administrador e a palavra-passe](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) para as VMs do cluster

