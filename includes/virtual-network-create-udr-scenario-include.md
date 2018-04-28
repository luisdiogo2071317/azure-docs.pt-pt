---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: b91ae155761f6357e286f4742d57b97cf96d909a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Cenário
Para ilustrar melhor como criar UDRs, este documento utiliza o seguinte cenário:

![DESCRIÇÃO DA IMAGEM](./media/virtual-network-create-udr-scenario-include/figure1.png)

Neste cenário, vai criar um UDR para o *sub-rede front-end* e UDR outro para o *sub-rede de Back-end*, da seguinte forma: 

* **UDR-front-end**. O front-end UDR é aplicada para a *front-end* sub-rede e contêm uma rota:    
  * **RouteToBackend**. Esta rota envia todo o tráfego para a sub-rede de back-end para o **FW1** máquina virtual.
* **UDR-back-end**. É aplicada a UDR de back-end para o *back-end* sub-rede e contêm uma rota:    
  * **RouteToFrontend**. Esta rota envia todo o tráfego para a sub-rede do front-end para o **FW1** máquina virtual.

A combinação destas rotas garante que todo o tráfego destinado a partir de uma sub-rede para outra é encaminhado para o **FW1** máquina virtual, que está a ser utilizada como uma aplicação virtual. Terá também de ativar o IP de reencaminhamento para o **FW1** VM, certifique-se de que pode receber o tráfego destinado ao outras VMs.

