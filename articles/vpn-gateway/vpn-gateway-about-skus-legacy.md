---
title: Rede virtual do Azure legado SKUs do VPN gateway | Microsoft Docs
description: Como trabalhar com o gateway de rede virtual antigo SKUs; Básico, Standard e HighPerformance.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2018
ms.author: cherylmc
ms.openlocfilehash: 4feecb9c1e91e1bc6c66a610c092e7bf894886e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30190233"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Trabalhar com o gateway de rede virtual SKUs (SKUs legados)

Este artigo contém informações sobre o gateway de rede virtual (antiga) SKUs de legado. O legado SKUs continuarão a funcionar em ambos os modelos de implementação para gateways VPN que já foram criados. Gateways de VPN clássicos continuam a utilizar os SKUs de legado, para gateways existentes tanto para novos gateways. Ao criar o novo Gestor de recursos VPN gateways, utilize o novo SKUs de gateway. Para obter informações sobre os SKUs de novo, consulte [sobre o Gateway de VPN](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>SKUs de Gateway

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Débito agregado estimado por SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Configurações suportadas pelo tipo SKU e a VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Redimensionar um gateway

Pode redimensionar o gateway para um SKU de gateway dentro da mesma família SKU. Por exemplo, se tiver um SKU Standard, pode redimensionar a um SKU HighPerformance. No entanto, não pode redimensionar o gateway de VPN entre os SKUs antigos e as novas famílias SKU. Por exemplo, não é possível voltar a partir de um SKU Standard para um SKU VpnGw2 ou um SKU básico para VpnGw1.

Redimensionar um gateway para o modelo de implementação clássica, utilize o seguinte comando:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Redimensionar um gateway para o modelo de implementação do Resource Manager com o PowerShell, utilize o seguinte comando:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
Também pode redimensionar um gateway no portal do Azure.

## <a name="change"></a>Mude para o novo SKUs de gateway

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre a nova SKUs de Gateway, consulte [SKUs de Gateway](vpn-gateway-about-vpngateways.md#gwsku).

Para mais informações sobre as definições de configuração, consulte [definições de configuração sobre o Gateway de VPN](vpn-gateway-about-vpn-gateway-settings.md).