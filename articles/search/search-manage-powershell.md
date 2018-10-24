---
title: Gerir o Azure Search com scripts do Powershell | Documentos da Microsoft
description: Gerir o seu serviço de Azure Search com scripts do PowerShell. Criar ou atualizar um serviço Azure Search e gerir chaves de administração do Azure Search
author: HeidiSteen
manager: cgronlun
tags: azure-resource-manager
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/15/2016
ms.author: heidist
ms.openlocfilehash: bae9e2dcb4320c1da4f1d8e3c6ad50ce90195544
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958572"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Gerir o seu serviço de Azure Search com o PowerShell
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Este tópico descreve os comandos do PowerShell para efetuar muitas das tarefas de gestão de serviços de pesquisa do Azure. Percorreremos a criação de um serviço de pesquisa, o dimensionamento-lo e gerir as chaves de API.
Estes comandos as opções de gestão disponíveis em paralelo os [API do REST de gestão do Azure Search](https://docs.microsoft.com/rest/api/searchmanagement).

## <a name="prerequisites"></a>Pré-requisitos
* Tem de ter o Azure PowerShell 1.0 ou superior. Para obter instruções, consulte [instalar e configurar o Azure PowerShell](/powershell/azure/overview).
* Tem de ter sessão iniciada sua subscrição do Azure no PowerShell, conforme descrito abaixo.

Em primeiro lugar, tem de iniciar sessão no Azure com este comando:

    Connect-AzureRmAccount

Especifica o endereço de e-mail da sua conta do Azure e a respetiva palavra-passe na caixa de diálogo de início de sessão da Microsoft Azure.

Em alternativa, pode [início de sessão forma não interativa com um principal de serviço](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

Se tiver várias subscrições do Azure, terá de definir a sua subscrição do Azure. Para ver uma lista das suas subscrições atuais, execute este comando.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Para especificar a subscrição, execute o seguinte comando. No exemplo a seguir, é o nome da subscrição `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Comandos para ajudá-lo a começar a utilizar
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a>Próximos Passos
Agora que o serviço é criado, pode efetuar os passos seguintes: criar uma [índice](search-what-is-an-index.md), [consultar um índice](search-query-overview.md)e, finalmente, criar e gerir o seu próprio aplicativo de pesquisa que utiliza a Azure Search.

* [Criar um índice da Azure Search no portal do Azure](search-create-index-portal.md)
* [Consultar um índice da Azure Search utilizando o Explorador de pesquisa no portal do Azure](search-explorer.md)
* [Configurar um indexador para carregar dados de outros serviços](search-indexer-overview.md)
* [Como utilizar o Azure Search no .NET](search-howto-dotnet-sdk.md)
* [Analisar o tráfego do Azure Search](search-traffic-analytics.md)

