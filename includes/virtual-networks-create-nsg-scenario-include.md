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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38760365"
---
## <a name="scenario"></a>Cenário
Para ilustrar melhor como criar NSGs, este documento usa o seguinte cenário:

![Cenário de VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Neste cenário, vai criar um NSG para cada sub-rede na **TestVNet** rede virtual, da seguinte forma: 

* **NSG-front-end**. O NSG de front-end é aplicado para o *front-end* sub-rede e contém duas regras:    
  * **regra de RDP**. Permita o tráfego RDP para o *front-end* sub-rede.
  * **regra de Web**. Permita o tráfego HTTP para o *front-end* sub-rede.
* **NSG-back-end**. O NSG de back-end é aplicado para o *back-end* sub-rede e contém duas regras:    
  * **regra de SQL**. Permite o tráfego SQL apenas a partir da *front-end* sub-rede.
  * **regra de Web**. Nega o tráfego de vinculado à internet de todos os *back-end* sub-rede.

A combinação dessas regras criar um cenário do tipo de rede de Perímetro, em que a sub-rede de back-end pode apenas de receber tráfego de entrada para o SQL da sub-rede do front-end e não tem acesso à Internet, enquanto a sub-rede do front-end pode comunicar com a Internet e receber pedidos HTTP recebidos apenas.

