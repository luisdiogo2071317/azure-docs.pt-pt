---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 10f723d5298e745520c4db41b994bd2b97aaa365
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
Embora a utilização das rotas de sistema facilita o tráfego automaticamente para a implementação, há casos em que pretende controlar o encaminhamento de pacotes através de uma aplicação virtual. Pode fazê-lo ao criar rotas definidas pelo utilizador que especificam o salto seguinte para os pacotes que fluem para uma sub-rede específica para aceder à sua aplicação virtual disso e ao ativar o reencaminhamento IP para a VM que funciona como aplicação virtual.

Alguns dos cenários em que aplicações virtuais podem ser utilizadas incluem:

* Tráfego de monitorização com um sistema de deteção de intrusões (IDS)
* Controlar o tráfego com uma firewall

Para obter mais informações sobre o reencaminhamento IP e UDR, visite [rotas definidas pelo utilizador e reencaminhamento IP](../articles/virtual-network/virtual-networks-udr-overview.md).

