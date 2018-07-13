---
title: Voltar a implementar máquinas virtuais do Linux no Azure | Documentos da Microsoft
description: Como voltar a implementar máquinas virtuais do Linux no Azure para atenuar problemas de ligação de SSH.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: 1bd13c35ed49aeaab1a4f4aa94c984dc28f6c111
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308010"
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Voltar a implementar a máquina virtual do Linux para o novo nó do Azure
Se tiver dificuldades, resolução de problemas de SSH ou acesso de aplicação a uma máquina virtual (VM) do Linux no Azure, a reimplementação da VM pode ajudar. Quando voltar a implementar uma VM, ele muda a VM para um novo nó dentro da infraestrutura do Azure e, em seguida, liga-o novamente. Todas as suas opções de configuração e os recursos associados são mantidos. Este artigo mostra-lhe como voltar a implementar uma VM com a CLI do Azure ou o portal do Azure.

> [!NOTE]
> Depois de Reimplementar uma VM, o disco temporário é perdido e endereços IP dinâmicos associados à interface de rede virtual são atualizados. 

Pode voltar a implementar uma VM com uma das seguintes opções. Só precisa de escolher uma opção para voltar a implementar a sua VM:

- [CLI 2.0 do Azure](#azure-cli-20)
- [CLI do Azure 1.0](#azure-cli-10)
- [Portal do Azure](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Utilizar CLI 2.0 do Azure
Instalar a versão mais recente [CLI do Azure 2.0](/cli/azure/install-az-cli2) e inicie sessão na sua do Azure através da conta [início de sessão az](/cli/azure/reference-index#az_login).

A implementar a VM com [reimplementação da vm de az](/cli/azure/vm#az_vm_redeploy). O exemplo seguinte reimplementa a VM com o nome *myVM* no grupo de recursos com o nome *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Utilizar a CLI 1.0 do Azure
Instalar o [mais recente CLI 1.0 do Azure](../../cli-install-nodejs.md) e inicie sessão na sua conta do Azure. Certifique-se de que está no modo Resource Manager (`azure config mode arm`).

O exemplo seguinte reimplementa a VM com o nome *myVM* no grupo de recursos com o nome *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Passos Seguintes
Se estiver a ter problemas de ligação à sua VM, pode encontrar ajuda específica sobre [resolução de problemas de ligações SSH](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ou [detalhadas passos de resolução de problemas de SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Se não conseguir aceder uma aplicação em execução na sua VM, pode ainda ler [resolução de problemas de aplicação](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

