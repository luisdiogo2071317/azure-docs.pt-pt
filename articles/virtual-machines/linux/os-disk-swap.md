---
title: Disco de SO de comutação para uma VM do Azure com a CLI | Microsoft Docs
description: Altere o disco do sistema operativo utilizado por uma máquina virtual do Azure utilizando a CLI.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: 1732b60ee135b765cdeea43f981bcef9575ff32c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>Alterar o disco do SO utilizado por uma VM do Azure utilizando a CLI


Se tiver uma VM existente, mas pretende trocar o disco para um disco de cópia de segurança ou de outro disco do SO, pode utilizar a CLI do Azure para os discos de SO de comutação. Não tem de eliminar e recriar a VM. Mesmo pode utilizar um disco gerido no outro grupo de recursos, desde que não está já em utilização.

A VM tem de ser stopped\deallocated, em seguida, o ID de recurso do disco gerido pode ser substituído com o ID de recurso de disco gerido diferente. 

Certifique-se de que o tipo de armazenamento e tamanho VM são compatíveis com o disco que pretende ligar. Por exemplo, se o disco que pretende utilizar no armazenamento Premium, em seguida, a VM tem de ser capaz de armazenamento Premium (como um tamanho de série DS).

Este artigo requer a CLI do Azure versão 2.0.25 ou superior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 


Utilize [lista de discos az](/cli/azure/disk#list) para obter uma lista dos discos no seu grupo de recursos.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Utilize [pare a vm az](/cli/azure/vm#stop) para stop\deallocate VM antes de trocar de discos.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Utilize [atualização az da vm](/cli/azure/vm#az-vm-update) com o ID de recurso completo do novo disco para o `--osdisk` parâmetro 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
Reiniciar a VM utilizando [início de vm az](/cli/azure/vm#start).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Passos seguintes?**

Para criar uma cópia de um disco, consulte [instantâneo de um disco](snapshot-copy-managed-disk.md).