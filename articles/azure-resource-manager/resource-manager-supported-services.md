---
title: Fornecedores de recursos do Azure e tipos de recursos | Documentos da Microsoft
description: Descreve os fornecedores de recursos do Resource Manager, os esquemas e versões de API disponíveis e as regiões que podem hospedar os recursos de suporte.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: fafc16bdf00f947d4ba8ffe56d7cf2ae3e0bc489
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344948"
---
# <a name="resource-providers-and-types"></a>Fornecedores e tipos de recursos

Durante a implantação de recursos, com freqüência precisar de obter informações sobre os fornecedores de recursos e os tipos. Neste artigo, vai aprender a:

* Ver todos os fornecedores de recursos no Azure
* Verificar o estado de registo de um fornecedor de recursos
* Registar um fornecedor de recursos
* Tipos de recursos de exibição para um fornecedor de recursos
* Ver as localizações válidas para um tipo de recurso
* Ver versões de API válidas para um tipo de recurso

Pode executar estes passos através do portal, o PowerShell ou a CLI do Azure.

## <a name="powershell"></a>PowerShell

Para ver todos os fornecedores de recursos no Azure e o estado do registo para a sua subscrição, utilize:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Que devolve resultados semelhantes:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Registar um fornecedor de recursos configura a sua subscrição para trabalhar com o fornecedor de recursos. O âmbito de registo é sempre a subscrição. Por padrão, muitos fornecedores de recursos são automaticamente registrados. No entanto, terá de registar manualmente alguns fornecedores de recursos. Para registar um fornecedor de recursos, tem de ter permissão para efetuar o `/register/action` operação para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Que devolve resultados semelhantes:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Não é possível anular o registo de um fornecedor de recursos quando ainda terá os tipos de recursos desse fornecedor de recursos na sua subscrição.

Para ver informações de um fornecedor de recursos específico, utilize:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Que devolve resultados semelhantes:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Para ver os tipos de recurso para um fornecedor de recursos, utilize:

```azurepowershell-interactive
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Que retorna:

```powershell
batchAccounts
operations
locations
locations/quotas
```

A versão de API corresponde a uma versão de operações de REST API que são lançadas pelo fornecedor de recursos. Como um fornecedor de recursos permite novas funcionalidades, ela lança uma nova versão da REST API.

Para obter as versões de API disponíveis para um tipo de recurso, utilize:

```azurepowershell-interactive
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Que retorna:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Gestor de recursos é suportado em todas as regiões, mas os recursos que implementar podem não ser suportados em todas as regiões. Além disso, pode haver limitações na sua subscrição que o impede de utilizar algumas regiões que suportam o recurso.

Para obter as localizações suportadas para um tipo de recurso, utilize.

```azurepowershell-interactive
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Que retorna:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>CLI do Azure

Para ver todos os fornecedores de recursos no Azure e o estado do registo para a sua subscrição, utilize:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Que devolve resultados semelhantes:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Registar um fornecedor de recursos configura a sua subscrição para trabalhar com o fornecedor de recursos. O âmbito de registo é sempre a subscrição. Por padrão, muitos fornecedores de recursos são automaticamente registrados. No entanto, terá de registar manualmente alguns fornecedores de recursos. Para registar um fornecedor de recursos, tem de ter permissão para efetuar o `/register/action` operação para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário.

```azurecli
az provider register --namespace Microsoft.Batch
```

Que retorna uma mensagem que registo está em curso.

Não é possível anular o registo de um fornecedor de recursos quando ainda terá os tipos de recursos desse fornecedor de recursos na sua subscrição.

Para ver informações de um fornecedor de recursos específico, utilize:

```azurecli
az provider show --namespace Microsoft.Batch
```

Que devolve resultados semelhantes:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Para ver os tipos de recurso para um fornecedor de recursos, utilize:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Que retorna:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

A versão de API corresponde a uma versão de operações de REST API que são lançadas pelo fornecedor de recursos. Como um fornecedor de recursos permite novas funcionalidades, ela lança uma nova versão da REST API.

Para obter as versões de API disponíveis para um tipo de recurso, utilize:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Que retorna:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Gestor de recursos é suportado em todas as regiões, mas os recursos que implementar podem não ser suportados em todas as regiões. Além disso, pode haver limitações na sua subscrição que o impede de utilizar algumas regiões que suportam o recurso.

Para obter as localizações suportadas para um tipo de recurso, utilize.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Que retorna:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portal

Para ver todos os fornecedores de recursos no Azure e o estado do registo para a sua subscrição, selecione **subscrições**.

![Selecione as subscrições](./media/resource-manager-supported-services/select-subscriptions.png)

Escolha a subscrição para ver.

![Especifique a subscrição](./media/resource-manager-supported-services/subscription.png)

Selecione **fornecedores de recursos** e ver a lista de fornecedores de recursos disponíveis.

![Mostrar os fornecedores de recursos](./media/resource-manager-supported-services/show-resource-providers.png)

Registar um fornecedor de recursos configura a sua subscrição para trabalhar com o fornecedor de recursos. O âmbito de registo é sempre a subscrição. Por padrão, muitos fornecedores de recursos são automaticamente registrados. No entanto, terá de registar manualmente alguns fornecedores de recursos. Para registar um fornecedor de recursos, tem de ter permissão para efetuar o `/register/action` operação para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário. Para registar um fornecedor de recursos, selecione **registar**.

![Registar fornecedor de recursos](./media/resource-manager-supported-services/register-provider.png)

Não é possível anular o registo de um fornecedor de recursos quando ainda terá os tipos de recursos desse fornecedor de recursos na sua subscrição.

Para ver informações de um fornecedor de recursos específico, selecione **todos os serviços**.

![Selecione todos os serviços](./media/resource-manager-supported-services/more-services.png)

Procure **Explorador de recursos** e selecione-o nas opções disponíveis.

![Selecione o Explorador de recursos](./media/resource-manager-supported-services/select-resource-explorer.png)

Selecione **Fornecedores**.

![Selecionar fornecedores](./media/resource-manager-supported-services/select-providers.png)

Selecione o fornecedor de recursos e o tipo de recurso que pretende visualizar.

![Selecione o tipo de recurso](./media/resource-manager-supported-services/select-resource-type.png)

Gestor de recursos é suportado em todas as regiões, mas os recursos que implementar podem não ser suportados em todas as regiões. Além disso, pode haver limitações na sua subscrição que o impede de utilizar algumas regiões que suportam o recurso. O resource explorer mostra os locais válidos para o tipo de recurso.

![Mostrar localizações](./media/resource-manager-supported-services/show-locations.png)

A versão de API corresponde a uma versão de operações de REST API que são lançadas pelo fornecedor de recursos. Como um fornecedor de recursos permite novas funcionalidades, ela lança uma nova versão da REST API. O resource explorer mostra as versões de API válidas para o tipo de recurso.

![Mostrar versões de API](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a criação de modelos do Resource Manager, veja [modelos Authoring Azure Resource Manager](resource-group-authoring-templates.md).
* Para saber mais sobre a implementação de recursos, veja [implementar uma aplicação com o modelo Azure Resource Manager](resource-group-template-deploy.md).
* Para ver as operações de um fornecedor de recursos, consulte [API REST do Azure](/rest/api/).
