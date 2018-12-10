---
title: Erros de registo do fornecedor de recursos do Azure | Documentos da Microsoft
description: Descreve como resolver erros de registo do fornecedor de recursos do Azure.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/07/2018
ms.author: tomfitz
ms.openlocfilehash: 787d6549eb8413c9dcfc0c906167cc36d4cff6b0
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135717"
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Resolver erros de registo do fornecedor de recursos

Este artigo descreve os erros que poderá encontrar ao utilizar um fornecedor de recursos que ainda não tenha utilizado na sua subscrição.

## <a name="symptom"></a>Sintoma

Durante a implantação de recursos, poderá receber o seguinte código de erro e a mensagem:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Em alternativa, poderá receber uma mensagem semelhante que indica:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

A mensagem de erro deverá dar-lhe sugestões para as localizações suportadas e versões de API. Pode alterar o modelo para um dos valores sugeridos. A maioria dos fornecedores são registrados automaticamente pelo portal do Azure ou a interface de linha de comandos que está a utilizar, mas não todos. Se ainda não utilizou um fornecedor de recursos específico antes, se pretender registar esse fornecedor.

## <a name="cause"></a>Causa

Receber estes erros para um dos três motivos:

1. O fornecedor de recursos não foi registado para a sua subscrição
1. Versão de API não suportada para o tipo de recurso
1. Localização não é suportada para o tipo de recurso

## <a name="solution-1---powershell"></a>Solução 1 - PowerShell

Para o PowerShell, utilize **Get-AzureRmResourceProvider** para ver o estado de registo.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Para registar um fornecedor, utilize **Register-AzureRmResourceProvider** e forneça o nome do fornecedor de recursos que pretende registar.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Para obter as localizações suportadas para um determinado tipo de recurso, utilize:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Para obter as versões de API suportadas para um determinado tipo de recurso, utilize:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Solução 2 - CLI do Azure

Para ver se o fornecedor está registado, utilize o `az provider list` comando.

```azurecli-interactive
az provider list
```

Para registar um fornecedor de recursos, utilize o `az provider register` comando e especifique o *espaço de nomes* para se registar.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Para ver as localizações suportadas e versões de API para um tipo de recurso, utilize:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Solução de 3 - portal do Azure

Pode ver o estado de registo e registe-se de um espaço de nomes do fornecedor de recursos através do portal.

1. No portal, selecione **todos os serviços**.

   ![Selecione todos os serviços](./media/resource-manager-register-provider-errors/select-all-services.png)

1. Selecione **Subscrições**.

   ![Selecionar subscrições](./media/resource-manager-register-provider-errors/select-subscriptions.png)

1. Na lista de subscrições, selecione a subscrição que pretende utilizar para registar o fornecedor de recursos.

   ![Selecione a subscrição para registar o fornecedor de recursos](./media/resource-manager-register-provider-errors/select-subscription-to-register.png)

1. Para a sua subscrição, selecione **fornecedores de recursos**.

   ![Selecionar fornecedores de recursos](./media/resource-manager-register-provider-errors/select-resource-provider.png)

1. Veja a lista de fornecedores de recursos e, se necessário, selecione o **registar** ligação para registar o fornecedor de recursos do tipo que está a tentar implementar.

   ![Fornecedores de recursos de lista](./media/resource-manager-register-provider-errors/list-resource-providers.png)
