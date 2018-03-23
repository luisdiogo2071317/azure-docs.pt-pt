---
title: Exemplo do Script da CLI do Azure – Montar o disco do sistema operativo | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Montar o disco do sistema operativo
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7b9f1624426c7f401756310cd4fbe2789c29999d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Resolver problemas de um disco do sistema operativo de VMs

Este script monta o disco do sistema operativo de uma máquina virtual com falhas ou problemática como um disco de dados numa segunda máquina virtual. Isto pode ser útil quando estiver a resolver problemas do disco ou a recuperar dados. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma máquina virtual e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az vm show](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Devolve uma lista de máquinas virtuais. Neste caso, a opção de consulta é utilizada para devolver o disco do sistema operativo da máquina virtual. Este valor é adicionado ao "uri" de um nome de variável. |
| [az vm delete](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Elimina uma máquina virtual. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Cria uma máquina virtual.  |
| [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Anexa um disco a uma máquina virtual. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Devolve os endereços IP de uma máquina virtual. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI da máquina virtual adicionais na [Documentação da VM Linux do Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
