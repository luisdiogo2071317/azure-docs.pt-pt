---
title: 'Redes de pilha do Azure: Diferenças e as considerações'
description: Saiba mais sobre as diferenças e as considerações ao trabalhar com redes na pilha do Azure.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.author: mabrigg
ms.date: 05/14/2018
ms.topic: article
ms.service: azure-stack
ms.openlocfilehash: 2a4c5bce072970f158a89763ebdf4132eafe9cbe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="considerations-for-azure-stack-networking"></a>Considerações para redes de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Rede de pilha do Azure tem muitas das funcionalidades fornecidas pelo redes do Azure. No entanto, existem algumas diferenças fundamentais que deve saber antes de implementar uma rede de pilha do Azure.

Este artigo fornece uma descrição geral das considerações de exclusivas para redes de pilha do Azure e as respetivas funcionalidades. Para saber mais sobre das principais diferenças entre a pilha do Azure e do Azure, consulte o [chave considerações](azure-stack-considerations.md) tópico.

## <a name="cheat-sheet-networking-differences"></a>Cheat folha: redes de diferenças

|Serviço | Funcionalidade | Azure (global) | Azure Stack |
| --- | --- | --- | --- |
| DNS | DNS de multi-inquilino | Suportadas| Ainda não suportado|
| |Registos DNS AAAA|Suportadas|Não suportado|
| |Zonas DNS por subscrição|100 (predefinição)<br>Pode ser aumentado no pedido.|100|
| |Conjuntos de registos DNS por zona|5000 (predefinição)<br>Pode ser aumentado no pedido.|5000|
||Servidores de nomes para a delegação de zona|O Azure oferece quatro servidores de nomes para cada zona de utilizador (inquilino) que é criada.|Pilha do Azure fornece dois servidores de nomes para cada zona de utilizador (inquilino) que é criada.|
| Rede virtual|Peering de rede virtual|Ligar duas redes virtuais na mesma região através da rede principal do Azure.|Ainda não suportado|
| |Endereços IPv6|Pode atribuir um endereço IPv6 como parte do [configuração de Interface de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions).|É apenas suportado o IPv4.|
|Gateways de VPN|Gateway de VPN de ponto a Site|Suportadas|Ainda não suportado|
| |Gateway de Vnet a Vnet|Suportadas|Ainda não suportado|
| |SKUs do VPN Gateway|Suporte para básico, GW1, GW2, GW3, padrão de elevado desempenho, Ultra-elevado desempenho. |Suporte para básico, Standard e SKUs de elevado desempenho.|
|Load balancer|Endereços IP públicos do IPv6|Suporte para atribuir um endereço IP público IPv6 a um balanceador de carga.|É apenas suportado o IPv4.|
|Observador de Rede|Capacidades de monitorização de rede de inquilino de observador de rede|Suportadas|Ainda não suportado|
|CDN|Perfis de rede de entrega de conteúdo|Suportadas|Ainda não suportado|
|Gateway de aplicação|Balanceamento de carga de 7 camadas|Suportadas|Ainda não suportado|
|Gestor de Tráfego|Encaminhar o tráfego de entrada para o desempenho da aplicação ideal e fiabilidade.|Suportadas|Ainda não suportado|
|ExpressRoute|Configure uma ligação privada rápida ao cloud services da Microsoft da sua infraestrutura ou de colocalização de instalações no local.|Suportadas|Suporte para ligar a pilha do Azure para um circuito Expressroute.|

## <a name="next-steps"></a>Passos Seguintes

[DNS no Azure Stack](azure-stack-dns.md)
