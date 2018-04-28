---
title: Tutorial - Conceder acesso a uma API Web ASP.NET Core a partir de uma aplicação de página única com o Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Active Directory B2C para proteger uma API Web .NET Core e chamá-la a partir de uma aplicação de página única.
services: active-directory-b2c
author: davidmu1
ms.author: davidmu
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 0e9e3074e2cdd9ec3adc814779811d150cd11010
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-app-using-azure-active-directory-b2c"></a>Tutorial - Conceder acesso a uma API Web ASP.NET Core a partir de uma aplicação de página única com o Azure Active Directory B2C

Este tutorial mostra-lhe como chamar um recurso de API Web ASP.NET Core protegida com o Azure Active Directory (Azure AD) B2C a partir de uma aplicação de página única.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar uma API Web no seu inquilino do Azure AD B2C
> * Definir e configurar âmbitos para uma API Web
> * Conceder permissões de acesso à API Web à sua aplicação
> * Atualizar o código de exemplo para utilizar o Azure AD B2C para proteger uma API Web

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Conclua o tutorial [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de página única](active-directory-b2c-tutorials-spa.md).
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com a carga de trabalho de **desenvolvimento na Web e em ASP.NET**.
* [SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior
* Instalar o [Node. js](https://nodejs.org/en/download/)

## <a name="register-web-api"></a>Registar uma API Web

Os recursos da API Web têm de ser registados no seu inquilino antes de poderem aceitar e responder a [pedidos de recursos protegidos](../active-directory/develop/active-directory-dev-glossary.md#resource-server) por parte de [aplicações cliente](../active-directory/develop/active-directory-dev-glossary.md#client-application) que apresentem [tokens de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) a partir do Azure Active Directory. O registo estabelece o [objeto da aplicação e do principal de serviço](../active-directory/develop/active-directory-dev-glossary.md#application-object) no seu inquilino. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecione **Azure AD B2C** na lista de serviços no portal do Azure.

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**.

    Para registar a aplicação Web de exemplo no inquilino, utilize as seguintes definições.
    
    ![Adicionar uma API nova](media/active-directory-b2c-tutorials-spa-webapi/web-api-registration.png)
    
    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | Hello Core API | Introduza um **Nome** que descreva a API Web aos programadores. |
    | **Incluir aplicação/API Web** | Sim | Selecione **Sim** para uma API Web. |
    | **Permitir fluxo implícito** | Sim | Selecione **Sim** se a aplicação utilizar o [Início de sessão OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de resposta** | `http://localhost:44332` | Os URLs de resposta são pontos finais para onde o Azure AD B2C devolve quaisquer tokens que a aplicação peça. Neste tutorial, a API Web de exemplo é executada localmente (localhost) e escuta na porta 5000. |
    | **URI do ID da Aplicação** | HelloCoreAPI | O URI identifica exclusivamente a API no inquilino. Isto permite-lhe registar várias APIs por inquilino. Os [âmbitos](../active-directory/develop/active-directory-dev-glossary.md#scopes) regem o acesso ao recurso protegido da API e são definidos por URI de ID de Aplicação. |
    | **Cliente nativo** | Não | Uma vez que se trata de uma API Web e não de um cliente nativo, selecione Não. |
    
3. Clique em **Criar** para registar a API.

As APIs registadas são apresentadas na lista de aplicações do inquilino do Azure AD B2C. Selecione a API Web na lista. É apresentado o painel de propriedades da API Web.

![Propriedades da API Web](./media/active-directory-b2c-tutorials-spa-webapi/b2c-web-api-properties.png)

Anote o **ID de Cliente da Aplicação**. O ID identifica exclusivamente a API e é necessário quando a configurar mais tarde no tutorial.

Registar a API Web no Azure AD B2C define uma relação de confiança. Uma vez que a API está registada no B2C, a API pode agora confiar nos tokens de acesso do B2C que recebe de outras aplicações.

## <a name="define-and-configure-scopes"></a>Definir e configurar âmbitos

Os [âmbitos](../active-directory/develop/active-directory-dev-glossary.md#scopes) proporcionam uma forma de reger o acesso a recursos protegidos. São utilizados pela API Web para implementar o controlo de acesso baseado no âmbito. Por exemplo, alguns utilizadores podem ter o acesso de leitura e de escrita, ao passo que outros podem ter apenas permissões só de leitura. Neste tutorial, vai definir as permissões de leitura para a API Web.

### <a name="define-scopes-for-the-web-api"></a>Definir âmbitos para a API Web

As APIs registadas são apresentadas na lista de aplicações do inquilino do Azure AD B2C. Selecione a API Web na lista. É apresentado o painel de propriedades da API Web.

Clique em **Âmbitos publicados (Pré-visualização)**.

Para configurar âmbitos para a API, adicione as seguintes entradas. 

![âmbitos definidos na api Web](media/active-directory-b2c-tutorials-spa-webapi/scopes-web-api.png)

| Definição      | Valor sugerido  | Descrição                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Âmbito** | demo.read | Acesso de leitura à API de demonstração |

Clique em **Guardar**.

Os âmbitos publicados podem ser utilizados para conceder permissão a uma aplicação cliente à API Web.

### <a name="grant-app-permissions-to-web-api"></a>Conceder permissões de acesso à API Web à sua aplicação

Para chamar uma API Web protegida a partir de uma aplicação, tem de conceder permissões à sua aplicação à API. Neste tutorial, utilize a aplicação de página única criada em [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de página única (JavaScript)](active-directory-b2c-tutorials-spa.md).

1. No portal do Azure, selecione **Azure AD B2C** na lista de serviços e clique em **Aplicações** para ver a lista de aplicações registadas.

2. Selecione **A minha aplicação de página única de exemplo** na lista de aplicações, clique em **Acesso à API (pré-visualização)** e clique em **Adicionar**.

3. Na lista pendente **Selecionar API**, selecione a API Web registada, **Hello Core API**.

4. Na lista pendente **Selecionar Âmbitos**, selecione os âmbitos que definiu no registo da API Web.

    ![selecionar âmbitos para a aplicação](media/active-directory-b2c-tutorials-spa-webapi/selecting-scopes-for-app.png)

5. Clique em **OK**.

**A minha aplicação de página única de exemplo** está registada para chamar a **Hello Core API** protegida. Para utilizar a aplicação de ambiente de trabalho WPF, os utilizadores [autenticam-se](../active-directory/develop/active-directory-dev-glossary.md#authentication) com o Azure AD B2C. A aplicação de ambiente de trabalho obtém uma [concessão de autorização](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) do Azure AD B2C para aceder à API Web protegida.

## <a name="update-code"></a>Atualizar código

Agora que a API Web está registada e já estão definidos âmbitos, tem de configurar o código da API Web para utilizar o seu inquilino do Azure AD B2C. Neste tutorial, irá configurar uma aplicação Web .NET Core de exemplo que pode transferir a partir do GitHub. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) ou clone a aplicação Web de exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Configurar a API Web

1. Abra a solução **B2C-WebAPI.sln** no Visual Studio.

2. Abra o ficheiro **appsettings.json**. Atualize os valores seguintes para configurar a API Web para utilizar o seu inquilino:

    ```javascript
    "AzureAdB2C": 
      {
        "Tenant": "<your tenant name>.onmicrosoft.com", 
        "ClientId": "<The Application ID for your web API obtained from the Azure portal>",
        "Policy": "<Your sign up sign in policy e.g. B2C_1_SiUpIn>",
        "ScopeRead": "demo.read"  
      },
    ```

#### <a name="enable-cors"></a>Ativar o CORS

Para permitir que a aplicação de página única chame a API Web ASP.NET Core, tem de ativar o [CORS](https://docs.microsoft.com/aspnet/core/security/cors).

1. Em **Startup.cs**, adicione o CORS ao método `ConfigureServices()`.

    ```C#
    public void ConfigureServices(IServiceCollection services) {
      services.AddCors();
    ```

2. Em **Startup.cs**, configure o CORS no método `Configure()`.

    ```C#
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
      app.UseCors(builder =>
        builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

### <a name="configure-the-single-page-app"></a>Configurar a aplicação de página única

A aplicação de página única utiliza o Azure AD B2C para inscrição e início de sessão de utilizadores, e chama a API Web ASP.NET Core protegida. Tem de atualizar a chamada da aplicação de página única da API Web .NET Core.
Para alterar as definições da aplicação:

1. Abra o ficheiro `index.html` no exemplo de aplicação de página única Node.js.
2. Configure o exemplo com as informações de registo de inquilino do Azure AD B2C. Altere os valores **b2cScopes** e **webApi** nas linhas de código seguintes:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_SiUpIn",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/HelloCoreAPI/demo.read"],
        webApi: 'http://localhost:58553/api/values',
    };
    ```

## <a name="run-the-spa-app-and-web-api"></a>Executar a API de aplicação SPA e Web

Tem de executar a aplicação de página única Node.js e a API Web .NET Core.

### <a name="run-the-aspnet-core-web-api"></a>Executar a API Web ASP.NET Core 

Prima **F5** para depurar a solução **B2C-WebAPI.sln** no Visual Studio.

Quando o projeto é iniciado, é apresentada uma página Web no browser predefinido a anunciar que a API Web está disponível para pedidos.

### <a name="run-the-single-page-app"></a>Executar a aplicação de página única

1. Inicie uma linha de comandos Node.js.
2. Mude para o diretório que contém o exemplo de Node.js. Por exemplo, `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Execute os seguintes comandos:
    ```
    npm install && npm update
    node server.js
    ```

    A janela da consola mostra o número de porta onde está alojada a aplicação.
    
    ```
    Listening on port 6420...
    ```

4. Utilize um browser para navegar para o endereço `http://localhost:6420` para ver a aplicação.
5. Inicie sessão com o endereço de e-mail e a palavra-passe utilizados em [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de página única (JavaScript)](active-directory-b2c-tutorials-spa.md).
6. Clique no botão **Chamar API**.

Depois de se inscrever ou iniciar sessão com uma conta de utilizador, o exemplo chama a API Web protegida e devolve um resultado.

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos seguintes

Este artigo mostrou-lhe como proteger uma API Web ao registar e definir âmbitos no Azure AD B2C. Saiba mais com os exemplos de código do Azure AD B2C disponíveis.

> [!div class="nextstepaction"]
> [Exemplos de código do Azure AD B2C](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
