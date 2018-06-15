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
ms.openlocfilehash: 2c1a4a1931bc2e38b0bee5f90518b01fdf4767a1
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
ms.locfileid: "30196829"
---
Se estiver a trabalhar com o modelo de implementação Resource Manager, pode alterar para o novo SKUs de gateway. Quando alterar a partir de um SKU de gateway legado para um SKU de novo, elimine o gateway VPN existente e criar um novo gateway VPN.

Fluxo de trabalho:

1. Remova várias ligações a um gateway de rede virtual.
2. Elimine o gateway de VPN antigo.
3. Crie o gateway de VPN novo.
4. Atualize os seus dispositivos VPN no local com o novo endereço IP do gateway do VPN (para ligações de rede).
5. Atualize o valor do endereço IP do gateway para gateways de rede local VNet para VNet que se ligam a este gateway.
6. Transfira novos pacotes de configuração do cliente VPN para clientes de P2S que se ligam à rede virtual por meio deste gateway VPN.
7. Volte a criar as ligações a um gateway de rede virtual.

Considerações:

* Para mover para os novo SKUs, o gateway de VPN tem de estar no modelo de implementação Resource Manager.
* Se tiver um gateway de VPN clássico, deve continuar a utilizar os SKUs de legado mais antigos para esse gateway, no entanto, pode redimensionar entre os SKUs de legado. Não é possível alterar para os SKUs de novo.
* Terá período de indisponibilidade da conectividade quando forem efetuadas alterações de um SKU legado para um SKU de novo.
* Quando alterar para um novo SKU de gateway, irá alterar o endereço IP público do gateway de VPN. Isto acontece mesmo se especificar o mesmo objeto de endereço IP público que utilizou anteriormente.