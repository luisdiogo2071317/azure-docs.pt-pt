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
ms.openlocfilehash: 8a5d05342a1db5f27d115364798885a278d09fc0
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197109"
---
1. No portal, do lado esquerdo, clique em **+ Criar um recurso** e escreva "Gateway de Rede Virtual" na pesquisa. Localize **Gateway de rede virtual** no resultado da pesquisa e clique na entrada. Na página do **Gateway de rede virtual**, clique em **Criar** na parte inferior da página para abrir a página **Criar gateway de rede virtual**.
2. Na página **Criar gateway de rede virtual**, preencha os valores para o gateway de rede virtual.

  ![Create virtual network gateway page fields](./media/vpn-gateway-add-gw-p2s-rm-portal-include/p2sgw.png "Create virtual network gateway page fields")
3. Na página **Criar gateway de rede virtual**, preencha os valores do gateway de rede virtual.

  - **Nome**: dê um nome ao gateway. Não é o mesmo que atribuir nome a uma sub-rede de gateway. É o nome do objeto de gateway que está a criar.
  - **Tipo de gateway**: selecione **VPN**. Os gateways de VPN utilizam o tipo de gateway de rede virtual **VPN**. 
  - **Tipo de VPN**: selecione o tipo de VPN especificado para a sua configuração. A maioria das configurações requerem um tipo de VPN baseado em rotas.
  - **SKU**: selecione o SKU de gateway na lista pendente. Os SKUs listados na lista pendente dependem do tipo de VPN que selecionar. Para obter mais informações sobre os SKUs de gateway, veja [SKUs de Gateway](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Localização**: poderá ter de deslocar para ver a Localização. Ajuste o campo **Localização** para apontar para a localização em que a sua rede virtual se encontra. Se a localização não estiver a apontar para a região onde reside a rede virtual, quando selecionar uma rede virtual no próximo passo, esta não será apresentada na lista pendente.
  - **Rede virtual**: escolha a rede virtual à qual pretende adicionar este gateway. Clique em **Rede virtual** para abrir a página "Escolher uma rede virtual". Selecione a VNet. Se não vir a VNet, confirme se o campo Localização aponta para a região na qual a rede virtual está localizada.
  - **Intervalo de endereços de sub-rede de gateway**: apenas será apresentada esta definição se não tiver criado anteriormente uma sub-rede de gateway para a rede virtual. Se tiver criado anteriormente uma sub-rede de gateway válida, esta definição não será apresentada.
  - **Configuração do primeiro IP**: a página "Escolher endereço IP público" cria um objeto de endereço IP público que é associado ao gateway de VPN. O endereço IP público é dinamicamente atribuído a este objeto quando o gateway de VPN é criado. O Gateway de VPN, atualmente, apenas suporta a alocação de endereços IP públicos *dinâmicos*. No entanto, isto não significa que o endereço IP é alterado após ser atribuído ao gateway de VPN. O endereço IP Público só é alterado quando o gateway é eliminado e recriado. Não é alterado ao redimensionar, repor ou ao realizar qualquer outra manutenção/atualização interna do gateway de VPN.

    - Primeiro, clique em **Criar configuração de IP do gateway** para abrir a página "Escolher endereço IP público" e clique em **+Criar novo** para abrir a página "Criar endereço IP público".
    - Em seguida, introduza um **Nome** para o endereço IP público. Mantenha o SKU como **Básico**, a menos que exista um motivo específico para alterá-lo e, em seguida, clique em **OK** na parte inferior desta página para guardar as alterações.

      ![Criar IP público](./media/vpn-gateway-add-gw-s2s-rm-portal-include/gwip.png "Criar PIP")

4. Verifique as definições. Pode selecionar **Afixar ao dashboard** na parte inferior da página se quiser que o gateway apareça no dashboard. 
5. Clique em **Criar** para começar a criar o gateway de VPN. As definições são validadas e verá o mosaico "A implementar gateway de rede Virtual" no dashboard. A criação de um gateway pode demorar até 45 minutos. Poderá ter de atualizar a página do portal para ver o estado concluído.

Uma vez criado o gateway, visualize o endereço IP que lhe foi atribuído vendo a rede virtual no portal. O gateway aparece como um dispositivo ligado. Pode clicar no dispositivo ligado (o seu gateway de rede virtual) para ver mais informações.