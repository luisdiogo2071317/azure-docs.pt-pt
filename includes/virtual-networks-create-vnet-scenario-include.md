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
ms.openlocfilehash: cff737b8c79c44494cb0151d6a6281550401b26e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Cenário

Para ilustrar como criar uma VNet e sub-redes, este documento utiliza o seguinte cenário:

![Cenário de VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Neste cenário, criar uma VNet com o nome **TestVNet**, com um bloco CIDR reservado de **192.168.0.0./16**. A VNet contém as seguintes sub-redes: 

* **Front-end**, utilizando **192.168.1.0/24** como bloco CIDR.
* **Back-end**, utilizando **192.168.2.0/24** como bloco CIDR.

