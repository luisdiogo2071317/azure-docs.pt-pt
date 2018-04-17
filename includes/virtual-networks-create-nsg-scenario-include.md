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
ms.openlocfilehash: 09c6871fc5243296da2f2defd594afb80c62ac95
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
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

