---
title: Início Rápido - Utilizar o PowerShell para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | Microsoft Docs
description: Neste manual de início rápido, utiliza o PowerShell para criar uma atribuição de Azure Policy para identificar recursos incompatíveis.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/10/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: f39177f5a2e5878570ed750a42009c2203257b13
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-using-the-azure-rm-powershell-module"></a>Início rápido: criar uma atribuição de política para identificar recursos incompatíveis com o módulo Azure RM PowerShell

O primeiro passo para compreender a conformidade no Azure consiste em identificar o estado dos seus recursos. Neste manual de início rápido, o utilizador cria uma atribuição de política para identificar máquinas virtuais que não utilizam discos geridos. Quando terminar, vai poder identificar máquinas virtuais *incompatíveis* com a atribuição de política.

O módulo AzureRM PowerShell é utilizado para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este guia explica como utilizar o AzureRM para criar uma atribuição de política. A política identifica is recursos incompatíveis no seu ambiente do Azure.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

- Antes de começar, certifique-se de que a versão mais recente do PowerShell está instalada. Veja [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para informações detalhadas.
- Atualize o módulo do AzureRM PowerShell para a versão mais recente. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).
- Utilize o Azure PowerShell para registar o fornecedor de recursos do Policy Insights. Registar o fornecedor de recursos assegura que a sua subscrição funciona com o mesmo. Para registar um fornecedor de recursos, tem de ter permissão para efetuar a operação de ação de registo para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário. Execute o seguinte comando para registar o fornecedor de recursos:

  ```
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
  ```

  Para obter mais informações sobre como registar e ver os fornecedores de recursos, veja [Resource Providers and Types](../azure-resource-manager/resource-manager-supported-services.md) (Fornecedores e Tipos de Recursos)

## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste guia de introdução vai criar uma atribuição de política e atribuir as *Máquinas Virtuais de Auditoria sem uma definição de Managed Disks*. Esta definição de política identifica os recursos que não estão em conformidade com as condições especificadas na definição de política.

Execute os seguintes comandos para criar uma nova atribuição de política:

```powershell
$rg = Get-AzureRmResourceGroup -Name "<resourceGroupName>"
$definition = Get-AzureRmPolicyDefinition | where {$_.properties.displayName -eq "Audit VMs that do not use managed disks"}
New-AzureRMPolicyAssignment -Name "Audit Virtual Machines without Managed Disks" -Scope $rg.ResourceId -PolicyDefinition $definition -Sku @{Name='A1';Tier='Standard'}
```

Os comandos anteriores utilizam as seguintes informações:

- **Nome** - nome a apresentar da atribuição de política. Neste caso, está a utilizar *Máquinas Virtuais de Auditoria sem Atribuição de Managed Disks*.
- **Definição** – A definição de política, com base na qual está a utilizar para criar a atribuição. Neste caso, é a definição de política – *Máquinas Virtuais de Auditoria sem Managed Disks*.
- **Âmbito** – Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos. Não se esqueça de substituir &lt;âmbito&gt; pelo nome do seu grupo de recursos.
- **Sku** – Este comando cria uma atribuição de política com o escalão standard. O escalão standard permite-lhe alcançar a gestão à escala, a avaliação de conformidade e a remediação. Para obter detalhes adicionais sobre os escalões de preço, veja [Preços do Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy).


Agora, está pronto para identificar recursos incompatíveis para compreender o estado de compatibilidade do seu ambiente.

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Utilize as seguintes informações para identificar recursos que não estão em conformidade com a atribuição de política que criou. Execute os seguintes comandos:

```powershell
$policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Virtual Machines without Managed Disks"}
```

```powershell
$policyAssignment.PolicyAssignmentId
```

Para obter mais informações sobre IDs de atribuição de política, veja [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Em seguida, execute o seguinte comando para obter os ID dos recursos que não estão em conformidade produzidos num ficheiro de JSON:

```powershell
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```
Os resultados assemelham-se ao seguinte exemplo:


```
{
"@odata.context":"https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
"@odata.count": 3,
"value": [
{
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

Os outros guias desta coleção baseiam-se neste manual de início rápido. Se pretende continuar a trabalhar com outros tutoriais, não limpe os recursos criados neste manual de início rápido. Se não pretende continuar, pode eliminar a atribuição de que criou ao executar este comando:

```powershell
Remove-AzureRmPolicyAssignment -Name "Audit Virtual Machines without Managed Disks Assignment" -Scope /subscriptions/<subscriptionID>/<resourceGroupName>
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, atribuiu uma definição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Para saber mais sobre a atribuição de políticas e certifique-se de que os **futuros** recursos criados estão em conformidade, avance para o tutorial para:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](./create-manage-policy.md)
