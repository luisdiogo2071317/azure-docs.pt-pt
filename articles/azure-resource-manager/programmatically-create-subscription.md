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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: jlian
ms.openlocfilehash: df66ba1ec2c855a24731387210b0127892f5796f
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234789"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programaticamente criar subscrições Azure Enterprise (pré-visualização)

Como um cliente do Azure [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), pode criar EA (MS-AZR - 0017 P) e as subscrições de EA Dev/teste (MS-AZR - 0148 P) através de programação. Neste artigo, irá aprender a criar subscrições através de programação com o Azure Resource Manager.

Quando cria uma subscrição do Azure a partir da API, essa subscrição é regida pelo contrato sob a qual tiver adquirido serviços do Microsoft Azure da Microsoft ou de um revendedor autorizado. Para obter mais informações, consulte [Informação Legal do Microsoft Azure](https://azure.microsoft.com/support/legal/).

## <a name="prerequisites"></a>Pré-requisitos

* A conta tem de ser um proprietário da conta de inscrição de EA do Azure. Se não estiver, peça ao seu administrador de inscrição para [adicioná-o como um proprietário da conta a utilizar o portal EA](https://ea.azure.com/helpdocs/addNewAccount) (início de sessão necessário). Siga as instruções no e-mail de convite que receberá para criar manualmente uma subscrição inicial. Confirmar a propriedade de conta e crie manualmente uma subscrição de EA inicial antes de avançar para o passo seguinte. Acabou de adicionar a conta para a inscrição não é suficiente.

* Se pretender utilizar um principal de serviço para criar a subscrição EA, tem de [conceder esse principal de serviço a capacidade de criar subscrições](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Localizar as contas que tem acesso

Depois da adicionado para uma inscrição EA do Azure como um proprietário da conta, o Azure utiliza a relação de inscrição de contas para determinar onde bem ao faturar os encargos de subscrição. Todas as subscrições criadas sob a conta são cobradas para a inscrição de EA que a conta está no. Para criar subscrições, tem de passar valores sobre a conta de inscrição e os principais de utilizador possuir a subscrição. 

Para executar os comandos seguintes, deve ter sessão iniciada proprietário da conta *diretório raiz*, que é o diretório de subscrições são criadas por predefinição.

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
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Utilize o [comando Get-AzureRmEnrollmentAccount](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) para listar todas as contas de inscrição que tem acesso.

```azurepowershell-interactive
Get-AzureRmEnrollmentAccount
```

Azure responde com uma lista dos endereços de e-mail e os IDs de objeto de contas.

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Utilize o [lista de contas de inscrição de faturação de az](https://aka.ms/EASubCreationPublicPreviewCLI) comando para listar todas as contas de inscrição que tem acesso.

```azurecli-interactive 
az billing enrollment-account list
```

Azure responde com uma lista dos endereços de e-mail e os IDs de objeto de contas.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

---

Utilize o `principalName` propriedade para identificar a conta que pretende que as subscrições para ser-lhe cobrado. Utilize o `id` como o `enrollmentAccount` valor que é utilizado para criar a subscrição no próximo passo.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Criar subscrições com uma conta de inscrição específicos 

O exemplo seguinte cria um pedido para criar subscrição com o nome *subscrição da equipa de desenvolvimento* e oferta da subscrição é *MS-AZR - 0017P* (regular EA). A conta de inscrição é `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (valor do marcador de posição, este valor é um GUID), que é a conta de inscrição para SignUpEngineering@contoso.com. Opcionalmente também adiciona dois utilizadores como proprietários do RBAC para a subscrição.

# <a name="resttabrest"></a>[REST](#tab/rest)

Utilize o `id` do `enrollmentAccount` no caminho do pedido para criar subscrição.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

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

Para utilizar este módulo de pré-visualização, instalá-lo executando `Install-Module AzureRM.Subscription -AllowPrerelease` primeiro. Para se certificar `-AllowPrerelease` funciona, instalar uma versão recente do PowerShellGet de [módulo PowerShellGet Get](/powershell/gallery/installing-psget).

Utilize o [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview) juntamente com `enrollmentAccount` ID como de objeto a `EnrollmentAccountObjectId` parâmetro ao criar uma nova subscrição. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Não      | Cadeia | O nome a apresentar da subscrição. Se não for especificado, está definido para o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `OfferType`   | Sim      | Cadeia | A oferta da subscrição. Existem duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização em produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/teste, tem de ser [ativada através do portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Sim       | Cadeia | O ID de objeto da conta de inscrição que a subscrição é criada em e cobrada para. Este valor é um GUID que obtêm de `Get-AzureRmEnrollmentAccount`. |
| `OwnerObjectId`      | Não       | Cadeia | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado.  |
| `OwnerSignInName`    | Não       | Cadeia | O endereço de e-mail de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `OwnerObjectId`.|
| `OwnerApplicationId` | Não       | Cadeia | O ID de quaisquer principal de serviço que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `OwnerObjectId`.| 

Para ver uma lista completa de todos os parâmetros, consulte [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Para utilizar esta extensão de pré-visualização, instalá-lo executando `az extension add --name subscription` primeiro.

Utilize o [criar conta az](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) juntamente com `enrollmentAccount` ID como de objeto a `enrollment-account-object-id` parâmetro ao criar uma nova subscrição.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Não      | Cadeia | O nome a apresentar da subscrição. Se não for especificado, está definido para o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `offer-type`   | Sim      | Cadeia | A oferta da subscrição. Existem duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização em produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/teste, tem de ser [ativada através do portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Sim       | Cadeia | O ID de objeto da conta de inscrição que a subscrição é criada em e cobrada para. Este valor é um GUID que obtêm de `az billing enrollment-account list`. |
| `owner-object-id`      | Não       | Cadeia | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado.  |
| `owner-upn`    | Não       | Cadeia | O endereço de e-mail de qualquer utilizador que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `owner-object-id`.|
| `owner-spn` | Não       | Cadeia | O ID de quaisquer principal de serviço que pretende adicionar como um proprietário de RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `owner-object-id`.| 

Para ver uma lista completa de todos os parâmetros, consulte [criar conta az](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Limitações da criação da subscrição do Azure Enterprise API

- Apenas as subscrições do Azure Enterprise podem ser criadas utilizando esta API.
- Não há um limite de 50 subscrições por conta. Depois disso, as subscrições só podem ser criadas utilizando o Centro de contas.
- Tem de haver, pelo menos, uma EA ou EA Dev/teste subscrições com a conta, o que significa que o proprietário da conta tornou-se através de inscrição manual, pelo menos, uma vez.
- Os utilizadores que não são proprietários de conta, mas foram adicionados a uma conta de inscrição através do RBAC, não é possível criar subscrições através do Centro de contas.
- Não é possível selecionar o inquilino da subscrição a serem criadas. A subscrição é sempre criada no inquilino do proprietário da conta raiz. Para mover a subscrição para um inquilino diferente, consulte [alterar o inquilino da subscrição](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Passos Seguintes

* Para obter um exemplo sobre como criar subscrições através do .NET, consulte [exemplo de código no GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Agora que criou uma subscrição, pode conceder essa capacidade para outros utilizadores e os principais de serviço. Para obter mais informações, consulte [conceder acesso ao criar subscrições Azure Enterprise (pré-visualização)](grant-access-to-create-subscription.md).
* Para saber mais sobre a gestão de grandes quantidades de subscrições através de grupos de gestão, consulte [organizar os recursos com grupos de gestão do Azure](management-groups-overview.md)
