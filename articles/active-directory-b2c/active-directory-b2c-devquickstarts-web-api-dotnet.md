---
title: Chamar um segura do ASP.NET web api no Azure Active Directory B2C | Documentos da Microsoft
description: Como criar uma aplicação .NET Web e chamar uma web api com os tokens de acesso do Azure Active Directory B2C e o OAuth 2.0.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 38ad6d8f3746d856d8c0a73520d55fad9d8344e6
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157926"
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>O Azure AD B2C: Chamar uma API web .NET a partir de uma aplicação web .NET

Ao utilizar o Azure AD B2C, pode adicionar funcionalidades de gestão de identidade poderosas para as suas aplicações web e web APIs. Este artigo discute como pedir tokens de acesso e chamadas de marca de uma aplicação web do .NET "lista de tarefas" para um .NET api da web.

Este artigo não abrange como implementar o início de sessão, inscrição e gestão de perfis com o Azure AD B2C. Ele se concentra nas chamadas das APIs web depois do utilizador já é autenticado. Se ainda não o fez, deve:

* Comece com um [aplicação web .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Comece com um [.NET api da web](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Pré-requisito

Para criar uma aplicação web que chama uma web api, terá de:

1. [Criar um inquilino do Azure AD B2C](active-directory-b2c-get-started.md).
2. [Registar uma web api](active-directory-b2c-app-registration.md#register-a-web-api).
3. [Registar uma aplicação web](active-directory-b2c-app-registration.md#register-a-web-app).
4. [Definir políticas](active-directory-b2c-reference-policies.md).
5. [Conceder permissões de aplicação para utilizar a web de web api](active-directory-b2c-access-tokens.md#publishing-permissions).

> [!IMPORTANT]
> A aplicação cliente e a API Web têm de utilizar o mesmo diretório do Azure AD B2C.
>

## <a name="download-the-code"></a>Transferir o código

O código deste tutorial é mantido no [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Pode clonar o exemplo ao executar:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Depois de transferir o código de exemplo, abra o ficheiro de .sln do Visual Studio para começar a utilizar. O ficheiro de solução contém dois projetos: `TaskWebApp` e `TaskService`. `TaskWebApp` é um aplicativo da web MVC que o usuário interage com. O `TaskService` é uma API Web de back-end da aplicação que armazena a lista de tarefas de cada utilizador. Este artigo não abrange a criação da `TaskWebApp` aplicação web ou o `TaskService` web api. Para saber como criar a aplicação web .NET com o Azure AD B2C, consulte nosso [tutorial da aplicação web .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md). Para saber como criar o .NET API web protegida com o Azure AD B2C, consulte nosso [tutorial de web API .NET](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Atualizar a configuração do Azure AD B2C

O nosso exemplo está configurado para utilizar as políticas e o ID de cliente do nosso inquilino de demonstração. Se gostaria de utilizar o seu inquilino:

1. Abra `web.config` no projeto `TaskService` e substitua os valores de:

    * `ida:Tenant` pelo nome do seu inquilino
    * `ida:ClientId` com o seu ID de aplicação de api web
    * `ida:SignUpSignInPolicyId` pelo nome da sua política de “Inscrição ou Início de Sessão”

2. Abra `web.config` no projeto `TaskWebApp` e substitua os valores de:

    * `ida:Tenant` pelo nome do seu inquilino
    * `ida:ClientId` pelo ID da sua aplicação Web
    * `ida:ClientSecret` pela chave de segredo da sua aplicação Web
    * `ida:SignUpSignInPolicyId` pelo nome da sua política de “Inscrição ou Início de Sessão”
    * `ida:EditProfilePolicyId` pelo nome da sua política de “Editar Perfil”
    * `ida:ResetPasswordPolicyId` pelo nome da sua política de “Repor Palavras-Passe”



## <a name="requesting-and-saving-an-access-token"></a>Pedir e guardar um token de acesso

### <a name="specify-the-permissions"></a>Especifique as permissões

Para fazer a chamada para a API web, tem de autenticar o utilizador (usando a política de início de sessão-inscrição/início de sessão) e [receber um token de acesso](active-directory-b2c-access-tokens.md) do Azure AD B2C. Para receber um token de acesso, primeiro tem de especificar as permissões que pretende que o token de acesso para conceder. As permissões estão especificadas na `scope` parâmetro ao fazer o pedido para o `/authorize` ponto final. Por exemplo, para adquirir um token de acesso com a permissão "ler" para a aplicação de recurso que tem o URI de ID de aplicação do `https://contoso.onmicrosoft.com/tasks`, o escopo seria `https://contoso.onmicrosoft.com/tasks/read`.

Para especificar o âmbito em nosso exemplo, abra o ficheiro `App_Start\Startup.Auth.cs` e defina o `Scope` variável no OpenIdConnectAuthenticationOptions.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (separated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a>O código de autorização para um token de acesso do Exchange

Depois de um utilizador conclui a experiência de inscrição ou início de sessão, a sua aplicação vai receber um código de autorização do Azure AD B2C. O middleware da OWIN OpenID Connect irá armazenar o código, mas não a trocarão para um token de acesso. Pode utilizar o [biblioteca MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para tornar o exchange. Em nosso exemplo, configuramos um retorno de chamada de notificação para o middleware de OpenID Connect sempre que for recebido um código de autorização. O retorno de chamada, utilizamos a MSAL para trocar o código de um token e guarde o token para a cache.

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract the code from the response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange the code for a token. Make sure to specify the necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-the-web-api"></a>Chamar a API web

Esta secção descreve como utilizar o token recebido durante a sessão-inscrição/início de sessão com o Azure AD B2C para acessar a API web.

### <a name="retrieve-the-saved-token-in-the-controllers"></a>Obter o token guardado em controladores de

O `TasksController` é responsável por comunicar com a API web e para enviar pedidos HTTP para a API para ler, criar e eliminar tarefas. Porque a API está protegida pelo Azure AD B2C, terá de obter primeiro o token que guardou no passo acima.

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL to retrieve the token from the cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve the token using the provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-the-web-api"></a>Ler tarefas da API web

Quando tem um token, pode anexá-lo para o HTTP `GET` pedir dos `Authorization` cabeçalho para chamar com segurança `TaskService`:

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve the token with the specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token to the Authorization header and make the request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Criar e eliminar tarefas na API web

Siga o mesmo padrão, quando envia `POST` e `DELETE` pedidos para a API web, através de MSAL para obter o token de acesso da cache.

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo

Por fim, crie e execute as duas aplicações. Inscreva-se e iniciar sessão para criar tarefas para o utilizador com sessão iniciada. Terminar sessão e inicie sessão como um utilizador diferente. Crie tarefas para esse utilizador. Observe como as tarefas são armazenadas por utilizador na API, porque a API extrai a identidade do utilizador do token que recebe. Experimente também a brincar com os âmbitos. Remova a permissão "escrever" e, em seguida, tente adicionar uma tarefa. Apenas Certifique-se terminar sessão sempre que alterar o âmbito.

