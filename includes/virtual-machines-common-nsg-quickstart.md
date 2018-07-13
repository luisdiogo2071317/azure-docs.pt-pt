---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d9c8a0e6a3bd6d79a11ee0d0dab0500a209e5571
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38941128"
---
Abrir uma porta ou criar um ponto de extremidade, a uma máquina virtual (VM) no Azure através da criação de um filtro de rede numa sub-rede ou a interface de rede VM. Estes filtros que controlam o tráfego de entrada e saído, coloca num grupo de segurança de rede anexado ao recurso que recebe o tráfego.

Vamos usar um exemplo comum de tráfego da web na porta 80. Assim que tiver uma VM que está configurada para atender solicitações da web padrão TCP da porta 80 (Lembre-se para iniciar os serviços apropriados e abrir quaisquer regras de firewall do SO na VM também),:

1. Crie um grupo de segurança de rede.
2. Crie uma regra de entrada a permitir o tráfego com:
   * o intervalo de portas de destino "80"
   * o intervalo de portas de origem de "*" (permitindo que qualquer porta de origem)
   * um valor de prioridade de menos 65,500 (para ser superior em prioridade alta do que o padrão catch-all negar a regra de entrada)
3. Associe o grupo de segurança de rede com a interface de rede VM ou a sub-rede.

É possível criar configurações de rede complexas para proteger o seu ambiente usando grupos de segurança de rede e regras. Nosso exemplo utiliza apenas uma ou duas regras que permitem o tráfego HTTP ou a gestão remota. Para obter mais informações, consulte o seguinte procedimento ["Obter mais informações"](#more-information-on-network-security-groups) secção ou [o que é um grupo de segurança de rede?](../articles/virtual-network/security-overview.md)

