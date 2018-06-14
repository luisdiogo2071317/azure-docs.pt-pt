---
title: 'Ligar uma rede virtual a um circuito de ExpressRoute: portal do Azure | Microsoft Docs'
description: Ligar uma VNet a um circuito ExpressRoute do Azure. Passos de procedimentos.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: cherylmc
ms.openlocfilehash: c2bef1d79d3133ea6306928a8c917e1bc3000a58
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
ms.locfileid: "29874557"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Ligar uma rede virtual para um circuito ExpressRoute com o portal
> [!div class="op_single_selector"]
> * [Portal do Azure](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [CLI do Azure](howto-linkvnet-cli.md)
> * [Vídeo - portal do Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (clássico)](expressroute-howto-linkvnet-classic.md)
> 

Este artigo ajuda-o a criar uma ligação para ligar uma rede virtual a um circuito de ExpressRoute do Azure no portal do Azure. As redes virtuais que se ligar ao seu circuito Azure ExpressRoute podem ser na mesma subscrição ou podem ser parte de outra subscrição.

## <a name="before-you-begin"></a>Antes de começar

* Reveja o [pré-requisitos](expressroute-prerequisites.md), [requisitos de encaminhamento](expressroute-routing.md), e [fluxos de trabalho](expressroute-workflows.md) antes de iniciar a configuração.

* Deve ter um circuito ExpressRoute ativo.
  * Siga as instruções para [criar um circuito ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) e ter o circuito ativado pelo seu fornecedor de conectividade.
  * Certifique-se de que tem o peering privado do Azure configurada para o seu circuito. Consulte o [configurar encaminhamento](expressroute-howto-routing-portal-resource-manager.md) artigo para obter instruções de encaminhamento.
  * Certifique-se de que o peering privado do Azure está configurado e o peering de BGP entre a rede e a Microsoft está a funcionar, de modo a que pode ativar a conetividade de ponto a ponto.
  * Certifique-se de que tem uma rede virtual e um gateway de rede virtual criada e totalmente aprovisionado. Siga as instruções para [criar um gateway de rede virtual para o ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Um gateway de rede virtual para o ExpressRoute utiliza o gatewaytype como 'ExpressRoute', não VPN.

* Pode ligar até 10 redes virtuais para um circuito do ExpressRoute standard. Todas as redes virtuais tem de ser na mesma região geopolítica quando utilizar um circuito do ExpressRoute standard.

* Pode ser associada uma única VNet para até quatro circuitos do ExpressRoute. Utilize o procedimento abaixo indicado para criar um novo objeto de ligação para cada circuito de ExpressRoute que está a ligar. Os circuitos do ExpressRoute podem ser na mesma subscrição, subscrições diferentes ou uma combinação de ambos.

* Pode ligar uma rede virtual fora da região geopolítica do circuito ExpressRoute ou ligar um grande número de redes virtuais para o circuito do ExpressRoute, se tiver ativado o suplemento ExpressRoute premium. Verifique o [FAQ](expressroute-faqs.md) para obter mais detalhes sobre o suplemento premium.

* Pode [ver um vídeo](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) antes de começar a compreender melhor os passos.

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Ligar uma VNet a um circuito - mesma subscrição

> [!NOTE]
> Informações de configuração de BGP não irão aparecer se o fornecedor de camada 3 configurado os peerings. Se o seu circuito está num Estado aprovisionado, deverá conseguir criar ligações.
>

### <a name="to-create-a-connection"></a>Para criar uma ligação

1. Certifique-se de que o circuito ExpressRoute e do peering privado do Azure foram configuradas com êxito. Siga as instruções em [criar um circuito ExpressRoute](expressroute-howto-circuit-arm.md) e [configurar encaminhamento](expressroute-howto-routing-arm.md). O circuito de ExpressRoute deverá ser semelhante a imagem seguinte:

  ![Captura de ecrã de circuito de ExpressRoute](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
2. Agora pode começar a aprovisionar uma ligação para ligar o gateway de rede virtual para o seu circuito do ExpressRoute. Clique em **ligação** > **adicionar** para abrir o **adicionar ligação** página e, em seguida, configure os valores.

  ![Adicionar ligação captura de ecrã](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)
3. Depois da ligação foi configurada com êxito, o objeto de ligação irá mostrar as informações da ligação.

  ![Captura de ecrã de objeto de ligação](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Ligar uma VNet a um circuito - subscrição diferente

Pode partilhar um circuito ExpressRoute entre várias subscrições. A figura abaixo mostra uma simples schematic de funciona como partilha para circuitos do ExpressRoute entre várias subscrições.

![Conectividade entre subscrições](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Cada uma das nuvens inferior na nuvem grande é utilizada para representar subscrições que pertencem aos diferentes departamentos dentro de uma organização.
- Cada um dos departamentos dentro da organização pode utilizar as seus próprios subscrição para implementar os seus serviços, mas podem partilhar um único circuito de ExpressRoute para ligar a sua rede no local.
- Um único departamento (neste exemplo: IT) pode ter o circuito ExpressRoute. Outras subscrições dentro da organização podem utilizar o circuito do ExpressRoute e autorizações associadas ao circuito, incluindo subscrições ligadas outros inquilinos do Azure Active Directory e inscrições através do contrato Enterprise.

  > [!NOTE]
  > Custos de largura de banda e de conectividade para o circuito dedicado serão aplicados ao proprietário do circuito ExpressRoute. Todas as redes virtuais partilham a mesma largura de banda.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Administração - sobre os proprietários de circuito e utilizadores de circuito

'Proprietário do circuito' é um utilizador autorizado de energia do recurso de circuito ExpressRoute. O proprietário do circuito pode criar autorizações que podem ser resgatadas por 'utilizadores circuito'. Utilizadores de circuito são proprietários dos gateways de rede virtual que não estão na mesma subscrição que o circuito ExpressRoute. Utilizadores de circuito podem resgatar autorizações (uma autorização por rede virtual).

O proprietário do circuito tem a capacidade de modificar e revogar autorizações em qualquer altura. Revogar um resultados de autorização em todas as ligações de ligação seja eliminados da subscrição cujo acesso foi revogado.

### <a name="circuit-owner-operations"></a>Operações de proprietário do circuito

**Para criar uma autorização de ligação**

O proprietário do circuito cria uma autorização. Isto resulta na criação de uma chave de autorização que pode ser utilizada por um utilizador do circuito para ligar os gateways de rede virtual para o circuito do ExpressRoute. Uma autorização é válida para apenas uma ligação.

1. Na página do ExpressRoute, clique em **autorizações** e, em seguida, escreva um **nome** para a autorização e clique em **guardar**.

  ![Autorizações](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Quando a configuração é guardada, copie o **ID de recurso** e **chave de autorização**.

  ![Chave de autorização](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Para eliminar uma autorização de ligação**

Pode eliminar uma ligação selecionando o **eliminar** ícone na página para a ligação.

### <a name="circuit-user-operations"></a>Operações de utilizador do circuito

O utilizador do circuito tem o ID de recurso e uma chave de autorização do proprietário do circuito.

**Para resgatar uma autorização de ligação**

1. Clique em de **+ novo** botão.

  ![Clique em novo](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Procurar **"Ligação"** no Marketplace, selecione-o e clique em **criar**.

  ![Pesquisa para a ligação](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. Certifique-se de que o **tipo de ligação** está definido como "ExpressRoute".
4. Preencha os detalhes, em seguida, clique em **OK** na página Noções básicas.

  ![Página de noções básicas](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. No **definições** página, selecione o **gateway de rede Virtual** e verifique o **resgatar autorização** caixa de verificação.
6. Introduza o **chave de autorização** e **elemento circuito URI** e atribua um nome de ligação. Clique em **OK**.

  ![Página de definições](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. Reveja as informações de **resumo** página e clique em **OK**.

**Para libertar uma autorização de ligação**

Pode disponibilizar uma autorização ao eliminar a ligação que liga o circuito de ExpressRoute para a rede virtual.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>Eliminar uma ligação ao desassociar uma VNet

Pode eliminar uma ligação e desassociar a VNet a um circuito ExpressRoute, selecionando o **eliminar** ícone na página para a ligação.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações acerca do ExpressRoute, veja as [FAQs do ExpressRoute](expressroute-faqs.md).