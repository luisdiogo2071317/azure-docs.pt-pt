---
title: "Gerir soluções do Azure com o PowerShell | Microsoft Docs"
description: Utilize o Azure PowerShell e do Resource Manager para gerir os recursos.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: tomfitz
ms.openlocfilehash: 96206482195cdcbd06ee2dafdc551f7b1f81d319
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="manage-resources-with-azure-powershell"></a>Gerir os recursos com o Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, consulte [módulo Azure PowerShell instalar](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="understand-scope"></a>Compreender o âmbito

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

Neste artigo, aplicar todas as definições de gestão a um grupo de recursos para poder remover facilmente essas definições quando terminar.

Vamos criar o grupo de recursos.

```azurepowershell-interactive
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Atualmente, o grupo de recursos está vazio.

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Atribuir uma função

Neste artigo, implementar uma máquina virtual e a rede virtual relacionadas. Para gerir soluções de máquina virtual, existem três funções específicas do recurso que fornecem acesso normalmente necessário:

* [Contribuinte de máquina virtual](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor)
* [Contribuidor de Rede](../active-directory/role-based-access-built-in-roles.md#network-contributor)
* [Contribuinte de conta de armazenamento](../active-directory/role-based-access-built-in-roles.md#storage-account-contributor)

Em vez de atribuir funções para utilizadores individuais, muitas vezes, é mais fácil [criar um grupo do Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md) para utilizadores que necessitam para efetuar ações semelhantes. Em seguida, atribua esse grupo à função adequada. Para simplificar este artigo, crie um grupo do Azure Active Directory sem membros. Pode ainda atribuir este grupo a uma função para um âmbito. 

O exemplo seguinte cria um grupo e atribui-o para a função de contribuinte de Máquina Virtual para o grupo de recursos. Para executar o `New-AzureAdGroup` comando, tem de utilizar o [Shell de nuvem do Azure](/azure/cloud-shell/overview) ou [transferir o módulo Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Normalmente, pode repete o processo para **contribuinte de rede** e **contribuinte de conta de armazenamento** para se certificar de que os utilizadores são atribuídos a gerir os recursos implementados. Neste artigo, pode ignorar esses passos.

## <a name="azure-policies"></a>Políticas do Azure

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Aplicar políticas

A subscrição já tem várias definições de política. Para ver as definições de política disponíveis, utilize:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Consulte as definições de política existente. O tipo de política está **BuiltIn** ou **personalizada**. Examine as definições para aqueles que descrevem uma condição que pretende atribuir. Neste artigo, pode atribuir políticas que:

* limitar as localizações para todos os recursos
* limitar os SKUs de máquinas virtuais
* máquinas virtuais que não utilize discos geridos de auditoria

```azurepowershell-interactive
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzureRmResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
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

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>Um recurso de bloqueio

Para bloquear a máquina virtual e o grupo de segurança de rede, utilize:

```azurepowershell-interactive
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

A máquina virtual só pode ser eliminada se especificamente a remover o bloqueio. Este passo é apresentado na [limpar recursos](#clean-up-resources).

## <a name="tag-resources"></a>Etiqueta de recursos

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Etiqueta de recursos

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

Para aplicar etiquetas a uma máquina virtual, utilize:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Localizar recursos por etiqueta

Para localizar recursos com um nome de tag e um valor, utilize:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Pode utilizar os valores devolvidos para tarefas de gestão, como parar todas as máquinas virtuais com um valor de etiqueta.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Custos de vista pelos valores de etiqueta

Depois de aplicar etiquetas a recursos, pode ver os custos para recursos com dessas etiquetas. Demora tempo para análise de custos ver a utilização mais recente, pelo que não poderá ver os custos ainda. Quando estiverem disponíveis os custos, pode ver os custos de recursos entre grupos de recursos na sua subscrição. Os utilizadores devem ter [acesso de nível de subscrição para as informações de faturação](../billing/billing-manage-access.md) para ver os custos.

Para ver os custos por tag no portal, selecione a sua subscrição e selecione **análise de custos**.

![Análise de custo](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Em seguida, o valor da etiqueta de filtro e selecione **aplicar**.

![Custo de vista por etiqueta](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

Também pode utilizar o [APIs de faturação do Azure](../billing/billing-usage-rate-card-overview.md) programaticamente ver os custos.

## <a name="clean-up-resources"></a>Limpar recursos

Não é possível eliminar o grupo de segurança de rede bloqueado até que o bloqueio é removido. Para remover o bloqueio, utilize:

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

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre como monitorizar as máquinas virtuais, consulte [monitorizar e atualizar uma Máquina Virtual do Windows com o Azure PowerShell](../virtual-machines/windows/tutorial-monitoring.md).
* Para saber mais sobre como utilizar o Centro de segurança do Azure para implementar as práticas de segurança recomendado, [monitorizar a segurança da máquina virtual utilizando o Centro de segurança do Azure](../virtual-machines/windows/tutorial-azure-security.md).
* Pode mover recursos existentes para um novo grupo de recursos. Para obter exemplos, consulte [mover recursos para o novo grupo de recursos ou subscrição](resource-group-move-resources.md).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](resource-manager-subscription-governance.md).
