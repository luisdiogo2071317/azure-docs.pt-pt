---
title: Como configurar identidades geridas para recursos do Azure na VM do Azure com o PowerShell
description: Instruções passo a passo instruções para configurar geridos identidades para recursos do Azure na VM do Azure com o PowerShell.
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
ms.openlocfilehash: 347d4132756f7570865ab5bfb98cc959b7fa1f81
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44349080"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Configurar identidades geridas para recursos do Azure na VM do Azure com o PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidades geridas para recursos do Azure fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, com o PowerShell, aprenderá a efetuar as seguintes identidades geridas para operações de recursos do Azure numa VM do Azure:

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com identidades geridas para recursos do Azure, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre uma identidade gerida atribuído de sistema e atribuído ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de controlo de acesso baseado em funções do Azure seguintes:
    > [!NOTE]
    > Nenhum adicionais do Azure AD directory as atribuições de funções necessárias.
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para criar uma VM e ativar e remover o sistema e/ou atribuído ao utilizador a identidade gerida de uma VM do Azure.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) identidade gerida de função para criar um atribuído ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover um atribuído ao utilizador identidade gerida de e para uma VM.
- Instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.

## <a name="system-assigned-managed-identity"></a>Atribuído ao sistema de identidade gerida

Nesta seção, aprenderá a ativar e desativar a identidade gerida atribuído de sistema com o Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Ativar a identidade gerida atribuído ao sistema durante a criação de uma VM do Azure

Para criar uma VM do Azure com a identidade gerida atribuídos do sistema ativada:

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM").
    
    Ao obter a secção "Criar a VM", não uma leve modificação para o [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Certifique-se de que adicionar um `-AssignIdentity:$SystemAssigned` parâmetro para aprovisionar a VM com a identidade atribuída por sistema ativada, por exemplo:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Criar uma máquina virtual do Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar as identidades geridas para recursos do Azure VM extensão (planeada para preterição em Janeiro de 2019) com o `-Type` parâmetro no [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também. As identidades geridas para a extensão VM de recursos do Azure está prevista para preterição em Janeiro de 2019. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Ativar a identidade gerida atribuído de sistema numa VM do Azure existente

Se precisar de ativar uma identidade gerida atribuído de sistema numa máquina Virtual existente:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Login-AzureRmAccount
   ```

2. Primeiro, recuperar as propriedades da VM com o `Get-AzureRmVM` cmdlet. Em seguida, para ativar uma identidade gerida atribuído de sistema, utilize o `-AssignIdentity` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Opcional) Adicionar as identidades geridas para recursos do Azure VM extensão (planeada para preterição em Janeiro de 2019) com o `-Type` parâmetro no [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Desativar atribuído de sistema de identidade gerida de uma VM do Azure

Se tiver uma Máquina Virtual que já não precisa de a identidade gerida atribuído de sistema, mas ainda precisa de identidades geridas atribuído ao utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Login-AzureRmAccount
   ```

2. Obter as propriedades da VM com o `Get-AzureRmVM` cmdlet e defina a `-IdentityType` parâmetro para `UserAssigned`:

   ```powershell   
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM    
   Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Se tiver uma máquina virtual que já não tem atribuído o sistema de identidade gerida e tem não identidades geridas atribuído ao utilizador, utilize os seguintes comandos:

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Para remover as identidades geridas para a extensão VM de recursos do Azure, utilizador-comutador de nome com o [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, especificando o mesmo nome que utilizou quando adicionou a extensão:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>Atribuído ao utilizador a identidade gerida

Nesta secção, saiba como adicionar e remover uma identidade gerida atribuído ao utilizador a partir de uma VM com o Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Atribuir uma identidade gerida atribuído ao utilizador a uma VM durante a criação

Para atribuir uma identidade gerida atribuído ao utilizador a uma VM do Azure ao criar a VM:

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM"). 
  
    Ao obter a secção "Criar a VM", não uma leve modificação para o [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) sintaxe de cmdlet. Adicionar a `-IdentityType UserAssigned` e `-IdentityID ` parâmetros para aprovisionar a VM com uma identidade de utilizador atribuído.  Substitua `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores.  Por exemplo:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Criar uma máquina virtual do Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar a identidade gerida para a utilização da extensão VM de recursos do Azure a `-Type` parâmetro sobre o [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:
      > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também. As identidades geridas para a extensão VM de recursos do Azure está prevista para preterição em Janeiro de 2019.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Atribuir uma identidade gerida atribuído ao utilizador a uma VM do Azure existente

Para atribuir uma identidade gerida atribuído ao utilizador a uma VM do Azure existente:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Connect-AzureRmAccount
   ```

2. Criar uma identidade gerida atribuído ao utilizador com o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet.  Tenha em atenção o `Id` na saída porque irá precisar no próximo passo.

   > [!IMPORTANT]
   > Criação de identidades geridas atribuído ao utilizador só suporta alfanuméricos e hífen (0 a 9 ou a-z ou A-Z ou -) carateres. Além disso, o nome deve ser limitado a 24 carateres para a atribuição de VM/VMSS funcione corretamente. Volte mais tarde para obter atualizações. Para obter mais informações consulte [FAQ e problemas conhecidos](known-issues.md)

   ```powershell
   New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Obter as propriedades da VM com o `Get-AzureRmVM` cmdlet. Em seguida, para atribuir uma identidade gerida utilizador atribuído à VM do Azure, utilize o `-IdentityType` e `-IdentityID` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet.  O valor para o`-IdentityId` parâmetro é o `Id` que anotou no passo anterior.  Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Adicionar a identidade gerida para recursos do Azure VM extensão (planeada para preterição em Janeiro de 2019) com o `-Type` parâmetro no [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Especificar a correta `-Location` parâmetro, que correspondem a localização da VM existente.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Remover uma identidade gerida atribuído ao utilizador a partir de uma VM do Azure

Se a VM tiver várias identidades geridas atribuído ao utilizador, pode remover todas, exceto a última está com os comandos seguintes. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VM NAME>` pelos seus próprios valores. O `<USER ASSIGNED IDENTITY NAME>` é propriedade de nome do atribuído ao utilizador gerido da identidade, que deve permanecer na VM. Estas informações podem ser encontradas na secção da identidade da VM usando `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Se a VM não tem um sistema atribuído geridos identidade e pretender remover todas as identidades geridas atribuído ao utilizador do mesmo, utilize o seguinte comando:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Se a VM tem o sistema atribuído e atribuído ao utilizador identidades geridas, pode remover todos os geridos identidiades atribuídas ao mudar para utilizar apenas sistema geridos identidiades atribuídas.

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Passos Seguintes

- [Identidades geridas de descrição geral de recursos do Azure](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
