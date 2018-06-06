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
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34684987"
---
Abrir uma porta ou criar um ponto final, para uma máquina virtual (VM) no Azure através da criação de um filtro de rede numa sub-rede ou interface de rede VM. Colocar estes filtros que controlam o tráfego de entrada e saído, num grupo de segurança de rede ligado para o recurso que recebe o tráfego.

Vamos utilizar um exemplo comum de tráfego da web na porta 80. Assim que tiver uma VM que está configurada para servir pedidos web em TCP padrão porta 80 (não se esqueça de iniciar os serviços adequados e abra quaisquer regras de firewall de SO da VM, bem como),:

1. Crie um grupo de segurança de rede.
2. Crie uma regra de entrada permitir o tráfego com:
   * o intervalo de portas de destino "80"
   * o intervalo de portas de origem "*" (permitir que qualquer porta de origem)
   * um valor de prioridade de menos 65,500 (para ser superior numa prioridade de que a predefinição catch-all regra de negação de entrada)
3. Associe o grupo de segurança de rede com a interface de rede VM ou sub-rede.

Pode criar configurações de rede complexas para proteger o seu ambiente utilizando grupos de segurança de rede e regras. O nosso exemplo utiliza apenas uma ou duas regras que permitam o tráfego HTTP ou de gestão remota. Para obter mais informações, consulte o seguinte ['Informação mais'](#more-information-on-network-security-groups) secção ou [que é um grupo de segurança de rede?](../articles/virtual-network/security-overview.md)

