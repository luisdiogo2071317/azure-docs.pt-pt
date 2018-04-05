---
title: Criar uma VM do Windows zoned com o portal do Azure | Microsoft Docs
description: Criar uma VM do Windows numa zona de disponibilidade com o portal do Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 3d3561cf1ad760930821fabeef9839c25d55f2a9
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-the-azure-portal"></a>Criar uma máquina virtual do Windows numa zona de disponibilidade com o portal do Azure

Passos neste artigo, através de utilizar o portal do Azure para criar uma máquina virtual uma zona de disponibilidade do Azure. Uma [zona de disponibilidade](../../availability-zones/az-overview.md) é uma zona separada fisicamente numa região do Azure. Utilize as zonas de disponibilidade para proteger as aplicações e os dados de uma falha pouco provável ou da perda de um datacenter completo.

Para utilizar uma zona de disponibilidade, crie a máquina virtual num [suportado região do Azure](../../availability-zones/az-overview.md#regions-that-support-availability-zones).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-virtual-machine"></a>Criar a máquina virtual

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.

2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 

3. Introduza as informações da máquina virtual. O nome de utilizador e palavra-passe introduzidos aqui são utilizados para iniciar sessão na máquina virtual. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](faq.md#what-are-the-password-requirements-when-creating-a-vm). Escolha uma localização, como EUA Leste 2 suporta zonas de disponibilidade. Quando terminar, clique em **OK**.

    ![Introduza as informações básicas sobre a VM no painel do portal](./media/create-portal-availability-zone/create-windows-vm-portal-basic-blade.png)

4. Escolha um tamanho para a VM. Selecione um tamanho recomendado ou filtro com base nas funcionalidades. Confirme que o tamanho está disponível na zona de que pretende utilizar.

    ![Selecione um tamanho VM](./media/create-portal-availability-zone/create-windows-vm-portal-sizes.png)  

5. Em **definições** > **elevada disponibilidade**, selecione uma das zonas de numerados do **zona disponibilidade** lista pendente, mantenha as restantes predefinições, e Clique em **OK**.

    ![Selecione uma zona de disponibilidade](./media/create-portal-availability-zone/create-windows-vm-portal-availability-zone.png)

6. Na página Resumo, clique em **criar** para iniciar a implementação da máquina virtual.

7. A VM será afixada ao dashboard do portal do Azure. Depois de concluída a implementação, o resumo da VM abre-se automaticamente.

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Confirme a zona de disco gerido e endereço IP

Quando a VM está implementada uma zona de disponibilidade, é criado um disco gerido para a VM no mesmo horário de disponibilidade. Por predefinição, um endereço IP público também é criado nessa zona.

Pode confirmar as definições de zona para estes recursos no portal.  

1. Clique em **grupos de recursos** e, em seguida, o nome do recurso de grupo para a VM, tais como *myResourceGroup*.

2. Clique no nome do recurso de disco. O **descrição geral** página inclui detalhes sobre a zona de disponibilidade e a localização do recurso.

    ![Zona de disponibilidade do disco gerido](./media/create-portal-availability-zone/create-windows-vm-portal-disk.png)

3. Clique no nome do recurso de endereço IP público. O **descrição geral** página inclui detalhes sobre a zona de disponibilidade e a localização do recurso.

    ![Zona de disponibilidade para o endereço IP](./media/create-portal-availability-zone/create-windows-vm-portal-ip.png)



## <a name="next-steps"></a>Passos seguintes

Neste artigo, aprendeu a criar uma VM uma zona de disponibilidade. Saiba mais sobre [regiões e disponibilidade](regions-and-availability.md) para VMs do Azure.
