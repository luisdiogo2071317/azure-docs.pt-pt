---
title: Criar programaticamente as subscrições do Azure Enterprise | Documentos da Microsoft
description: Saiba como criar subscrições do Azure Enterprise ou o Enterprise programador/teste adicionais por meio de programação.
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
ms.openlocfilehash: 1b772fdbda8e58db9414e09ef3ef7c98fc9f86b8
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55486984"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Criar programaticamente as subscrições do Azure Enterprise (pré-visualização)

Como cliente do Azure no [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), pode criar subscrições de EA programador/teste (MS-AZR - 0148 P) e de EA (MS-AZR - 0017 P) por meio de programação. Neste artigo, saiba como criar subscrições por meio de programação com o Azure Resource Manager.

Quando cria uma subscrição do Azure a partir desta API, essa subscrição é regida pelo contrato sob a qual obteve serviços do Microsoft Azure da Microsoft ou de um revendedor autorizado. Para obter mais informações, consulte [Informação Legal do Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma função de proprietário ou contribuinte na conta de inscrição que pretende criar subscrições no. Existem duas formas de obter estas funções:

* O administrador de inscrição pode [torná-lo um proprietário da conta](https://ea.azure.com/helpdocs/addNewAccount) (início de sessão necessário) que torna o proprietário da conta de inscrição. Siga as instruções no e-mail de convite que recebeu a criar manualmente uma subscrição inicial. Confirme a propriedade da conta e criar manualmente uma subscrição de EA inicial antes de prosseguir para o passo seguinte. Basta adicionar a conta para a inscrição não é suficiente.

* Proprietário de uma existente da conta de inscrição pode [lhe conceder acesso](grant-access-to-create-subscription.md). Da mesma forma, se pretender utilizar um principal de serviço para criar a subscrição EA, terá [conceder esse principal de serviço a capacidade de criar subscrições](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Encontrar contas que tem acesso

Depois de serão adicionados a uma inscrição EA do Azure como proprietário da conta, o Azure utiliza a relação de inscrição de conta para determinar onde os custos de subscrição são faturadas. Criado sob a conta de todas as subscrições são faturadas em direção a inscrição de EA que a conta está no. Para criar subscrições, tem de passar valores sobre a conta de inscrição e os principais de utilizador para o proprietário da subscrição. 

Para executar os comandos seguintes, deve ter sessão iniciada proprietário da conta *diretório raiz*, que é o diretório de subscrições criadas por predefinição.

# <a name="resttabrest"></a>[REST](#tab/rest)

Pedido para listar todas as contas de inscrição:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure responde com uma lista de todas as contas de inscrição que tem acesso a:

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

Utilize o [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet para listar todas as contas de inscrição que tem acesso.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure responde com uma lista os endereços de e-mail e IDs de objeto de contas.

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Utilize o [lista de conta de inscrição de faturação az](https://aka.ms/EASubCreationPublicPreviewCLI) comando para listar todas as contas de inscrição que tem acesso.

```azurecli-interactive 
az billing enrollment-account list
```

Azure responde com uma lista os endereços de e-mail e IDs de objeto de contas.

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

Utilize o `principalName` propriedade para identificar a conta que pretende que as subscrições para ser-lhe cobrado. Utilizar o `id` como o `enrollmentAccount` valor que irá utilizar para criar a subscrição no próximo passo.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Criar subscrições com uma conta de inscrição específicas 

O exemplo seguinte cria um pedido para criar a subscrição com o nome *subscrição da equipe de desenvolvimento* e é a oferta de subscrição *MS-AZR - 0017P* (regular EA). A conta de inscrição é `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (valor de marcador de posição, este valor é um GUID), que é a conta de inscrição para SignUpEngineering@contoso.com. Opcionalmente, também adiciona dois usuários como proprietários do RBAC para a subscrição.

# <a name="resttabrest"></a>[REST](#tab/rest)

Utilize o `id` do `enrollmentAccount` no caminho do pedido para criar a subscrição.

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
| `displayName` | Não      | String | O nome a apresentar da subscrição. Se não for especificado, ele é definido como o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `offerType`   | Sim      | String | A oferta da subscrição. São as duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização de produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (programador/teste, tem de ser [ativada com o portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Não       | String | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário do RBAC na subscrição quando é criado.  |

Em resposta, recebe um `subscriptionOperation` objeto para monitorização. Quando a criação de subscrição estiver concluída, o `subscriptionOperation` objeto retornaria um `subscriptionLink` objeto, que tem o ID de subscrição.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Para utilizar este módulo de pré-visualização, instale-o executando `Install-Module Az.Subscription -AllowPrerelease` primeiro. Para se certificar `-AllowPrerelease` funciona, instalar uma versão recente do PowerShellGet partir [obter o módulo PowerShellGet](/powershell/gallery/installing-psget).

Utilize o [New-AzSubscription](/powershell/module/az.subscription) juntamente com `enrollmentAccount` ID como de objeto a `EnrollmentAccountObjectId` parâmetro para criar uma nova subscrição. 

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Não      | String | O nome a apresentar da subscrição. Se não for especificado, ele é definido como o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `OfferType`   | Sim      | String | A oferta da subscrição. São as duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização de produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (programador/teste, tem de ser [ativada com o portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Sim       | String | O ID de objeto do que a subscrição é criada em e cobrada para a conta de inscrição. Este valor é um GUID que obtém da `Get-AzEnrollmentAccount`. |
| `OwnerObjectId`      | Não       | String | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário do RBAC na subscrição quando é criado.  |
| `OwnerSignInName`    | Não       | String | O endereço de e-mail de qualquer utilizador que pretende adicionar como um proprietário do RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `OwnerObjectId`.|
| `OwnerApplicationId` | Não       | String | O ID de aplicação de qualquer principal de serviço que pretende adicionar como um proprietário do RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `OwnerObjectId`. Ao utilizar este parâmetro, o principal de serviço tem de ter [acesso de leitura para o diretório](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Para ver uma lista completa de todos os parâmetros, consulte [New-AzSubscription](/powershell/module/az.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

Para utilizar esta extensão de pré-visualização, instale-o executando `az extension add --name subscription` primeiro.

Utilize o [criar conta de az](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) juntamente com `enrollmentAccount` ID como de objeto a `enrollment-account-object-id` parâmetro para criar uma nova subscrição.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Nome do elemento  | Necessário | Tipo   | Descrição                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Não      | String | O nome a apresentar da subscrição. Se não for especificado, ele é definido como o nome da oferta, como "Microsoft Azure Enterprise."                                 |
| `offer-type`   | Sim      | String | A oferta da subscrição. São as duas opções para EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (utilização de produção) e [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (programador/teste, tem de ser [ativada com o portal EA](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Sim       | String | O ID de objeto do que a subscrição é criada em e cobrada para a conta de inscrição. Este valor é um GUID que obtém da `az billing enrollment-account list`. |
| `owner-object-id`      | Não       | String | O ID de objeto de qualquer utilizador que pretende adicionar como um proprietário do RBAC na subscrição quando é criado.  |
| `owner-upn`    | Não       | String | O endereço de e-mail de qualquer utilizador que pretende adicionar como um proprietário do RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `owner-object-id`.|
| `owner-spn` | Não       | String | O ID de aplicação de qualquer principal de serviço que pretende adicionar como um proprietário do RBAC na subscrição quando é criado. Pode utilizar este parâmetro, em vez de `owner-object-id`. Ao utilizar este parâmetro, o principal de serviço tem de ter [acesso de leitura para o diretório](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Para ver uma lista completa de todos os parâmetros, consulte [criar conta de az](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Limitações de criação da subscrição do Azure Enterprise API

- Apenas as subscrições do Azure Enterprise podem ser criadas com esta API.
- Existe um limite de 50 subscrições por conta. Depois disso, as subscrições apenas podem ser criadas utilizando o Centro de contas.
- Deve haver pelo menos um EA ou EA programador/teste subscrições sob a conta, o que significa que o proprietário da conta já passaram por manual Inscreva-se pelo menos uma vez.
- Os utilizadores que não são proprietários da conta, mas foram adicionados a uma conta de inscrição através do RBAC, não é possível criar assinaturas usando o Centro de contas.
- Não é possível selecionar o inquilino para a subscrição a ser criado em. A subscrição é sempre criada no inquilino principal do proprietário da conta. Para mover a subscrição para um inquilino diferente, consulte [alterar o inquilino da subscrição](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Passos Seguintes

* Para obter um exemplo sobre como criar subscrições através do .NET, consulte [código no GitHub de exemplo](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Agora que criou uma subscrição, pode conceder essa habilidade para outros utilizadores e os principais de serviço. Para obter mais informações, consulte [conceder acesso de criar subscrições do Azure Enterprise (pré-visualização)](grant-access-to-create-subscription.md).
* Para saber mais sobre como gerir um grande número de subscrições com grupos de gestão, veja [organizar os recursos com grupos de gestão do Azure](management-groups-overview.md)
