---
title: Criar a identidade da aplicação Azure com o PowerShell | Microsoft Docs
description: Descreve como utilizar o Azure PowerShell para criar uma aplicação do Azure Active Directory e um principal de serviço, e conceder acesso a recursos através do controlo de acesso baseado em funções. Mostra como a autenticar a aplicação com um certificado.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/10/2018
ms.author: tomfitz
ms.openlocfilehash: aec79655dde48a18891776f40c51aa5a5c859ef8
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577269"
---
# <a name="use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Utilizar o Azure PowerShell para criar um principal de serviço com um certificado

Quando tem uma aplicação ou script que precisa de aceder a recursos, pode configurar uma identidade para a aplicação e autenticar a aplicação com as suas próprias credenciais. Esta identidade é conhecida como um principal de serviço. Esta abordagem permite-lhe:

* Atribuir permissões para a identidade da aplicação, que são diferentes das suas própria permissões. Normalmente, estas permissões estão restritas a exatamente aquilo que a aplicação precisa de fazer.
* Utilize um certificado para autenticação ao executar um script automático.

> [!IMPORTANT]
> Em vez de criar um principal de serviço, considere utilizar a Identidade de Serviço Gerida do Azure AD para a identidade da aplicação. O MSI do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory que simplifica a criação de uma identidade para o código. Se o seu código é executado num serviço que suporta o MSI do Azure AD e acede a recursos que suportam a autenticação do Azure Active Directory, o MSI do Azure AD é uma opção melhor para si. Para saber mais sobre o MSI do Azure AD, incluindo os serviços que atualmente o suportam, veja [Identidade de Serviço Gerida para recursos do Azure](../active-directory/managed-service-identity/overview.md).

Este artigo mostra como criar um principal de serviço que faz a autenticação com um certificado. Para configurar um principal de serviço com palavra-passe, veja [Criar um principal de serviço do Azure com o Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).

Para este artigo, tem de ter a [versão mais recente](/powershell/azure/get-started-azureps) do PowerShell.

## <a name="required-permissions"></a>Permissões obrigatórias

Para concluir este artigo, tem de ter permissões suficientes na sua subscrição do Azure e do Azure Active Directory. Mais concretamente, tem de ser capaz de criar uma aplicação no Azure Active Directory e atribuir um principal de serviço a uma função.

A forma mais fácil de verificar se a sua conta tem permissões adequadas é utilizar o portal. Veja [Permissões obrigatórias](resource-group-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Criar um principal de serviço com um certificado autoassinado

O exemplo seguinte inclui um cenário simples. Utiliza [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) para criar um principal de serviço com um certificado autoassinado e utiliza [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) para atribuir a função [Contribuidor](../role-based-access-control/built-in-roles.md#contributor) ao principal de serviço. A atribuição da função está confinada à sua subscrição do Azure atualmente selecionada. Para selecionar uma subscrição diferente, utilize [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

O exemplo permanece suspenso durante 20 segundos para dar algum tempo para que o novo principal de serviço se propague pelo Azure Active Directory. Se o script não aguardar o tempo suficiente, verá um erro que indica: "{ID} de principal não existe no diretório {DIR-ID}." Para resolver este erro, aguarde um momento e, em seguida, execute o comando **New-AzureRmRoleAssignment** novamente.

Pode definir o âmbito de atribuição da função para um grupo de recursos específico, utilizando o parâmetro **ResourceGroupName**. Pode definir o âmbito para um recurso específico, utilizando também os parâmetros **ResourceType** e **ResourceName**. 

Se **não tiver o Windows 10 ou o Windows Server 2016**, precisa de descarregar o [Gerador de certificados autoassinados](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) a partir do Microsoft Script Center. Extraia o respetivo conteúdo e importe o cmdlet de que precisa.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

No script, substitua as duas linhas seguintes para gerar o certificado.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Fornecer o certificado através do script do PowerShell automatizado

Sempre que iniciar sessão como um principal de serviço, tem de fornecer o ID de inquilino do diretório para a sua aplicação AD. Um inquilino é uma instância do Azure Active Directory.

```powershell
$TenantId = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzureRmADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Criar um principal de serviço com um certificado da Autoridade de Certificação

O exemplo seguinte utiliza um certificado emitido por uma Autoridade de Certificação para criar o principal de serviço. A atribuição é confinada à subscrição do Azure especificada. Adiciona o principal de serviço à função [Contribuidor](../role-based-access-control/built-in-roles.md#contributor). Se ocorrer um erro durante a atribuição da função, repete a atribuição.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Fornecer o certificado através do script do PowerShell automatizado
Sempre que iniciar sessão como um principal de serviço, tem de fornecer o ID de inquilino do diretório para a sua aplicação AD. Um inquilino é uma instância do Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

O ID da aplicação e o ID de inquilino não são sensíveis, pelo que pode incorporá-los diretamente no seu script. Se precisar de obter o ID do inquilino, utilize:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Se precisar de obter o ID da aplicação, utilize:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Alterar credenciais

Para alterar as credenciais para uma aplicação AD, devido a um comprometimento de segurança ou à expiração de credenciais, utilize os cmdlets [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) e [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential).

Para remover todas as credenciais para uma aplicação, utilize:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | Remove-AzureRmADAppCredential
```

Para adicionar um valor de certificado, crie um certificado autoassinado, conforme explicado neste artigo. Em seguida, utilize:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | New-AzureRmADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Depurar

Pode obter os seguintes erros ao criar um principal de serviço:

* **"Authentication_Unauthorized"** ou **"Nenhuma subscrição encontrada no contexto."** - Este erro é apresentado quando a sua conta não tem as [permissões obrigatórias](#required-permissions) no Azure Active Directory para registar uma aplicação. Normalmente, este erro ocorre quando apenas os utilizadores de administração do seu Azure Active Directory podem registar aplicações e a sua conta não é uma conta de administrador. Peça ao seu administrador para lhe atribuir uma função de administrador ou para permitir que os utilizadores registem aplicações.

* A sua conta **"não tem autorização para executar a ação 'Microsoft.Authorization/roleAssignments/write' no âmbito '/subscriptions/{guid}'."** - Este erro é apresentado quando a sua conta não tem permissões suficientes para atribuir uma função a uma identidade. Peça ao administrador da sua subscrição para adicioná-lo à função Administrador de Acesso dos Utilizadores.

## <a name="next-steps"></a>Passos Seguintes
* Para configurar um principal de serviço com palavra-passe, veja [Criar um principal de serviço do Azure com o Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).
* Para obter passos detalhados sobre como integrar uma aplicação no Azure para gerir recursos, veja [Guia para programadores para autorização com a API do Azure Resource Manager](resource-manager-api-authentication.md).
* Para obter uma explicação mais detalhada de principais de serviço e de aplicações, veja [Objetos de Aplicações e Objetos de Principais de Serviço](../active-directory/develop/app-objects-and-service-principals.md). 
* Para obter mais informações sobre a autenticação do Azure Active Directory, veja [Cenários de Autenticação para o Azure AD](../active-directory/develop/authentication-scenarios.md).
