---
title: Programaticamente criar políticas e ver os dados de conformidade com a política do Azure
description: Este artigo explica-lhe como criar e gerir políticas para a política do Azure programaticamente.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-policy
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 5737c33fc4c139e3b0a5535d371ef7cc1d11b9e6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
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

2. Crie a definição de política utilizando a seguinte chamada:

  ```
  armclient PUT "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
  ```

  Substitua precedente &lt;subscriptionId&gt; com o ID da sua subscrição pretendido.

Para obter mais informações sobre a estrutura da consulta, consulte [definições de política – criar ou atualizar](/rest/api/resources/policydefinitions/createorupdate).

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

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Numa atribuição, um recurso não é compatível caso não cumpra política ou iniciativa regras. A tabela seguinte mostra como outra política efeitos trabalham com a avaliação de condição para o estado de conformidade resultante:

| Estado do recurso | Efeito | Avaliação da política | Estado de compatibilidade |
| --- | --- | --- | --- |
| Existe | Negar, Auditar, Acrescentar\*, DeployIfNotExist\*, AuditIfNotExist\* | Verdadeiro | Em Não Conformidade |
| Existe | Negar, Auditar, Acrescentar\*, DeployIfNotExist\*, AuditIfNotExist\* | Falso | Compatível |
| Novo | Audit, AuditIfNotExist\* | Verdadeiro | Em Não Conformidade |
| Novo | Audit, AuditIfNotExist\* | Falso | Compatível |

\* Os efeitos de acréscimo, DeployIfNotExist e AuditIfNotExist requerem a instrução caso seja verdadeiro. Os efeitos também precisam que a condição de existência para ser falso para estar em não conformidade. Quando for TRUE, a condição IF aciona a avaliação da condição de existência dos recursos relacionados.

Para compreender melhor como os recursos são sinalizados como não conformes, vamos utilizar o exemplo de atribuição de política criado acima.

Por exemplo, suponha que tem um grupo de recursos – ContsoRG, com algumas contas de armazenamento (realçadas a vermelho) que estão expostas a redes públicas.

![Contas do Storage expostas a redes públicas](media/policy-insights/resource-group01.png)

Neste exemplo, tem de ser wary de riscos de segurança. Agora que criou a atribuição de política, esta é avaliada para todas as contas de armazenamento no grupo de recursos de ContosoRG. Auditorias das três contas de armazenamento não conformes, por conseguinte, alterar os respetivos Estados para **incompatíveis.**

![Auditar as contas de armazenamento não conformes](media/policy-insights/resource-group03.png)

Utilize o procedimento seguinte para identificar recursos num grupo de recursos que não estão em conformidade com a atribuição de política. No exemplo, os recursos são contas de armazenamento no grupo de recursos de ContosoRG.

1. Obter o ID de atribuição de política, executando os seguintes comandos:

  ```azurepowershell-interactive
  $policyAssignment = Get-AzureRmPolicyAssignment | Where-Object { $_.Properties.displayName -eq 'Audit Storage Accounts with Open Public Networks' }
  $policyAssignment.PolicyAssignmentId
  ```

  Para obter mais informações sobre como obter o ID de uma atribuição de política, consulte [Get-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/Get-AzureRmPolicyAssignment).

2. Execute o seguinte comando para que os IDs de recurso dos recursos incompatíveis copiados para um ficheiro JSON:

  ```
  armclient POST "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
  ```

3. Os resultados devem assemelhar-se ao seguinte exemplo:

  ```json
  {
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
      "@odata.count": 3,
      "value": [{
              "@odata.id": null,
              "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
              "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount1Id>"
          },
          {
              "@odata.id": null,
              "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
              "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount2Id>"
          },
          {
              "@odata.id": null,
              "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
              "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount3ID>"
          }
      ]
  }
  ```

Os resultados são equivalentes para que, normalmente, verá listados na **recursos não compatível** no [vista do portal do Azure](assign-policy-definition.md#identify-non-compliant-resources).

Atualmente, recursos incompatíveis são apenas identificados através do portal do Azure e com pedidos de HTTP. Para obter mais informações sobre consulta de Estados de política, consulte o [política estado](/rest/api/policy-insights/policystates) artigo de referência de API.

## <a name="view-policy-events"></a>Ver eventos de política

Quando um recurso é criado ou atualizado, é gerado um resultado de avaliação da política. Os resultados são denominados _eventos política_. Execute a seguinte consulta para ver todos os eventos de política associados a atribuição de política.

```
armclient POST "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2017-12-12-preview"
```

Os resultados assemelham-se ao seguinte exemplo:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 3
    }]
}
```

Como Estados de política, só pode ver eventos de política com pedidos de HTTP. Para obter mais informações sobre consulta de eventos de política, consulte o [política eventos](/rest/api/policy-insights/policyevents) artigo de referência.

## <a name="next-steps"></a>Passos Seguintes

Reveja os artigos seguintes para obter mais informações sobre os comandos e consultas neste artigo.

- [Recursos de API de REST do Azure](/rest/api/resources/)
- [Módulos do PowerShell do Azure RM](/powershell/module/azurerm.resources/#policies)
- [Comandos de política da CLI do Azure](/cli/azure/policy?view=azure-cli-latest)
- [Fornecedor de recursos de informações de política referência da REST API](/rest/api/policy-insights)