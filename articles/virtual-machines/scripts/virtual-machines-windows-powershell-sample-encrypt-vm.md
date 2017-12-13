---
title: Script do PowerShell do Azure de exemplo - encriptar uma VM do Windows | Microsoft Docs
description: Script do PowerShell do Azure de exemplo - encriptar uma VM do Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: iainfou
ms.openlocfilehash: f405cdaf61d6aaafa8568a9d7f21614071285c17
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>Encriptar uma máquina virtual do Windows com o Azure PowerShell

Este script cria um cofre de chaves segura do Azure, as chaves de encriptação, principal de serviço do Azure Active Directory e uma máquina virtual (VM) do Windows. A VM é então encriptada com a chave de encriptação do Cofre de chaves e as credenciais de principal de serviço.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos para criar a implementação. Cada item nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [Novo-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [Novo-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | Cria um cofre de chaves do Azure para armazenar os dados seguros, tais como chaves de encriptação. |
| [Adicionar-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | Cria uma chave de encriptação no Cofre de chaves. |
| [Novo AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Cria um Azure Active Directory principal de serviço para autenticar e controlar o acesso às chaves de encriptação de forma segura. |
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Define as permissões no Cofre de chaves para conceder o acesso de principal de serviço para as chaves de encriptação. |
| [Novo-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria a máquina virtual e liga-o para a placa de rede, a rede virtual, a sub-rede e o grupo de segurança de rede. Este comando também abre-se a porta 80 e define as credenciais administrativas. |
| [Get-AzureRmKeyVault](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | Obtém as informações necessárias no Cofre de chaves |
| [Conjunto AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | Ativa a encriptação numa VM com as credenciais de principal de serviço e a chave de encriptação. |
| [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | Mostra o estado do processo de encriptação de VM. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo Azure PowerShell, consulte [documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos de script do PowerShell de máquina virtual adicional que podem ser encontrados no [documentação do Azure Windows VM](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
