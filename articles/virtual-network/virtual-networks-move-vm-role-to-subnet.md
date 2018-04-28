---
title: Mover VM (clássica) ou instância de função de serviços em nuvem para outra sub-rede - Azure PowerShell | Microsoft Docs
description: Saiba como mover VMs (clássica) e instâncias de função de serviços em nuvem para outra sub-rede com o PowerShell.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eb1576b7b842537dbdc49e6b9d402ef47ad9fae9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a>Mover VM (clássica) ou instância de função de serviços em nuvem para outra sub-rede com o PowerShell
Pode utilizar o PowerShell para mover as VMs (clássica) de uma sub-rede para outra na mesma rede virtual (VNet). Instâncias de função podem ser movidas ao editar o ficheiro CSCFG, em vez de utilizar o PowerShell.

> [!NOTE]
> Este artigo explica como mover VMs implementadas através do modelo de implementação clássica apenas.
> 
> 

Por que motivo mover as VMs para outra sub-rede? Migração de sub-rede é útil quando a sub-rede mais antiga é demasiado pequena e não pode ser expandida devido a existentes VMs em execução nessa sub-rede. Nesse caso, pode criar uma sub-rede nova, maior e migrar as VMs para a sub-rede nova, em seguida, após a conclusão da migração, pode eliminar a sub-rede vazia antiga.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Como mover uma VM para outra sub-rede
Para mover uma VM, execute o cmdlet Set-AzureSubnet PowerShell, utilizando o exemplo abaixo como um modelo. No exemplo abaixo, iremos estiver a mover TestVM da respetiva sub-rede presente, a sub-rede-2. É necessário editar o exemplo para refletir o seu ambiente. Tenha em atenção que sempre que executar o cmdlet Update-AzureVM como parte de um procedimento, irá reiniciar a VM como parte do processo de atualização.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Se tiver especificado um IP privado internos estático para a VM, terá de limpar essa definição antes de pode mover a VM para uma nova sub-rede. Nesse caso, utilize o seguinte:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Para mover uma instância de função para outra sub-rede
Para mover uma instância de função, edite o ficheiro CSCFG. No exemplo abaixo, iremos estiver a mover "Role0" na rede virtual *VNETName* da respetiva sub-rede presente para *sub-rede 2*. Porque a instância de função já foi implementada, apenas terá de alterar o nome de sub-rede = 2 de sub-rede. É necessário editar o exemplo para refletir o seu ambiente.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
