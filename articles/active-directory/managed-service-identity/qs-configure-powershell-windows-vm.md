---
title: Como configurar MSI numa VM do Azure com o PowerShell
description: Passo pelo passo as instruções para configurar uma identidade de serviço geridas (MSI) na VM do Azure, utilizando o PowerShell.
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
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 0c316a2c73b451e4d8f67ace7b41c38dcfbc52f0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configurar uma VM geridos serviço de identidade (MSI) com o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

<a name="in-this-article-you-learn-how-to-perform-the-following-managed-service-identity-operations-on-an-azure-vm-using-powershell"></a>Neste artigo, irá aprender a efetuar as seguintes operações de identidade de serviço gerida numa VM do Azure, utilizando o PowerShell:
- 

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instalar [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.

## <a name="system-assigned-identity"></a>Atribuído a identidade de sistema

Nesta secção, irá aprender como ativar e desativar a identidade de sistema atribuída com o Azure PowerShell.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Ativar o sistema atribuído identidade durante a criação de uma VM do Azure

Para criar uma VM do Azure com o sistema atribuído identidade ativada:

1. Consulte um do seguinte VM inícios rápidos do Azure, concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar grupo de recursos", "Criar grupo de rede", "Criar a VM").
    
    Quando chegar à secção "Criar a VM", efetuar uma pequena modificação para a [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Certifique-se de adicionar um `-AssignIdentity "SystemAssigned"` parâmetro para aprovisionar a VM com a identidade do sistema atribuído ativada, por exemplo:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity "SystemAssigned" ...
    ```

   - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Criar uma máquina virtual de Linux através do PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar a extensão de VM de MSI com o `-Type` parâmetro no [conjunto AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nome-la utilizando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto final token OAuth de aquisição de token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional como pode utilizar o ponto de final para a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem.

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Ativar o sistema atribuído identidade numa VM do Azure existente

Se precisar de ativar uma identidade de sistema atribuído numa máquina Virtual existente:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Login-AzureRmAccount
   ```

2. Obtenha primeiro as propriedades da VM utilizando o `Get-AzureRmVM` cmdlet. Em seguida, para ativar uma identidade de sistema atribuída, utilize o `-AssignIdentity` comutador no [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity "SystemAssigned"
   ```

3. (Opcional) Adicionar a extensão de VM de MSI com o `-Type` parâmetro no [conjunto AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nome-la utilizando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto final token OAuth para aquisição do token. É necessário especificar o correto `-Location` parâmetro correspondente a localização da VM existente:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional como pode utilizar o ponto de final para a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem.

## <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Desativar o sistema atribuído a identidade de uma VM do Azure

> [!NOTE]
>  Desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuídas e identidades atribuídas do utilizador.

Se tiver uma Máquina Virtual que já não tem o sistema atribuído de identidade, mas ainda tem atribuído identidades de utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Login-AzureRmAccount
   ```

2. Execute o seguinte cmdlet: 
    ```powershell       
    Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -IdentityType "UserAssigned"
    ```
Para remover a extensão de VM de MSI, utilizador-comutador de nome com o [remover AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, especificando o mesmo nome que utilizou quando adicionou a extensão:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-identity"></a>Utilizador atribuído identidade

Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de uma VM com o Azure PowerShell.

### <a name="assign-a-user-assigned-identity-to-a-vm-during-creation"></a>Atribuir um utilizador atribuído a identidade para uma VM durante a criação

Para atribuir uma identidade de utilizador atribuída a uma VM do Azure, ao criar a VM:

1. Consulte um do seguinte VM inícios rápidos do Azure, concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar grupo de recursos", "Criar grupo de rede", "Criar a VM"). 
  
    Quando chegar à secção "Criar a VM", efetuar uma pequena modificação para a [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Adicionar o `-IdentityType UserAssigned` e `-IdentityID ` parâmetros para aprovisionar a VM com uma identidade de utilizador atribuído.  Substitua `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<MSI NAME>` com os seus próprios valores.  Por exemplo:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>..."
    ```
    
    - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Criar uma máquina virtual de Linux através do PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar a extensão de VM de MSI com o `-Type` parâmetro no [conjunto AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nome-la utilizando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto final token OAuth para aquisição do token. É necessário especificar o correto `-Location` parâmetro correspondente a localização da VM existente:
      > [!NOTE]
    > Este passo é opcional como pode utilizar o ponto de final para a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-identity-to-an-existing-azure-vm"></a>Atribuir uma identidade de utilizador para uma VM do Azure existente

Para atribuir um utilizador atribuído a identidade para uma VM do Azure existente:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. Criar um utilizador atribuído a identidade utilizando o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet.  Tenha em atenção o `Id` na saída porque irá precisar no próximo passo.

    > [!IMPORTANT]
    > Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

  ```powershell
  New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
  ```
3. Obter as propriedades da VM utilizando o `Get-AzureRmVM` cmdlet. Em seguida, para atribuir uma identidade de utilizador atribuído à VM do Azure, utilize o `-IdentityType` e `-IdentityID` comutador no [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet.  O valor para o`-IdentityId` parâmetro é o `Id` que anotou no passo anterior.  Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<USER ASSIGNED IDENTITY NAME>` com os seus próprios valores.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Adicionar a extensão de VM de MSI com o `-Type` parâmetro no [conjunto AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nome-la utilizando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto final token OAuth para aquisição do token. Especifique o correto `-Location` parâmetro, a localização da VM existente de correspondência.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Remover um utilizador atribuído gerida identidade da VM do Azure

> [!NOTE]
>  Remover todas as identidades de utilizador atribuído a partir de uma Máquina Virtual atualmente não é suportada, a menos que tenha um sistema atribuído de identidade. Verifique novamente para as atualizações.

Se a VM tem múltiplas identidades de utilizador atribuído, pode remover todos os mas dos últimos usando os seguintes comandos. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. O `<MSI NAME>` é uma propriedade name a identidade de utilizador atribuída, que deve permanecer na VM. Podem encontrar estas informações, na secção da identidade da VM utilizando `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm
```

Se a VM possui sistema atribuída e atribuído identidades de utilizador, pode remover todos os a atribuída ao utilizador identidades por mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = $null
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Conteúdo relacionado

- [Descrição geral de identidade de serviço gerida](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















