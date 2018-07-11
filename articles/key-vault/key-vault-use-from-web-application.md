---
title: Tutorial Utilizar o Azure Key Vault a partir de uma Aplicação Web | Microsoft Docs
description: Utilize este tutorial para aprender a utilizar o Azure Key Vault a partir de uma aplicação Web.
services: key-vault
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: adhurwit
ms.openlocfilehash: 5cd764395e91a82973318da7284b28d7a43d35ea
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37115109"
---
# <a name="tutorial-use-azure-key-vault-from-a-web-application"></a>Tutorial: Utilizar o Azure Key Vault a partir de uma aplicação Web
Utilize este tutorial para aprender a utilizar o Azure Key Vault a partir de uma aplicação Web no Azure. Este tutorial mostra o processo de aceder a um segredo a partir de um Azure Key Vault para utilizar numa aplicação Web. Em seguida, o tutorial desenvolve o processo e utiliza um certificado em vez de um segredo do cliente. Este tutorial destina-se a programadores Web que compreendem as noções básicas da criação de aplicações Web no Azure. 

Neste tutorial, ficará a saber como: 

> [!div class="checklist"]
> * Adicionar definições da aplicação ao ficheiro web.config
> * Adicionar um método para obter um token de acesso
> * Obter o token no Início da Aplicação
> * Fazer a autenticação com um certificado 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, tem de ter os seguintes itens:

* Um URI para um segredo num Azure Key Vault
* Um ID de Cliente e um Segredo do Cliente para uma aplicação Web registada no Azure Active Directory que tem acesso ao seu Key Vault
* Uma aplicação Web. Este tutorial mostra os passos para uma aplicação MVC ASP.NET implementada no Azure como uma Aplicação Web.

Execute os passos do artigo [Introdução ao Azure Key Vault](key-vault-get-started.md) para obter o URI para um segredo, ID de Cliente, Segredo do Cliente, e registar a aplicação. A aplicação Web irá aceder ao cofre e tem de ser registada no Azure Active Directory. Também precisa de ter direitos de acesso ao Key Vault. Se não tiver, volte à secção Registar uma Aplicação no tutorial de Introdução e repita os passos apresentados. Para obter mais informações sobre a criação de Aplicações Web do Azure, veja [Descrição geral das Aplicações Web](../app-service/app-service-web-overview.md).

Este exemplo depende do aprovisionamento manual de Identidades do Azure Active Directory. Atualmente, existe uma nova funcionalidade na pré-visualização chamada [Identidade de Serviço Gerida (MSI)](https://docs.microsoft.com/azure/active-directory/msi-overview), que pode aprovisionar automaticamente identidades do Azure AD. Para obter mais informações, veja o exemplo no [GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) e o [tutorial MSI com o Serviço de Aplicações e Funções](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity). 


## <a id="packages"></a>Adicionar pacotes NuGet

Existem dois pacotes que a sua aplicação Web tem de ter instalados.

* Biblioteca de Autenticação do Active Directory – tem métodos para interagir com o Azure Active Directory e gerir a identidade de utilizador
* Biblioteca do Azure Key Vault – tem métodos para interagir com o Azure Key Vault

Ambos os pacotes podem ser instalados com a Consola do Gestor de Pacotes, com o comando Install-Package.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory 
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>Modificar o ficheiro web.config

Existem três configurações de aplicação que têm de ser adicionadas ao ficheiro web.config da seguinte forma. Vamos adicionar os valores reais no portal do Azure para um nível adicional de segurança.

```xml
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
    <!-- If you aren't hosting your app as an Azure Web App, then you should use the actual ClientId, Client Secret, and Secret URI values -->
```



## <a id="gettoken"></a>Adicionar um método para obter um token de acesso

Para utilizar a API do Key Vault, é necessário um token de acesso. O Cliente do Key Vault processa as chamadas para a API do Key Vault, mas é necessário fornecer uma função que obtém o token de acesso. O exemplo seguinte é código para obter um token de acesso a partir do Azure Active Directory. Este código pode ser colocado em qualquer parte da sua aplicação. Optámos por adicionar uma classe Utils ou EncryptionHelper.  

```cs
//add these using statements
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Threading.Tasks;
using System.Web.Configuration;

//this is an optional property to hold the secret after it is retrieved
public static string EncryptSecret { get; set; }

//the method that will be provided to the KeyVaultClient
public static async Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                WebConfigurationManager.AppSettings["ClientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
// Using Client ID and Client Secret is a way to authenticate an Azure AD application.
// Using it in your web application allows for a separation of duties and more control over your key management. 
// However, it does rely on putting the Client Secret in your configuration settings.
// For some people, this can be as risky as putting the secret in your configuration settings.
```

## <a id="appstart"></a>Obter o segredo no Início da Aplicação

Agora, precisamos de código para chamar a API do Key Vault e obter o segredo. O código seguinte pode ser colocado em qualquer lugar, desde que seja chamado antes de precisar de utilizá-lo. Colocámos este código no evento Início da Aplicação no ficheiro Global.asax, para que seja executado uma vez no início e disponibilize o segredo para a aplicação.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>Adicionar definições da aplicação no Portal do Azure

Na Aplicação Web do Azure, agora pode adicionar os valores reais para AppSettings no portal do Azure. Ao executar este passo, os valores reais não estarão no ficheiro web.config, mas estarão protegidos através do Portal onde tem capacidades de controlo de acesso separadas. Estes valores serão substituídos pelos valores que introduziu no ficheiro web.config. Certifique-se de que os nomes são os mesmos.

![Definições da Aplicação apresentadas no portal do Azure][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Fazer a autenticação com um certificado em vez de um segredo do cliente

Agora que compreende a autenticação de uma aplicação do Azure AD com o ID de Cliente e o Segredo do Cliente, vamos utilizar um ID de Cliente e um certificado. Para utilizar um certificado numa Aplicação Web do Azure, utilize os seguintes passos:

1. Obter ou criar um Certificado
2. Associar o certificado a uma aplicação do Azure AD
3. Adicionar código à sua aplicação Web para utilizar o certificado
4. Adicionar um certificado à sua aplicação Web

### <a name="get-or-create-a-certificate"></a>Obter ou criar um certificado

 Vamos fazer um certificado de teste para este tutorial. Eis um script para criar um certificado autoassinado. Mude para o diretório onde pretende que os ficheiros de certificado sejam criados.  Para a data de início e de fim do certificado, pode utilizar a data atual acrescida de um ano.

```powershell
#Create self-signed certificate and export pfx and cer files 
$PfxFilePath = "c:\data\KVWebApp.pfx" 
$CerFilePath = "c:\data\KVWebApp.cer" 
$DNSName = "MyComputer.Contoso.com" 
$Password ="MyPassword" 
$SecStringPw = ConvertTo-SecureString -String $Password -Force -AsPlainText 
$Cert = New-SelfSignedCertificate -DnsName $DNSName -CertStoreLocation "cert:\LocalMachine\My" -NotBefore 05/15/2018 -NotAfter 05/15/2019 
Export-PfxCertificate -cert $cert -FilePath $PFXFilePath -Password $SecStringPw 
Export-Certificate -cert $cert -FilePath $CerFilePath 
```

Tome nota da data de fim e da palavra-passe para o .pfx (neste exemplo: 15 de maio de 2019 e MyPassword). Precisará destes dados para o script abaixo. 
### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Associar o certificado a uma aplicação do Azure AD

Agora que já tem um certificado, terá de associá-lo a uma aplicação do Azure AD. A associação pode ser concluída através do PowerShell. Execute os comandos seguintes para associar o certificado à aplicação do Azure AD:

```powershell
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets all -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Depois de executar estes comandos, pode ver a aplicação no Azure AD. Ao pesquisar os registos da aplicação, certifique-se de que seleciona **As minhas aplicações** em vez de "Todas as aplicações" na caixa de diálogo de pesquisa. 

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Adicionar código à sua aplicação Web para utilizar o certificado

Agora, vamos adicionar código à sua Aplicação Web para aceder ao certificado e utilizá-lo para a autenticação. 

Em primeiro lugar, há código para aceder ao certificado. Note que StoreLocation é CurrentUser em vez de LocalMachine. Note também que estamos a indicar «false» para o método Find porque estamos a utilizar um certificado de teste.

```cs
//Add this using statement
using System.Security.Cryptography.X509Certificates;  

public static class CertificateHelper
{
    public static X509Certificate2 FindCertificateByThumbprint(string findValue)
    {
        X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
        try
        {
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                findValue, false); // Don't validate certs, since the test root isn't installed.
            if (col == null || col.Count == 0)
                return null;
            return col[0];
        }
        finally
        {
            store.Close();
        }
    }
}
```



Em seguida, há código que utiliza o CertificateHelper e cria um ClientAssertionCertificate que é necessário para a autenticação.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Eis o novo código para obter o token de acesso. Este código substitui o método GetToken no exemplo anterior. Demos-lhe um nome diferente para sua conveniência. Colocámos todo este código na classe Utils do nosso projeto de Aplicação Web para ser mais fácil de utilizar.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```



A última alteração de código é no método Application_Start. Primeiro, precisamos de chamar o método GetCert() para carregar o ClientAssertionCertificate. E, em seguida, alteramos o método de chamada de retorno que fornecemos ao criar um novo KeyVaultClient. Este código substitui o código que tínhamos no exemplo anterior.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Adicionar um certificado à sua aplicação Web através do Portal do Azure

A adição de um certificado à sua Aplicação Web é um processo de dois passos simples. Em primeiro lugar, aceda ao portal do Azure e navegue até à sua Aplicação Web. Nas Definições da sua Aplicação Web, clique na entrada relativa a **definições de SSL**. Quando abrir, carregue o Certificado que criou no exemplo anterior, KVWebApp.pfx. Certifique-se de que se lembra da palavra-passe para o pfx.

![Adicionar um Certificado a uma Aplicação Web no portal do Azure][2]

A última coisa que precisa de fazer é adicionar uma Definição de Aplicação à sua Aplicação Web com o nome WEBSITE\_LOAD\_CERTIFICATES e o valor *. Este passo irá garantir que todos os certificados são carregados. Se quisesse carregar apenas os Certificados que enviou, podia introduzir uma lista separada por vírgulas dos respetivos thumbprints.


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, elimine o serviço de aplicações, o cofre de chaves e a aplicação do Azure AD que utilizou para o tutorial.  


## <a id="next"></a>Passos seguintes
> [!div class="nextstepaction"]
>[Referência da API de Gestão do Azure Key Vault](/dotnet/api/overview/azure/keyvault/management).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
 