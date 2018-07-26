---
title: 'Ligar uma rede virtual a um circuito do ExpressRoute: PowerShell: clássico: Azure | Documentos da Microsoft'
description: Este documento fornece uma descrição geral de como ligar redes virtuais (VNets) para circuitos do ExpressRoute com o modelo de implementação clássica e o PowerShell.
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: ''
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2018
ms.author: ganesr
ms.openlocfilehash: 7e1faa9dc5901861aab8e7911c241e6704b805b1
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257848"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>Ligar uma rede virtual a um circuito do ExpressRoute com o PowerShell (clássico)
> [!div class="op_single_selector"]
> * [Portal do Azure](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [CLI do Azure](howto-linkvnet-cli.md)
> * [Vídeo - portal do Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (clássico)](expressroute-howto-linkvnet-classic.md)
>

Este artigo irá ajudá-lo a ligar redes virtuais (VNets) para circuitos do ExpressRoute do Azure com o modelo de implementação clássica e o PowerShell. Redes virtuais podem ser na mesma subscrição ou podem fazer parte de outra subscrição.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Acerca dos modelos de implementação do Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Pré-requisitos da configuração
1. Tem a versão mais recente dos módulos do Azure PowerShell. Pode baixar os módulos do PowerShell mais recente da seção do PowerShell do [página de transferências do Azure](https://azure.microsoft.com/downloads/). Siga as instruções em [como instalar e configurar o Azure PowerShell](/powershell/azure/overview) para obter orientações passo a passo sobre como configurar o seu computador para utilizar os módulos do Azure PowerShell.
2. Tem de rever o [pré-requisitos](expressroute-prerequisites.md), [requisitos de encaminhamento](expressroute-routing.md), e [fluxos de trabalho](expressroute-workflows.md) antes de iniciar a configuração.
3. Deve ter um circuito ExpressRoute ativo.
   * Siga as instruções para [criar um circuito do ExpressRoute](expressroute-howto-circuit-classic.md) e ter o seu fornecedor de conectividade, ativar o circuito.
   * Certifique-se de que tem o peering privado do Azure configurado para o seu circuito. Consulte a [configurar encaminhamento](expressroute-howto-routing-classic.md) artigo para obter instruções de encaminhamento.
   * Certifique-se de que o peering privado do Azure está configurado e o peering de BGP entre a rede e a Microsoft está ativo, para que pode habilitar a conectividade de ponto-a-ponto.
   * Tem de ter uma rede virtual e um gateway de rede virtual criada e totalmente aprovisionado. Siga as instruções para [configurar uma rede virtual para o ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Pode ligar até 10 redes virtuais a um circuito do ExpressRoute. Tem de ser todas as redes virtuais na mesma região geopolítica. Pode associar um grande número de redes virtuais para o seu circuito do ExpressRoute ou redes virtuais de ligação que estão em outras regiões geopolíticas, se tiver ativado o suplemento ExpressRoute premium. Verifique os [FAQ](expressroute-faqs.md) para obter mais detalhes sobre o suplemento premium.

Pode ser associada uma VNet única para até quatro circuitos do ExpressRoute. Utilize o processo abaixo para criar uma nova ligação para cada circuito do ExpressRoute que está a ligar. Os circuitos do ExpressRoute podem ser na mesma subscrição, subscrições diferentes ou uma combinação de ambos.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Ligar uma rede virtual na mesma subscrição a um circuito
Pode ligar uma rede virtual a um circuito do ExpressRoute com o cmdlet seguinte. Certifique-se de que o gateway de rede virtual é criado e está pronto para a ligação antes de executar o cmdlet.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned
    
## <a name="remove-a-virtual-network-link-to-a-circuit"></a>Remover uma ligação de rede virtual para um circuito
Pode remover uma ligação de rede virtual para um circuito do ExpressRoute com o cmdlet seguinte. Certifique-se de que a subscrição atual está selecionada para a rede virtual fornecida. 

    Remove-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
 

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Ligar uma rede virtual de uma subscrição diferente a um circuito
Pode partilhar um circuito do ExpressRoute em várias subscrições. A figura seguinte mostra um simples esquemática funciona como a partilha dos circuitos do ExpressRoute em várias subscrições.

Cada uma das nuvens menores na cloud da grande é utilizada para representar as subscrições que pertencem a diferentes departamentos dentro de uma organização. Cada um dos departamentos dentro da organização pode utilizar a sua própria subscrição para implementar os serviços, mas os departamentos pode partilhar um único circuito de ExpressRoute para ligar a voltar à sua rede no local. Um único departamento (neste exemplo: IT) pode ser proprietário do circuito do ExpressRoute. Outras subscrições dentro da organização podem utilizar o circuito do ExpressRoute.

> [!NOTE]
> Custos de largura de banda e de conectividade para o circuito dedicado serão aplicados para o proprietário do circuito ExpressRoute. Todas as redes virtuais partilham a mesma largura de banda.
> 
> 

![Conectividade entre subscrições](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Administração
O *proprietário do circuito* é o administrador/coadministrador da subscrição na qual é criado o circuito do ExpressRoute. O proprietário do circuito pode autorizar os administradores/coadministradores de outras subscrições, denominadas *circuito utilizadores*, para utilizar o circuito dedicado que possuem. Os utilizadores do circuito que estão autorizados a utilizar o circuito do ExpressRoute da organização podem ligar a rede virtual na sua subscrição para o circuito do ExpressRoute depois estiverem autorizadas.

O proprietário do circuito tem o poder para modificar e revogar autorizações em qualquer altura. Revogar uma autorização resultará em todas as ligações a ser eliminadas da subscrição cujo acesso foi revogado.

### <a name="circuit-owner-operations"></a>Operações de proprietário do circuito

**Criar uma autorização**

O proprietário do circuito autoriza os administradores de outras subscrições para utilizar o circuito especificado. No exemplo a seguir, o administrador do circuito (TI da Contoso) permite que o administrador da subscrição (Dev-Test) para ligar até duas redes virtuais ao circuito do outro. O administrador de TI de Contoso permite isso ao especificar o ID de desenvolvimento e teste Microsoft. O cmdlet não envie um email para o ID especificado de Microsoft. O proprietário do circuito precisa explicitamente notificar o proprietário de subscrição que a autorização está concluída.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Rever as autorizações**

O proprietário do circuito pode rever todas as autorizações emitidos num determinado circuito, execute o seguinte cmdlet:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**A atualizar as autorizações**

O proprietário do circuito pode modificar as autorizações utilizando o cmdlet seguinte:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**A eliminar autorizações**

O proprietário do circuito pode revogar/eliminar autorizações para o usuário executando o seguinte cmdlet:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Operações de utilizador do circuito

**Rever as autorizações**

O utilizador de circuito pode rever as autorizações utilizando o cmdlet seguinte:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Resgatar autorizações de ligações**

O utilizador de circuito pode executar o cmdlet seguinte para resgatar uma autorização de ligação:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Execute este comando na subscrição recentemente ligada para a rede virtual:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações acerca do ExpressRoute, veja as [FAQs do ExpressRoute](expressroute-faqs.md).

