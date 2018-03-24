---
title: Exemplos do PowerShell do Azure para a rede virtual | Microsoft Docs
description: Exemplos do PowerShell do Azure para a rede virtual.
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
ms.openlocfilehash: c9cb8da2a6a7512daa6721af90d5b21c6ba5e8e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Exemplos do PowerShell do Azure para a rede virtual

A tabela seguinte inclui ligações para scripts do Powershell do Azure:

| | |
|----|----|
| [Criar uma rede virtual para aplicações de várias camadas](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Cria uma rede virtual com as sub-redes de front-end e back-end. O tráfego para a sub-rede do front-end está limitado a HTTP, enquanto o tráfego para a sub-rede de back-end está limitado a SQL Server, a porta 1433. |
| [Elemento duas redes virtuais](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Cria e ligar duas redes virtuais na mesma região. |
| [Encaminhar o tráfego através de uma aplicação virtual de rede](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Cria uma rede virtual com uma VM que tenha capacidade para encaminhar o tráfego entre as duas sub-redes e as sub-redes de front-end e back-end. |
| [Filtrar o tráfego de rede VM de entrada e saído](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Cria uma rede virtual com as sub-redes de front-end e back-end. Tráfego de rede de entrada para a sub-rede do front-end está limitado a HTTP e HTTPS. Não é permitido o tráfego de saída à internet da sub-rede de back-end. |
