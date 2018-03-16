---
title: "Governar máquinas virtuais do Azure com a CLI do Azure | Microsoft Docs"
description: "Tutorial - gerir máquinas virtuais do Azure através da aplicação RBAC, políticas, bloqueios e etiquetas com a CLI do Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: tomfitz
ms.openlocfilehash: ac6f7b0d32479e9e7e9945f83dc63a5847cba6a4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="virtual-machine-governance-with-azure-cli"></a>Governação de máquina virtual com a CLI do Azure

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Para instalar e utilizar a CLI localmente, consulte [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="understand-scope"></a>Compreender o âmbito

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

Neste tutorial, aplicar todas as definições de gestão a um grupo de recursos para poder remover facilmente essas definições quando terminar.

Vamos criar nesse grupo de recursos.

```azurecli-interactive
az group create --name myResourceGroup --location "East US"
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

```azurecli-interactive
adgroupId=$(az ad group create --display-name VMDemoContributors --mail-nickname vmDemoGroup --query objectId --output tsv)
```

Demora alguns momentos depois de linha de comandos devolve para o grupo propagar ao longo do Azure Active Directory. Depois de aguardar 20 ou 30 segundos, utilize o [de criação da atribuição de função az](/cli/azure/role/assignment#az_role_assignment_create) comando para atribuir o novo grupo no Azure Active Directory para a função de contribuinte de Máquina Virtual para o grupo de recursos.  Se executar o seguinte comando antes de este tiver sido propagada, receberá um erro a indicar **Principal <guid> não existe no diretório**. Tente executar novamente o comando.

```azurecli-interactive
az role assignment create --assignee-object-id $adgroupId --role "Virtual Machine Contributor" --resource-group myResourceGroup
```

Normalmente, pode repete o processo para *contribuinte de rede* e *contribuinte de conta de armazenamento* para se certificar de que os utilizadores são atribuídos a gerir os recursos implementados. Neste artigo, pode ignorar esses passos.

## <a name="azure-policies"></a>Políticas do Azure

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Aplicar políticas

A subscrição já tem várias definições de política. Para ver as definições de política disponíveis, utilize o [lista de definição de política de az](/cli/azure/policy/definition#az_policy_definition_list) comando:

```azurecli-interactive
az policy definition list --query "[].[displayName, policyType, name]" --output table
```

Consulte as definições de política existente. O tipo de política está **BuiltIn** ou **personalizada**. Examine as definições para aqueles que descrevem uma condição que pretende atribuir. Neste artigo, pode atribuir políticas que:

* Limite as localizações para todos os recursos.
* Limite os SKUs de máquinas virtuais.
* Máquinas virtuais que não utilize discos geridos de auditoria.

No exemplo seguinte, é possível obter três definições de política com base no nome de apresentação. Utilizar o [de criação da atribuição de política de az](/cli/azure/policy/assignment#az_policy_assignment_create) comando para atribuir essas definições para o grupo de recursos. Para algumas políticas, forneça valores de parâmetro para especificar os valores permitidos.

```azurecli-interactive
# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
locationDefinition=$(az policy definition list --query "[?displayName=='Allowed locations'].name | [0]" --output tsv)
skuDefinition=$(az policy definition list --query "[?displayName=='Allowed virtual machine SKUs'].name | [0]" --output tsv)
auditDefinition=$(az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks'].name | [0]" --output tsv)

# Assign policy for allowed locations
az policy assignment create --name "Set permitted locations" \
  --resource-group myResourceGroup \
  --policy $locationDefinition \
  --params '{ 
      "listOfAllowedLocations": {
        "value": [
          "eastus", 
          "eastus2"
        ]
      }
    }'

# Assign policy for allowed SKUs
az policy assignment create --name "Set permitted VM SKUs" \
  --resource-group myResourceGroup \
  --policy $skuDefinition \
  --params '{ 
      "listOfAllowedSKUs": {
        "value": [
          "Standard_DS1_v2", 
          "Standard_E2s_v2"
        ]
      }
    }'

# Assign policy for auditing unmanaged disks
az policy assignment create --name "Audit unmanaged disks" \
  --resource-group myResourceGroup \
  --policy $auditDefinition
```

O anterior exemplo assume que já conhece os parâmetros para uma política. Se precisar de ver os parâmetros, utilize:

```azurecli-interactive
az policy definition show --name $locationDefinition --query parameters
```

## <a name="deploy-the-virtual-machine"></a>Implementar a máquina virtual

Atribuiu funções e as políticas de, pelo que está pronto para implementar a sua solução. O tamanho predefinido é Standard_DS1_v2, que é um dos seus SKUs permitidos. O comando cria chaves SSH, caso não existam numa localização predefinida.

```azurecli-interactive
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Após a conclusão da sua implementação, pode aplicar as definições de gestão mais para a solução.

## <a name="lock-resources"></a>Bloquear recursos

[Bloqueios de recurso](../../azure-resource-manager/resource-group-lock-resources.md) impedir que os utilizadores na sua organização acidentalmente eliminem ou modificar a recursos críticos. Ao contrário do controlo de acesso baseado em funções, bloqueios de recurso aplicam-se uma restrição em todos os utilizadores e funções. Pode definir o bloqueio para *CanNotDelete* ou *ReadOnly*.

Para criar ou eliminar as bloqueios de gestão, tem de ter acesso a `Microsoft.Authorization/locks/*` ações. Das funções incorporadas, apenas **proprietário** e **administrador de acesso de utilizador** concedidas essas ações.

Para bloquear a máquina virtual e o grupo de segurança de rede, utilize o [bloqueio az criar](/cli/azure/lock#az_lock_create) comando:

```azurecli-interactive
# Add CanNotDelete lock to the VM
az lock create --name LockVM \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVM \
  --resource-type Microsoft.Compute/virtualMachines

# Add CanNotDelete lock to the network security group
az lock create --name LockNSG \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVMNSG \
  --resource-type Microsoft.Network/networkSecurityGroups
```

Para testar os bloqueios, tente executar o seguinte comando:

```azurecli-interactive 
az group delete --name myResourceGroup
```

Verá um erro a indicar que a operação de eliminação não pode ser efetuada devido a um bloqueio. O grupo de recursos só pode ser eliminado se especificamente a remover os bloqueios. Este passo é apresentado na [limpar recursos](#clean-up-resources).

## <a name="tag-resources"></a>Etiqueta de recursos

Aplicar [etiquetas](../../azure-resource-manager/resource-group-using-tags.md) aos seus recursos do Azure para organizar logicamente por categorias. Cada etiqueta é constituída por um nome e um valor. Por exemplo, pode aplicar o nome "Ambiente" e o valor "Produção" em todos os recursos na produção.

[!INCLUDE [Resource Manager governance tags CLI](../../../includes/resource-manager-governance-tags-cli.md)]

Para aplicar etiquetas a uma máquina virtual, utilize o [etiquetas de recurso az](/cli/azure/resource#az_resource_tag) comando. As etiquetas existentes no recurso não são mantidas.

```azurecli-interactive
az resource tag -n myVM \
  -g myResourceGroup \
  --tags Dept=IT Environment=Test Project=Documentation \
  --resource-type "Microsoft.Compute/virtualMachines"
```

### <a name="find-resources-by-tag"></a>Localizar recursos por etiqueta

Para localizar recursos com um nome de tag e um valor, utilize o [lista de recursos de az](/cli/azure/resource#az_resource_list) comando:

```azurecli-interactive
az resource list --tag Environment=Test --query [].name
```

Pode utilizar os valores devolvidos para tarefas de gestão, como parar todas as máquinas virtuais com um valor de etiqueta.

```azurecli-interactive
az vm stop --ids $(az resource list --tag Environment=Test --query "[?type=='Microsoft.Compute/virtualMachines'].id" --output tsv)
```

### <a name="view-costs-by-tag-values"></a>Custos de vista pelos valores de etiqueta

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Não é possível eliminar o grupo de segurança de rede bloqueado até que o bloqueio é removido. Para remover o bloqueio, obter os IDs de bloqueios e forneça-lhes o [bloqueio az eliminar](/cli/azure/lock#az_lock_delete) comando:

```azurecli-interactive
vmlock=$(az lock show --name LockVM \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Compute/virtualMachines \
  --resource-name myVM --output tsv --query id)
nsglock=$(az lock show --name LockNSG \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Network/networkSecurityGroups \
  --resource-name myVMNSG --output tsv --query id)
az lock delete --ids $vmlock $nsglock
```

Quando já não for necessário, pode utilizar o comando [az group delete](/cli/azure/group#az_group_delete) para remover o Grupo de Recursos, a VM e todos os recursos relacionados. Encerre a sessão SSH da VM e, em seguida, elimine os recursos da seguinte forma:

```azurecli-interactive 
az group delete --name myResourceGroup
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

