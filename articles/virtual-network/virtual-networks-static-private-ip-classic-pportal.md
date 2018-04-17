---
title: Configurar endereços IP privados para as VMs (clássica) - portal do Azure | Microsoft Docs
description: Saiba como configurar endereços IP privados para máquinas virtuais (clássicas) no portal do Azure.
services: virtual-network
documentationcenter: na
author: genli
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7b81f0fc1d504a0cb3422e003aa4ad7986396652
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Configurar endereços IP privados para uma máquina virtual (clássica) no portal do Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Este artigo abrange o modelo de implementação clássica. Também pode [gerir um endereço IP privado estático no modelo de implementação Resource Manager](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Os passos de exemplo que se seguem esperam num ambiente simple que já criado. Se pretender executar os passos, como são apresentados neste documento, criar primeiro o ambiente de teste descrito em [criar uma vnet](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Como especificar um endereço IP privado estático quando criar uma VM
Para criar uma VM chamada *DNS01* no *front-end* sub-rede de uma VNet com o nome *TestVNet* com um IP privado estático de *192.168.1.101*, completa os seguintes passos:

1. Num browser, navegue para https://portal.azure.com e, se necessário, inicie sessão com a sua conta do Azure.
2. Selecione **novo** > **computação** > **Windows Server 2012 R2 Datacenter**, tenha em atenção que o **selecionar um modelo de implementação** já lista mostra **clássico**e, em seguida, selecione **criar**.
   
    ![Criar a VM no portal do Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. Em **criar VM**, introduza o nome da VM que sejam criados (*DNS01* no cenário), a conta de administrador local e a palavra-passe.
   
    ![Criar a VM no portal do Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Selecione **configuração opcional** > **rede** > **rede Virtual**e, em seguida, selecione **TestVNet** . Se **TestVNet** não está disponível, certifique-se de que está a utilizar o *EUA Central* localização e tiver criado o ambiente de teste descrito no início deste artigo.
   
    ![Criar a VM no portal do Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. Em **rede**, certifique-se de que a sub-rede selecionada atualmente *front-end*, em seguida, selecione **endereços IP**, em **atribuição de endereços IP** Selecione **estático**e, em seguida, introduza *192.168.1.101* para **endereço IP** como mostrado abaixo.
   
    ![Criar a VM no portal do Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Selecione **OK** em **endereços IP**, selecione **OK** em **rede**e, em seguida, selecione **OK** em **Configuração opcional**.
7. Em **criar VM**, selecione **criar**. Repare no mosaico abaixo apresentado no dashboard:
   
    ![Criar a VM no portal do Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Como obter privadas informações endereços IP estáticos para uma VM
Para ver as estáticas informações de endereço IP privadas para a VM criada com os passos acima, execute os passos abaixo.

1. A partir do portal do Azure, selecione **Procurar tudo** > **máquinas virtuais (clássicas)** > **DNS01** > **todos os definições** > **endereços IP** e repare o endereço IP e a atribuição de endereços IP, como mostrado abaixo.
   
    ![Criar a VM no portal do Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Como remover um endereço IP privado estático de uma VM

Em **endereços IP**, selecione **dinâmica** à direita do **atribuição de endereços IP**, selecione **guardar**e, em seguida, selecione  **Sim**, conforme mostrado na imagem seguinte:
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Como adicionar um endereço IP privado estático para uma VM existente

1. Em **endereços IP**, apresentado anteriormente, selecione **estático** à direita do **atribuição de endereços IP**.
2. Tipo *192.168.1.101* para **endereço IP**, selecione **guardar**e, em seguida, selecione **Sim**.

## <a name="set-ip-addresses-within-the-operating-system"></a>Conjunto de endereços IP no sistema operativo

É recomendado que não estaticamente atribui o IP privado atribuído à máquina virtual do Azure no sistema operativo de uma VM, a menos que necessário. Se definir manualmente o endereço IP privado no sistema operativo, certifique-se de que é o mesmo endereço como o endereço IP privado atribuído à VM do Azure ou pode perder a conectividade à máquina virtual. Nunca manualmente deve atribuir o endereço IP público atribuído a uma máquina virtual do Azure dentro do sistema operativo da máquina virtual.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [reservado de IP público](virtual-networks-reserved-public-ip.md) endereços.
* Saiba mais sobre [instância ao nível do IP público (ILPIP)](virtual-networks-instance-level-public-ip.md) endereços.
* Consulte o [reservado APIs REST do IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).

