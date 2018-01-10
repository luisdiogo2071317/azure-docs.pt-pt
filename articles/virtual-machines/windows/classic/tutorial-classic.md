---
title: Criar uma VM no portal do Azure | Microsoft Docs
description: "Crie uma máquina virtual do Windows no portal do Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 99a67821ab926983205e2327c428e854d20a0cf5
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Criar uma máquina virtual com o Windows no portal do Azure
> [!div class="op_single_selector"]
> * [Portal do Azure](tutorial.md)
> * [PowerShell: Implementação clássica](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássico](../../../resource-manager-deployment-model.md). Este artigo abrange utilizando o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Saiba como [executar estes passos, utilizando o modelo de implementação Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) utilizando o **portal do Azure**.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Este tutorial mostra como criar uma máquina virtual do Azure (VM) com o Windows no portal do Azure. Utilizaremos uma imagem do Windows Server como exemplo, mas que é apenas uma das muitas imagens que o Azure oferece. Tenha em atenção que as suas opções de imagem dependem da sua subscrição. Por exemplo, imagens de ambiente de trabalho do Windows poderão estar disponíveis para subscritores do MSDN.

Esta secção mostra como utilizar o **Dashboard** no portal do Azure para selecionar e, em seguida, criar a máquina virtual.

Também pode criar VMs utilizando [as suas próprias imagens](createupload-vhd.md). Para saber mais acerca desta e de outros métodos, consulte [diferentes formas para criar uma máquina virtual do Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a id="createvirtualmachine"></a>Criar a máquina virtual
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [criar uma VM utilizando o modelo de implementação Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) no portal do Azure.
* Inicie sessão na máquina virtual. Para obter instruções, consulte [inicie sessão numa máquina virtual com o Windows Server](connect-logon.md).
* Ligar um disco para armazenar dados. Pode anexar vazios discos e os discos que contêm dados. Para obter instruções, consulte o [anexar um disco de dados para uma máquina virtual do Windows criada com o modelo de implementação clássica](attach-disk.md).
