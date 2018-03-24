---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5b2aa7fedbc203c50796a0e0c8d9cdb3895ae6c1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Passo 1: Navegue para o gateway de rede virtual

1. No [portal do Azure](https://portal.azure.com), navegue para **todos os recursos**. 
2. Para abrir a página de gateway de rede virtual, navegue para o gateway de rede virtual que pretende eliminar e clique no mesmo.

### <a name="step-2-delete-connections"></a>Passo 2: Ligações de eliminação

1. Na página para o gateway de rede virtual, clique em **ligações** para ver todas as ligações para o gateway.
2. Clique em de **'...'** na linha do nome da ligação, em seguida, selecione **eliminar** na lista pendente.
3. Clique em **Sim** para confirmar que pretende eliminar a ligação. Se tiver várias ligações, elimine a cada ligação.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Passo 3: Elimine o gateway de rede virtual

Lembre-se de que se tiver uma configuração de P2S para esta VNet, além da sua configuração de S2S, eliminação do gateway de rede virtual será automaticamente desligue todos os clientes de P2S sem aviso.

1. Na página de gateway de rede virtual, clique em **descrição geral**.
2. No **descrição geral** página, clique em **eliminar** para eliminar o gateway.
