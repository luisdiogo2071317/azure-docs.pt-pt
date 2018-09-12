---
title: 'Criar um gateway VPN baseado na rota: portal do Azure | Documentos da Microsoft'
description: Crie rapidamente um Gateway de VPN baseado na rota com o portal do Azure
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2018
ms.author: cherylmc
ms.openlocfilehash: fe05ab36f971105cf72342b8df5e2a82de7fc2b8
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391798"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Criar um gateway VPN baseado na rota com o portal do Azure

Este artigo ajuda-o a criar rapidamente um gateway de VPN do Azure baseado na rota com o portal do Azure.  Um gateway de VPN é utilizado quando criar uma ligação VPN à sua rede no local. Também pode utilizar um gateway de VPN para ligar VNets. 

Os passos neste artigo irão criar uma VNet, uma sub-rede, uma sub-rede de gateway e um gateway de VPN baseado na rota (gateway de rede virtual). Depois de concluída a criação de gateway, em seguida, pode criar ligações. Estes passos requerem uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="vnet"></a>Criar uma rede virtual

1. Num browser, navegue para o [Portal do Azure](http://portal.azure.com) e inicie sessão com a sua conta do Azure.
2. Clique em **Criar um recurso**. No campo **Procurar no Marketplace**, escreva "rede virtual". Localize a **Rede virtual** na lista devolvida e clique para abrir a página **Rede Virtual**.
3. Perto da parte inferior da página Rede Virtual, na **selecionar um modelo de implementação** lista, certifique-se de que **Gestor de recursos** está selecionado na lista pendente e, em seguida, clique em **criar**. Esta ação abre o **criar rede virtual** página.
4. Na página **Criar rede virtual**, configure as definições da VNet. Depois de preencher os campos, o sinal de exclamação vermelho muda para uma marca de verificação verde se os caracteres introduzidos nos campos forem válidos. Utilize os seguintes valores:

  - **Nome**: TestVNet1
  - **Espaço de endereços**: 10.1.0.0/16
  - **Subscrição**: Certifique-se de que a subscrição listada é aquele que pretende utilizar. Pode utilizar o menu pendente para mudar de subscrição.
  - **Grupo de recursos**: TestRG1
  - **Localização**: E.U.A. leste
  - **Sub-rede**: front-end
  - **Intervalo de endereços**: 10.1.0.0/24

  ![Página Criar rede virtual](./media/create-routebased-vpn-gateway-portal/create-virtual-network.png "Página Criar rede virtual")
5. Depois de introduzir os valores, selecione **afixar ao dashboard** torna mais fácil encontrar a sua VNet no dashboard e, em seguida, clique em **criar**. Depois de clicar em **criar**, verá um mosaico no dashboard que reflete o progresso da VNet. O mosaico muda enquanto a VNet está a ser criada.

## <a name="gwsubnet"></a>Adicionar uma sub-rede de gateway

A sub-rede do gateway contém os endereços IP reservados que utilizam os serviços do gateway de rede virtual. Crie uma sub-rede de gateway.

1. No portal, navegue para a rede virtual para a qual pretende criar um gateway de rede virtual.
2. Na página de rede virtual, clique em **sub-redes** para expandir **VNet1 - sub-redes** página.
3. Clique em **+ sub-rede do Gateway** na parte superior para abrir o **adicionar sub-rede** página.

  ![Add the gateway subnet](./media/create-routebased-vpn-gateway-portal/gateway-subnet.png "Add the gateway subnet")
4. O **nome** da sub-rede é preenchido automaticamente com o valor necessário "GatewaySubnet". Ajustar o preenchido automaticamente **intervalo de endereços** valores de acordo com os seguintes valores:

  **Intervalo de endereços (bloco CIDR)**: 10.1.255.0/27

  ![Adicionar a subrede do gateway](./media/create-routebased-vpn-gateway-portal/add-gateway-subnet.png "Adicionar a subrede do gateway")
5. Para criar a sub-rede do gateway, clique em **OK** na parte inferior da página.

## <a name="gwvalues"></a>Configurar definições do gateway

1. No lado esquerdo da página do portal, clique em **+ criar um recurso** e escreva "Gateway de rede Virtual" na caixa de pesquisa, em seguida, prima **Enter**. Em **Resultados**, localize e clique em **Gateway de rede Virtual**.
2. Na parte inferior da página "Gateway de rede Virtual", clique em **Create** para abrir o **criar o gateway de rede virtual** página.
3. Na página **Criar gateway de rede virtual**, preencha os valores do gateway de rede virtual.

  - **Nome**: Vnet1GW
  - **Tipo de gateway**: VPN 
  - **Tipo de VPN**: baseado na rota
  - **SKU**: VpnGw1
  - **Localização**: E.U.A. leste
  - **Rede virtual**: clique em **Virtual rede/escolher uma rede virtual** para abrir o **escolher uma rede virtual** página. Selecione **VNet1**.

  ![Configurar definições do gateway](./media/create-routebased-vpn-gateway-portal/configure-gateway.png "configurar definições do gateway")

## <a name="pip"></a>Criar um endereço IP público

Um gateway de VPN tem de ter um endereço IP público alocado dinamicamente. Quando cria uma ligação a um gateway VPN, este é o endereço IP que o dispositivo no local se liga a.

1. Selecione **configuração de IP do gateway do primeiro IP configuração criar** para pedir um endereço IP público.

  ![Configuração do primeiro IP](./media/create-routebased-vpn-gateway-portal/add-public-ip-address.png "configuração de IP primeiro")
2. Sobre o **escolha página IP pública**, clique em **+ criar novo** para abrir o **Criar endereço IP público** página.
3. Configure as definições com os seguintes valores:

  - **Nome**: **VNet1GWIP**
  - **SKU**: **básico**

  ![Criar IP público](./media/create-routebased-vpn-gateway-portal/public-ip-address-name.png "Criar PIP")
4. Clique em **OK** na parte inferior desta página para guardar as alterações.

## <a name="creategw"></a>Criar o gateway VPN

1. Verifique as definições no **criar gateway de rede virtual** página. Ajuste valores, se necessário.

  ![Criar gateway de VPN](./media/create-routebased-vpn-gateway-portal/create-vpn-gateway.png "gateway de VPN de criar")
2. Clique em **criar** na parte inferior da página.

Depois de clicar em **Create**, as definições são validadas e o **gateway de rede Virtual Implantando** é apresentado um mosaico no dashboard. Um gateway VPN pode demorar até 45 minutos. Poderá ter de atualizar a página do portal para ver o estado concluído.

## <a name="viewgw"></a>Ver o gateway de VPN

1. Depois do gateway ser criado, navegue para a VNet1 no portal. O gateway de VPN é apresentada na página Descrição geral de como um dispositivo ligado.

  ![Dispositivos ligados](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "dispositivos ligados")

2. Na lista de dispositivos, clique em **VNet1GW** para ver mais informações.

  ![Gateway de VPN de modo de exibição](./media/create-routebased-vpn-gateway-portal/view-gateway.png "gateway de VPN de modo de exibição")

## <a name="next-steps"></a>Passos Seguintes

Depois do gateway tenha terminado de criar, pode criar uma ligação entre a rede virtual e a outra VNet. Em alternativa, criar uma ligação entre a rede virtual e uma localização no local.

> [!div class="nextstepaction"]
> [Criar uma ligação site a site](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Criar uma ligação ponto a site](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Criar uma ligação a outra VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
