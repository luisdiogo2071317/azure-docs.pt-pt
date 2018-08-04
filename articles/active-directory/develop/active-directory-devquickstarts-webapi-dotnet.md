---
title: API de Web .NET do Azure AD de introdução | Documentos da Microsoft
description: Como criar uma API que se integra com o Azure AD para autenticação e autorização da web de MVC do .NET.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: ca506d821fe3534468c0d370dd51464e5df90f79
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504666"
---
# <a name="azure-ad-net-web-api-getting-started"></a>API de Web .NET do Azure AD de introdução
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Se estiver criando um aplicativo que fornece acesso a recursos protegidos, precisa saber como impedir injustificado acesso a esses recursos.
Ele simples ajudar a proteger uma API web utilizando o acesso de portador do OAuth 2.0 e tokens com apenas algumas linhas de código do Azure torna o Active Directory (Azure AD).

Nas aplicações web do ASP.NET, pode conseguir essa proteção utilizando a implementação Microsoft do middleware da OWIN dirigidas pela Comunidade incluído no .NET Framework 4.5. Aqui, usaremos OWIN para criar uma API da web de "Lista de tarefas" que:

* Designa que APIs são protegidos.
* Valida que as chamadas de web API contém um token de acesso válido.

Para criar o para fazer API de lista, terá primeiro de:

1. Registar uma aplicação com o Azure AD.
2. Configure a aplicação para utilizar o pipeline de autenticação OWIN.
3. Configure uma aplicação de cliente para chamar a API web.

Para começar, [transferir a estrutura de aplicação](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) ou [transfira o exemplo concluído](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Cada um é uma solução do Visual Studio 2013. Também precisa de um inquilino do Azure AD na qual é possível registar a sua aplicação. Se ainda não tiver um, [Saiba como obter um](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Passo 1: Registar uma aplicação com o Azure AD
Para ajudar a proteger a sua aplicação, tem primeiro de criar uma aplicação no seu inquilino e fornecer algumas informações cruciais do Azure AD.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Escolha o seu inquilino do Azure AD ao clicar na sua conta no canto superior direito da página, em seguida, clicando no **trocar diretório** navegação e, em seguida, selecione o inquilino adequado.
 * Ignore este passo, se tiver apenas um inquilino do Azure AD na sua conta ou se já tiver selecionado o adequado inquilino do Azure AD.

3. No painel de navegação da esquerda, clique em **Azure Active Directory**.

4. Clique em **registos de aplicações**e, em seguida, selecione **Add**.

5. Siga as instruções e crie um novo **aplicação Web e/ou Web API**.
  * **Nome** descreve a sua aplicação aos utilizadores. Introduza **para listar serviço**.
  * **Uri de redirecionamento** é uma combinação de esquema e a cadeia de caracteres que o Azure AD utiliza para devolver quaisquer tokens que solicitou a sua aplicação. Introduza `https://localhost:44321/` para este valor.

6. Partir do **definições** -> **propriedades** página para a sua aplicação, atualize o URI de ID de aplicação. Introduza um identificador de inquilino específico. Por exemplo, introduza `https://contoso.onmicrosoft.com/TodoListService`.

7. Guarde a configuração. Deixe o portal aberta, uma vez que também precisará registar a aplicação de cliente em breve.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Passo 2: Configurar a aplicação para utilizar o pipeline de autenticação OWIN
Para validar a entrada de pedidos e tokens, terá de configurar a aplicação para comunicar com o Azure AD.

1. Para começar, abra a solução e adicione os pacotes de NuGet de middleware do OWIN para o projeto de TodoListService utilizando a consola do Gestor de pacotes.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Adicionar uma classe de Startup da OWIN para o projeto de TodoListService chamado `Startup.cs`.  Com o botão direito no projeto, selecione **Add** > **Novo Item**e, em seguida, procure **OWIN**. O middleware da OWIN vai invocar o método `Configuration(…)` quando a aplicação for iniciada.

3. Alterar a declaração de classe para `public partial class Startup`. Já Implementámos parte dessa classe para em outro ficheiro. Na `Configuration(…)` método, crie uma chamada para `ConfgureAuth(…)` para configurar a autenticação para a sua aplicação web.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Abra o ficheiro `App_Start\Startup.Auth.cs` e implemente o `ConfigureAuth(…)` método. Os parâmetros fornecidos na `WindowsAzureActiveDirectoryBearerAuthenticationOptions` irá servir como coordenadas para a sua aplicação para comunicar com o Azure AD.

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Agora, pode utilizar `[Authorize]` atributos para ajudar a proteger os controladores e ações com a autenticação de portador do JSON Web Token (JWT). Decorar o `Controllers\TodoListController.cs` classe com uma etiqueta de autorizar. Isto irá forçar o utilizador iniciar sessão antes de aceder a essa página.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Quando um chamador autorizado com êxito invoca um do `TodoListController` APIs, a ação poderá ter acesso a informações sobre o autor da chamada. OWIN fornece acesso às declarações dentro do token de portador através do `ClaimsPrincpal` objeto.  

6. Um requisito comum das APIs Web é validar os “âmbitos” presentes no token. Isto garante que o utilizador consentiu as permissões necessárias para aceder para fazer lista serviço.

    ```csharp
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Abra o `web.config` do ficheiro na raiz do projeto TodoListService e introduza os valores de configuração no `<appSettings>` secção.
  * `ida:Tenant` é o nome do inquilino do Azure AD – por exemplo, contoso.onmicrosoft.com.
  * `ida:Audience` é o URI de ID de aplicação do aplicativo que introduziu no portal do Azure.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Passo 3: Configurar uma aplicação cliente e executar o serviço
Pode ver o para fazer lista serviço em ação, terá de configurar o cliente de lista de tarefas para que possa obter tokens do Azure AD e fazer chamadas para o serviço.

1. Volte para o [portal do Azure](https://portal.azure.com).

2. Crie uma nova aplicação no seu inquilino do Azure AD e selecione **aplicação cliente nativa** no prompt resultante.
  * **Nome** descreve a sua aplicação aos utilizadores.
  * Introduza `http://TodoListClient/` para o **Uri de redirecionamento** valor.

3. Após ter concluído o registo, o Azure AD atribui um ID exclusivo para a sua aplicação. Precisará desse valor nos passos seguintes, por isso, copie-o partir da página de aplicativo.

4. Do **configurações** página, selecione **permissões obrigatórias**e, em seguida, selecione **Add**. Localize e selecione o para fazer lista serviço, adicione a **acesso TodoListService** permissão sob **permissões delegadas**e, em seguida, clique em **feito**.

5. No Visual Studio, abra `App.config` no TodoListClient projeto e, em seguida, introduza os valores de configuração no `<appSettings>` secção.

  * `ida:Tenant` é o nome do inquilino do Azure AD – por exemplo, contoso.onmicrosoft.com.
  * `ida:ClientId` é o ID de aplicação que copiou do portal do Azure.
  * `todo:TodoListResourceId` é o URI de ID de aplicação do aplicativo para o fazer de serviço de lista que introduziu no portal do Azure.

## <a name="next-steps"></a>Passos Seguintes
Por fim, limpar, criar e executar cada projeto. Se ainda não o tiver feito, agora é o momento de criar um novo utilizador no seu inquilino com um *. onmicrosoft.com domínio. Inicie sessão para o cliente de lista de tarefas com esse utilizador e adicionar algumas tarefas à lista de tarefas do utilizador.

Para referência, o exemplo concluído (sem os valores de configuração) está disponível no [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Agora pode continuar com a mais cenários de identidade.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
