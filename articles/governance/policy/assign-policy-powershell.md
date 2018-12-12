---
title: Início Rápido - Utilizar o PowerShell para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure
description: Neste manual de início rápido, utiliza o PowerShell para criar uma atribuição de Azure Policy para identificar recursos incompatíveis.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 79e192bae8a1d9ac3e27f35dc2f64befc5a75a84
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078821"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-using-the-azure-rm-powershell-module"></a>Início rápido: criar uma atribuição de política para identificar recursos incompatíveis com o módulo Azure RM PowerShell

O primeiro passo para compreender a conformidade no Azure consiste em identificar o estado dos seus recursos. Neste início rápido, vai criar uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos. Quando terminar, vai poder identificar máquinas virtuais *incompatíveis* com a atribuição de política.

O módulo AzureRM PowerShell é utilizado para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este guia explica como utilizar o AzureRM para criar uma atribuição de política. A política identifica is recursos incompatíveis no seu ambiente do Azure.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

- Se ainda não o fez, instale o [ARMClient](https://github.com/projectkudu/ARMClient). É uma ferramenta que envia pedidos de HTTP para APIs baseadas no Azure Resource Manager.
- Antes de começar, certifique-se de que a versão mais recente do PowerShell está instalada. Veja [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para informações detalhadas.
- Atualize o módulo do AzureRM PowerShell para a versão mais recente. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).
- Utilize o Azure PowerShell para registar o fornecedor de recursos do Policy Insights. Registar o fornecedor de recursos assegura que a sua subscrição funciona com o mesmo. Para registar um fornecedor de recursos, tem de ter permissão para a operação de fornecedor de recursos de registo. Esta operação está incluída nas funções de Contribuinte e Proprietário. Execute o seguinte comando para registar o fornecedor de recursos:

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Para obter mais informações sobre como registar e ver os fornecedores de recursos, veja [Resource Providers and Types](../../azure-resource-manager/resource-manager-supported-services.md) (Fornecedores e Tipos de Recursos)

## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste início rápido, pode criar uma atribuição de política e atribuir a *VMs de auditoria sem discos geridos* definição. Esta definição de política identifica os recursos que não estão em conformidade para as condições definidas na definição de política.

Execute os seguintes comandos para criar uma nova atribuição de política:

```azurepowershell-interactive
$rg = Get-AzureRmResourceGroup -Name '<resourceGroupName>'
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs that do not use managed disks' }
New-AzureRmPolicyAssignment -Name 'audit-vm-manageddisks' -DisplayName 'Audit VMs without managed disks Assignment' -Scope $rg.ResourceId -PolicyDefinition $definition
```

Os comandos anteriores utilizam as seguintes informações:

- **Nome** – O nome real da atribuição.  Neste exemplo, foi utilizado *audit-vm-manageddisks*.
- **DisplayName** – O nome da atribuição de política a apresentar. Neste caso, está usando *VMs de auditoria sem discos geridos atribuição*.
- **Definição** – A definição de política, com base na qual está a utilizar para criar a atribuição. Neste caso, é o ID de definição de política *VMs de auditoria que não utilizam discos geridos*.
- **Âmbito** – Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos. Não se esqueça de substituir &lt;âmbito&gt; pelo nome do seu grupo de recursos.

Agora, está pronto para identificar recursos incompatíveis para compreender o estado de compatibilidade do seu ambiente.

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Utilize as seguintes informações para identificar recursos que não estão em conformidade com a atribuição de política que criou. Execute os seguintes comandos:

```azurepowershell-interactive
$policyAssignment = Get-AzureRmPolicyAssignment | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs without managed disks Assignment' }
$policyAssignment.PolicyAssignmentId
```

Para obter mais informações sobre IDs de atribuição de política, veja [Get-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Em seguida, execute o seguinte comando para obter os ID dos recursos que não estão em conformidade produzidos num ficheiro de JSON:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Os resultados assemelham-se ao seguinte exemplo:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
        }

    ]
}
```

Os resultados são comparáveis aos que normalmente vê listados em **recursos que não estão em conformidade** na vista do portal do Azure.

## <a name="clean-up-resources"></a>Limpar recursos

Para remover a atribuição de criado, utilize o seguinte comando:

```azurepowershell-interactive
Remove-AzureRmPolicyAssignment -Name 'audit-vm-manageddisks' -Scope '/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>'
```

## <a name="next-steps"></a>Passos Seguintes

Neste guia de introdução, atribuiu uma definição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Para saber mais sobre a atribuição de políticas para validar que os novos recursos estão em conformidade, avance para o tutorial para:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](./tutorials/create-and-manage.md)