---
title: Criar a identidade da aplicação do Azure com o PowerShell | Microsoft Docs
description: Descreve como utilizar o Azure PowerShell para criar uma aplicação do Azure Active Directory e um principal de serviço e conceder acesso a recursos através do controlo de acesso baseado em funções. Mostra como a aplicação com um certificado de autenticação.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 6ab1b2357e88525f4730b5ad550cfcf3acbb906e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Utilizar o Azure PowerShell para criar um principal de serviço com um certificado

Quando tiver uma aplicação ou script que precisa de aceder a recursos, pode configurar uma identidade para a aplicação e autenticar a aplicação com as suas próprias credenciais. Esta identidade é conhecida como um principal de serviço. Esta abordagem permite-lhe:

* Atribua permissões para a identidade de aplicação que sejam diferentes a suas própria permissões. Normalmente, estas permissões são restritos exatamente o que a aplicação tem de fazer.
* Utilize um certificado para autenticação ao executar um script automático.

> [!IMPORTANT]
> Em vez de criar um principal de serviço, considere utilizar identidade de serviço gerida do Azure AD para a identidade da aplicação. Azure AD MSI é uma funcionalidade de pré-visualização pública do Azure Active Directory simplifica a criação de uma identidade para o código. Se o seu código é executado num serviço que suporta o Azure AD MSI e acede a recursos que suportam a autenticação do Azure Active Directory, Azure AD MSI é uma opção melhor para si. Para saber mais sobre o Azure AD MSI, incluindo os serviços atualmente o suporta, consulte [identidade de serviço geridas para recursos do Azure](../active-directory/managed-service-identity/overview.md).

Este artigo mostra como criar um principal de serviço que efetua a autenticação com um certificado. Para configurar um principal de serviço com palavra-passe, consulte [criar um Azure principal de serviço com o Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).

Tem de ter o [versão mais recente](/powershell/azure/get-started-azureps) do PowerShell para este artigo.

## <a name="required-permissions"></a>Permissões obrigatórias

Para concluir este artigo, tem de ter permissões suficientes no seu Azure Active Directory e a subscrição do Azure. Especificamente, tem de ser capaz de criar uma aplicação no Azure Active Directory e atribua o principal de serviço a uma função.

A forma mais fácil de verificar se a sua conta tem permissões adequadas é utilizar o portal. Consulte [verificar a permissão necessária](resource-group-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Criar o serviço principal com o certificado autoassinado

O exemplo seguinte inclui um cenário simples. Utiliza [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) para criar um principal de serviço com um certificado autoassinado e utiliza [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) para atribuir a [contribuinte](../role-based-access-control/built-in-roles.md#contributor)função para o principal de serviço. A atribuição de função é confinada à sua subscrição do Azure atualmente selecionada. Para selecionar uma subscrição diferente, utilize [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext).

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

O exemplo permanecerá suspenso durante 20 segundos permitir a algum tempo para o novo serviço principal para propagar ao longo do Azure Active Directory. Se o script não puder aguardar suficientemente longa, verá um erro a indicar: "{ID} de Principal não existe no diretório de {DIR-ID}." Para resolver este erro, aguarde um momento, em seguida, execute o **New-AzureRmRoleAssignment** novamente o comando.

Pode definir o âmbito de atribuição de função a um grupo de recurso específico utilizando o **ResourceGroupName** parâmetro. Pode definir o âmbito para um recurso específico utilizando também o **ResourceType** e **ResourceName** parâmetros. 

Se lhe **não tem o Windows 10 ou Windows Server 2016**, tem de transferir o [gerador do certificado autoassinado](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) do Microsoft Script Center. Extrair o respetivo conteúdo e importe-o cmdlet que tem.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

O script, substitua as seguintes duas linhas para gerar o certificado.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -StoreName My `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Forneça o certificado através do script do PowerShell automatizada

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

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Criar o serviço principal com o certificado de autoridade de certificação

O exemplo seguinte utiliza um certificado emitido por uma autoridade de certificação para criar o serviço principal. A atribuição é confinada à subscrição do Azure especificada. Adiciona o principal de serviço para o [contribuinte](../role-based-access-control/built-in-roles.md#contributor) função. Se ocorrer um erro durante a atribuição de função, repete a atribuição.

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

### <a name="provide-certificate-through-automated-powershell-script"></a>Forneça o certificado através do script do PowerShell automatizada
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

## <a name="change-credentials"></a>Alterar as credenciais

Para alterar as credenciais para uma aplicação AD, devido a um comprometimento de segurança ou expiração de credencial, utilize o [remover AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) e [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.

Para remover todas as credenciais para uma aplicação, utilize:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | Remove-AzureRmADAppCredential
```

Para adicionar um valor de certificado, crie um certificado autoassinado, conforme mostrado neste artigo. Em seguida, utilize:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | New-AzureRmADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Depurar

Pode obter os seguintes erros ao criar um principal de serviço:

* **"Authentication_Unauthorized"** ou **"nenhuma subscrição encontrado no contexto."** -Vir este erro quando a sua conta não tem o [as permissões necessárias](#required-permissions) no Azure Active Directory para registar uma aplicação. Normalmente, este erro ocorrer quando apenas os utilizadores de administração do seu Azure Active Directory podem registar aplicações e a sua conta não é um administrador. Peça ao seu administrador para lhe atribuir ou para uma função de administrador ou permitir que os utilizadores registar as aplicações.

* A conta **"não tem autorização para executar a ação 'Microsoft.Authorization/roleAssignments/write' no âmbito"subscrições / {guid}"."**  -Ver este erro quando a sua conta não tem permissões suficientes para atribuir uma função para uma identidade. Peça ao seu administrador de subscrição para adicioná-lo à função de administrador de acesso de utilizador.

## <a name="next-steps"></a>Passos Seguintes
* Para configurar um principal de serviço com palavra-passe, consulte [criar um Azure principal de serviço com o Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps).
* Para obter passos detalhados sobre como integrar a uma aplicação no Azure para gerir recursos, consulte [guia para programadores para autorização com a API do Azure Resource Manager](resource-manager-api-authentication.md).
* Para obter uma explicação mais detalhada de principais de serviço e de aplicações, consulte [objectos da aplicação e objetos de principais de serviço](../active-directory/active-directory-application-objects.md). 
* Para obter mais informações sobre a autenticação do Azure Active Directory, consulte [cenários de autenticação para o Azure AD](../active-directory/active-directory-authentication-scenarios.md).
