---
title: Como configurar o MSI numa VM do Azure com o PowerShell
description: Passo a passo de instruções para configurar uma identidade de serviço gerida (MSI) numa VM do Azure, com o PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 9a466a5c695277a7b5833f997e2ad7281c962f3f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611004"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configurar uma VM Managed Service Identity (MSI) com o PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como ativar e remover o MSI para uma VM do Azure, com o PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Além disso, instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) (versão 4.3.1 ou posterior) se ainda não o fez.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Ativar o MSI durante a criação de uma VM do Azure

Para criar uma VM com capacidade de MSI:

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM"). 

   > [!IMPORTANT] 
   > Ao obter a secção "Criar a VM", não uma leve modificação para o [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Certifique-se de que adicionar um `-IdentityType "SystemAssigned"` parâmetro para aprovisionar a VM com um MSI, por exemplo:
   >  
   > `$vmConfig = New-AzureRmVMConfig -VMName myVM -IdentityType "SystemAssigned" ...`

   - [Criar uma máquina virtual do Windows com o PowerShell](~/articles/virtual-machines/windows/quick-create-powershell.md)
   - [Criar uma máquina virtual do Linux com o PowerShell](~/articles/virtual-machines/linux/quick-create-powershell.md)



2. Adicionar a extensão de VM de MSI com o `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Ativar o MSI numa VM do Azure existente

Se precisar de ativar o MSI numa máquina Virtual existente:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. Primeiro, recuperar as propriedades da VM com o `Get-AzureRmVM` cmdlet. Em seguida, para ativar o MSI, utilize o `-IdentityType` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. Adicionar a extensão de VM de MSI com o `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Remover MSI a partir de uma VM do Azure

Se tiver uma Máquina Virtual que já não necessita de um MSI, pode usar o `RemoveAzureRmVMExtension` cmdlet para remover o MSI da VM:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. Utilize o `-Name` mudar com o [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, especificando o mesmo nome que utilizou quando adicionou a extensão:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>Conteúdo relacionado

- [Descrição geral de identidade do serviço gerido](msi-overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](~/articles/virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](~/articles/virtual-machines/linux/quick-create-powershell.md) 

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
















