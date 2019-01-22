---
title: Tutorial – Governar máquinas virtuais do Azure com o Azure PowerShell | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar o Azure PowerShell para gerir máquinas virtuais do Azure através da aplicação de RBAC, políticas, bloqueios e etiquetas
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 10/12/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 6b36cdecb178a7189773abbfe963411e19764401
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438665"
---
# <a name="tutorial-learn-about-windows-virtual-machine-governance-with-azure-powershell"></a>Tutorial: Saiba mais sobre a governação de máquina virtual do Windows com o Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Os exemplos neste artigo requerem a versão 6.0 ou posterior do Azure PowerShell. Se estiver a executar o PowerShell localmente e não tiver a versão 6.0 ou posterior, [atualize a sua versão](/powershell/azure/azurerm/install-azurerm-ps). Também precisa de executar `Connect-AzureRmAccount` para criar uma ligação ao Azure. Para instalações locais, tem também de [transferir o módulo do PowerShell do Azure AD](https://www.powershellgallery.com/packages/AzureAD/) para criar um novo grupo no Azure Active Directory.

## <a name="understand-scope"></a>Compreender o âmbito

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

Neste tutorial, vai aplicar todas as definições de gestão a um grupo de recursos para poder remover facilmente essas definições quando terminar.

Vamos então criar esse grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Atualmente, o grupo de recursos está vazio.

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

Deve confirmar que os utilizadores na sua organização possuem o nível adequado de acesso a estes recursos. Não vai querer conceder acesso ilimitado aos utilizadores, mas também precisa de confirmar que estes podem fazer o seu trabalho. O [Controlo de acesso baseado em funções](../../role-based-access-control/overview.md) permite-lhe gerir os utilizadores que têm permissão para concluir ações específicas num âmbito.

Para criar e remover atribuições de funções, os utilizadores devem ter acesso `Microsoft.Authorization/roleAssignments/*`. Este acesso é concedido através das funções Proprietário ou Administrador de Acesso dos Utilizadores.

Para gerir soluções de máquina virtual, existem três funções de recursos específicos que fornecem o acesso normalmente necessário:

* [Contribuidor de Máquina Virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Contribuidor de Rede](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Contribuidor de Conta de Armazenamento](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Em vez de atribuir funções a utilizadores individuais, muitas vezes, é mais fácil utilizar um grupo do Azure Active Directory que tem utilizadores que precisam de realizar ações semelhantes. Em seguida, atribua esse grupo à função adequada. Neste artigo, utilize um grupo existente para gerir a máquina virtual ou utilize o portal para [criar um grupo do Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Depois de criar um grupo novo ou encontrar um existente, utilize o comando [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) para atribuir o grupo do Azure Active Directory à função Contribuidor de Máquina Virtual do grupo de recursos.  

```azurepowershell-interactive
$adgroup = Get-AzureRmADGroup -DisplayName <your-group-name>

New-AzureRmRoleAssignment -ObjectId $adgroup.id `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Se receber um erro a indicar que o **Principal<guid> não existe no diretório**, o novo grupo ainda não foi propagado pelo Azure Active Directory. Tente executar o comando novamente.

Normalmente, pode repetir o processo para o *Contribuidor de Rede* e o *Contribuidor de Conta de Armazenamento* para confirmar que os utilizadores estão atribuídos para gerir os recursos implementados. Neste artigo, pode ignorar esses passos.

## <a name="azure-policy"></a>Azure Policy

O [Azure Policy](../../azure-policy/azure-policy-introduction.md) ajuda-o a certificar-se de que todos os recursos na subscrição cumprem os padrões empresariais. A subscrição já tem várias definições de política. Para ver as definições de política disponíveis, utilize o comando [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition):

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Poderá ver as definições de política existentes. O tipo de política é **BuiltIn** ou **Personalizado**. Examine as definições para encontrar aquelas que descrevem uma condição que pretenda atribuir. Neste artigo, pode atribuir políticas para:

* Limitar as localizações de todos os recursos.
* Limitar os SKUs das máquinas virtuais.
* Auditar máquinas virtuais que não utilizam discos geridos.

No exemplo seguinte, obtém três definições de políticas baseadas no nome a apresentar. Utiliza o comando [New-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) para atribuir essas definições ao grupo de recursos. Para algumas políticas, forneça valores de parâmetros para especificar os valores permitidos.

```azurepowershell-interactive
# Values to use for parameters
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

# Get the resource group
$rg = Get-AzureRmResourceGroup -Name myResourceGroup

# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

# Assign policy for allowed locations
New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations

# Assign policy for allowed SKUs
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus

# Assign policy for auditing unmanaged disks
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Implementar a máquina virtual

Atribuiu funções e políticas, pelo que está pronto para implementar a sua solução. O tamanho predefinido é Standard_DS1_v2, que é um dos seus SKUs permitidos. Ao executar este passo, serão pedidas credenciais. Os valores que introduzir são configurados, como o nome de utilizador e a palavra-passe para a máquina virtual.

```azurepowershell-interactive
New-AzureRmVm -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -VirtualNetworkName "myVnet" `
     -SubnetName "mySubnet" `
     -SecurityGroupName "myNetworkSecurityGroup" `
     -PublicIpAddressName "myPublicIpAddress" `
     -OpenPorts 80,3389
```

Após a conclusão da implementação, pode aplicar mais definições de gestão à solução.

## <a name="lock-resources"></a>Bloquear recursos

Os [bloqueios de recursos](../../azure-resource-manager/resource-group-lock-resources.md) impedem que os utilizadores na sua organização eliminem ou modifiquem acidentalmente recursos críticos. Ao contrário do controlo de acesso baseado em funções, os bloqueios de recursos aplicam uma restrição a todos os utilizadores e a todas as funções. Pode definir o nível do bloqueio para *CanNotDelete* ou *ReadOnly*.

Para bloquear a máquina virtual e o grupo de segurança de rede, utilize o comando [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock):

```azurepowershell-interactive
# Add CanNotDelete lock to the VM
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup

# Add CanNotDelete lock to the network security group
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Para testar os bloqueios, tente executar o seguinte comando:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Verá um erro a indicar que a operação de eliminação não pode ser concluída devido a um bloqueio. O grupo de recursos só poderá ser eliminado se remover especificamente os bloqueios. Este passo é apresentado em [Limpar recursos](#clean-up-resources).

## <a name="tag-resources"></a>Etiquetar recursos

O utilizador aplica as [etiquetas](../../azure-resource-manager/resource-group-using-tags.md) aos recursos do Azure para os organizar por categorias. Cada etiqueta é constituída por um nome e um valor. Por exemplo, pode aplicar o nome "Ambiente" e o valor "Produção" em todos os recursos na produção.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Para aplicar etiquetas a uma máquina virtual, utilize o comando [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource):

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Localizar recursos por etiqueta

Para localizar recursos com um nome e valor de etiqueta, utilize o comando [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource):

```azurepowershell-interactive
(Get-AzureRmResource -Tag @{ Environment="Test"}).Name
```

Pode utilizar os valores devolvidos das tarefas de gestão, como parar todas as máquinas virtuais com um valor de etiqueta.

```azurepowershell-interactive
Get-AzureRmResource -Tag @{ Environment="Test"} | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Ver custos por valores de etiqueta

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Limpar recursos

O grupo de segurança de rede bloqueado não pode ser eliminado até que o bloqueio seja removido. Para remover o bloqueio, utilize o comando [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock):

```azurepowershell-interactive
Remove-AzureRmResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzureRmResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Quando já não for necessário, pode utilizar o comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o Grupo de Recursos, a VM e todos os recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma imagem de VM personalizada. Aprendeu a:

> [!div class="checklist"]
> * Atribuir utilizadores a uma função
> * Aplicar políticas que impõem normas
> * Proteger recursos críticos com bloqueios
> * Etiquetar recursos para faturação e gestão

Avance para o próximo tutorial, para saber mais sobre máquinas virtuais de elevada disponibilidade.

> [!div class="nextstepaction"]
> [Monitorizar máquinas virtuais](tutorial-monitoring.md)

