---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0dcb591db5f487a103feccf92ffa577a1cbf8b23
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
1. Navegue até à página e abra-a para o seu gateway de rede virtual. Existem múltiplas formas de efetuar este procedimento. Pode navegar até ao gateway "VNet1GW" acedendo a **TestVNet1 -> Descrição geral -> Dispositivos ligados -> VNet1GW**.
2. Na página VNet1GW, clique em **Ligações**. No topo da página Ligações, clique em **+Adicionar** para abrir a página **Adicionar ligação**.

  ![Criar uma ligação Site a Site](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. Na página **Adicionar ligação**, configure os valores para a ligação.

  - **Nome:** Atribua um nome à sua ligação.
  - **Tipo de ligação:** Selecione **Site a site(IPSec)**.
  - **Gateway de rede virtual:** O valor é fixo porque está a ligar a partir deste gateway.
  - **Gateway de rede local:** Clique em **Escolher um gateway de rede local** e selecione o gateway de rede local que pretende utilizar.
  - **Chave Partilhada:** Aqui, o valor tem de corresponder ao valor que está a utilizar para o seu dispositivo VPN local no local. O exemplo utiliza "abc123", mas pode e (deve) utilizar algo mais complexo. O importante é que o valor que especificar aqui seja igual ao valor especificado ao configurar o dispositivo VPN.
  - Os restantes valores para **Subscrição**, **Grupo de Recursos** e **Localização** são fixos.

4. Clique em **OK** para criar a ligação. Verá *A Criar Ligação* a piscar no ecrã.
5. A ligação é apresentada na página **Ligações** do gateway de rede virtual. O estado será alterado de *Desconhecido* para *A ligar*e, em seguida, *Efetuado com êxito*.