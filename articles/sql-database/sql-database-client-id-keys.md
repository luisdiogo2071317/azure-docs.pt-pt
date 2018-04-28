---
title: Obter os valores para autenticação da aplicação - SQL Database do Azure | Microsoft Docs
description: Crie um principal de serviço para aceder à base de dados SQL a partir do código.
services: sql-database
author: stevestein
manager: jhubbard
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 31dd476244d578acf1ced3881fac94d5262bcb82
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Obter os valores necessários para a autenticação de uma aplicação para aceder à base de dados SQL a partir do código
Para criar e gerir a base de dados SQL a partir do código tem de registar a sua aplicação no domínio do Azure Active Directory (AAD) na subscrição, onde os recursos do Azure foram criados.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Criar a um serviço principal para aceder a recursos a partir de uma aplicação
Tem de ter a versão mais recente [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) instalado e em execução. Para informações detalhadas, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs).

O seguinte script do PowerShell cria a aplicação do Active Directory (AD) e o principal de serviço que é necessário para autenticar a nossa aplicação C#. O script produz os valores de necessários para a amostra do C# precedente. Para obter informações detalhadas, consulte [Utilize o Azure PowerShell para criar um principal de serviço para aceder aos recursos](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Connect-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Consulte também
* [Criar uma base de dados SQL com c#](sql-database-get-started-csharp.md)
* [Ligar à base de dados do SQL Server utilizando a autenticação do Azure Active Directory](sql-database-aad-authentication.md)

