---
title: Autenticação, inscreva-se, Editar perfil no Azure Active Directory B2C | Documentos da Microsoft
description: Como criar um aplicativo de desktop do Windows que inclui o início de sessão, inscreva-se, e a gestão de perfis com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 96cdbca1b0a5a75c24b9d0e75eaf1fc861bf764a
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54843128"
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Compilar uma aplicação de ambiente de trabalho do Windows
Ao utilizar o Azure Active Directory (Azure AD) B2C, pode adicionar funcionalidades de gestão de identidade poderosas self-service para seu aplicativo de desktop em poucos passos. Este artigo irá mostrar como criar uma aplicação de "lista de tarefas" do .NET Windows Presentation Foundation (WPF) que inclui a inscrição e início de sessão do utilizador e gestão de perfis. A aplicação irá incluir suporte para inscrição e início de sessão utilizando um nome de utilizador ou e-mail. Ele também irão incluir o suporte para inscrição e início de sessão através da utilização de contas de redes sociais como o Facebook e Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Obter um diretório do Azure AD B2C
Para poder utilizar o Azure AD B2C, tem de criar um diretório ou inquilino. Um diretório é um contentor para todos os seus utilizadores, aplicações, grupos, etc. Se ainda não tiver um, [crie um diretório do B2C](tutorial-create-tenant.md) antes de prosseguir neste guia.

## <a name="create-an-application"></a>Criar uma aplicação
Em seguida, precisa de criar uma aplicação no diretório do B2C. Isto proporciona ao Azure AD as informações de que necessita para comunicar de forma segura com a sua aplicação. Para criar uma aplicação, siga [estas instruções](active-directory-b2c-app-registration.md). É necessário:

* Incluir uma **cliente nativo** no aplicativo.
* Copiar o **URI de redirecionamento** `urn:ietf:wg:oauth:2.0:oob`. É o URL predefinido para este exemplo de código.
* Copiar a **ID da Aplicação** atribuída à aplicação. Precisará dele mais tarde.

## <a name="create-your-policies"></a>Criar as políticas
No Azure AD B2C, cada experiência de utilizador é definida por uma [política](active-directory-b2c-reference-policies.md). Este exemplo de código contém três experiências de identidade: inscrição, inicie sessão e Editar perfil. Ao criar as políticas, certifique-se de que para:

* Escolher a **Inscrição de ID de utilizador** ou **Inscrição de email** no painel de fornecedores de identidade.
* Escolher o **Nome a apresentar** e outros atributos de inscrição na sua política de inscrição.
* Escolher as afirmações **Nome a apresentar** e **ID de objeto** como afirmações de aplicação em cada política. Também pode escolher outras afirmações.
* Copiar o **Nome** de cada política depois de a criar. Deve ter o prefixo `b2c_1_`. Precisará destes nomes de políticas mais tarde.

Depois de ter criado com êxito as políticas, está pronto para criar a sua aplicação.

## <a name="download-the-code"></a>Transferir o código
O código deste tutorial [é mantido no GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Para criar o exemplo à medida que avança, pode [transferir o projeto de estrutura como um ficheiro .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Também pode clonar a estrutura:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

A aplicação concluída está também [disponível como um ficheiro .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) ou no `complete` ramo do mesmo repositório.

Depois de transferir o código de exemplo, abra o ficheiro de .sln do Visual Studio para começar a utilizar. O `TaskClient` projeto é o aplicativo de área de trabalho do WPF que o usuário interage com. Para efeitos deste tutorial, ele chama uma API, alojado no Azure, que armazena a lista de tarefas pendentes de cada usuário da web de tarefa de back-end. Não é necessário criar a API web, já temos-lo em execução para.

Para saber como uma API web com segurança o autentica o pedidos utilizando o Azure AD B2C, consulte a [introdução de web API ao artigo](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Executar políticas
Seu aplicativo se comunica com o Azure AD B2C através do envio de mensagens de autenticação que especifique a política que pretendem executar como parte da solicitação HTTP. .NET para aplicativos de desktop, pode utilizar a pré-visualização Microsoft Authentication Library (MSAL) para enviar mensagens de autenticação OAuth 2.0, execute as políticas e obter os tokens que chamam as APIs web.

### <a name="install-msal"></a>Instalar a MSAL
Adicionar a MSAL para o `TaskClient` projeto utilizando a consola do Gestor de pacotes do Visual Studio.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Introduza os seus detalhes do B2C
Abra o ficheiro `Globals.cs` e substituir cada um dos valores de propriedade por seus próprios. Esta classe é utilizada em todo `TaskClient` valores de referência utilizada frequentemente.

```csharp
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-the-publicclientapplication"></a>Criar o PublicClientApplication
A classe principal de MSAL é `PublicClientApplication`. Esta classe representa a sua aplicação no sistema do Azure AD B2C. Quando os initalizes de aplicação, crie uma instância do `PublicClientApplication` em `MainWindow.xaml.cs`. Isso pode ser usado em toda a janela.

```csharp
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default. Since we want tokens to persist when the user closes the app,
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a>Iniciar um fluxo de inscrição
Quando um utilizador opta ativamente por participar sinais de cópia de segurança, que pretende iniciar um fluxo Inscreva-se de que utiliza a política de inscrição que criou. Ao utilizar a MSAL, apenas de chamar `pca.AcquireTokenAsync(...)`. Os parâmetros a transmitir para `AcquireTokenAsync(...)` determinar qual token receber, a política utilizada no pedido de autenticação e muito mais.

```csharp
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API). Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Iniciar um fluxo de início de sessão
Pode iniciar um fluxo de início de sessão da mesma forma que inicia um fluxo de inscrição. Quando um utilizador inicia sessão, fazer a chamada mesmo para MSAL, desta vez com a sua política de início de sessão:

```csharp
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Iniciar um fluxo de edição de perfil
Novamente, pode executar uma política de edição de perfil da mesma maneira:

```csharp
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Em todos os casos, MSAL ou retorna um token no `AuthenticationResult` ou gera uma exceção. Sempre que receber um token de MSAL, pode usar o `AuthenticationResult.User` objeto para atualizar os dados de utilizador na aplicação, tais como a interface do Usuário. ADAL também coloca em cache o token para utilização em outras partes do aplicativo.

### <a name="check-for-tokens-on-app-start"></a>Verifique a existência de tokens no início da aplicação
Também pode utilizar a MSAL para controlar o estado de início de sessão do utilizador. Nesta aplicação, queremos que o usuário permaneça com sessão iniciada, mesmo depois de fechar a aplicação e voltar a abrir. Dentro de volta a `OnInitialized` de substituição, utilize da MSAL `AcquireTokenSilent` tokens em cache de método para procurar:

```csharp
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache. Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>Chamar a API de tarefa
Agora, utilizou a MSAL para executar as políticas e obter os tokens. Quando pretender utilizar um destes tokens para chamar a API de tarefa, pode utilizar novamente da MSAL `AcquireTokenSilent` tokens em cache de método para procurar:

```csharp
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Quando a chamada para `AcquireTokenSilentAsync(...)` for concluída com êxito e um token for encontrado na cache, pode adicionar o token para o `Authorization` cabeçalho da solicitação HTTP. A API da web de tarefas irá utilizar este cabeçalho para autenticar o pedido para ler a lista de tarefas do utilizador:

```csharp
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>O utilizador terminar sessão
Por fim, pode utilizar a MSAL para terminar uma sessão de utilizador com a aplicação quando o usuário seleciona **terminar sessão**. Ao utilizar a MSAL, isso é conseguido ao desmarcar todos os tokens da cache de token:

```csharp
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo
Por fim, crie e execute o exemplo. Inscrever-se a aplicação utilizando um nome de utilizador ou endereço de e-mail. Termine sessão e inicie sessão novamente como o mesmo utilizador. Edite do perfil do usuário. Termine sessão e inscrever-se através de um utilizador diferente.

## <a name="add-social-idps"></a>Adicionar IDPs sociais
Atualmente, a aplicação suporta apenas inscrição de utilizador e início de sessão que utilizam **contas locais**. Estas são contas armazenadas no diretório do B2C que utilizam um nome de utilizador e palavra-passe. Ao utilizar o Azure AD B2C, pode adicionar suporte para outros fornecedores de identidade (IDPs) sem alterar nenhum código.

Para adicionar IDPs sociais para a sua aplicação, comece por seguir as instruções detalhadas nestes artigos. Para cada IDP pretende oferecer apoio técnico, terá de registar uma aplicação no sistema e obter um ID de cliente.

* [Configurar o Facebook como um IDP](active-directory-b2c-setup-fb-app.md)
* [Configurar o Google como um IDP](active-directory-b2c-setup-goog-app.md)
* [Configurar o Amazon como um IDP](active-directory-b2c-setup-amzn-app.md)
* [Configurar o LinkedIn como um IDP](active-directory-b2c-setup-li-app.md)

Depois de adicionar os fornecedores de identidade para o seu diretório do B2C, terá de editar cada um dos seus três políticas para incluir os IDPs novo, conforme descrito no [artigo de referência de política](active-directory-b2c-reference-policies.md). Depois de guardar as suas políticas, execute novamente a aplicação. Deverá ver os novo IDPs adicionados como o início de sessão e experiências de opções de inscrição em cada uma das sua identidade.

Pode experimentar com as suas políticas e observe os efeitos em seu aplicativo de exemplo. Adicionar ou remover IDPs, manipular afirmações de aplicação ou alterar os atributos de inscrição. Experimentação até que pode ver como as políticas, os pedidos de autenticação e MSAL ligarem.

Para referência, o exemplo concluído [é fornecido como um ficheiro. zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Também pode clonar a partir do GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
