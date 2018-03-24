---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c06e69dd9d1997500589659e936dc25ee01ed145
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
Para os SKUs atuais (VpnGw1, VpnGw2 e VPNGW3) que pretende redimensionar o gateway de SKU para atualizar para um mais poderoso, pode utilizar o `Resize-AzureRmVirtualNetworkGateway` cmdlet do PowerShell. Também pode alterar o tamanho do SKU utilizar este cmdlet do gateway. Se estiver a utilizar o SKU de gateway básico [em alternativa, utilize estas instruções](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) para redimensionar o gateway.

O exemplo do PowerShell seguinte mostra um gateway que está a ser redimensionada para VpnGw2 SKU.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Também pode redimensionar um gateway no portal do Azure acedendo ao **configuração** página para o gateway de rede virtual e selecionar um SKU diferentes na lista pendente.