---
title: O Azure Stack diferenças e considerações de rede | Documentos da Microsoft
description: Saiba mais sobre as diferenças e considerações ao trabalhar com o funcionamento em rede no Azure Stack.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.author: brenduns
ms.date: 08/02/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: scottnap
ms.openlocfilehash: 50fe3c0c7fda745047c71afb8eedf7fa8806c4ec
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055568"
---
# <a name="considerations-for-azure-stack-networking"></a>Considerações sobre o funcionamento em rede do Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Redes de pilha do Azure tem muitas das funcionalidades fornecidas pelo sistema de rede do Azure. No entanto, existem algumas diferenças importantes que deve saber antes de implementar uma rede do Azure Stack.

Este artigo fornece uma descrição geral das considerações de exclusivas para o sistema de rede do Azure Stack e as respetivas funcionalidades. Para saber mais sobre das principais diferenças entre o Azure Stack e o Azure, consulte a [considerações da chave](azure-stack-considerations.md) artigo.

## <a name="cheat-sheet-networking-differences"></a>Referência rápida: diferenças de sistema de rede

| Serviço | Funcionalidade | (Global) do Azure | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | DNS do multi-inquilino | Suportadas | Ainda não é suportado |
|  | Registros AAAA de DNS | Suportadas | Não suportado |
|  | Zonas DNS por subscrição | 100 (predefinição)<br>Pode ser aumentado no pedido. | 100 |
|  | Conjuntos de registos DNS por zona | 5000 (predefinição)<br>Pode ser aumentado no pedido. | 5000 |
|  | Servidores de nomes para a delegação de zona | O Azure fornece quatro servidores de nomes para cada zona de utilizador (inquilino) que é criada. | O Azure Stack fornece dois servidores de nomes para cada zona de utilizador (inquilino) que é criada. |
| Rede Virtual | Peering de rede virtual | Ligar duas redes virtuais na mesma região através da rede de backbone do Azure. | Ainda não é suportado |
|  | Endereços IPv6 | Pode atribuir um endereço IPv6 como parte da [configuração de Interface de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions). | É apenas suportado o IPv4. |
|  | Plano de proteção DDoS | Suportadas | Ainda não é suportado. |
|  | Configurações de IP do conjunto de dimensionamento | Suportadas | Ainda não é suportado. |
|  | Serviços de acesso privado (sub-rede) | Suportadas | Ainda não é suportado. |
|  | Pontos Finais de Serviço | Suporte para interno (não-ligação à Internet) para serviços do Azure. | Ainda não é suportado. |
| É apenas suportado o IPv4. | Políticas de ponto final de serviço | Suportadas | Ainda não é suportado. |
|  | Túneis de serviço | Suportadas | Ainda não é suportado.  |
| Grupos de Segurança de Rede | Regras de segurança aumentadas | Suportadas | Ainda não é suportado. |
|  | Regras de segurança efetivas | Suportadas | Ainda não é suportado. |
|  | Grupos de Segurança de Aplicações | Suportadas | Ainda não é suportado. |
| Gateways de Rede Virtual | Gateway de VPN ponto a Site | Suportadas | Ainda não é suportado. |
|  | Gateway de Vnet a Vnet | Suportadas | Ainda não é suportado. |
|  | Tipo de Gateway de rede virtual | O Azure suporta VPN<br> ExpressRoute <br> Hyper Net | Neste momento, o Azure Stack suporta apenas os tipo de VPN. |
|  | SKUs de Gateway VPN | Suporte para Basic, GW1, GW2, GW3, padrão de alto desempenho, desempenho de assegurar a elevada. | Suporte para básico, Standard e SKUs de alto desempenho. |
|  | Tipo de VPN | O Azure suporta tanto baseada em políticas e a rota com base. | O Azure Stack oferece suporte a rota com base apenas. |
|  | Definições de BGP | O Azure suporta a configuração de endereço de Peering de BGP e o peso do Peer. | Endereço de Peering de BGP e o peso do Peer são automaticamente configurados no Azure Stack. Não é possível para o utilizador configurar estas definições com seus próprios valores. |
|  | Site de Gateway predefinido | O Azure suporta a configuração de um site padrão para o túnel forçado. | Ainda não é suportado. |
|  | Redimensionamento de gateway | O Azure suporta a redimensionar o gateway após a implementação. | Novamente o dimensionamento não suportados. |
|  | Configuração de ativo/ativo | Suportadas | Ainda não é suportado. |
|  | Políticas de IKE/IPSec | O Azure suporta IPSec política configurações personalizadas. | Ainda não é suportado. |
|  | UsePolicyBasedTrafficSelectors | O Azure suporta a utilização de seletores de tráfego baseado em políticas com ligações de gateway baseado na rota. | Ainda não é suportado. |
| Load balancer | SKU | Básica e Balanceadores de carga Standard são suportados | Apenas o Balanceador de carga básico é suportado.  A propriedade SKU não é suportada. |
|  | Zonas | As zonas de disponibilidade são suportadas. | Ainda não é suportado |
|  | Regras NAT de entrada de suporte para pontos finais de serviço | O Azure suporta a especificação pontos finais de serviço para as regras NAT de entrada. | O Azure Stack ainda não suporta pontos finais de serviço, para que estes não podem ser especificados. |
|  | Protocolo | O Azure suporta a especificação de GRE ou ESP. | Classe de protocolo não é suportada no Azure Stack. |
| Endereço IP Público | Versão do endereço IP público | O Azure suporta IPv6 e IPv4 | É apenas suportado o IPv4. |
| Interface de Rede | Obter a tabela de rotas em vigor | Suportadas | Ainda não é suportado. |
|  | Obter as ACLs em vigor | Suportadas | Ainda não é suportado. |
|  | Ativar o funcionamento em rede acelerado | Suportadas | Ainda não é suportado. |
|  | Reencaminhamento IP | Desativado por predefinição.  Pode ser ativado. | Ativar esta definição não é suportada.  Por predefinição. |
|  | Várias configurações de IP por interface | Suportadas | Ainda não é suportado. |
|  | Grupos de Segurança de Aplicações | Suportadas | Ainda não é suportado. |
|  | Etiqueta de nome DNS interno | Suportadas | Ainda não é suportado. |
|  | Versão do endereço IP privado | São suportados IPv4 e IPv6. | É apenas suportado o IPv4. |
|  | Configuração de IP primária | Suportado. Identifica a configuração de IP primária na interface. | Ainda não é suportado. |
| Observador de Rede | Capacidades de monitorização de rede de inquilino de observador de rede | Suportadas | Ainda não é suportado. |
| CDN | Perfis de rede de entrega de conteúdos | Suportadas | Ainda não é suportado. |
| Gateway de aplicação | Balanceamento de carga de camada 7 | Suportadas | Ainda não é suportado. |
| Gestor de Tráfego | Encaminhe o tráfego de entrada de aplicações ideal de desempenho e confiabilidade. | Suportadas | Ainda não é suportado. |
| ExpressRoute | Configure uma ligação privada e rápida para serviços cloud da Microsoft de sua infraestrutura ou partilha de localização de instalação no local. | Suportadas | Suporte para o Azure Stack a ligar a um circuito do Expressroute. |

## <a name="next-steps"></a>Passos Seguintes

[DNS no Azure Stack](azure-stack-dns.md)