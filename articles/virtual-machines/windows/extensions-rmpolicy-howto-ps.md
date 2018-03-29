---
title: Utilizar a política do Azure para restringir a instalação da extensão de VM | Microsoft Docs
description: Utilize a política do Azure para restringir as implementações de extensão.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: danis;cynthn
ms.openlocfilehash: da5b0db997ba1aa0e998f6fe2645e955b490951d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-windows-vms"></a>Utilizar a política do Azure para restringir a instalação de extensões em VMs do Windows

Se pretender impedir a utilização ou a instalação de determinadas extensões no seu VMs do Windows, pode criar uma política do Azure através do PowerShell para restringir as extensões para VMs dentro de um grupo de recursos. 

Este tutorial utiliza o Azure PowerShell na Shell de nuvem, que é constantemente atualizado para a versão mais recente. Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 3,6 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). 

## <a name="create-a-rules-file"></a>Criar um ficheiro de regras

Para restringir a que extensões podem ser instalados, tem de ter um [regra](/azure/azure-policy/policy-definition#policy-rule) para fornecer a lógica para identificar a extensão.

Este exemplo mostra como negar extensões de publicado por 'Microsoft. Compute' através da criação de um ficheiro de regras na Shell de nuvem do Azure, mas se estiver a trabalhar no PowerShell localmente, também pode criar um ficheiro local e substitua o caminho ($home/clouddrive) com o caminho para o ficheiro local no seu computador.

Num [nuvem Shell](https://shell.azure.com/powershell), tipo:

```azurepowershell-interactive
nano $home/clouddrive/rules.json
```

Copie e cole o seguinte. de JSON para o ficheiro.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Quando tiver terminado, prima a **Ctrl + O** e, em seguida, **Enter** para guardar o ficheiro. Atingiu o **Ctrl + X** para fechar o ficheiro e saia.

## <a name="create-a-parameters-file"></a>Criar um ficheiro de parâmetros

Também precisa de um [parâmetros](/azure/azure-policy/policy-definition#parameters) ficheiro que cria uma estrutura para que possa utilizar para transmitir numa lista de extensões para bloquear. 

Este exemplo mostra como criar um ficheiro de parâmetros para VMs na Shell de nuvem, mas se estiver a trabalhar no PowerShell localmente, também pode criar um ficheiro local e substitua o caminho ($home/clouddrive) com o caminho para o ficheiro no seu computador local.

No [nuvem Shell](https://shell.azure.com/powershell), tipo:

```azurepowershell-interactive
nano $home/clouddrive/parameters.json
```

Copie e cole o seguinte. de JSON para o ficheiro.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

Quando tiver terminado, prima a **Ctrl + O** e, em seguida, **Enter** para guardar o ficheiro. Atingiu o **Ctrl + X** para fechar o ficheiro e saia.

## <a name="create-the-policy"></a>Criar uma política

Uma definição de política é um objeto utilizado para armazenar a configuração que pretende utilizar. A definição de política utiliza os ficheiros de parâmetros e as regras para definir a política. Criar uma definição de política utilizando o [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) cmdlet.

 Os parâmetros e as regras de política são ficheiros criados e armazenados como ficheiros. JSON na sua shell de nuvem.


```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
   -Name "not-allowed-vmextension-windows" `
   -DisplayName "Not allowed VM Extensions" `
   -description "This policy governs which VM extensions that are explicitly denied."   `
   -Policy 'C:\Users\ContainerAdministrator\clouddrive\rules.json' `
   -Parameter 'C:\Users\ContainerAdministrator\clouddrive\parameters.json'
```




## <a name="assign-the-policy"></a>Atribuir a política

Neste exemplo atribui a política a um grupo de recursos utilizando [New-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment). Qualquer VM criada no **myResourceGroup** grupo de recursos não será possível instalar as extensões de agente de acesso de VM ou um Script personalizado. 

Utilize o [Get-AzureRMSubscription | Formato de tabela](/powershell/module/azurerm.profile/get-azurermsubscription) cmdlet para obter o seu ID de subscrição para utilizar em vez do exemplo.

```azurepowershell-interactive
$scope = "/subscriptions/<subscription id>/resourceGroups/myResourceGroup"
$assignment = New-AzureRMPolicyAssignment `
   -Name "not-allowed-vmextension-windows" `
   -Scope $scope `
   -PolicyDefinition $definition `
   -PolicyParameter '{
    "notAllowedExtensions": {
        "value": [
            "VMAccessAgent",
            "CustomScriptExtension"
        ]
    }
}'
$assignment
```

## <a name="test-the-policy"></a>Testar a política

Para testar a política, experimente utilizar a extensão de acesso de VM. Que deve falhar com a mensagem "Set-AzureRmVMAccessExtension: recurso 'myVMAccess' não foi permitido pela política."

```azurepowershell-interactive
Set-AzureRmVMAccessExtension `
   -ResourceGroupName "myResourceGroup" `
   -VMName "myVM" `
   -Name "myVMAccess" `
   -Location EastUS 
```

No portal, a alteração de palavra-passe deve falhar com o "a implementação do modelo falhou devido a violação de política." Mensagem.

## <a name="remove-the-assignment"></a>Remover a atribuição

```azurepowershell-interactive
Remove-AzureRMPolicyAssignment -Name not-allowed-vmextension-windows -Scope $scope
```

## <a name="remove-the-policy"></a>Remover a política

```azurepowershell-interactive
Remove-AzureRmPolicyDefinition -Name not-allowed-vmextension-windows
```
    
## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [política Azure](../../azure-policy/azure-policy-introduction.md).
