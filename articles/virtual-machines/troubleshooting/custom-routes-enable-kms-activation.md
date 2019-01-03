---
title: Utilizar rotas personalizadas do Azure para ativar a ativação do KMS com túnel forçado | Documentos da Microsoft
description: Mostra como utilizar rotas personalizadas do Azure para ativar a ativação do KMS com túnel forçado no Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: f1e2ab6a954361a7807d78dc2baf5d24af52a679
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53798090"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Falha de ativação do Windows no cenário de túnel forçado

Este artigo descreve como resolver o problema de ativação do KMS que podem ocorrer quando ativou forçado túnel numa ligação de VPN de site a site ou ExpressRoute cenários.

## <a name="symptom"></a>Sintoma

Ativar [túnel forçado](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) sobre o Azure sub-redes da rede virtual para direcionar todo o tráfego vinculado à Internet de volta para a sua rede no local. Neste cenário, as máquinas virtuais (VM) do Azure que executem o Windows Server 2012 R2 ou versões posteriores com êxito pode ativar o Windows. No entanto, as VMs que executam a versão anterior do Windows não ativar o Windows. 

## <a name="cause"></a>Causa

As VMs do Windows Azure tem de ligar ao servidor do Azure KMS para ativação do Windows. A ativação exige que o pedido de ativação tem de ser um endereço IP público do Azure. O cenário de túnel forçado, a ativação falhará porque o pedido de ativação é da sua rede no local em vez de partir de um IP público do Azure. 

## <a name="solution"></a>Solução

Para resolver este problema, utilize o tráfego de ativação de rota à rota personalizada do Azure para o servidor do KMS do Azure (23.102.135.246). 

O endereço IP 23.102.135.246 é o endereço IP do servidor KMS para a nuvem Global do Azure. O nome DNS é kms.core.windows.net. Se usar outras plataformas do Azure, como o Azure Alemanha, tem de utilizar o endereço IP do servidor KMS correspond. Para obter mais informações, consulte a tabela seguinte:

|Plataforma| DNS DO KMS|IP DE KMS|
|------|-------|-------|
|Azure Global|KMS.Core.Windows.NET|23.102.135.246|
|Azure Alemanha|KMS.Core.cloudapi.de|51.4.143.248|
|Azure US Government|KMS.Core.usgovcloudapi.NET|23.97.0.13|
|Azure China 21Vianet|KMS.Core.chinacloudapi.CN|42.159.7.249|


Para adicionar a rota personalizada, siga estes passos:

### <a name="for-resource-manager-vms"></a>Para VMs do Resource Manager

1. Abra o Azure PowerShell e, em seguida [iniciar sessão na sua subscrição do Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Execute os seguintes comandos:

    ```powershell
    # First, we will get the virtual network hosts the VMs that has activation problems. In this case, I get virtual network ArmVNet-DM in Resource Group ArmVNet-DM

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, we create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out

    $RouteTable = New-AzureRmRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzureRmRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzureRmRouteTable -RouteTable $RouteTable
    ```
3. Vá para a VM que tem o problema de ativação, utilize [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) para testar se ele pode contactar o servidor KMS:

        psping kms.core.windows.net:1688

4. Tente ativar o Windows e veja se o problema está resolvido.

### <a name="for-classic-vms"></a>Para VMs clássicas

1. Abra o Azure PowerShell e, em seguida [iniciar sessão na sua subscrição do Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Execute os seguintes comandos:

    ```powershell
    # First, we will create a new route table
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the routetable that was created
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply KMS route table to the subnet that host the problem VMs (in this case, I will apply it to the subnet named Subnet-1)
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Vá para a VM que tem o problema de ativação, utilize [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) para testar se ele pode contactar o servidor KMS:

        psping kms.core.windows.net:1688

4. Tente ativar o Windows e veja se o problema está resolvido.

## <a name="next-steps"></a>Passos Seguintes

- [Chaves de configuração de cliente do KMS](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Revisão e métodos de ativação selecione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)