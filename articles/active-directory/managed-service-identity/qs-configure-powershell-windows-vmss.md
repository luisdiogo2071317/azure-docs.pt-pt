---
title: Como configurar o MSI num VMSS do Azure com o PowerShell
description: Instruções passo a passo instruções para configurar um sistema e um utilizador atribuído identidades num VMSS do Azure, com o PowerShell.
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
ms.openlocfilehash: 6033269ab1dd35721aff17b1732d1f02cc452b31
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216161"
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Configurar um VMSS Managed Service Identity (MSI) com o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, irá aprender a realizar as operações de identidade do serviço gerido numa máquina Virtual de dimensionamento definido (VMSS), com o PowerShell:
- Ativar e desativar o sistema de identidade no VMSS Azure atribuído
- Adicionar e remover um utilizador atribuído a identidade num VMSS do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) criar um conjunto de dimensionamento de máquinas virtuais e ativar e remover sistema atribuído geridos e/ou definir a identidade de utilizador de um dimensionamento de máquina virtual.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para um conjunto de dimensionamento de máquina virtual.
- Instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez. 

## <a name="system-assigned-managed-identity"></a>Sistema atribuído a identidade gerida

Nesta secção, saiba como ativar e remover uma identidade de sistema atribuída com o Azure PowerShell.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Ativar o sistema de identidade atribuído durante a criação de um VMSS do Azure

Para criar um VMSS com a identidade de sistema atribuído ativada:

1. Consulte a *exemplo 1* no [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) artigo de referência do cmdlet para criar um VMSS com uma identidade de sistema atribuído.  Adicione o parâmetro `-IdentityType SystemAssigned` para o `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Opcional) Adicionar a extensão de VMSS de MSI com o `-Name` e `-Type` parâmetro sobre o [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Ativar o sistema de identidade num VMSS do Azure existente atribuído

Se precisar de ativar uma identidade de sistema atribuído num VMSS do Azure existente:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual":

   ```powershell
   Login-AzureRmAccount
   ```

2. Primeiro, recuperar as propriedades VMSS com o [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Em seguida, para ativar uma identidade de sistema atribuído, utilize o `-IdentityType` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. Adicionar a extensão de VMSS de MSI com o `-Name` e `-Type` parâmetro sobre o [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Desativar o sistema de identidade atribuído de um VMSS do Azure

> [!NOTE]
> Desativar a identidade do serviço gerido de um conjunto de dimensionamento de Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuído e identidades de utilizador atribuída.

Se tiver uma Máquina Virtual conjunto de dimensionamento que já não tem o sistema de identidade atribuído, mas ainda precisa de identidades de atribuída ao utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual":

2. Execute o seguinte cmdlet:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de um VMSS com o Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Atribuir um utilizador a identidade atribuído durante a criação de um VMSS do Azure

Criar um novo VMSS com um identidade atribuída ao utilizador atualmente não é suportada através do PowerShell. Consulte a secção seguinte sobre como adicionar uma identidade de utilizador atribuída a um VMSS existente. Volte mais tarde para obter atualizações.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>Atribuir uma identidade de utilizador a um VMSS do Azure existente

Para atribuir um utilizador atribuído a identidade a um VMSS do Azure existente:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM, por exemplo, "Contribuinte de Máquina Virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. Primeiro, recuperar as propriedades da VM com o `Get-AzureRmVM` cmdlet. Em seguida, para atribuir uma identidade atribuída ao utilizador para o VMSS do Azure, utilize o `-IdentityType` e `-IdentityID` alternar o [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet. Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` pelos seus próprios valores.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Remover um utilizador atribuído a identidade do VMSS do Azure

> [!NOTE]
> Remover todas as identidades de utilizador atribuído de um conjunto de dimensionamento de máquinas virtuais atualmente não é suportada, a menos que tenha um sistema de identidade atribuído. Volte mais tarde para obter atualizações.

Se sua VMSS tem várias identidades de utilizador atribuído, pode remover todas, exceto a última está com os comandos seguintes. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VMSS NAME>` pelos seus próprios valores. O `<MSI NAME>` é propriedade de nome da identidade de utilizador atribuído, que deve permanecer no VMSS. Estas informações podem ser encontradas na secção da identidade do VMSS `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

Se sua VMSS tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Conteúdo relacionado

- [Descrição geral de identidade do serviço gerido](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















