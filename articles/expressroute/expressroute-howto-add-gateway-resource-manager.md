---
title: 'Adicionar um gateway de rede virtual a uma VNet para o ExpressRoute: PowerShell: Azure | Documentos da Microsoft'
description: Este artigo orienta-o processo de adicionar um gateway de VNet a uma VNet do Resource Manager já criada para o ExpressRoute.
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: charwen
ms.openlocfilehash: 32e49a11b02afedf69e5aa61ca2f626ffe5a125e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239581"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Configurar um gateway de rede virtual para o ExpressRoute com o PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager - portal do Azure](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Clássica – PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Vídeo - portal do Azure](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Este artigo orienta-o pelos passos para adicionar, redimensionar e remover um gateway de rede virtual (VNet) para uma VNet já existente. Os passos para esta configuração são especificamente para VNets que foram criadas utilizando o modelo de implementação do Resource Manager que será utilizado uma configuração de ExpressRoute. Para obter mais informações sobre gateways de rede virtual e as definições de configuração do gateway para o ExpressRoute, veja [sobre os gateways de rede virtual para o ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Antes de começar
Certifique-se de que tem instalado o cmdlets do Azure PowerShell mais recente. Se ainda não instalou os cmdlets mais recentes, terá de fazê-lo antes de iniciar os passos de configuração. Para obter mais informações, veja [nstall and configure Azure PowerShell (Instalar e configurar o Azure PowerShell)](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Passos Seguintes
Depois de ter criado o gateway de VNet, pode ligar a VNet a um circuito do ExpressRoute. Ver [ligar uma rede Virtual a um circuito de ExpressRoute](expressroute-howto-linkvnet-arm.md).

