---
title: Governar máquinas virtuais do Azure com o Azure PowerShell | Microsoft Docs
description: Tutorial - gerir máquinas virtuais do Azure através da aplicação RBAC, políticas, bloqueios e etiquetas com o Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: tomfitz
ms.openlocfilehash: 30f5fe83c46f2dbe1933e8347242be7fbb30a3e3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="virtual-machine-governance-with-azure-powershell"></a>Governação de máquina virtual com o Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, consulte [módulo Azure PowerShell instalar](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. Para instalações locais, tem também [transferir o módulo Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/) para criar um novo grupo no Azure Active Directory.

## <a name="understand-scope"></a>Compreender o âmbito

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

Neste tutorial, aplicar todas as definições de gestão a um grupo de recursos para poder remover facilmente essas definições quando terminar.

Vamos criar nesse grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Atualmente, o grupo de recursos está vazio.

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

Deve certificar-se de que os utilizadores na sua organização tiverem o nível adequado de acesso a estes recursos. Não pretende conceder acesso ilimitado a utilizadores, mas terá também de certificar-se de que podem fazer o seu trabalho. [Controlo de acesso baseado em funções](../../active-directory/role-based-access-control-what-is.md) permite-lhe gerir os utilizadores que têm permissão para concluir as ações específicas a um âmbito.

Para criar e remover atribuições de funções, os utilizadores devem ter `Microsoft.Authorization/roleAssignments/*` acesso. Este acesso é concedido através de funções de proprietário ou administrador de acesso de utilizador.

Para gerir soluções de máquina virtual, existem três funções específicas do recurso que fornecem acesso normalmente necessário:

* [Contribuinte de máquina virtual](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor)
* [Contribuidor de Rede](../../active-directory/role-based-access-built-in-roles.md#network-contributor)
* [Contribuinte de conta de armazenamento](../../active-directory/role-based-access-built-in-roles.md#storage-account-contributor)

Em vez de atribuir funções para utilizadores individuais, muitas vezes, é mais fácil [criar um grupo do Azure Active Directory](../../active-directory/active-directory-groups-create-azure-portal.md) para utilizadores que necessitam para efetuar ações semelhantes. Em seguida, atribua esse grupo à função adequada. Para simplificar este artigo, crie um grupo do Azure Active Directory sem membros. Pode ainda atribuir este grupo a uma função para um âmbito. 

O exemplo seguinte cria um grupo do Azure Active Directory com o nome *VMDemoContributors* com uma alcunha de correio de *vmDemoGroup*. A alcunha correio serve como um alias para o grupo.

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
```

Demora alguns momentos depois de linha de comandos devolve para o grupo propagar ao longo do Azure Active Directory. Depois de aguardar 20 ou 30 segundos, utilize o [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) comando para atribuir o novo grupo no Azure Active Directory para a função de contribuinte de Máquina Virtual para o grupo de recursos.  Se executar o seguinte comando antes de este tiver sido propagada, receberá um erro a indicar **Principal <guid> não existe no diretório**. Tente executar novamente o comando.

```azurepowershell-interactive
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Normalmente, pode repete o processo para *contribuinte de rede* e *contribuinte de conta de armazenamento* para se certificar de que os utilizadores são atribuídos a gerir os recursos implementados. Neste artigo, pode ignorar esses passos.

## <a name="azure-policies"></a>Políticas do Azure

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Aplicar políticas

A subscrição já tem várias definições de política. Para ver as definições de política disponíveis, utilize o [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) comando:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Consulte as definições de política existente. O tipo de política está **BuiltIn** ou **personalizada**. Examine as definições para aqueles que descrevem uma condição que pretende atribuir. Neste artigo, pode atribuir políticas que:

* Limite as localizações para todos os recursos.
* Limite os SKUs de máquinas virtuais.
* Máquinas virtuais que não utilize discos geridos de auditoria.

No exemplo seguinte, é possível obter três definições de política com base no nome de apresentação. Utilizar o [New-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) comando para atribuir essas definições para o grupo de recursos. Para algumas políticas, forneça valores de parâmetro para especificar os valores permitidos.

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

Atribuiu funções e as políticas de, pelo que está pronto para implementar a sua solução. O tamanho predefinido é Standard_DS1_v2, que é um dos seus SKUs permitidos. Ao executar este passo, serão pedidas credenciais. Os valores que introduzir são configurados, como o nome de utilizador e a palavra-passe para a máquina virtual.

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

Após a conclusão da sua implementação, pode aplicar as definições de gestão mais para a solução.

## <a name="lock-resources"></a>Bloquear recursos

[Bloqueios de recurso](../../azure-resource-manager/resource-group-lock-resources.md) impedir que os utilizadores na sua organização acidentalmente eliminem ou modificar a recursos críticos. Ao contrário do controlo de acesso baseado em funções, bloqueios de recurso aplicam-se uma restrição em todos os utilizadores e funções. Pode definir o bloqueio para *CanNotDelete* ou *ReadOnly*.

Para bloquear a máquina virtual e o grupo de segurança de rede, utilize o [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) comando:

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

Verá um erro a indicar que a operação de eliminação não pode ser efetuada devido a um bloqueio. O grupo de recursos só pode ser eliminado se especificamente a remover os bloqueios. Este passo é apresentado na [limpar recursos](#clean-up-resources).

## <a name="tag-resources"></a>Etiqueta de recursos

Aplicar [etiquetas](../../azure-resource-manager/resource-group-using-tags.md) aos seus recursos do Azure para organizar logicamente por categorias. Cada etiqueta é constituída por um nome e um valor. Por exemplo, pode aplicar o nome "Ambiente" e o valor "Produção" em todos os recursos na produção.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Para aplicar etiquetas a uma máquina virtual, utilize o [conjunto AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) comando:

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Localizar recursos por etiqueta

Para localizar recursos com um nome de tag e um valor, utilize o [localizar AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) comando:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Pode utilizar os valores devolvidos para tarefas de gestão, como parar todas as máquinas virtuais com um valor de etiqueta.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Custos de vista pelos valores de etiqueta

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Não é possível eliminar o grupo de segurança de rede bloqueado até que o bloqueio é removido. Para remover o bloqueio, utilize o [remover AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) comando:

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

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma imagem de VM personalizada. Aprendeu a:

> [!div class="checklist"]
> * Atribuir utilizadores a uma função
> * Aplicar políticas que impõem as normas
> * Proteger recursos críticos com bloqueios
> * Recursos de etiqueta de faturação e gestão

Avançar para o próximo tutorial para saber mais sobre como elevadas máquinas virtuais.

> [!div class="nextstepaction"]
> [Monitorizar máquinas virtuais](tutorial-monitoring.md)

