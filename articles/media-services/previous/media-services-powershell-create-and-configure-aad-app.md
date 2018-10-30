---
title: Utilizar o PowerShell para criar uma aplicação do Azure AD para aceder à API de serviços de multimédia do Azure | Documentos da Microsoft
description: Saiba como utilizar o PowerShell para criar uma aplicação do Azure Active Directory (Azure AD) e configurá-lo para acesso à API de serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: juliako
ms.openlocfilehash: bd7938fb600a1a0cd65b89fde6ca25135306b76d
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232647"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>Utilizar o PowerShell para criar uma aplicação do Azure AD para utilizar com a API de serviços de multimédia do Azure

Saiba como utilizar um script do PowerShell para criar uma aplicação do Azure Active Directory (Azure AD) e o principal de serviço para aceder aos recursos de serviços de multimédia do Azure.  

## <a name="prerequisites"></a>Pré-requisitos

- Uma conta do Azure. Se não tiver uma conta, comece com um [avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Uma conta dos Media Services. Para obter mais informações, consulte [criar uma conta de Media Services do Azure no portal do Azure](media-services-portal-create-account.md).
- O Azure PowerShell versão 0.8.8 ou uma versão posterior. Para obter mais informações, consulte [como utilizar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Cmdlets do Gestor de recursos do Azure.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Criar uma aplicação do Azure AD com o PowerShell  

```powershell
Connect-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Para obter mais informações, veja os artigos seguintes:

- [Utilizar o Azure PowerShell para criar um principal de serviço para aceder aos recursos](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Gerir o controlo de acesso baseado em funções com o Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Como configurar manualmente as aplicações daemon com certificados](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Passos Seguintes

Introdução ao [carregar ficheiros para a conta](media-services-portal-upload-files.md).
