---
title: 'Repor um circuito ExpressRoute do Azure com falhas: PowerShell | Microsoft Docs'
description: Este artigo ajuda-o a reposição de um circuito de ExpressRoute que se encontra num Estado de falha.
documentationcenter: na
services: expressroute
author: anzaman
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 423bc1d6409e5b7fe02339a05d0775f4ff42de49
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31590688"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Repor um circuito ExpressRoute com falhas

Quando uma operação num circuito ExpressRoute não for concluída com êxito, o circuito poderá entrar em estado de 'Falha'. Este artigo ajuda-o a reposição de um circuito ExpressRoute do Azure falhou.

## <a name="reset-a-circuit"></a>Repor um circuito

1. Instale a versão mais recente dos cmdlets do PowerShell do Azure Resource Manager. Para obter mais informações, veja [nstall and configure Azure PowerShell (Instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps).

2. Abra a consola do PowerShell com privilégios elevados e ligue-se à sua conta. Utilize o exemplo seguinte para o ajudar na ligação:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Se tiver múltiplas subscrições do Azure, verifique as subscrições da conta.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Especifique a subscrição que pretende utilizar.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Execute os seguintes comandos para repor um circuito que se encontra num Estado com falhas:

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

O circuito deve ser bom estado de funcionamento. Abra um pedido de suporte com [suporte da Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) se o circuito ainda está em estado de falha.

## <a name="next-steps"></a>Passos Seguintes

Abra um pedido de suporte com [suporte da Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) se ainda ocorrerem problemas.
