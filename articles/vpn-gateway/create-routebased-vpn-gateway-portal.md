---
title: 'Criar um gateway VPN baseado na rota: portal do Azure | Microsoft Docs'
description: Criar rapidamente um Gateway de VPN baseado na rota com o portal do Azure
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: cherylmc
ms.openlocfilehash: 2d6133e974e24c8c4f769995d8245b30a29a3983
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Criar um gateway VPN baseado na rota com o portal do Azure

Este artigo ajuda-o a criar rapidamente um gateway de VPN do Azure baseados na rota com o portal do Azure.  Um gateway VPN é utilizado ao criar uma ligação VPN à sua rede no local. Também pode utilizar um gateway de VPN a ligar VNets. 

Os passos neste artigo irão criar uma VNet, uma sub-rede, uma sub-rede de gateway e um gateway de VPN baseado na rota (gateway de rede virtual). Depois de concluída a criação do gateway, em seguida, pode criar ligações. Estes passos exigem uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="vnet"></a>Criar uma rede virtual

1. Num browser, navegue para o [Portal do Azure](http://portal.azure.com) e inicie sessão com a sua conta do Azure.
2. Clique em **Criar um recurso**. No campo **Procurar no Marketplace**, escreva "rede virtual". Localize a **Rede virtual** na lista devolvida e clique para abrir a página **Rede Virtual**.
3. Perto da parte inferior da página de rede Virtual, do **selecionar um modelo de implementação** lista, verifique se **Resource Manager** está selecionado na lista pendente e, em seguida, clique em **criar**. Esta ação abre o **criar rede virtual** página.
4. Na página **Criar rede virtual**, configure as definições da VNet. Depois de preencher os campos, o sinal de exclamação vermelho muda para uma marca de verificação verde se os caracteres introduzidos nos campos forem válidos. Utilize os seguintes valores:

  - **Name**: TestVNet1
  - **Espaço de endereços**: 10.1.0.0/16
  - **Subscrição**: Certifique-se de que a subscrição listada é aquele que pretende utilizar. Pode utilizar o menu pendente para mudar de subscrição.
  - **Grupo de recursos**: TestRG1
  - **Localização**: EUA leste
  - **Sub-rede**: front-end
  - **Intervalo de endereços**: 10.1.0.0/24

  ![Página Criar rede virtual](./media/create-routebased-vpn-gateway-portal/vnet1.png "Página Criar rede virtual")
5. Depois de introduzir os valores, selecione **afixar ao dashboard** torna mais fácil localizar a VNet no dashboard e, em seguida, clique em **criar**. Depois de clicar em **criar**, verá um mosaico no dashboard que reflete o progresso da VNet. O mosaico muda enquanto a VNet está a ser criada.

## <a name="gwsubnet"></a>Adicionar uma sub-rede de gateway

A sub-rede do gateway contém os endereços IP reservados que utilizam os serviços de gateway de rede virtual. Crie uma sub-rede de gateway.

1. No portal, navegue para a rede virtual para a qual pretende criar um gateway de rede virtual.
2. Na sua página de rede virtual, clique em **sub-redes** para expandir **VNet1 - sub-redes** página.
3. Clique em **+ sub-rede do Gateway** na parte superior para abrir o **adicionar sub-rede** página.

  ![Add the gateway subnet](./media/create-routebased-vpn-gateway-portal/gateway_subnet.png "Add the gateway subnet")
4. O **nome** para a sub-rede é preenchida automaticamente com o valor necessário "GatewaySubnet". Ajustar o preenchimento automático **intervalo de endereços** valores para fazer corresponder os seguintes valores:

  **(Bloco CIDR) do intervalo de endereços**: 10.1.255.0/27

  ![Adicionar a subrede do gateway](./media/create-routebased-vpn-gateway-portal/add_gw_subnet.png "Adicionar a subrede do gateway")
5. Para criar a sub-rede do gateway, clique em **OK** na parte inferior da página.

## <a name="gwvalues"></a>Configurar definições do gateway

1. No lado esquerdo da página de portal, clique em **+ criar um recurso** e escreva "Gateway de rede Virtual" na caixa de pesquisa. Em **Resultados**, localize e clique em **Gateway de rede Virtual**.
2. Na parte inferior da página 'Gateway de rede Virtual', clique em **criar** para abrir o **criar gateway de rede virtual** página.
3. Na página **Criar gateway de rede virtual**, preencha os valores do gateway de rede virtual.

  - **Name**: Vnet1GW
  - **Tipo de gateway**: VPN 
  - **Tipo de VPN**: baseadas na rota
  - **SKU**: VpnGw1
  - **Localização**: EUA leste
  - **Rede virtual**: clique em **Virtual rede/escolha uma rede virtual** para abrir o **escolha uma rede virtual** página. Selecione **VNet1**.

  ![Configurar definições do gateway](./media/create-routebased-vpn-gateway-portal/configure_gw.png "configurar definições do gateway")

## <a name="pip"></a>Criar um endereço IP público

Um gateway de VPN tem de ter um endereço IP público dinamicamente atribuído. Quando cria uma ligação a um gateway VPN, este é o endereço IP que liga o dispositivo no local.

1. Selecione **IP primeiro criar gateway IP configuração** para pedir um endereço IP público.

  ![Configuração de IP primeiro](./media/create-routebased-vpn-gateway-portal/ip.png "configuração de IP primeiro")
2. No **página IP pública Escolha**, clique em **+ criar nova** para abrir o **Criar endereço IP público** página.
3. Configure as definições com os seguintes valores:

  - **Nome**: **VNet1GWPIP**
  - **SKU**: **básico**

  ![Criar IP público](./media/create-routebased-vpn-gateway-portal/gw_ip.png "Criar PIP")
4. Clique em **OK** na parte inferior desta página para guardar as alterações.

## <a name="creategw"></a>Criar o gateway VPN

1. Verifique as definições no **criar gateway de rede virtual** página. Ajuste os valores, se necessário.

  ![Criar gateway de VPN](./media/create-routebased-vpn-gateway-portal/create_gw.png "gateway de VPN criar")
2. Clique em **criar** na parte inferior da página.

Depois de clicar em **criar**, as definições são validadas e **gateway de rede Virtual implementar** apresentada no mosaico no dashboard. Um gateway de VPN pode demorar até 45 minutos. Poderá ter de atualizar a página do portal para ver o estado concluído.

## <a name="viewgw"></a>Ver o gateway VPN

1. Depois de criado o gateway, navegue para a VNet1 no portal. O gateway VPN é apresentado na página Descrição geral do como um dispositivo ligado.

  ![Dispositivos ligados](./media/create-routebased-vpn-gateway-portal/connected_devices.png "dispositivos ligados")

2. Na lista de dispositivos, clique em **VNet1GW** para visualizar mais informações.

  ![Gateway de VPN de vista](./media/create-routebased-vpn-gateway-portal/view_gw2.png "gateway de VPN de vista")

## <a name="next-steps"></a>Passos Seguintes

Depois do gateway foi concluída a criação, pode criar uma ligação entre a rede virtual e outra VNet. Em alternativa, criar uma ligação entre a rede virtual e uma localização no local.

> [!div class="nextstepaction"]
> [Criar uma ligação site a site](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> [criar uma ligação ponto a site](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> [criar uma ligação para outra VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)