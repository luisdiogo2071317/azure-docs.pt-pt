---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 84bfec375878fe31e085f7de3cd4a41ace645c41
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458649"
---
Pode verificar se a ligação teve êxito com o cmdlet "Get-AzureRmVirtualNetworkGatewayConnection", com ou sem "-Debug". 

1. Utilize o seguinte exemplo de cmdlet, configurando os valores para corresponder aos seus. Se lhe for pedido, selecione "A" para executar "Todos". No exemplo, '--name' refere-se ao nome da ligação que pretende testar.

  ```azurepowershell-interactive
  Get-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
  ```
2. Quando o cmdlet terminar, veja os valores. No exemplo abaixo, o estado da ligação é apresentado como "Ligado" e pode ver bytes de entrada e de saída.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```