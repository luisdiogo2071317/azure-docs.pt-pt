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
ms.openlocfilehash: 29b2ac1a5dc128028dbd40e683c1b45e6208d124
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Cenário

Para ilustrar como criar uma VNet e sub-redes, este documento utiliza o seguinte cenário:

![Cenário de VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Neste cenário, criar uma VNet com o nome **TestVNet**, com um bloco CIDR reservado de **192.168.0.0./16**. A VNet contém as seguintes sub-redes: 

* **Front-end**, utilizando **192.168.1.0/24** como bloco CIDR.
* **Back-end**, utilizando **192.168.2.0/24** como bloco CIDR.

