---
title: Modificar os prefixos de endereço IP de gateway de rede local e o endereço IP do Gateway de VPN | Azure | Portal | Documentos da Microsoft
description: Este artigo orienta-o por meio de alterar os prefixos de endereços IP para o seu gateway de rede local com o portal do Azure.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: bdd6f90fe97408bd45a72adf58bfdc190207de46
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38651941"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Modificar as definições de gateway de rede local com o portal do Azure

Por vezes, alterar as definições para o seu gateway de rede local AddressPrefix ou GatewayIPAddress. Este artigo mostra como modificar as definições do gateway de rede local. Também pode modificar essas configurações usando um método diferente ao selecionar uma opção diferente na lista seguinte:

> [!div class="op_single_selector"]
> * [Portal do Azure](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [CLI do Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="ipaddprefix"></a>Modificar prefixos de endereços IP

Ao modificar prefixos de endereços IP, os passos que segue dependem se o gateway de rede local tem uma ligação.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="gwip"></a>Modificar o endereço IP do gateway

Se o dispositivo VPN que pretende ligar foi alterou o respetivo endereço IP público, tem de modificar o gateway de rede local para que essa alteração seja refletida. Quando altera o endereço IP público, os passos que segue dependem se o gateway de rede local tem uma ligação.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Passos Seguintes

Pode verificar a ligação de gateway. Ver [Certifique-se de uma ligação de gateway](vpn-gateway-verify-connection-resource-manager.md).