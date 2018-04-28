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
ms.openlocfilehash: 873549442284ede2e9f020bd90879f721b9c1a18
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Cenário
Para ilustrar melhor como criar NSGs, este documento utiliza o seguinte cenário:

![Cenário de VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Neste cenário, vai criar um NSG para cada sub-rede no **TestVNet** rede virtual, da seguinte forma: 

* **NSG-front-end**. O front-end NSG é aplicado ao *front-end* sub-rede e contém duas regras:    
  * **regra de RDP**. Permite que o tráfego RDP de *front-end* sub-rede.
  * **regra de Web**. Permite que o tráfego HTTP para o *front-end* sub-rede.
* **NSG-back-end**. O NSG de back-end é aplicado a *back-end* sub-rede e contém duas regras:    
  * **regra de SQL**. Permite que o tráfego do SQL Server apenas a partir de *front-end* sub-rede.
  * **regra de Web**. Nega todos os internet vinculado tráfego a partir do *back-end* sub-rede.

A combinação destas regras criar um cenário semelhante a rede de Perímetro, onde a sub-rede de back-end pode receber apenas tráfego de entrada para o SQL partir da sub-rede do front-end e não tem acesso à Internet, enquanto a sub-rede do front-end pode comunicar com a Internet e receber pedidos HTTP recebidos apenas.

