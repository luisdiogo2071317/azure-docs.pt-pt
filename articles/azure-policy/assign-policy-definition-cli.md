---
title: Utilizar a CLI do Azure para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | Microsoft Docs
description: Utilize o PowerShell para criar uma atribuição de política do Azure para identificar recursos incompatíveis.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/30/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 356521c0d1771906e41c816d523d687fe8902acd
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure com a CLI do Azure

O primeiro passo para compreender a conformidade no Azure consiste em identificar o estado dos seus recursos. Este início rápido acompanha-o ao longo do processo de criação de uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos.

No final deste processo, vai identificar com êxito as máquinas virtuais que não estão a utilizar discos geridos. Estão em *não conformidade* com a atribuição de política.

A CLI do Azure é utilizada para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este guia utiliza a CLI do Azure para criar uma atribuição de política para identificar recursos que não estão em conformidade no seu ambiente do Azure.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

O início rápido requer que execute a versão 2.0.4 da CLI do Azure ou posterior para instalar a utilizar a CLI local. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Pré-requisitos

Utilize a CLI do Azure para registar o fornecedor de recursos do Policy Insights. Registar o fornecedor de recursos assegura que a sua subscrição funciona com o mesmo. Para registar um fornecedor de recursos, tem de ter permissão para efetuar a operação de ação de registo para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário. Execute o seguinte comando para registar o fornecedor de recursos:

```
az provider register –-namespace 'Microsoft.PolicyInsights
```
Para obter mais informações sobre como registar e ver os fornecedores de recursos, veja [Resource Providers and Types](../azure-resource-manager/resource-manager-supported-services.md) (Fornecedores e Tipos de Recursos)


## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste início rápido, vai criar uma atribuição de política e atribuir as Máquinas Virtuais de Auditoria sem uma definição de Managed Disks. Esta definição de política identifica os recursos que não estão em conformidade com as condições especificadas na definição de política.

Execute o seguinte comando para criar uma atribuição de política:

```
az policy assignment create --name 'Audit Virtual Machines without Managed Disks Assignment' --scope '<scope>' --policy '<policy definition ID>' --sku 'standard'
```

O comando anterior utiliza as seguintes informações:

- **Nome** - nome a apresentar da atribuição de política. Neste caso, está a utilizar *Máquinas Virtuais de Auditoria sem Atribuição de Managed Disks*.
- **Política** – O ID de definição de política, com base no qual está a utilizar para criar a atribuição. Neste caso, é a definição de política – *Máquinas Virtuais de Auditoria sem Managed Disks*. Para obter o ID de definição de política, execute este comando: `az policy definition show --name 'Audit Virtual Machines without Managed Disks Assignment'`
- **Âmbito** – Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos. Não se esqueça de substituir &lt;âmbito&gt; pelo nome do seu grupo de recursos.
- **Sku** – Este comando cria uma atribuição de política com o escalão standard. O escalão standard permite-lhe alcançar a gestão à escala, a avaliação de conformidade e a remediação. Para obter detalhes adicionais sobre os escalões de preço, veja [Preços do Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy).


## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Para ver os recursos que não estão em conformidade com esta nova atribuição, obtenha o ID de atribuição de política ao executar os seguintes comandos:

```
$policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Virtual Machines without Managed Disks"}
```

```
$policyAssignment.PolicyAssignmentId
```

Para obter mais informações sobre IDs de atribuição de política, veja [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Em seguida, execute o seguinte comando para obter os ID dos recursos que não estão em conformidade produzidos num ficheiro de JSON:

```
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

Outros guias desta coleção têm por base este guia de introdução. Se quiser continuar a trabalhar com os tutoriais seguintes, não limpe os recursos criados neste início rápido. Se não quiser continuar, elimine a atribuição que criou ao executar este comando:

```azurecli
az policy assignment delete –name Audit Virtual Machines without Managed Disks Assignment --scope /subscriptions/ <subscriptionID> / <resourceGroupName>
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, atribuiu uma definição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Para saber mais sobre a atribuição de políticas e assegurar que os recursos que cria no **futuro** estão em conformidade, avance para o tutorial:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](./create-manage-policy.md)
