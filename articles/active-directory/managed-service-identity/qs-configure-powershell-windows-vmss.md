---
title: Como configurar a identidade do serviço gerido num VMSS do Azure com o PowerShell
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
ms.openlocfilehash: 5d4539c05d05053ac2ea6cd1c5fadbd161b41173
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257747"
---
# <a name="configure-a-vmss-managed-service-identity-using-powershell"></a>Configurar um VMSS identidade do serviço gerido com o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as operações de identidade do serviço gerido num conjunto de dimensionamento de máquina virtual do Azure, com o PowerShell:
- Ativar e desativar a identidade do sistema atribuído num conjunto de dimensionamento de máquinas virtuais
- Adicionar e remover uma identidade de utilizador atribuída num conjunto de dimensionamento de máquinas virtuais

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

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído durante a criação de um conjunto de dimensionamento de máquina virtual do Azure

Para criar um VMSS com a identidade de sistema atribuído ativada:

1. Consulte a *exemplo 1* no [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) artigo de referência do cmdlet para criar um VMSS com uma identidade de sistema atribuído.  Adicione o parâmetro `-IdentityType SystemAssigned` para o `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Opcional) Adicionar a extensão de VMSS de identidade de serviço gerido com o `-Name` e `-Type` parâmetro sobre o [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de dimensionamento de máquinas virtuais definido e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

    > [!NOTE]
    > Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure existente

Se precisar de ativar uma identidade de sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure existente:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita no conjunto de dimensionamento de máquina virtual, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Login-AzureRmAccount
   ```

2. Obter primeiro o dimensionamento de máquinas virtuais definir propriedades usando o [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Em seguida, para ativar uma identidade de sistema atribuído, utilize o `-IdentityType` alternar o [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. Adicionar a extensão de VMSS de identidade de serviço gerido com o `-Name` e `-Type` parâmetro sobre o [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de dimensionamento de máquinas virtuais definido e nomeie-o usando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto de final do token do OAuth para aquisição do token:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Desativar a identidade de sistema atribuída de um conjunto de dimensionamento de máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não tem o sistema de identidade atribuído, mas ainda precisa de identidades de atribuída ao utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita no conjunto de dimensionamento de máquina virtual, tais como "Contribuinte de Máquina Virtual":

2. Execute o seguinte cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de sistema de identidade atribuído e não tem nenhum utilizador identidades atribuída, utilize os seguintes comandos:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de um conjunto de dimensionamento com o Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Atribuir uma identidade atribuída ao utilizador durante a criação de um conjunto de dimensionamento de máquina virtual do Azure

Criar um novo conjunto de dimensionamento com um identidade atribuída ao utilizador atualmente não é suportada através do PowerShell. Veja a secção seguinte sobre como adicionar uma identidade de utilizador atribuída a um conjunto de dimensionamento de máquina virtual existente. Volte mais tarde para obter atualizações.

### <a name="assign-a-user-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Atribuir uma identidade de utilizador a um conjunto de dimensionamento de máquina virtual do Azure existente

Para atribuir uma identidade de utilizador atribuída a um conjunto de dimensionamento de máquina virtual do Azure existente:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita no conjunto de dimensionamento de máquina virtual, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. Obter primeiro o dimensionamento de máquinas virtuais definir propriedades usando o `Get-AzureRmVM` cmdlet. Em seguida, para atribuir uma identidade atribuída ao utilizador para o conjunto de dimensionamento de máquina virtual, utilize o `-IdentityType` e `-IdentityID` alternar o [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet. Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` pelos seus próprios valores.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Remover um utilizador atribuído a identidade gerida de um conjunto de dimensionamento de máquina virtual do Azure

Se o conjunto de dimensionamento de máquina virtual tiver várias identidades de utilizador atribuído, pode remover todas, exceto a última está com os comandos seguintes. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VMSS NAME>` pelos seus próprios valores. O `<MSI NAME>` é propriedade de nome da identidade de utilizador atribuído, que deve permanecer no conjunto de dimensionamento de máquina virtual. Estas informações podem ser encontradas na secção da identidade do conjunto com o dimensionamento da máquina virtual `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<MSI NAME>"
```
Se o conjunto de dimensionamento de máquina virtual não tem um sistema de identidade atribuído e que pretende remover o utilizador de todas as identidades atribuídas da mesma, utilize o seguinte comando:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Se o conjunto de dimensionamento de máquina virtual tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Conteúdo relacionado

- [Descrição geral de identidade do serviço gerido](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















