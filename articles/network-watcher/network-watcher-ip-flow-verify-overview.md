---
title: Introdução ao fluxo IP verificar na observador de rede do Azure | Microsoft Docs
description: Esta página fornece uma descrição geral do IP de observador de rede fluxo Certifique-se de capacidade
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 8a59047a586f3d7ad7c1f29b218396bd688caafd
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Introdução ao fluxo IP verificar na observador de rede do Azure

Fluxo IP Certifique-se verifica se um pacote é permitido ou negado para ou a partir de uma máquina virtual. As informações constituem em direção, protocolo, local IP, remoto IP, porta local e porta remota. Se o pacote é negado por um grupo de segurança, o nome da regra que negado o pacote é devolvido. Enquanto qualquer IP de origem ou de destino pode ser selecionado, o fluxo IP verificar ajuda a administradores Diagnostique rapidamente problemas de conectividade do ou à internet e de ou para o ambiente no local.

Fluxo IP verificar destina-se uma interface de rede de uma máquina virtual. Fluxo de tráfego, em seguida, é verificado com base nas definições configuradas de ou para essa interface de rede. Verificar o fluxo IP é útil em confirmar se uma regra num grupo de segurança de rede está a bloquear o tráfego de entrada ou saída de ou para uma máquina virtual.

Certifique-se de uma instância do observador de rede tem de ser criada em todas as regiões que pretende executar o fluxo IP. Observador de rede é um serviço regional e só pode ser executada relativamente aos recursos na mesma região. A instância utilizada não afeta os resultados de IP fluxo verificar, tal como a rota associada com a NIC está ainda ser devolvido.

![1][1]

## <a name="next-steps"></a>Passos Seguintes

Visite o seguinte artigo para saber se um pacote é permitido ou negado para uma máquina virtual específica através do portal. [Verifique se o tráfego é permitido numa VM com o fluxo Certifique-se de IP utilizando o portal](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












