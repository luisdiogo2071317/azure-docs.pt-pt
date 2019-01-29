---
title: Como configurar identidades geridas para recursos do Azure na VM do Azure com o PowerShell
description: Instruções passo a passo instruções para configurar geridos identidades para recursos do Azure na VM do Azure com o PowerShell.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: priyamo
ms.openlocfilehash: a92e543ea8a633d4d7dfd1b276fadc0224696153
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169982"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Configurar identidades geridas para recursos do Azure na VM do Azure com o PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidades geridas para recursos do Azure fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, com o PowerShell, irá aprender a executar as seguintes identidades geridas para operações de recursos do Azure numa VM do Azure.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com identidades geridas para recursos do Azure, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre uma identidade gerida atribuído de sistema e atribuído ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [a versão mais recente do Azure PowerShell](/powershell/azure/install-az-ps) se ainda não o fez.

## <a name="system-assigned-managed-identity"></a>Atribuído ao sistema de identidade gerida

Nesta seção, aprenderá a ativar e desativar a identidade gerida atribuído de sistema com o Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Ativar a identidade gerida atribuído ao sistema durante a criação de uma VM do Azure

Para criar uma VM do Azure com a identidade gerida atribuídos do sistema ativada, a conta tem do [contribuinte de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) atribuição de função.  Não existem adicionais do Azure atribuições de funções de diretório do AD são necessárias.

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM").
    
    Ao obter a secção "Criar a VM", não uma leve modificação para o [New-AzVMConfig](/powershell/module/az.compute/new-azvm) sintaxe de cmdlet. Certifique-se de que adicionar um `-AssignIdentity:$SystemAssigned` parâmetro para aprovisionar a VM com a identidade atribuída por sistema ativada, por exemplo:
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Criar uma máquina virtual do Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar as identidades geridas para recursos do Azure VM extensão (planeada para preterição em Janeiro de 2019) com o `-Type` parâmetro no [conjunto AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também. As identidades geridas para a extensão VM de recursos do Azure está prevista para preterição em Janeiro de 2019. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Ativar a identidade gerida atribuído de sistema numa VM do Azure existente

Para ativar a identidade gerida atribuído de sistema numa VM que foi originalmente aprovisionada sem ele, a conta tem do [contribuinte de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) atribuição de função.  Não existem adicionais do Azure atribuições de funções de diretório do AD são necessárias.

1. Inicie sessão no Azure com `Connect-AzAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Connect-AzAccount
   ```

2. Primeiro, recuperar as propriedades da VM com o `Get-AzVM` cmdlet. Em seguida, para ativar uma identidade gerida atribuído de sistema, utilize o `-AssignIdentity` alternar o [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet:

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Opcional) Adicionar as identidades geridas para recursos do Azure VM extensão (planeada para preterição em Janeiro de 2019) com o `-Type` parâmetro no [conjunto AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

### <a name="add-vm-system-assigned-identity-to-a-group"></a>Adicionar a identidade de sistema atribuída de VM a um grupo

Depois de ter ativado o sistema de identidade numa VM atribuído, pode adicioná-lo a um grupo.  O procedimento a seguir adiciona uma identidade de sistema atribuída de uma VM a um grupo.

1. Inicie sessão no Azure com `Connect-AzAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Connect-AzAccount
   ```

2. Obter e tenha em atenção a `ObjectID` (conforme especificado no `Id` campo de valores devolvidos) do principal de serviço da VM:

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. Obter e tenha em atenção a `ObjectID` (conforme especificado no `Id` campo de valores devolvidos) do grupo:

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. Adicione principal de serviço da VM para o grupo:

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Desativar atribuído de sistema de identidade gerida de uma VM do Azure

Para desativar a identidade gerida atribuído de sistema numa VM, a conta tem do [contribuinte de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) atribuição de função.  Não existem adicionais do Azure atribuições de funções de diretório do AD são necessárias.

Se tiver uma Máquina Virtual que já não precisa de a identidade gerida atribuído de sistema, mas ainda precisa de identidades geridas atribuído ao utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Connect-AzAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Connect-AzAccount
   ```

2. Obter as propriedades da VM com o `Get-AzVM` cmdlet e defina a `-IdentityType` parâmetro para `UserAssigned`:

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Se tiver uma máquina virtual que já não tem atribuído o sistema de identidade gerida e tem não identidades geridas atribuído ao utilizador, utilize os seguintes comandos:

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Para remover as identidades geridas para a extensão VM de recursos do Azure, utilizador-comutador de nome com o [Remove-AzVMExtension](/powershell/module/az.compute/remove-azvmextension) cmdlet, especificando o mesmo nome que utilizou quando adicionou a extensão:

   ```powershell
   Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>Atribuído ao utilizador a identidade gerida

Nesta secção, saiba como adicionar e remover uma identidade gerida atribuído ao utilizador a partir de uma VM com o Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Atribuir uma identidade gerida atribuído ao utilizador a uma VM durante a criação

Para atribuir uma identidade de utilizador atribuído a uma VM, a conta tem do [contribuinte de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) e [operador de identidade gerida](/azure/role-based-access-control/built-in-roles#managed-identity-operator) atribuições de funções. Não existem adicionais do Azure atribuições de funções de diretório do AD são necessárias.

1. Consulte um dos inícios rápidos de VM do Azure concluir apenas as secções necessárias ("Iniciar sessão no Azure", "Criar o grupo de recursos", "Criar o grupo de rede", "Criar a VM"). 
  
    Ao obter a secção "Criar a VM", não uma leve modificação para o [ `New-AzVMConfig` ](/powershell/module/az.compute/new-azvm) sintaxe de cmdlet. Adicionar a `-IdentityType UserAssigned` e `-IdentityID ` parâmetros para aprovisionar a VM com uma identidade de utilizador atribuído.  Substitua `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores.  Por exemplo:
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Criar uma máquina virtual do Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Opcional) Adicionar a identidade gerida para a utilização da extensão VM de recursos do Azure a `-Type` parâmetro no [conjunto AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Certifique-se de que especifique o correto `-Location` parâmetro, que correspondem a localização da VM existente:
      > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também. As identidades geridas para a extensão VM de recursos do Azure está prevista para preterição em Janeiro de 2019.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Atribuir uma identidade gerida atribuído ao utilizador a uma VM do Azure existente

Para atribuir uma identidade de utilizador atribuído a uma VM, a conta tem do [contribuinte de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) e [operador de identidade gerida](/azure/role-based-access-control/built-in-roles#managed-identity-operator) atribuições de funções. Não existem adicionais do Azure atribuições de funções de diretório do AD são necessárias.

1. Inicie sessão no Azure com `Connect-AzAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```powershell
   Connect-AzAccount
   ```

2. Criar uma identidade gerida atribuído ao utilizador com o [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet.  Tenha em atenção o `Id` na saída porque irá precisar no próximo passo.

   > [!IMPORTANT]
   > Criação de identidades geridas atribuído ao utilizador só suporta alfanuméricos e hífen (0 a 9 ou a-z ou A-Z ou -) carateres. Além disso, o nome deve ser limitado a 24 carateres para a atribuição de VM/VMSS funcione corretamente. Volte mais tarde para obter atualizações. Para obter mais informações consulte [FAQ e problemas conhecidos](known-issues.md)

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Obter as propriedades da VM com o `Get-AzVM` cmdlet. Em seguida, para atribuir uma identidade gerida utilizador atribuído à VM do Azure, utilize o `-IdentityType` e `-IdentityID` alternar o [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet.  O valor para o`-IdentityId` parâmetro é o `Id` que anotou no passo anterior.  Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores.

   > [!WARNING]
   > Para manter qualquer identidades geridas anteriormente atribuído ao utilizador atribuídas à VM, consulte a `Identity` propriedade do objeto VM (por exemplo, `$vm.Identity`).  Se a qualquer utilizador atribuído identidades geridas são devolvidas, incluí-los no comando seguinte, juntamente com o novo utilizador atribuído a identidade gerida que pretende atribuir à VM.

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Adicionar a identidade gerida para recursos do Azure VM extensão (planeada para preterição em Janeiro de 2019) com o `-Type` parâmetro no [conjunto AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token. Especificar a correta `-Location` parâmetro, que correspondem a localização da VM existente.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Remover uma identidade gerida atribuído ao utilizador a partir de uma VM do Azure

Para remover uma identidade de utilizador atribuído a uma VM, a conta tem do [contribuinte de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) atribuição de função.

Se a VM tiver várias identidades geridas atribuído ao utilizador, pode remover todas, exceto a última está com os comandos seguintes. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VM NAME>` pelos seus próprios valores. O `<USER ASSIGNED IDENTITY NAME>` é propriedade de nome do atribuído ao utilizador gerido da identidade, que deve permanecer na VM. Estas informações podem ser encontradas ao consultar o `Identity` propriedade do objeto VM.  Por exemplo, `$vm.Identity`:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Se a VM não tem um sistema atribuído geridos identidade e pretender remover todas as identidades geridas atribuído ao utilizador do mesmo, utilize o seguinte comando:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Se a VM tem o sistema atribuído e atribuído ao utilizador identidades geridas, pode remover todos os geridos identidiades atribuídas ao mudar para utilizar apenas sistema geridos identidiades atribuídas.

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Passos Seguintes

- [Identidades geridas de descrição geral de recursos do Azure](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
