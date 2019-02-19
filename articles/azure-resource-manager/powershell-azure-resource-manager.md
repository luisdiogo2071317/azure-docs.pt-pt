---
title: Gerir soluções do Azure com o PowerShell | Documentos da Microsoft
description: Utilize o Azure PowerShell e do Resource Manager para gerir os recursos.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 7a3a603b6b1b76f61536adee5d33663bd7af9813
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342078"
---
# <a name="manage-resources-with-azure-powershell"></a>Gerir recursos com o Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzAccount` para criar uma ligação com o Azure.

## <a name="understand-scope"></a>Compreender o âmbito

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

Neste artigo, aplica todas as definições de gestão para um grupo de recursos para que pode remover facilmente essas definições, quando tiver terminado.

Vamos criar o grupo de recursos.

```azurepowershell-interactive
Set-AzContext -Subscription <subscription-name>
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

Atualmente, o grupo de recursos está vazio.

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Atribuir uma função

Neste artigo, vai implementar uma máquina virtual e a respetiva rede virtual relacionado. Para gerir soluções de máquina virtual, existem três funções de recursos específicos que fornecem o acesso normalmente necessário:

* [Contribuidor de Máquina Virtual](../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Contribuidor de Rede](../role-based-access-control/built-in-roles.md#network-contributor)
* [Contribuidor de Conta de Armazenamento](../role-based-access-control/built-in-roles.md#storage-account-contributor)

Em vez de atribuir funções a utilizadores individuais, muitas vezes, é mais fácil [criar um grupo do Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) para utilizadores que precisam de realizar ações semelhantes. Em seguida, atribua esse grupo à função adequada. Para simplificar este artigo, crie um grupo do Azure Active Directory sem membros. Pode ainda atribuir este grupo a uma função dentro de um âmbito.

O exemplo seguinte cria um grupo e o atribui à função de contribuinte de Máquina Virtual para o grupo de recursos. Para executar o `New-AzureAdGroup` comando, tem de utilizar o [Azure Cloud Shell](/azure/cloud-shell/overview) ou [transferir o módulo do Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Normalmente, pode repetir o processo para o **Contribuidor de Rede** e o **Contribuidor de Conta de Armazenamento** para confirmar que os utilizadores estão atribuídos para gerir os recursos implementados. Neste artigo, pode ignorar esses passos.

## <a name="azure-policy"></a>Azure Policy

O [Azure Policy](../governance/policy/overview.md) ajuda-o a certificar-se de que todos os recursos na subscrição cumprem os padrões empresariais. A subscrição já tem várias definições de política. Para ver as definições de política disponíveis, utilize:

```azurepowershell-interactive
(Get-AzPolicyDefinition).Properties | Format-Table displayName, policyType
```

Poderá ver as definições de política existentes. O tipo de política é **BuiltIn** ou **Personalizado**. Examine as definições para encontrar aquelas que descrevem uma condição que pretenda atribuir. Neste artigo, pode atribuir políticas para:

* limitar as localizações para todos os recursos
* limitar as SKUs para máquinas virtuais
* máquinas virtuais que não utilizam discos geridos de auditoria

```azurepowershell-interactive
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
New-AzPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Implementar a máquina virtual

Atribuiu funções e políticas, pelo que está pronto para implementar a sua solução. O tamanho predefinido é Standard_DS1_v2, que é um dos seus SKUs permitidos. Ao executar este passo, serão pedidas credenciais. Os valores que introduzir são configurados, como o nome de utilizador e a palavra-passe para a máquina virtual.

```azurepowershell-interactive
New-AzVm -ResourceGroupName "myResourceGroup" `
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

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>Um recurso de bloqueio

Para bloquear a máquina virtual e o grupo de segurança de rede, utilize:

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

A máquina virtual só pode ser eliminada se especificamente a remover o bloqueio. Este passo é apresentado em [Limpar recursos](#clean-up-resources).

## <a name="tag-resources"></a>Etiquetar recursos

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Etiquetar recursos

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

Para aplicar etiquetas a uma máquina virtual, utilize:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Localizar recursos por etiqueta

Para localizar recursos com um nome de etiqueta e valor, utilize:

```azurepowershell-interactive
(Find-AzResource -TagName Environment -TagValue Test).Name
```

Pode utilizar os valores devolvidos das tarefas de gestão, como parar todas as máquinas virtuais com um valor de etiqueta.

```azurepowershell-interactive
Find-AzResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzVM
```

### <a name="view-costs-by-tag-values"></a>Ver custos por valores de etiqueta

Depois de aplicar etiquetas a recursos, pode ver os custos dos recursos com essas etiquetas. Demora algum tempo para a análise de custos mostrar a utilização mais recente, pelo que poderá ainda não ver os custos. Quando os custos estão disponíveis, pode ver os custos dos recursos entre os grupos de recursos na sua subscrição. Os utilizadores devem ter [acesso ao nível de subscrição das informações de faturação](../billing/billing-manage-access.md) para ver os custos.

Para ver os custos por etiqueta no portal, selecione a sua subscrição e selecione **Análise de Custos**.

![Análise de custos](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Em seguida, filtre por valor da etiqueta e selecione **Aplicar**.

![Ver custo por etiqueta](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

Também pode utilizar as [APIs de Faturação do Azure](../billing/billing-usage-rate-card-overview.md) para ver os custos programaticamente.

## <a name="clean-up-resources"></a>Limpar recursos

O grupo de segurança de rede bloqueado não pode ser eliminado até que o bloqueio seja removido. Para remover o bloqueio, utilize:

```azurepowershell-interactive
Remove-AzResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Quando já não for necessário, pode utilizar o [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) de comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a monitorização de suas máquinas virtuais, veja [monitorizar e atualizar uma Máquina Virtual do Windows com o Azure PowerShell](../virtual-machines/windows/tutorial-monitoring.md).
* Para saber mais sobre como utilizar o Centro de segurança do Azure para implementar práticas de segurança recomendadas [monitorizar a segurança da máquina virtual utilizando o Centro de segurança do Azure](../virtual-machines/windows/tutorial-azure-security.md).
* Pode mover os recursos existentes para um novo grupo de recursos. Para obter exemplos, consulte [mover recursos para um novo grupo de recursos ou subscrição](resource-group-move-resources.md).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).
