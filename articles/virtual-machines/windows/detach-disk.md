---
title: Desanexar um disco de dados de uma VM do Windows - Azure | Documentos da Microsoft
description: Desanexe um disco de dados de uma máquina virtual no Azure com o modelo de implementação do Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: e27826629873566bf7b746649923c25e6ab70827
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457161"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Como desanexar um disco de dados de uma máquina virtual do Windows

Quando já não precisar de um disco de dados que esteja ligado a uma máquina virtual, pode desligá-lo facilmente. Isso remove o disco da máquina virtual, mas não o remove do armazenamento.

> [!WARNING]
> Se desligar um disco que não é eliminado automaticamente. Se tiver inscrito para o armazenamento Premium, continuará a incorrer em custos de armazenamento para o disco. Para obter mais informações, consulte [preços e faturação quando utiliza o armazenamento Premium](premium-storage.md#pricing-and-billing).
>
>

Se pretender voltar a utilizar os dados existentes no disco, pode voltar a ligá-lo à mesma máquina virtual ou a outra.


## <a name="detach-a-data-disk-using-powershell"></a>Desanexar um disco de dados com o PowerShell

Pode *frequente* remover um disco de dados com o PowerShell, mas certifique-se de que nada é ativamente a utilizar o disco antes de desligá-lo a partir da VM.

Neste exemplo, podemos remover o disco com o nome **myDisk** da VM **myVM** no **myResourceGroup** grupo de recursos. Primeiro de remover o disco com o [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk) cmdlet. Em seguida, atualize o estado da máquina virtual, utilizando o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet, para concluir o processo de remover o disco de dados.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "myDisk"
Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

O disco permanece no armazenamento, mas já não está ligado a uma máquina virtual.


## <a name="detach-a-data-disk-using-the-portal"></a>Desanexar um disco de dados com o portal

1. No menu da esquerda, selecione **máquinas virtuais**.
2. Selecione a máquina virtual que tem o disco de dados que pretende desanexar e clique em **parar** ao desalocar a VM.
3. No painel da máquina virtual, selecione **discos**.
4. Na parte superior a **discos** painel, selecione **editar**.
5. Na **discos** painel, na extrema direita do disco de dados que pretende desanexar, clique nas ![imagem do botão de anulação de exposições](./media/detach-disk/detach.png) desanexar o botão.
5. Depois do disco tiver sido removido, clique em **guardar** na parte superior do painel.
6. No painel da máquina virtual, clique em **descrição geral** e, em seguida, clique nas **iniciar** botão na parte superior do painel para reiniciar a VM.

O disco permanece no armazenamento, mas já não está ligado a uma máquina virtual.

## <a name="next-steps"></a>Passos Seguintes
Se quiser reutilizar o disco de dados, simplesmente [anexá-lo a outra VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

