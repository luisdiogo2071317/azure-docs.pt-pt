---
title: O Azure AD v2.0 início rápido para aplicações web ASP.NET Core | Documentos da Microsoft
description: Aprenda a implementar Microsoft início de sessão numa aplicação Web do ASP.NET Core através do OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4ab3d0b74e8305d67af862020197c69b15221086
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830230"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Adicionar início de sessão com a Microsoft a uma aplicação web ASP.NET Core

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Este início rápido contém um código de exemplo que demonstra como uma aplicação Web do ASP.NET Core pode iniciar sessão no pessoal (hotmail.com, live.com, outras pessoas) e profissionais e escolares contas a partir de qualquer instância do Azure Active Directory.

![Como funciona a aplicação de exemplo gerada por este Início Rápido](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registar a sua aplicação e transferir a aplicação do início rápido
>
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Registar e configurar a aplicação e o exemplo de código
> #### <a name="step-1-register-your-application"></a>Passo 1: Registar a aplicação
> 
> 1. Aceda ao [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app).
> 1. Introduza um nome para a sua aplicação, certifique-se de que a opção **Guided Setup** (Configuração Assistida) está desmarcada e clique em **Create** (Criar).
> 1. Clique em `Add Platform` e selecione `Web`.
> 1. Certifique-se de que a caixa **Permitir Fluxo Implícito** está *marcada*.
> 1. Em **URLs de redirecionamento**, introduza `http://localhost:3110/`.
> 1. Desloque a página até ao fim e clique em **Guardar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Passo 1: Configurar a aplicação no portal do Azure
> Para o código de exemplo deste início rápido funcionar, terá de adicionar um URL de resposta como `http://localhost:3110/`.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Fazer esta alteração por mim]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Já configurada](media/quickstart-v2-aspnet-core-webapp/green-check.png) A sua aplicação está configurada com este atributo

#### <a name="step-2-download-your-aspnet-core-project"></a>Passo 2: Transfira o seu projeto ASP.NET Core

- [Transfira o projeto do ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Passo 3: Configurar o projeto

1. Por exemplo, a extrair o ficheiro zip para uma pasta local mais próximo para a pasta de raiz - **C:\Azure-Samples**
1. Se usar o Visual Studio 2017, abra o projeto no Visual Studio (opcional)
1. Editar **appSettings** e substitua o valor para `ClientId` com o Id de aplicação a partir da aplicação que acabou de registar:

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "common"
    ```
1. Se a sua aplicação for uma *aplicação de inquilino único* (objetivando contas no diretório atual apenas), na sua **appSettings** ficheiro, localize o valor para `TenantId` e substitua `common`com o seu **Id do inquilino** ou **nome do inquilino** (por exemplo, contoso.microsoft.com). Pode obter o nome do inquilino na **página de descrição geral**.

## <a name="more-information"></a>Mais informações

Esta secção fornece uma descrição geral do código necessário para o início de sessão dos utilizadores. Isso pode ser útil para entender como o código funciona, os principais argumentos e também se pretende adicionar início de sessão a uma aplicação ASP.NET Core existente.

### <a name="startup-class"></a>Classe de inicialização

*Microsoft.AspNetCore.Authentication* middleware usa uma classe de inicialização que é executada quando o processo de hospedagem inicializa:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddAzureAd(options => Configuration.Bind("AzureAd", options))
    .AddCookie();

    services.AddMvc();
}
```

O método `AddAuthentication` configura o serviço para adicionar a autenticação baseada em cookies - utilizada em cenários de browser, bem como definir o desafio para OpenIdConnect. 

A linha que contém `.AddAzureAd` adiciona a autenticação do Active Directory do Azure à sua aplicação.

Além disso, o ficheiro **AzureAdAuthenticationBuilderExtensions.cs** adiciona o método de extensão para o pipeline de autenticação do AzureAd. Esse método de extensão configurar os atributos necessários para autenticação do Azure AD. O `Configure` método na `IConfigureNamedOptions` interface contém o seguinte:

```csharp
public void Configure(string name, OpenIdConnectOptions options)
{
    options.ClientId = _azureOptions.ClientId;
    options.Authority = $"{_azureOptions.Instance}common/v2.0";   // V2 specific
    options.UseTokenLifetime = true;
    options.RequireHttpsMetadata = false;
    options.TokenValidationParameters.ValidateIssuer = false;     // accept several tenants (here simplified)
}
```
> |Onde  |  |
> |---------|---------|
> |ClientId     |Id da aplicação da aplicação registada no portal do Azure|
> |Autoridade | O utilizador fo ponto de extremidade STS que se autenticar. Normalmente https://login.microsoftonline.com/{tenant}/v2.0 para a cloud pública, em que {tenant} é o nome do seu inquilino, o Id do seu inquilino, ou *common* para uma referência ao ponto final comum (utilizado para aplicações multi-inquilino)|
> |UseTokenLifetime |Indica que o cookie de autenticação deve corresponder ao que o token de autenticação|
> |RequireHttpsMetadata     |Exigir HTTPS para o endereço de metadados ou autoridade. É recomendado alterar este valor para `True`|
> |TokenValidationParameters     | Uma lista de parâmetros para a validação do token. Neste caso, `ValidateIssuer` está definido como `false` para indicar que ele pode aceitar inícios de sessão de qualquer pessoais, ou contas escolares ou profissionais|

### <a name="initiate-an-authentication-challenge"></a>Iniciar um desafio de autenticação

Pode forçar o utilizador iniciar sessão, solicitando um desafio de autenticação no seu controlador de tal como na **AccountController.cs**:

```csharp
public IActionResult SignIn()
{
    var redirectUrl = Url.Action(nameof(HomeController.Index), "Home");
    return Challenge(
        new AuthenticationProperties { RedirectUri = redirectUrl },
        OpenIdConnectDefaults.AuthenticationScheme);
}
```

> [!TIP]
> Pedir um desafio de autenticação usando o método acima é opcional e commonsly usadas quando desejar uma vista para estar disponível para utilizadores autenticados e sem autenticação. Pode proteger controladores usando o método descrito na secção seguinte.

### <a name="protect-a-controller-or-a-controllers-method"></a>Proteger um controlador ou o método de um controlador

Pode proteger um controlador ou controlador methodss utilizando o `[Authorize]` atributo. Este atributo restringe o acesso para o controlador ou métodos, permitindo apenas que usuários autenticados - o que significa que o desafio de autenticação pode ser iniciado para acessar o controlador, se o utilizador não está autenticado.

## <a name="next-steps"></a>Próximos Passos

Veja o repositório do GitHub para este guia de introdução do ASP.NET Core para obter mais informações - incluindo instruções sobre como adicionar autenticação a uma aplicação ASP.NET Core Web totalmente nova:

> [!div class="nextstepaction"]
> [Exemplo de código da aplicação Web ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]