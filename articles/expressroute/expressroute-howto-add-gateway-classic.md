---
title: 'Configurar um gateway de VNet para ExpressRoute com o PowerShell: clássico: Azure | Documentos da Microsoft'
description: Configurar um gateway de VNet para uma implementação clássica VNet com o PowerShell para uma configuração de ExpressRoute do modelo.
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: ''
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: ec7e4a326bc0d5e9393db28c83c9d859a93dc8a3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055895"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Configurar um gateway de rede virtual para o ExpressRoute com o PowerShell (clássico)
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Clássica – PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Vídeo - Portal do Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Este artigo irá guiá-lo pelos passos para adicionar, redimensionar e remover um gateway de rede virtual (VNet) para uma VNet já existente. Os passos para esta configuração são especificamente para VNets que foram criadas utilizando o **modelo de implementação clássica** e que será utilizada uma configuração de ExpressRoute. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Acerca dos modelos de implementação do Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Antes de começar
Certifique-se de que instalou os cmdlets do PowerShell do Azure necessários para esta configuração (1.0.2 ou posterior). Se ainda não instalou os cmdlets, terá de fazê-lo antes de iniciar os passos de configuração. Para obter mais informações sobre como instalar o Azure PowerShell, consulte [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Passos Seguintes
Depois de ter criado o gateway de VNet, pode ligar a VNet a um circuito do ExpressRoute. Ver [ligar uma rede Virtual a um circuito de ExpressRoute](expressroute-howto-linkvnet-classic.md).

