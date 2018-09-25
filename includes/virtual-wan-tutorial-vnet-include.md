---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1fae3c3889242dfbf8f270d3762ea7434ceda6da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004195"
---
Se ainda não tiver uma VNet, pode criar uma rapidamente com o PowerShell. Também pode criar uma rede virtual com o portal do Azure.

* Verifique que o espaço de endereços da VNet que criar não se sobrepõe a nenhum dos intervalos de endereços de outras VNets às quais pretenda ligar nem a nenhum dos espaços de endereços da sua rede no local. 
* Se já tiver uma VNet, confirme que a mesma cumpre os critérios obrigatórios e que não tem um gateway de rede virtual.

Pode criar facilmente a VNet ao clicar em “Experimentar”, no artigo, para abrir uma consola do PowerShell. Ajuste os valores e copie e cole os comandos na janela da consola.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Ajuste os comandos do PowerShell e crie um grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Criar uma VNet

Ajuste os comandos do PowerShell para criar a VNet que é compatível com o seu ambiente.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```