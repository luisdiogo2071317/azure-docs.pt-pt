---
title: Início rápido do servidor Web ASP.NET do Azure AD v2.0 | Microsoft Docs
description: Saiba como implementar o Início de sessão com a Microsoft numa aplicação Web ASP.NET com o OpenID Connect.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: cabc4f2212e18d774066efad70d9654bb70020d5
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831505"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Início Rápido: adicionar início de sessão com a Microsoft a uma aplicação Web ASP.NET

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Neste início rápido, vai ficar a saber como uma aplicação Web ASP.NET pode iniciar sessão em contas pessoais (hotmail.com, outlook.com, entre outras) e em contas escolares e profissionais a partir de qualquer instância do Azure Active Directory (Azure AD).

![Como funciona a aplicação de exemplo gerada por este início rápido](media/quickstart-v2-aspnet-webapp/aspnetwebapp-intro.png)

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
> 1. Em **URLs de redirecionamento**, introduza `https://localhost:44368/`.
> 1. Desloque a página até ao fim e clique em **Guardar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Passo 1: Configurar a aplicação no portal do Azure
> Para o código de exemplo deste início rápido funcionar, terá de adicionar um URL de resposta como `https://localhost:44368/`.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Fazer esta alteração por mim]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Já configurada](media/quickstart-v2-aspnet-webapp/green-check.png) A sua aplicação está configurada com este atributo

#### <a name="step-2-download-your-project"></a>Passo 2: Transferir o projeto

[Transfira a solução do Visual Studio 2017](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Passo 3: Configurar o projeto do Visual Studio

1. Extraia o ficheiro zip para uma pasta local próxima da pasta raiz, por exemplo, **C:\Azure-Samples**
1. Abra a solução no Visual Studio (AppModelv2-WebApp-OpenIDConnect-DotNet.sln)
1. Edite o ficheiro **Web.config** e substitua `Enter_the_Application_Id_here` pelo ID de Aplicação da aplicação que acabou de registar:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    ```
    
> [!div class="sxs-lookup" renderon="portal"]
> [!IMPORTANT]
> Se a sua aplicação for uma *aplicação de inquilino único* (que visa contas apenas neste diretório), no ficheiro **Web.config**, localize o valor para `Tenant` e substitua `common` pelo **Id do inquilino** ou **Nome do inquilino** (por exemplo, contoso.microsoft.com). Pode obter o nome do inquilino na **página de descrição geral**.

## <a name="more-information"></a>Mais informações

Esta secção fornece uma descrição geral do código necessário para o início de sessão dos utilizadores. Pode ser útil para compreender como o código funciona, os principais argumentos e também se pretende adicionar início de sessão a uma aplicação ASP.NET existente.

### <a name="owin-middleware-nuget-packages"></a>Pacotes NuGet de middleware OWIN

Pode configurar o pipeline de autenticação com a autenticação baseada em cookies através do OpenID Connect em ASP.NET com pacotes de Middleware OWIN. Pode instalar estes pacotes ao executar os comandos seguintes na **Consola do Gestor de Pacotes** do Visual Studio:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="owin-startup-class"></a>Classe de Arranque OWIN

O middleware OWIN utiliza uma *classe de arranque* que é executada quando o processo de alojamento é inicializado (no caso deste início rápido, o ficheiro *startup.cs* localizado na pasta raiz). O código seguinte mostra o parâmetro utilizado por este início rápido:

```csharp
public void Configuration(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());
    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            // Sets the ClientId, authority, RedirectUri as obtained from web.config
            ClientId = clientId,
            Authority = authority,
            RedirectUri = redirectUri,
            // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
            PostLogoutRedirectUri = redirectUri,
            Scope = OpenIdConnectScope.OpenIdProfile,
            // ResponseType is set to request the id_token - which contains basic information about the signed-in user
            ResponseType = OpenIdConnectResponseType.IdToken,
            // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
            // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
            // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter 
            TokenValidationParameters = new TokenValidationParameters()
            {
                ValidateIssuer = false
            },
            // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = OnAuthenticationFailed
            }
        }
    );
}
```

> |Onde  |  |
> |---------|---------|
> | `ClientId`     | O ID de Aplicação da aplicação registada no portal do Azure |
> | `Authority`    | O ponto final STS para o utilizador autenticar. Normalmente https://login.microsoftonline.com/{tenant}/v2.0 para a cloud pública, em que {tenant} é o nome do seu inquilino, o Id do seu inquilino, ou *common* para uma referência ao ponto final comum (utilizado para aplicações multi-inquilino) |
> | `RedirectUri`  | URL para onde os utilizadores são enviados após a autenticação no ponto final v2.0 do Azure AD |
> | `PostLogoutRedirectUri`     | URL para onde os utilizadores são enviados depois de terminarem sessão |
> | `Scope`     | A lista dos âmbitos que estão a ser solicitados, separados por espaços |
> | `ResponseType`     | Pedido de que a resposta da autenticação contenha um token de ID |
> | `TokenValidationParameters`     | Uma lista de parâmetros para a validação do token. Neste caso, `ValidateIssuer` está definido como `false` para indicar que pode aceitar inícios de sessão de todos os tipos de conta: pessoal, escolar ou profissional |
> | `Notifications`     | Uma lista de delegados que podem ser executados em diferentes mensagens *OpenIdConnect* |

### <a name="initiate-an-authentication-challenge"></a>Iniciar um desafio de autenticação

Pode forçar um utilizador a iniciar sessão, solicitando um desafio de autenticação no seu controlador:

```csharp
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

> [!TIP]
> Pedir um desafio de autenticação com o método acima é opcional e, normalmente, utilizado quando pretende que uma vista seja acessível para utilizadores autenticados e não autenticados. Em alternativa, pode proteger os controladores utilizando o método descrito na secção seguinte.

### <a name="protect-a-controller-or-a-controllers-method"></a>Proteger um controlador ou o método de um controlador

Pode proteger um controlador ou as ações do controlador com o atributo `[Authorize]`. Este atributo restringe o acesso ao controlador ou às ações ao permitir que apenas os utilizadores autenticados acedam às ações no controlador, o que significa que o desafio de autenticação acontecerá automaticamente quando um utilizador *não autenticado* tentar aceder a uma das ações ou ao controlador decorado pelo atributo `[Authorize]`.

## <a name="next-steps"></a>Passos seguintes

Experimente o tutorial para ASP.NET para obter um guia passo a passo completo sobre a criação de aplicações e novas funcionalidades, incluindo uma explicação completa deste início rápido.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Conheça os passos para criar a aplicação utilizada neste início rápido

> [!div class="nextstepaction"]
> [Tutorial de início de sessão](.\tutorial-v2-asp-webapp.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]