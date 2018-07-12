---
title: Criar uma VM no portal do Azure | Documentos da Microsoft
description: Crie uma máquina virtual do Windows no portal do Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: 5fd2128ff436d3211f41c7dfdcc4c2b8aabd0eb0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232331"
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Criar uma máquina virtual com Windows no portal do Azure
> [!div class="op_single_selector"]
> * [Portal do Azure](tutorial.md)
> * [PowerShell: Implementação clássica](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Saiba como [executar estes passos com o modelo de implementação do Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) utilizando o **portal do Azure**.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Este tutorial mostra-lhe como criar uma máquina virtual do Azure (VM) executando o Windows no portal do Azure. Vamos utilizar uma imagem do Windows Server como exemplo, mas esta é apenas uma das muitas imagens que o Azure oferece. Tenha em atenção de que suas opções de imagem dependem da sua subscrição. Por exemplo, imagens de desktop do Windows podem estar disponíveis para os assinantes do MSDN.

Esta secção mostra-lhe como utilizar o **Dashboard** no portal do Azure para selecionar e, em seguida, criar a máquina virtual.

Também pode criar VMs que utilizem [suas próprias imagens](createupload-vhd.md). Para saber mais sobre este e outros métodos, consulte [diferentes formas de criar uma máquina virtual do Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a id="createvirtualmachine"> </a>Criar a máquina virtual
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [criar uma VM com o modelo de implementação do Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) no portal do Azure.
* Inicie sessão máquina virtual. Para obter instruções, consulte [iniciar sessão numa máquina virtual com o Windows Server](connect-logon.md).
* Anexe um disco para armazenar dados. Pode anexar discos vazios e discos que contêm dados. Para obter instruções, consulte a [anexar um disco de dados a uma máquina virtual do Windows criada com o modelo de implementação clássica](attach-disk.md).
