---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 29ec30840e550b87f83de67d52348b9723400e0f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111879"
---
1. Na [portal do Azure](http://portal.azure.com), selecione a rede virtual do Resource Manager para o qual pretende criar um gateway de rede virtual.

2. Na **definições** secção da página da rede virtual, selecione **sub-redes** para expandir o **sub-redes** página.

3. Sobre o **sub-redes** página, selecione **sub-rede do Gateway** para abrir o **adicionar sub-rede** página.

  ![Add the gateway subnet](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Add the gateway subnet")

4. O **Name** para a sub-rede é automaticamente autofilled com o valor *GatewaySubnet*. Este valor é necessário para o Azure reconheça a sub-rede como a sub-rede do gateway. Ajustar o autofilled **intervalo de endereços** valores para corresponder aos requisitos de configuração, em seguida, selecione **OK** para criar a sub-rede.

  ![Adding the subnet](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Adding the subnet")