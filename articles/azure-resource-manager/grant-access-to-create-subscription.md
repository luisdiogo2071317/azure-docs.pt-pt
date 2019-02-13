---
title: Conceder acesso de criar subscrições do Azure Enterprise | Documentos da Microsoft
description: Saiba como conceder a um utilizador ou principal de serviço a capacidade de criar subscrições do Azure Enterprise programaticamente.
services: azure-resource-manager
author: adpick
manager: adpick
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: adpick
ms.openlocfilehash: 7a2397328f715dbf63246e8d4aaa789b5986b3b4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112568"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Conceder acesso a criar subscrições do Azure Enterprise (pré-visualização)

Como cliente do Azure no [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), pode dar outro utilizador ou serviço principal permissão para criar subscrições faturadas à sua conta. Neste artigo, irá aprender a utilizar [controlo de acesso baseado em funções (RBAC)](../active-directory/role-based-access-control-configure.md) para partilhar a capacidade de criar subscrições e como efetuar a auditoria de criações de subscrição. Tem de ter a função de proprietário da conta que pretende partilhar.

Para criar uma subscrição, veja [programaticamente criar subscrições do Azure Enterprise (pré-visualização)](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Delegar o acesso a uma conta de inscrição através do RBAC

Para dar a capacidade de criar subscrições em relação a uma conta específica, a outro utilizador ou principal de serviço [dar-lhes uma função de proprietário do RBAC no âmbito da conta de inscrição](../active-directory/role-based-access-control-manage-access-rest.md). O exemplo a seguir fornece um utilizador no inquilino com `principalId` dos `<userObjectId>` (para SignUpEngineering@contoso.com) uma função de proprietário da conta de inscrição. Para localizar a conta de inscrição de ID e o ID de principal, consulte [programaticamente criar subscrições do Azure Enterprise (pré-visualização)](programmatically-create-subscription.md).

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

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Utilize o [New-AzRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) para conceder acesso de proprietário de outro utilizador à sua conta de inscrição.

```azurepowershell-interactive
New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Utilize o [criação da atribuição de função de az](../active-directory/role-based-access-control-manage-access-azure-cli.md) para conceder acesso de proprietário de outro utilizador à sua conta de inscrição.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Assim que um utilizador for um proprietário do RBAC para a sua conta de inscrição, pode criar programaticamente subscrições sob a mesma. Uma subscrição criada pelo utilizador delegado ainda tem o proprietário original da conta Administrador de serviços, mas ela também tem o utilizador delegado como um proprietário por predefinição. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Auditoria que criou as subscrições com os registos de atividade

Para controlar as subscrições criadas através desta API, utilize o [API do Log de atividade de inquilino](/rest/api/monitor/tenantactivitylogs). Atualmente não é possível utilizar o PowerShell, CLI ou o portal do Azure para controlar a criação da subscrição.

1. Como administrador do inquilino do Azure AD, [eleve o acesso](../active-directory/role-based-access-control-tenant-admin-access.md) e, em seguida, atribua uma função de Leitor ao utilizador de auditoria no âmbito `/providers/microsoft.insights/eventtypes/management`.
1. Como o utilizador de auditoria, chamar o [API do Log de atividade de inquilino](/rest/api/monitor/tenantactivitylogs) para ver as atividades de criação de subscrição. Exemplo:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Para chamar convenientemente esta API a partir da linha de comandos, experimente [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Passos Seguintes

* Agora que o utilizador ou principal de serviço tem permissão para criar uma subscrição, pode utilizar essa identidade para [programaticamente criar subscrições do Azure Enterprise](programmatically-create-subscription.md).
* Para obter um exemplo sobre como criar subscrições através do .NET, consulte [código no GitHub de exemplo](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Para saber mais sobre o Azure Resource Manager e das respetivas APIs, veja [descrição geral do Azure Resource Manager](resource-group-overview.md).
* Para saber mais sobre como gerir um grande número de subscrições com grupos de gestão, veja [organizar os recursos com grupos de gestão do Azure](management-groups-overview.md)
* Para ver uma abrangente diretrizes de práticas recomendadas para grandes organizações na governação de subscrições, veja [scaffold enterprise do Azure - governação de subscrições prescritiva](/azure/architecture/cloud-adoption-guide/subscription-governance)
