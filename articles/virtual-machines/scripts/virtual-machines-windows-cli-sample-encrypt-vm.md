---
title: Exemplo do Script da CLI do Azure – Encriptar uma VM do Windows | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Encriptar uma VM do Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 9d7b2b0c731c5fc820fe649394a13ee586e47606
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Encriptar uma máquina virtual do Windows no Azure

Este script cria um Azure Key Vault seguro, as chaves de encriptação, o principal do serviço Azure Active Directory e uma máquina virtual (VM) do Windows. A VM é encriptada com a chave de encriptação a partir do Key Vault e das credenciais do principal do serviço.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, o Azure Key Vault, o principal do serviço, uma máquina virtual e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Cria um Azure Key Vault para armazenar dados seguros, tais como chaves de encriptação. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Cria uma chave de encriptação no Key Vault. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Cria um principal do serviço Azure Active Directory para autenticar e controlar o acesso às chaves de encriptação de forma segura. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Define as permissões no Key Vault para conceder ao principal do serviço acesso às chaves de encriptação. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Cria a máquina virtual e liga-a à placa de rede, à rede virtual, à sub-rede e ao NSG. Este comando também especifica a imagem de máquina virtual a ser utilizada e as credenciais administrativas.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_enable) | Ativa a encriptação numa VM com as credenciais do principal do serviço e a chave de encriptação. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | Mostra o estado do processo de encriptação da VM. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI da máquina virtual adicionais na [Documentação da VM Windows do Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
