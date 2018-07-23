---
title: Como configurar o MSI numa VM do Azure com o PowerShell
description: Passo a passo de instruções para configurar uma identidade de serviço gerida (MSI) numa VM do Azure, com o PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: e8714086a334576db120f82a1f2470a1de6ea6a1
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186021"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configurar uma VM Managed Service Identity (MSI) com o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

<a name="in-this-article-you-learn-how-to-perform-the-following-managed-service-identity-operations-on-an-azure-vm-using-powershell"></a>Neste artigo, aprenderá a efetuar as seguintes operações de identidade do serviço gerido na VM do Azure, com o PowerShell:
- 

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para criar uma VM e ativar e remover sistema atribuído a identidade gerida a partir de uma VM do Azure.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para uma VM.
- Instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta seção, aprenderá a ativar e desativar a identidade de sistema atribuída com o Azure PowerShell.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Ativar o sistema de identidade atribuído durante a criação de uma VM do Azure

Para criar uma VM do Azure com o sistema de identidade ativada atribuído:

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM").
    
    Ao obter a secção "Criar a VM", não uma leve modificação para o [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Certifique-se de que adicionar um `-AssignIdentity:$SystemAssigned` parâmetro para aprovisionar a VM com a identidade de sistema atribuído ativada, por exemplo:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Criar uma máquina virtual do Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar a extensão de VM de MSI com o `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Ativar o sistema de identidade numa VM do Azure existente atribuído

Se precisar de ativar uma identidade de sistema atribuído numa máquina Virtual existente:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Login-AzureRmAccount
   ```

2. Primeiro, recuperar as propriedades da VM com o `Get-AzureRmVM` cmdlet. Em seguida, para ativar uma identidade de sistema atribuído, utilize o `-AssignIdentity` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Opcional) Adicionar a extensão de VM de MSI com o `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

## <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Desativar o sistema de identidade atribuído a partir de uma VM do Azure

> [!NOTE]
>  A desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuído e identidades de utilizador atribuída.

Se tiver uma Máquina Virtual que já não tem o sistema de identidade atribuído, mas ainda precisa de identidades de atribuída ao utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Login-AzureRmAccount
   ```

2. Execute o seguinte cmdlet: 
    ```powershell       
    Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -IdentityType "UserAssigned"
    ```
Para remover a extensão de VM de MSI, utilizador-comutador de nome com o [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, especificando o mesmo nome que utilizou quando adicionou a extensão:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de uma VM com o Azure PowerShell.

### <a name="assign-a-user-assigned-identity-to-a-vm-during-creation"></a>Atribuir um utilizador a identidade atribuído a uma VM durante a criação

Para atribuir uma identidade de utilizador atribuída a uma VM do Azure ao criar a VM:

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM"). 
  
    Ao obter a secção "Criar a VM", não uma leve modificação para o [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Adicionar a `-IdentityType UserAssigned` e `-IdentityID ` parâmetros para aprovisionar a VM com uma identidade atribuída ao utilizador.  Substitua `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<MSI NAME>` pelos seus próprios valores.  Por exemplo:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>..."
    ```
    
    - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Criar uma máquina virtual do Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar a extensão de VM de MSI com o `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:
      > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-identity-to-an-existing-azure-vm"></a>Atribuir uma identidade de utilizador a uma VM do Azure existente

Para atribuir um identidade atribuída a uma VM do Azure existente ao utilizador:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Connect-AzureRmAccount
   ```

2. Criar um utilizador atribuído a identidade a utilizar o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet.  Tenha em atenção o `Id` na saída porque irá precisar no próximo passo.

   > [!IMPORTANT]
   > Criar identidades atribuídas por utilizadores só suporta alfanuméricos e hífen (0 a 9 ou a-z ou A-Z ou -) carateres. Além disso, o nome deve ser limitado a 24 carateres para a atribuição de VM/VMSS funcione corretamente. Volte mais tarde para obter atualizações. Para obter mais informações consulte [FAQ e problemas conhecidos](known-issues.md)

   ```powershell
   New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Obter as propriedades da VM com o `Get-AzureRmVM` cmdlet. Em seguida, para atribuir uma identidade de utilizador atribuído à VM do Azure, utilize o `-IdentityType` e `-IdentityID` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet.  O valor para o`-IdentityId` parâmetro é o `Id` que anotou no passo anterior.  Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Adicionar a extensão de VM de MSI com o `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Especificar a correta `-Location` parâmetro, que correspondem a localização da VM existente.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Remover um utilizador atribuído identidade gerida a partir de uma VM do Azure

> [!NOTE]
>  Remover todas as identidades de utilizador atribuída uma Máquina Virtual atualmente não é suportada, a menos que tenha um sistema de identidade atribuído. Volte mais tarde para obter atualizações.

Se a VM tiver múltiplas identidades de utilizador atribuído, pode remover todas, exceto a última está com os comandos seguintes. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VM NAME>` pelos seus próprios valores. O `<MSI NAME>` é propriedade de nome da identidade de utilizador atribuído, que deve permanecer na VM. Estas informações podem ser encontradas na secção da identidade da VM usando `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm
```

Se a VM tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = $null
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Conteúdo relacionado

- [Descrição geral de identidade do serviço gerido](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
