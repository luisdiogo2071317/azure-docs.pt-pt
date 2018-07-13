---
title: Criar uma VM com um endereço IP público estático - portal do Azure | Documentos da Microsoft
description: Saiba como criar uma VM com um endereço IP público estático através do portal do Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 524293f9a1ded73ee7cb6ba4f53208a9f9c54ffa
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670989"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Criar uma VM com um endereço IP público estático através do portal do Azure

> [!div class="op_single_selector"]
> * [Portal do Azure](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [CLI do Azure](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (Clássico)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com os recursos: [Resource Manager e clássico](../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação do Resource Manager, que é recomendado pela Microsoft para as implementações mais novas em vez do modelo de implementação clássica.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Criar uma VM com um IP público estático

Para criar uma VM com um endereço IP público estático no portal do Azure, conclua os seguintes passos:

1. Num browser, navegue para o [Portal do Azure](https://portal.azure.com) e, se necessário, inicie sessão com a sua conta do Azure.
2. No canto superior esquerdo do portal, clique em **criar um recurso**>>**computação**>**Windows Server 2012 R2 Datacenter**.
3. Na **selecionar um modelo de implementação** , selecione **Gestor de recursos** e clique em **criar**.
4. Na **Noções básicas** painel, introduza as informações de VM da seguinte forma e, em seguida, clique em **OK**.
   
    ![Portal do Azure - Noções básicas](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. Na **escolher um tamanho** painel, clique em **A1 padrão** como a forma e, em seguida, clique **selecionar**.
   
    ![Portal do Azure – escolha um tamanho](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. Na **definições** painel, clique em **endereço IP público**, em seguida, no **Criar endereço IP público** painel, em **atribuição**, clique em  **Estático** da seguinte forma. E, em seguida, clique em **OK**.
   
    ![Portal do Azure – Criar endereço IP público](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. Na **configurações** painel, clique em **OK**.
8. Reveja os **resumo** painel, como segue e, em seguida, clique **OK**.
   
    ![Portal do Azure – Criar endereço IP público](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Tenha em atenção o novo mosaico no dashboard.
   
    ![Portal do Azure – Criar endereço IP público](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Assim que a VM é criada, o **definições** painel apresentado da seguinte forma:
    
    ![Portal do Azure – Criar endereço IP público](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

## <a name="set-ip-addresses-within-the-operating-system"></a>Conjunto de endereços IP no sistema operativo

Nunca manualmente deve atribuir o endereço IP público atribuído a uma máquina virtual do Azure no sistema de operativo da máquina virtual. É recomendável que não atribuir estaticamente IP privado atribuído à máquina virtual do Azure no sistema operativo de uma VM, a menos que necessário, como quando [atribuição de IP de vários endereços para uma VM do Windows](virtual-network-multiple-ip-addresses-portal.md). Se definir manualmente o endereço IP privado no sistema operativo, certifique-se de que é o mesmo endereço como o endereço IP privado atribuído para o Azure [interface de rede](virtual-network-network-interface-addresses.md#change-ip-address-settings), ou pode perder a conectividade para a máquina virtual. Saiba mais sobre [endereço IP privado](virtual-network-network-interface-addresses.md#private) definições.

## <a name="next-steps"></a>Passos Seguintes

Qualquer tráfego de rede pode fluir de e para a VM que criou neste artigo. Pode definir regras de segurança de entrada e saída dentro de um grupo de segurança de rede que limitam o tráfego que pode fluir de e para a interface de rede, a sub-rede ou ambos. Para saber mais sobre os grupos de segurança de rede, veja [descrição geral de grupo de segurança de rede](security-overview.md).