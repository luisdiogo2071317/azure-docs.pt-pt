---
title: Introdução ao próximo salto na observador de rede do Azure | Microsoft Docs
description: Este artigo fornece uma descrição geral do observador de rede de capacidade de salto seguinte.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bbb782e700781dcfedbbd340c7d10db53767b035
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32180390"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Utilize o salto seguinte para diagnosticar problemas de encaminhamento de máquina virtual

Tráfego de uma máquina virtual (VM) é enviado para um destino com base nas rotas efetivas associadas uma interface de rede (NIC). Salto seguinte obtém o endereço IP de um pacote e o tipo de próximo salto de uma VM específica e a NIC. Saber o salto seguinte ajuda-o a determinar se o tráfego está a ser direcionado para o destino pretendido, ou se o tráfego está a ser enviado nowhere. Uma configuração incorreta de rotas, onde o tráfego é direcionado para uma localização no local ou uma aplicação virtual, pode dar origem a problemas de conectividade. Também o salto seguinte devolve a tabela de rota associada o salto seguinte. Se a rota é definida como uma rota definida pelo utilizador, é devolvida rota. Caso contrário, próximo salto devolve **rota de sistema**.

![Descrição geral de salto seguinte](./media/network-watcher-next-hop-overview/figure1.png)

Os seguintes saltos que poderão ser devolvidos pela capacidade de salto seguinte são os seguintes:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* VnetPeering
* Nenhuma

Para obter mais informações sobre cada tipo de próximo salto, consulte o artigo [descrição geral de encaminhamento](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Passos Seguintes

Para saber como utilizar o salto seguinte para diagnosticar problemas de encaminhamento de rede VM, consulte [VM diagnosticar problemas de encaminhamento de rede](diagnose-vm-network-routing-problem.md).
