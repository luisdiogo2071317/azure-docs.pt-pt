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
ms.openlocfilehash: e81e083b012c4cc29f2d6c09f8254025d712a97c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
### <a name="noconnection"></a>Para modificar prefixos de endereço de IP do gateway de rede local - sem ligação de gateway

Para adicionar prefixos de endereços adicionais:

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

Para remover um prefixos de endereço:<br>
Não inclua os prefixos que já não são necessários. Neste exemplo, já não precisamos do prefixo 20.0.0.0/24 (do exemplo anterior), pelo que atualizamos o gateway de rede local, excluindo esse prefixo.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <a name="withconnection"></a>Para modificar prefixos de endereço de IP do gateway de rede local - ligação de gateway existente

Se tiver uma ligação de gateway e pretender adicionar ou remover os prefixos de endereço IP contidos no gateway de rede local, tem de realizar os seguintes passos por ordem. Este procedimento resulta num período de indisponibilidade da ligação VPN. Ao modificar prefixos de endereços IP, não precisa de eliminar o gateway de VPN. Só tem de remover a ligação.


1. Remova a ligação.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Modifique os prefixos de endereços IP do gateway da sua rede local.
   
  Defina a variável para o LocalNetworkGateway.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Modifique os prefixos.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Crie a ligação. Neste exemplo, configuramos uma ligação do tipo IPsec. Ao recriar a ligação, utilize o tipo de ligação especificado para a sua configuração. Para tipos de ligação adicionais, veja a página [cmdlet do PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).
   
  Defina a variável para o VirtualNetworkGateway.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Crie a ligação. Este exemplo utiliza a variável $local definida no passo 2.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```