---
title: Como configurar MSI um VMSS do Azure com o PowerShell
description: Passo a passo instruções para configurar um sistema e o utilizador atribuído identidades no VMSS do Azure, utilizando o PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 97c5e2dde3faeaad13317597bef4f70455d22102
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Configurar um VMSS geridos serviço de identidade (MSI) com o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a realizar as seguintes operações de identidade de serviço geridas numa Azure Virtual Machine escala definido (VMSS), através do PowerShell:
- Ativar e desativar o sistema atribuído a identidade de um VMSS do Azure
- Adicionar e remover um utilizador atribuído a identidade de um VMSS do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instalar [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez. 

## <a name="system-assigned-managed-identity"></a>Sistema de identidade gerida atribuídas

Nesta secção, saiba como ativar e remover uma identidade de sistema atribuída com o Azure PowerShell.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Ativar o sistema atribuído identidade durante a criação de um VMSS do Azure

Para criar um VMSS com a identidade do sistema atribuído ativada:

1. Consulte *exemplo 1* no [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) artigo de referência de cmdlet para criar um VMSS com uma identidade de sistema atribuída.  Adicione o parâmetro `-IdentityType SystemAssigned` para o `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Opcional) Adicionar a extensão de MSI VMSS com o `-Name` e `-Type` parâmetro no [adicionar AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nome-la utilizando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto final token OAuth de aquisição de token:

    > [!NOTE]
    > Este passo é opcional como pode utilizar o ponto de final para a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Ativar o sistema atribuído a identidade de um VMSS existente do Azure

Se precisar de ativar uma identidade de sistema atribuída uma VMSS existente do Azure:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Login-AzureRmAccount
   ```

2. Obtenha primeiro as propriedades VMSS utilizando o [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Em seguida, para ativar uma identidade de sistema atribuída, utilize o `-IdentityType` comutador no [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. Adicionar a extensão de MSI VMSS com o `-Name` e `-Type` parâmetro no [adicionar AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. Pode passar "ManagedIdentityExtensionForWindows" ou "ManagedIdentityExtensionForLinux", dependendo do tipo de VM e nome-la utilizando o `-Name` parâmetro. O `-Settings` parâmetro especifica a porta utilizada pelo ponto final token OAuth de aquisição de token:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Desativar o sistema atribuído a identidade de um VMSS do Azure

> [!NOTE]
> Desativar a identidade de serviço geridos de um conjunto de dimensionamento de Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuídas e identidades atribuídas do utilizador.

Se tiver uma Máquina Virtual conjunto de dimensionamento que já não tem o sistema atribuído de identidade, mas ainda tem atribuído identidades de utilizador, utilize o seguinte cmdlet:

1. Inicie sessão no Azure com `Login-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

2. Execute o seguinte cmdlet:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Utilizador atribuído identidade

Nesta secção, saiba como adicionar e remover um utilizador atribuído a identidade de um VMSS com o Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Atribuir um utilizador atribuído identidade durante a criação de um VMSS do Azure

Criar um novo VMSS com um utilizador atribuído identidade não é atualmente suportada através do PowerShell. Consulte a secção seguinte sobre como adicionar uma identidade de utilizador atribuída a um VMSS existente. Verifique novamente para as atualizações.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>Atribuir uma identidade de utilizador para um VMSS existente do Azure

Para atribuir um utilizador atribuído identidade um VMSS existente do Azure:

1. Inicie sessão no Azure com `Connect-AzureRmAccount`. Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. Obtenha primeiro as propriedades da VM utilizando o `Get-AzureRmVM` cmdlet. Em seguida, para atribuir uma identidade de utilizador atribuída para o Azure VMSS, utilize o `-IdentityType` e `-IdentityID` comutador no [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet. Substitua `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` com os seus próprios valores.

   > [!IMPORTANT]
   > Criar identidades de utilizador atribuída só suporta alfanuméricos e hífenes (0-9 ou a-z ou A-Z ou -) carateres. Além disso, o nome deve ser limitado a 24 carateres para a atribuição a VM/VMSS funcione corretamente. Verifique novamente para as atualizações. Para obter mais informações, consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)


   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Remover um utilizador atribuído a identidade de um VMSS do Azure

> [!NOTE]
> Remover todas as identidades de utilizador atribuída de um conjunto de dimensionamento de Máquina Virtual atualmente não é suportada, a menos que tenha um sistema atribuído de identidade. Verifique novamente para as atualizações.

Se a sua VMSS tiver múltiplas identidades de utilizador atribuído, pode remover todos os mas dos últimos usando os seguintes comandos. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VMSS NAME>` valores de parâmetros com os seus próprios valores. O `<MSI NAME>` é uma propriedade name a identidade de utilizador atribuída, que deve permanecer no VMSS. Podem encontrar estas informações, na secção da identidade da utilização de VMSS `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

Se tiver o VMSS sistema atribuída e atribuído identidades de utilizador, pode remover todos os a atribuída ao utilizador identidades por mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Conteúdo relacionado

- [Descrição geral de identidade de serviço gerida](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte:
  
  - [Criar uma máquina virtual do Windows com o PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Criar uma máquina virtual Linux com o PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















