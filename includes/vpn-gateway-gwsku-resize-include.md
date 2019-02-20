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
ms.openlocfilehash: 081352a23e6a0d8f9e2daa77eca1f8ac85172ff6
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418233"
---
Para as SKUs atuais (VpnGw1, VpnGw2 e VPNGW3) que deseja redimensionar o gateway de SKU para atualizar para um mais poderoso, pode usar o `Resize-AzVirtualNetworkGateway` cmdlet do PowerShell. Também podem passar o utilizar este cmdlet de tamanho SKU de gateway. Se estiver a utilizar o SKU, de gateway básico [em alternativa, utilize estas instruções](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) para redimensionar o gateway.

O exemplo de PowerShell seguinte mostra um redimensionada para VpnGw2 SKU de gateway.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Também pode redimensionar um gateway no portal do Azure ao aceder a **configuração** página para o seu gateway de rede virtual e selecionar um SKU de diferente na lista pendente.