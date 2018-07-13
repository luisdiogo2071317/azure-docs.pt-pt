---
title: Exemplo de Script do Azure PowerShell – Encriptar uma VM do Windows | Microsoft Docs
description: Exemplo de Script do Azure PowerShell – Encriptar uma VM do Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ms.openlocfilehash: af12630839d31bd5e3baa4c67cdd881f936e9d19
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927534"
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>Encriptar uma máquina virtual do Windows com o Azure PowerShell

Este script cria um Azure Key Vault seguro, as chaves de encriptação, o principal do serviço Azure Active Directory e uma máquina virtual (VM) do Windows. A VM é encriptada com a chave de encriptação a partir do Key Vault e das credenciais do principal do serviço.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar a implementação. Cada item na tabela liga a documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | Cria um Azure Key Vault para armazenar dados seguros, tais como chaves de encriptação. |
| [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | Cria uma chave de encriptação no Key Vault. |
| [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Cria um principal do serviço Azure Active Directory para autenticar e controlar o acesso às chaves de encriptação de forma segura. |
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Define as permissões no Key Vault para conceder ao principal do serviço acesso às chaves de encriptação. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria a máquina virtual e liga-a à placa de rede, à rede virtual, à sub-rede e ao grupo de segurança de rede. Este comando também abre a porta 80 e define as credenciais administrativas. |
| [Get-AzureRmKeyVault](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | Obtém as informações necessárias no Key Vault |
| [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | Ativa a encriptação numa VM com as credenciais do principal do serviço e a chave de encriptação. |
| [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | Mostra o estado do processo de encriptação da VM. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver exemplos adicionais de scripts do PowerShell da máquina virtual na [Documentação da VM Windows do Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
