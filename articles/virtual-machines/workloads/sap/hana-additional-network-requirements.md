---
title: Requisitos de rede adicionais do SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Requisitos de rede adicionais para o SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ee89553846309bd96f3cad3648e337488cf07d3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164733"
---
# <a name="additional-network-requirements"></a>Requisitos de rede adicionais

Às vezes, pode ter requisitos de rede adicionais como parte da implementação de instâncias grandes do HANA. Este artigo ilustra os seguintes requisitos de rede:
- [Adicionar mais endereços IP ou sub-rede ](#adding-more-ip-addresses-or-subnets)
- [Adicionar virtual vetworks](#adding-vnets)
- [Aumentar a largura de banda do circuito do expressroute](#increasing-expressroute-circuit-bandwidth)
- [Adição de um circuito do expressroute adicionais](#adding-an-additional-expressroute-circuit)
- [A eliminar a uma sub-rede](#deleting-a-subnet)
- [A eliminar uma rede virtual](#deleting-a-vnet)
- [A eliminar um circuito do router do express](#deleting-an-expressroute-circuit)


## <a name="adding-more-ip-addresses-or-subnets"></a>Adicionando mais sub-redes ou endereços IP

Utilize o portal do Azure, PowerShell, ou CLI quando a adicionar IP de mais endereços ou sub-redes.

Neste caso, a recomendação é adicionar o novo intervalo de endereços IP como intervalo novo para o espaço de endereços da VNet em vez de gerar um novo intervalo de agregados. Em ambos os casos, tem de submeter esta alteração para a Microsoft para permitir a conetividade fora desse novo intervalo de endereços IP para as unidades de instância grande do HANA no cliente. É possível abrir um pedido de suporte do Azure para obter o novo espaço de endereço VNet adicionado. Depois de receber a confirmação, execute os passos seguintes.

Para criar uma sub-rede adicional a partir do portal do Azure, consulte o artigo [criar uma rede virtual com o portal do Azure](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)e para criar a partir do PowerShell, veja [criar uma rede virtual com o PowerShell](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="adding-vnets"></a>Adição de VNets

Depois de ligar inicialmente uma ou mais VNets do Azure, pode querer adicionar outros adicionais que aceder ao SAP HANA no Azure (instâncias grandes). Primeiro, submeta um pedido de suporte do Azure, em que a solicitação inclui duas informações específicas, identificar a implementação do Azure específica e os intervalos de espaço de endereços IP do espaço de endereços da VNet do Azure. Em seguida, o SAP HANA no Azure Service Management fornece as informações necessárias, tem de se ligar as VNets e ExpressRoute adicionais. Para cada VNet, tem uma chave de autorização exclusiva para ligar ao circuito do ExpressRoute para instâncias grandes do HANA.

Passos para adicionar uma nova VNet do Azure:

1. Completa da primeira etapa no processo de integração, consulte a _VNet do Azure de criação de_ secção.
2. Completa da segunda etapa do processo de integração, consulte a _criar a sub-rede do gateway_ secção.
3. Para ligar as VNets adicionais para o circuito do ExpressRoute de instância grande do HANA, abra um pedido de suporte do Azure com informações sobre a nova VNet e uma nova chave de autorização do pedido.
4. Assim que é notificado de que a autorização está concluída, utilize as informações de autorização fornecidos pelo Microsoft para concluir o terceiro passo no processo de integração, consulte a _ligar VNets_ secção.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Aumentar a largura de banda de circuito de ExpressRoute

Consulte o SAP HANA no Azure Service Management. Se é aconselhados a aumentar a largura de banda do SAP HANA no circuito do ExpressRoute do Azure (instâncias grandes), crie um pedido de suporte do Azure. (Pode pedir um aumento de uma largura de banda do circuito único até um máximo de 10 Gbps). Em seguida, receber notificação depois da operação foi concluída; nenhuma ação adicional necessária para ativar esta velocidade superior no Azure.

## <a name="adding-an-additional-expressroute-circuit"></a>Adição de um circuito do ExpressRoute adicional

Consulte o SAP HANA no Azure Service Management, se é aconselhado que é necessário um circuito do ExpressRoute adicional, o que fazer o pedido para criar um novo circuito de ExpressRoute (e para obter informações de autorização para se conectar a ele) de suporte do Azure. O espaço de endereço que vai ser utilizado em VNets em modo de tem de ser definidas antes de efetuar o pedido, por ordem para SAP HANA no Azure Service Management para fornecer autorização.

Quando o novo circuito é criado e o SAP HANA na configuração de gestão de serviço do Azure estiver concluído, vai receber notificação com as informações que precisa continuar. Siga os passos fornecidos acima para criar e ligar-se a nova VNet para este circuito adicional. Não é possível ligar VNets do Azure para este circuito adicional, se ainda estiver ligado ao outro SAP HANA no circuito de ExpressRoute do Azure (instância grande) na mesma região do Azure.

## <a name="deleting-a-subnet"></a>A eliminar a uma sub-rede

Para remover uma sub-rede de VNet, pode ser utilizado o o portal do Azure, PowerShell ou a CLI. No caso de seu Azure VNet IP endereço intervalo/Azure espaço de endereços VNet foi um intervalo de agregados, não existe nenhum siga para com a Microsoft. Exceto que a VNet está ainda propagar o espaço de endereços de rota do BGP, que inclui a sub-rede eliminada. Se tiver definido o Azure VNet IP endereço intervalo/Azure espaço de endereços da VNet como vários intervalos de endereços IP que um foi atribuído à sua sub-rede foi eliminada, deve excluí-lo fora do seu espaço de endereços da VNet e, em seguida, informar o SAP HANA no Azure Service Management para Remova-a de intervalos de SAP HANA no Azure (instâncias grandes) tem permissão para comunicar com.

Para eliminar uma sub-rede, veja [eliminar uma sub-rede](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet) para obter mais informações sobre a criação de sub-redes.

## <a name="deleting-a-vnet"></a>A eliminar uma VNet

Para eliminar uma rede virtual, veja [eliminar uma rede virtual](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network). SAP HANA no Azure Service Management remove as autorizações existentes sobre o SAP HANA no circuito do ExpressRoute do Azure (instâncias grandes) e remover o IP de VNet do Azure endereço intervalo/Azure espaço de endereços do VNet para a comunicação com instâncias grandes do HANA.

Assim que a VNet tiver sido removida, abra um pedido de suporte do Azure para fornecer os intervalos de espaço de endereços IP a ser removido.

Para garantir que tudo o que é removido, elimine os seguintes itens:

- A ligação do ExpressRoute, o Gateway de VNet, IP público do Gateway de VNet e, VNet

## <a name="deleting-an-expressroute-circuit"></a>A eliminar um circuito do ExpressRoute

Para remover um SAP HANA adicionais no circuito do ExpressRoute do Azure (instâncias grandes), abra um pedido de suporte do Azure com o SAP HANA no Azure Service Management e solicitar que o circuito deve ser eliminado. A subscrição do Azure, pode eliminar ou manter a VNet conforme necessário. No entanto, tem de eliminar a ligação entre o circuito do ExpressRoute de instâncias grandes do HANA e o gateway de VNet ligado.

Se também pretender remover uma VNet, siga as orientações sobre como eliminar uma VNet na secção acima.

**Passos seguintes?**

- Consultar [como instalar e configurar o SAP HANA (instâncias grandes) no Azure](hana-installation.md).
