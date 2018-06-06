---
title: Programaticamente criar políticas e ver os dados de conformidade com a política do Azure
description: Este artigo explica-lhe como criar e gerir políticas para a política do Azure programaticamente.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: a83402316854b23fe85bff813dc9f5665bccd1fb
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794815"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Programaticamente criar políticas e ver os dados de conformidade

Este artigo explica-lhe como criar e gerir políticas programaticamente. Também mostra como ver Estados de compatibilidade de recursos e as políticas. Definições de política impor regras diferentes e efeitos sobre os recursos. Imposição certifica-se de que os recursos se manter em conformidade com os padrões empresariais e contratos de nível de serviço.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que são cumpridos os seguintes pré-requisitos:

1. Se ainda não o fez, instale o [ARMClient](https://github.com/projectkudu/ARMClient). É uma ferramenta que envia pedidos de HTTP para APIs do Azure Resource Manager.
2. Atualize o módulo do AzureRM PowerShell para a versão mais recente. Para mais informações sobre a versão mais recente, consulte [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).
3. Registe o fornecedor de recursos de informações de política com o Azure PowerShell para se certificar de que a sua subscrição funciona com o fornecedor de recursos. Para registar um fornecedor de recursos, tem de ter permissão para efetuar a operação de ação de registo para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário. Execute o seguinte comando para registar o fornecedor de recursos:

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Para obter mais informações sobre como registar e visualização de fornecedores de recursos, consulte [fornecedores de recursos e tipos](../azure-resource-manager/resource-manager-supported-services.md).
4. Se ainda não o fez, instale o CLI do Azure. Pode obter a versão mais recente em [instalar o 2.0 CLI do Azure no Windows](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Criar e atribuir uma definição de política

É o primeiro passo para uma maior visibilidade dos seus recursos para criar e atribuir políticas sobre os recursos. O passo seguinte é para saber como criar e atribuir uma política programaticamente. A política de exemplo auditorias de contas de armazenamento que estão abertas para todas as redes públicas, através do PowerShell, CLI do Azure e pedidos de HTTP.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Criar e atribuir uma definição de política com o PowerShell

1. Utilize o fragmento JSON seguinte para criar um ficheiro JSON com o nome AuditStorageAccounts.json.

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

  Para obter mais informações sobre a criação de uma definição de política, consulte [estrutura de definição de política do Azure](policy-definition.md).
2. Execute o seguinte comando para criar uma definição de política através do ficheiro AuditStorageAccounts.json.

  ```azurepowershell-interactive
  New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
  ```

  O comando cria uma definição de política com o nome _auditoria armazenamento contas aberto para redes públicas_. Para mais informações sobre outros parâmetros que pode utilizar, consulte [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition).
3. Depois de criar a definição de política, pode criar uma atribuição de política, executando os seguintes comandos:

  ```azurepowershell-interactive
  $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
  $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
  New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
  ```

  Substitua _ContosoRG_ com o nome do grupo de recursos pretendidos.

Para obter mais informações sobre a gestão de políticas de recursos utilizando o módulo PowerShell do Azure Resource Manager, consulte [AzureRM.Resources](/powershell/module/azurerm.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Criar e atribuir uma definição de política utilizar ARMClient

Utilize o procedimento seguinte para criar uma definição de política.

1. Copie o fragmento JSON seguinte para criar um ficheiro JSON. Irá chamar o ficheiro no próximo passo.

  ```json
  "properties": {
      "displayName": "Audit Storage Accounts Open to Public Networks",
      "policyType": "Custom",
      "mode": "Indexed",
      "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
      "parameters": {},
      "policyRule": {
          "if": {
              "allOf": [{
                      "field": "type",
                      "equals": "Microsoft.Storage/storageAccounts"
                  },
                  {
                      "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                      "equals": "Allow"
                  }
              ]
          },
          "then": {
              "effect": "audit"
          }
      }
  }
  ```

2. Crie a definição de política através de um das chamadas seguintes:

  ```
  # For defining a policy in a subscription
  armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

  # For defining a policy in a management group
  armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
  ```

  Substitui o anterior a {subscriptionId} com o ID da sua subscrição ou o {managementGroupId} com o ID do seu [grupo de gestão](../azure-resource-manager/management-groups-overview.md).

  Para obter mais informações sobre a estrutura da consulta, consulte [definições de política – criar ou atualizar](/rest/api/resources/policydefinitions/createorupdate) e [definições de política – criar ou a atualização no grupo de gestão](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Utilize o procedimento seguinte para criar uma atribuição de política e atribuir a definição de política ao nível do grupo de recursos.

1. Copie o fragmento JSON seguinte para criar um ficheiro de atribuição de política JSON. Substituir as informações de exemplo no &lt; &gt; símbolos com os seus próprios valores.

  ```json
  {
      "properties": {
          "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
          "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
          "parameters": {},
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
          "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
      }
  }
  ```

2. Crie a atribuição de política utilizando a seguinte chamada:

  ```
  armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
  ```

  Substituir as informações de exemplo no &lt; &gt; símbolos com os seus próprios valores.

  Para obter mais informações sobre como efetuar chamadas HTTP para a API REST, consulte [recursos de API de REST de Azure](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Criar e atribuir uma definição de política com a CLI do Azure

Para criar uma definição de política, utilize o seguinte procedimento:

1. Copie o fragmento JSON seguinte para criar um ficheiro de atribuição de política JSON.

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

2. Execute o seguinte comando para criar uma definição de política:

  ```azurecli-interactive
az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
  ```

3. Utilize o seguinte comando para criar uma atribuição de política. Substituir as informações de exemplo no &lt; &gt; símbolos com os seus próprios valores.

  ```azurecli-interactive
  az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
  ```

Pode obter o ID de definição de política com o PowerShell com o seguinte comando:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

O ID de definição de política para a definição de política que criou deve assemelhar-se ao seguinte exemplo:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Para obter mais informações sobre como pode gerir as políticas de recursos com a CLI do Azure, consulte [as políticas de recursos do Azure CLI](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Passos Seguintes

Reveja os artigos seguintes para obter mais informações sobre os comandos e consultas neste artigo.

- [Recursos de API de REST do Azure](/rest/api/resources/)
- [Módulos do PowerShell do Azure RM](/powershell/module/azurerm.resources/#policies)
- [Comandos de política da CLI do Azure](/cli/azure/policy?view=azure-cli-latest)
- [Fornecedor de recursos de informações de política referência da REST API](/rest/api/policy-insights)
- [Organizar os recursos com grupos de gestão do Azure](../azure-resource-manager/management-groups-overview.md)