---
title: Programaticamente criar subscrições do Azure Enterprise | Microsoft Docs
description: Saiba como criar subscrições Azure Enterprise ou Enterprise programador/teste adicionais através de programação.
services: azure-resource-manager
author: jlian
manager: jlian
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/18/2018
ms.author: jlian
ms.openlocfilehash: 8d495bf89697a5e14ff79953ab98f241ef8972e8
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programaticamente criar subscrições Azure Enterprise (pré-visualização)

Como um cliente do Azure [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), pode criar EA (MS-AZR - 0017 P) e as subscrições de EA Dev/teste (MS-AZR - 0148 P) através de programação. Para conceder a outro utilizador ou principal de serviço a permissão para criar subscrições cobradas à sua conta, dar-lhes [controlo de acesso baseado em funções (RBAC)](../active-directory/role-based-access-control-configure.md) acesso à sua conta de inscrição. 

> [!IMPORTANT]
> As subscrições do Azure criadas através desta API são regidas pelo contrato abrigo do qual adquiriu serviços do Microsoft Azure da Microsoft ou de um revendedor autorizado. Para obter mais informações, consulte [Informação Legal do Microsoft Azure](https://azure.microsoft.com/support/legal/).

Neste artigo, irá:

> [!div class="checklist"]
> * Saiba como criar subscrições através de programação com o Azure Resource Manager
> * Compreender como utilizar o RBAC para partilhar a capacidade de criar subscrições cobradas à sua conta EA

Além disso, consulte o [código de exemplo .NET no GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).

## <a name="ask-your-ea-enrollment-admin-to-add-you-as-account-owner"></a>Peça ao seu administrador de inscrição EA para adicionar, como proprietário da conta

Para começar, peça ao seu administrador de inscrição para [adicioná-o como um proprietário da conta na através do portal EA](https://ea.azure.com/helpdocs/addNewAccount) (início de sessão necessário). Siga as instruções no e-mail de convite que receberá para criar manualmente uma subscrição inicial.

> [!IMPORTANT]
> Tem de confirmar a propriedade de conta e crie manualmente uma subscrição de EA inicial antes de avançar para o passo seguinte. Apenas a adição da conta para a inscrição não é suficiente.

## <a name="find-accounts-you-have-access-to"></a>Localizar as contas que tem acesso

Depois da adicionado para uma inscrição EA do Azure como um proprietário da conta, o Azure utiliza a relação de inscrição de contas para determinar onde bem ao faturar os encargos de subscrição. Para criar subscrições, primeiro descobrir que contas de inscrição que tem acesso. Se tiver atualmente um proprietário da conta EA e tentar utilizar esta API, o Azure verifica as seguintes condições:

- A conta foi adicionada uma inscrição EA
- Tiver uma ou mais subscrições EA ou EA Dev/teste, que significa que já passou pelo manual inscrição, pelo menos, uma vez
- Tem sessão iniciada para o proprietário da conta *diretório raiz*, que é o diretório de subscrições são criadas por predefinição

Se forem satisfeitas as duas condições acima, uma `enrollmentAccount` recurso é devolvido e pode começar a criar subscrições com essa conta. Todas as subscrições criadas sob a conta são cobradas para a inscrição de EA que a conta está no.

# <a name="resttabrest"></a>[REST](#tab/rest)

O pedido para listar todas as contas de inscrição:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure responde com uma lista de todas as contas de inscrição que tem acesso ao:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileOnboardingEng@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileBackendEng@contoso.com"
      }
    }
  ]
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Utilize o [comando Get-AzureRmEnrollmentAccount](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) para listar todas as contas de inscrição que tem acesso.

```azurepowershell-interactive
Get-AzureRmEnrollmentAccount
```

Azure responde com uma lista dos endereços de e-mail e os IDs de objeto de contas.

```azurepowershell
ObjectId                               | PrincipalName
<enrollmentAccountId>   | MobileOnboardingEng@contoso.com
<enrollmentAccountId>   | MobileBackendEng@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Utilize o [lista de contas de inscrição de faturação de az](https://aka.ms/EASubCreationPublicPreviewCLI) comando para listar todas as contas de inscrição que tem acesso.

```azurecli-interactive 
az billing enrollment-account list
```
Azure responde com uma lista dos endereços de e-mail e os IDs de objeto de contas.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileOnboardingEng@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileBackendEng@contoso.com"
      }
    }
  ]
}
```

---

Utilize o `principalName` propriedade para identificar a conta que pretende que as subscrições para ser-lhe cobrado. Utilize o `id` como o `enrollmentAccount` valor que é utilizado para criar a subscrição no próximo passo.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Criar subscrições com uma conta de inscrição específicos 

O exemplo seguinte cria um pedido para criar subscrição com o nome *subscrição da equipa de desenvolvimento* e oferta da subscrição é *MS-AZR - 0017P* (regular EA). A conta de inscrição é `<enrollmentAccountId>`, que é a conta de inscrição para MobileOnboardingEng@contoso.com. Opcionalmente também adiciona dois utilizadores como proprietários do RBAC para a subscrição.

# <a name="resttabrest"></a>[REST](#tab/rest)

Utilize o `id` do `enrollmentAccount` no caminho do pedido para criar subscrição.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Não      | Cadeia | O nome a apresentar da subscrição. Se não for especificado, está definido para o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `offerType`   | Sim      | Cadeia | A oferta da subscrição. Existem duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização em produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/teste, tem de ser [ativada através do portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Não       | Cadeia | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado.  |

Na resposta, regressar um `subscriptionOperation` objeto para monitorização. Quando a criação da subscrição está concluída, o `subscriptionOperation` objeto iria devolver um `subscriptionLink` objeto, que tem o ID da subscrição.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Para utilizar este módulo de pré-visualização, instalá-lo executando `Install-Module AzureRM.Subscription -AllowPrerelease` primeiro. Para se certificar `-AllowPrerelease` funciona, instalar uma versão recente do PowerShellGet de [módulo PowerShellGet Get](/powershell/gallery/psget/get_psget_module).

Utilize o [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview) juntamente com `enrollmentAccount` nome como o `EnrollmentAccountObjectId` parâmetro ao criar uma nova subscrição. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountId> -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Não      | Cadeia | O nome a apresentar da subscrição. Se não for especificado, está definido para o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `OfferType`   | Sim      | Cadeia | A oferta da subscrição. Existem duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização em produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/teste, tem de ser [ativada através do portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `OwnerObjectId`      | Não       | Cadeia | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado.  |
| `OwnerSignInName`    | Não       | Cadeia | O endereço de e-mail de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `OwnerObjectId`.|
| `OwnerApplicationId` | Não       | Cadeia | O ID de quaisquer principal de serviço que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `OwnerObjectId`.| 

Para ver uma lista completa de todos os parâmetros, consulte [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Para utilizar esta extensão de pré-visualização, instalá-lo executando `az extension add --name subscription` primeiro.

Utilize o [criar conta az](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) juntamente com `enrollmentAccount` nome como o `enrollment_account_name` parâmetro ao criar uma nova subscrição.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-name "<enrollmentAccountId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Não      | Cadeia | O nome a apresentar da subscrição. Se não for especificado, está definido para o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `offer-type`   | Sim      | Cadeia | A oferta da subscrição. Existem duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização em produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/teste, tem de ser [ativada através do portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owner-object-id`      | Não       | Cadeia | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado.  |
| `owner-upn`    | Não       | Cadeia | O endereço de e-mail de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `owner-object-id`.|
| `owner-spn` | Não       | Cadeia | O ID de quaisquer principal de serviço que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `owner-object-id`.| 

Para ver uma lista completa de todos os parâmetros, consulte [criar conta az](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Delegar o acesso a uma conta de inscrição através do RBAC

Para conceder a outro utilizador ou principal de serviço a capacidade de criar subscrições de uma conta específica, [conceder-lhes uma função de proprietário do RBAC no âmbito da conta inscrição](../active-directory/role-based-access-control-manage-access-rest.md). O exemplo seguinte fornece um utilizador no inquilino com `principalId` de `<userObjectId>` (para MobileOnboardingEng@contoso.com) uma função de proprietário da conta de inscrição. 

# <a name="resttabrest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

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
    "scope": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
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
New-AzureRmRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Utilize o [de criação da atribuição de função az](../active-directory/role-based-access-control-manage-access-azure-cli.md) para dar acesso de proprietário de outro utilizador à sua conta de inscrição.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>
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

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Limitações da criação da subscrição do Azure Enterprise API

- Apenas as subscrições do Azure Enterprise podem ser criadas utilizando esta API.
- Não há um limite de 50 subscrições por conta. Depois disso, as subscrições só podem ser criadas utilizando o Centro de contas.
- Tem de haver, pelo menos, uma EA ou EA Dev/teste subscrições com a conta, o que significa que o proprietário da conta tornou-se através de inscrição manual, pelo menos, uma vez.
- Os utilizadores que não são proprietários de conta, mas foram adicionados a uma conta de inscrição através do RBAC, não é possível criar subscrições através do Centro de contas.
- Não é possível selecionar o inquilino da subscrição a serem criadas. A subscrição é sempre criada no inquilino do proprietário da conta raiz. Para mover a subscrição para um inquilino diferente, consulte [alterar o inquilino da subscrição](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Passos Seguintes

* Para obter um exemplo sobre como criar subscrições através do .NET, consulte [exemplo de código no GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Para saber mais sobre o Azure Resource Manager e das respetivas APIs, consulte o artigo [descrição geral do Azure Resource Manager](resource-group-overview.md).
* Para saber mais sobre a gestão de grandes quantidades de subscrições através de grupos de gestão, consulte [organizar os recursos com grupos de gestão do Azure](management-groups-overview.md)
* Para ver uma abrangente melhor prática documentação de orientação para organizações grandes no setor governativo de subscrição, consulte [andaime enterprise do Azure - governação prescritiva subscrição](resource-manager-subscription-governance.md)
