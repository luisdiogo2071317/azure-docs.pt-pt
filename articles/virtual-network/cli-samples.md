---
title: Exemplos da CLI do Azure para a rede virtual | Microsoft Docs
description: Exemplos da CLI do Azure para a rede virtual.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 9459bad1060daa0c9f6c34272cecfbc67463be66
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-cli-samples-for-virtual-network"></a>Exemplos da CLI do Azure para a rede virtual

A tabela seguinte inclui ligações para bash scripts com os comandos da CLI do Azure:

| | |
|----|----|
| [Criar uma rede virtual para aplicações de várias camadas](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Cria uma rede virtual com as sub-redes de front-end e back-end. O tráfego para a sub-rede do front-end é limitado para HTTP e SSH, enquanto o tráfego para a sub-rede de back-end está limitado a MySQL, porta 3306. |
| [Elemento duas redes virtuais](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Cria e ligar duas redes virtuais na mesma região. |
| [Encaminhar o tráfego através de uma aplicação virtual de rede](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Cria uma rede virtual com uma VM que tenha capacidade para encaminhar o tráfego entre as duas sub-redes e as sub-redes de front-end e back-end. |
| [Filtrar o tráfego de rede VM de entrada e saído](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Cria uma rede virtual com as sub-redes de front-end e back-end. Tráfego de rede de entrada para a sub-rede do front-end está limitado a HTTP, HTTPS e SSH. Não é permitido o tráfego de saída à internet da sub-rede de back-end. |