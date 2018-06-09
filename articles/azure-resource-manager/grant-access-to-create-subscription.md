---
title: Conceder acesso ao criar as subscrições Azure empresariais | Microsoft Docs
description: Saiba como conceder um utilizador ou principal de serviço a capacidade para programaticamente criar subscrições do Azure Enterprise.
services: azure-resource-manager
author: jlian
manager: jlian
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: jlian
ms.openlocfilehash: 4c5d505f431ef684b73adc04629464883d336a5b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238288"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Conceder acesso ao criar subscrições Azure Enterprise (pré-visualização)

Como um cliente do Azure [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), pode dar outro utilizador ou serviço principal permissão para criar subscrições cobradas à sua conta. Neste artigo, irá aprender a utilizar [controlo de acesso baseado em funções (RBAC)](../active-directory/role-based-access-control-configure.md) para partilhar a capacidade de criar subscrições e como efetuar a auditoria de criações de subscrição.

Para criar uma subscrição, consulte [programaticamente criar subscrições Azure Enterprise (pré-visualização)](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Delegar o acesso a uma conta de inscrição através do RBAC

Para conceder a outro utilizador ou principal de serviço a capacidade de criar subscrições de uma conta específica, [conceder-lhes uma função de proprietário do RBAC no âmbito da conta inscrição](../active-directory/role-based-access-control-manage-access-rest.md). O exemplo seguinte fornece um utilizador no inquilino com `principalId` de `<userObjectId>` (para SignUpEngineering@contoso.com) uma função de proprietário da conta de inscrição. Para localizar a conta de inscrição de ID e o ID de principal, consulte [programaticamente criar subscrições Azure Enterprise (pré-visualização)](programmatically-create-subscription.md).

# <a name="resttabrest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>"
  }
}
```
Quando a função de proprietário é atribuída com êxito no âmbito da conta de inscrição, o Azure responde com as informações de atribuição de função:

```json
{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>",
    "scope": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "createdOn": "2018-03-05T08:36:26.4014813Z",
    "updatedOn": "2018-03-05T08:36:26.4014813Z",
    "createdBy": "<assignerObjectId>",
    "updatedBy": "<assignerObjectId>"
  },
  "id": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "<roleAssignmentGuid>"
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Utilize o [New-AzureRmRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) para dar acesso de proprietário de outro utilizador à sua conta de inscrição.

```azurepowershell-interactive
New-AzureRmRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Utilize o [de criação da atribuição de função az](../active-directory/role-based-access-control-manage-access-azure-cli.md) para dar acesso de proprietário de outro utilizador à sua conta de inscrição.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Depois de um utilizador torna-se um proprietário de RBAC para a sua conta de inscrição, através de programação podem criar subscrições sob a mesma. Uma subscrição criada por um utilizador delegado ainda tem o proprietário da conta original como administrador de serviço, mas ela também tem o utilizador de delegado como um proprietário por predefinição. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Auditoria que criou subscrições através de registos de atividade

Para controlar as subscrições criadas através desta API, utilize o [API do registo de atividade de inquilino](/rest/api/monitor/tenantactivitylogs). Atualmente não é possível utilizar o PowerShell, o CLI ou o portal do Azure para controlar a criação da subscrição.

1. Como um administrador de inquilino do inquilino do Azure AD, [elevar o acesso](../active-directory/role-based-access-control-tenant-admin-access.md) , em seguida, atribuir uma função de leitor ao utilizador auditoria sobre o âmbito `/providers/microsoft.insights/eventtypes/management`.
1. Como o utilizador de auditoria, chame o [API do registo de atividade de inquilino](/rest/api/monitor/tenantactivitylogs) para ver as atividades de criação de subscrição. Exemplo:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Para comodamente chamar esta API na linha de comandos, tente [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Passos Seguintes

* Agora que o utilizador ou principal de serviço tem permissão para criar uma subscrição, pode utilizar essa identidade para [programaticamente criar subscrições do Azure Enterprise](programmatically-create-subscription.md).
* Para obter um exemplo sobre como criar subscrições através do .NET, consulte [exemplo de código no GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Para saber mais sobre o Azure Resource Manager e das respetivas APIs, consulte o artigo [descrição geral do Azure Resource Manager](resource-group-overview.md).
* Para saber mais sobre a gestão de grandes quantidades de subscrições através de grupos de gestão, consulte [organizar os recursos com grupos de gestão do Azure](management-groups-overview.md)
* Para ver uma abrangente melhor prática documentação de orientação para organizações grandes no setor governativo de subscrição, consulte [andaime enterprise do Azure - governação prescritiva subscrição](/azure/architecture/cloud-adoption-guide/subscription-governance)
