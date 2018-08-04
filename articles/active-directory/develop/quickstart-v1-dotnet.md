---
title: O Azure AD .NET ambiente de trabalho (WPF) de introdução | Documentos da Microsoft
description: Como criar uma aplicação de ambiente de trabalho do .NET Windows que se integra com o Azure AD para início de sessão e chama do Azure AD protegidos as APIs com o OAuth.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e7a662afd78ff053c60aeb5de3af9f0070a7c458
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506529"
---
# <a name="azure-ad-net-desktop-wpf-getting-started"></a>O Azure AD .NET ambiente de trabalho (WPF) de introdução
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Se estiver desenvolvendo um aplicativo de desktop, do Azure AD torna simples e direto para que possa autenticar os utilizadores com as suas contas do Active Directory. Também permite que seu aplicativo consumir com segurança qualquer API web protegida pelo Azure AD, como as APIs do Office 365 ou a API do Azure.

Para .NET clientes nativos que precisam de aceder a recursos protegidos, o Azure AD fornece o Active Directory Authentication Library ou o ADAL. Única finalidade da ADAL é torna mais fácil para a sua aplicação obter os tokens de acesso. Para demonstrar apenas como é fácil, aqui vamos criar um aplicativo de lista de tarefas de WPF do .NET que:

* Obtém acesso tokens para chamar o Azure AD Graph API utilizando o [protocolo de autenticação OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Procura um diretório para os utilizadores com um alias de determinado.
* Utilizadores de sinais horizontalmente.

Para criar a aplicação de trabalho completo, precisará:

1. Registe a sua aplicação com o Azure AD.
2. Instalar e configurar o ADAL.
3. Utilize a ADAL para obter os tokens do Azure AD.

Para começar, [transferir a estrutura de aplicação](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) ou [transfira o exemplo concluído](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Também precisará um inquilino do Azure AD em que pode criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino [Saiba como obter um](quickstart-create-new-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>1. Registar a aplicação de DirectorySearcher
Para ativar a sua aplicação obter os tokens, terá primeiro de registá-lo no seu inquilino do Azure AD e conceder permissão para aceder à API do Azure AD Graph:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Na barra superior, clique na sua conta e, no **Directory** lista, escolha o inquilino do Active Directory em que deseja registar a sua aplicação.
3. Clique em **todos os serviços** na barra de navegação esquerda e escolha **Azure Active Directory**.
4. Clique em **registos de aplicações** e escolha **Add**.
5. Siga as instruções e crie um novo **aplicação cliente nativa**.
  * O **nome** da aplicação irá descrever seu aplicativo para os utilizadores finais
  * O **Uri de redirecionamento** é uma combinação de esquema e a cadeia de caracteres que o Azure AD irá utilizar para devolver respostas token. Introduza um valor específico para seu aplicativo, por exemplo, `http://DirectorySearcher`.
6. Depois de concluir o registo, AAD vai atribuir a aplicação um ID de aplicação exclusivo. Precisará desse valor nas próximas seções, por isso, copie-o partir da página de aplicativo.
7. Do **configurações** página, selecione **permissões obrigatórias** e escolha **Add**. Selecione o **Microsoft Graph** como a API e adicione o **Read Directory Data** permissão sob **permissões delegadas**. Isso permitirá que seu aplicativo consultar a Graph API para os utilizadores.

## <a name="2-install--configure-adal"></a>2. Instalar e configurar a ADAL
Agora que tem uma aplicação no Azure AD, pode instalar o ADAL e escrever seu código relacionadas com a identidade. Ordem da ADAL conseguir comunicar com o Azure AD, terá de fornecê-lo com algumas informações sobre o registo de aplicação.

* Comece adicionando a ADAL para o projeto de DirectorySearcher utilizando a consola do Gestor de pacotes.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* No projeto DirectorySearcher, abra `app.config`. Substitua os valores dos elementos no `<appSettings>` secção para refletir os valores de entrada no Portal do Azure. Seu código irá referenciar estes valores, sempre que utilizar a ADAL.
  * O `ida:Tenant` é o domínio de inquilino do Azure AD, por exemplo, contoso.onmicrosoft.com
  * O `ida:ClientId` é o clientId da sua aplicação que copiou do portal.
  * O `ida:RedirectUri` é o redirecionamento url registado no portal.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Utilizar a ADAL para obter os Tokens do AAD
O princípio básico por trás da ADAL é que sempre que a sua aplicação precisa de um token de acesso, ele simplesmente chama `authContext.AcquireTokenAsync(...)`, e a ADAL faz o resto. 

* Na `DirectorySearcher` projeto, abra `MainWindow.xaml.cs` e localize o `MainWindow()` método. O primeiro passo é inicializar da sua aplicação `AuthenticationContext` -da ADAL da classe principal. Isso é onde passar a ADAL as coordenadas que precisa para comunicar com o Azure AD e dizer a ele como colocar em cache tokens.

```csharp
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Localizar agora o `Search(...)` método, que será chamado quando o usuário clica no botão "Pesquisar" na IU da aplicação. Este método faz um pedido GET para o Azure AD Graph API para consulta para os utilizadores cujo UPN começa com o termo de pesquisa especificada. Mas para consultar a Graph API, tem de incluir um access_token no `Authorization` cabeçalho do pedido - este é aí que entra ADAL.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* Quando a aplicação solicita um token ao chamar `AcquireTokenAsync(...)`, ADAL irá tentar devolver um token sem pedir ao utilizador as credenciais. Se ADAL determina que o utilizador tem de iniciar sessão obter um token, ele irá apresentar uma caixa de diálogo de início de sessão, recolher as credenciais do usuário e devolver um token de autenticação com êxito. Se a ADAL não consegue devolver um token por qualquer motivo, ele irá gerar um `AdalException`.
* Tenha em atenção que o `AuthenticationResult` objeto contém um `UserInfo` objeto que pode ser usado para recolher informações de seu aplicativo pode necessitar. No DirectorySearcher, `UserInfo` é usado para personalizar a IU da aplicação com o id do utilizador.
* Quando o usuário clica no botão "Terminar sessão", queremos garantir que a chamada seguinte para `AcquireTokenAsync(...)` pedirá ao utilizador para iniciar sessão. Com a ADAL, isso é tão fácil como limpar a cache de token:

```csharp
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* No entanto, se o utilizador não clicar no botão "Terminar sessão", desejará manter a sessão do utilizador para a próxima vez que o DirectorySearcher é executada. Quando a aplicação é iniciada, pode verificar o cache de token da ADAL para um token e atualizar a interface do Usuário de maneira adequada. Na `CheckForCachedToken()` método, fazer outra chamada para `AcquireTokenAsync(...)`, desta vez passando a `PromptBehavior.Never` parâmetro. `PromptBehavior.Never` informará ADAL que não deve ser pedido ao utilizador para início de sessão e ADAL em vez disso, deve lançar uma exceção se não conseguir devolver um token.

```csharp
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user. If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Parabéns! Agora tem um aplicativo WPF do .NET que tem a capacidade para autenticar utilizadores, de forma segura chamar as APIs da Web com o OAuth 2.0 em funcionamento e obtenha informações básicas sobre o utilizador. Se ainda não o tiver feito, agora é o tempo para preencher o seu inquilino com alguns usuários. Executar a aplicação de DirectorySearcher e inicie sessão com um desses usuários. Procurar por outros utilizadores com base no respetivo UPN. Feche a aplicação e execute-a novamente. Observe como a sessão do utilizador permanece intacta. Termine sessão e volte a iniciar sessão como outro utilizador.

ADAL torna mais fácil incorporar todas estas funcionalidades de identidade comum em seu aplicativo. Ele cuida de todo o trabalho sujo para si – gestão de cache, o suporte de protocolo de OAuth, apresentando ao usuário um início de sessão da interface do Usuário, atualizando tokens expirados e muito mais. Tudo o que realmente precisa saber é uma única chamada à API, `authContext.AcquireTokenAsync(...)`.

Para referência, o exemplo concluído (sem os valores de configuração) é fornecido [aqui](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Agora pode passar para cenários adicionais. Poderá querer experimentar:

[Proteger uma API com o Azure AD de Web do .NET >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

