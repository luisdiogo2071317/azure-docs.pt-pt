---
title: Azure AD v2 UWP introdução | Microsoft Docs
description: Como aplicações da plataforma Universal do Windows (UWP) podem chamar uma API que necessita de tokens de acesso pelo ponto final do Azure Active Directory v2
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/20/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: c2d5681e30651aac7a09a8ead923015e9a892d42
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796619"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Chamada Microsoft Graph API a partir de uma aplicação plataforma Universal do Windows (XAML)

Este guia explica como uma aplicação plataforma Universal do Windows (UWP) nativo pode pedir um token de acesso e, em seguida, chame Microsoft Graph API. O guia também se aplica a outras APIs que necessitam de tokens de acesso do ponto final do Azure Active Directory v2.

No final deste guia, a aplicação chama uma API protegida através da utilização de contas pessoais. Os exemplos são outlook.com, live.com e outros. A aplicação também chamadas contas profissional e escolar de qualquer da empresa ou organização que tenha o Azure Active Directory.

>[!NOTE]
> Este guia requer 2017 do Visual Studio com o desenvolvimento de plataforma Universal do Windows instalado. Consulte [obter configurar](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) para obter instruções para transferir e configurar o Visual Studio para desenvolver aplicações da plataforma Universal do Windows.

### <a name="how-this-guide-works"></a>Como funciona este guia

![Este guia funcionamento gráfico](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

Este guia cria um exemplo de aplicação de UWP que consulta o Microsoft Graph API ou uma API Web que aceita tokens de ponto final do Azure Active Directory v2. Neste cenário, um token é adicionado a pedidos de HTTP através do cabeçalho de autorização. Biblioteca de autenticação da Microsoft (MSAL) processa aquisições token e renovações.

### <a name="nuget-packages"></a>Pacotes NuGet

Este guia utiliza os seguintes pacotes de NuGet:

|Biblioteca|Descrição|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticação da Microsoft|


## <a name="set-up-your-project"></a>Configurar o projeto

Esta secção fornece instruções passo a passo para integrar uma aplicação .NET de ambiente de trabalho do Windows (XAML) com *início de sessão com a Microsoft*. Em seguida, este pode consultar APIs da Web que necessitam de um token, como o Microsoft Graph API.

Este guia cria uma aplicação que apresenta um botão que consultas Graph API, um botão de início de sessão e caixas de texto a apresentar os resultados das chamadas.

>[!NOTE]
> Pretende transferir o projeto do Visual Studio este exemplo em vez disso? [Transferir um projeto](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) e avance para o [registo de aplicação](#register-your-application "passo de registo de aplicação") passo para configurar o exemplo de código antes de executar.


### <a name="create-your-application"></a>Criar a sua aplicação
1. No Visual Studio, selecione **ficheiro** > **novo** > **projeto**.
2. Em **modelos**, selecione **Visual c#**.
3. Selecione **Aplicação em branco (Universal Windows)**.
4. Nome da aplicação e selecione **OK**.
5. Se lhe for pedido, selecione qualquer versão para **destino** e **mínimo** versões e selecione **OK**.

    >![Versões mínimo e de destino](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Adicionar a biblioteca de autenticação da Microsoft ao seu projeto
1. No Visual Studio, selecione **ferramentas** > **Gestor de pacotes NuGet** > **consola do Gestor de pacotes**.
2. Copie e cole o seguinte comando no **consola do Gestor de pacotes** janela:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> Este comando instala [biblioteca de autenticação da Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL adquire, coloca em cache e atualiza os tokens de utilizador que o acesso a APIs protegidas pelo Azure Active Directory v2.

## <a name="initialize-msal"></a>Inicializar MSAL
Este passo ajuda a criar uma classe para processar a interação com MSAL, tais como processar os tokens.

1. Abra o **App.xaml.cs** de ficheiros e adicionar a referência para MSAL para a classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Adicione as duas linhas seguintes à classe da aplicação (dentro <code>sealed partial class App : Application</code> blocos):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>Criar a IU da aplicação

A **MainPage.xaml** ficheiro é criado automaticamente como parte do seu modelo de projeto. Abrir este ficheiro e, em seguida, siga as instruções:

* Substitua a sua aplicação **grelha** nó com o seguinte código:

    ```xml
    <Grid>
        <StackPanel Background="Azure">
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
                <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
            </StackPanel>
            <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
            <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
        </StackPanel>
    </Grid>
    ```
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Utilizar MSAL para obter um token para o Microsoft Graph API

Esta secção mostra como utilizar MSAL para obter um token para o Microsoft Graph API.

1.  No **MainPage.xaml.cs**, adicione a referência para MSAL à classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Substitua o código do seu <code>MainPage</code> classe com o seguinte código:

    ```csharp
    public sealed partial class MainPage : Page
    {
        // Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";
    
        // Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };
    
        public MainPage()
        {
            this.InitializeComponent();
        }
    
        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;
    
            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
    
                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }
    
            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

### <a name="more-information"></a>Mais informações
#### <a name="get-a-user-token-interactively"></a>Obter um utilizador token interativamente
Uma chamada para o `AcquireTokenAsync` método resulta numa janela que solicita aos utilizadores iniciar sessão. As aplicações necessitam normalmente aos utilizadores iniciar sessão interativamente na primeira vez que necessitam para aceder a recursos protegidos. Também poderá ter de iniciar sessão quando ocorre uma falha de uma operação automática para adquirir um token. Um exemplo é quando palavra-passe um utilizador está expirado.

#### <a name="get-a-user-token-silently"></a>Obter um utilizador token automaticamente
O `AcquireTokenSilentAsync` método processa aquisições token e renovações sem qualquer interação do utilizador. Depois de `AcquireTokenAsync` é executado pela primeira vez e é pedido ao utilizador para as credenciais, o `AcquireTokenSilentAsync` método deve ser utilizado para pedir tokens para chamadas subsequentes porque adquirir tokens silenciosamente. MSAL processará a cache de tokens e a renovação.

Eventualmente, o `AcquireTokenSilentAsync` falha do método. Motivos da falha podem ser que os utilizadores tem sessão ou alterar a palavra-passe noutro dispositivo. Quando MSAL Deteta que é possível resolver o problema, exigindo que uma ação interativa, é acionado um `MsalUiRequiredException` exceção. A aplicação pode processar esta exceção de duas formas:

* Pode efetuar uma chamada contra `AcquireTokenAsync` imediatamente. Esta chamada resulta numa pedir ao utilizador para iniciar sessão. Normalmente, este padrão é utilizado em aplicações online em que não existe conteúdo disponível offline para o utilizador. O exemplo gerado por esta configuração orientada segue o padrão. Vê-lo no prazo de ação primeiro que executa o exemplo. 
    * Porque nenhum utilizador tiver utilizado a aplicação, `PublicClientApp.Users.FirstOrDefault()` contém um valor nulo e um `MsalUiRequiredException` exceção é emitida.
    * O código no exemplo, em seguida, processa a exceção ao chamar `AcquireTokenAsync`. Esta chamada resulta numa pedir ao utilizador para iniciar sessão.

* Ou, em vez disso, esta apresenta uma indicação de visual aos utilizadores que é necessário um início de sessão interativo. Em seguida, podem selecionar no momento certo para iniciar sessão. Ou pode repetir a aplicação `AcquireTokenSilentAsync` mais tarde. Frequentemente, este padrão é utilizado quando os utilizadores podem utilizar outras funcionalidades de aplicação sem interrupção. Um exemplo é quando o conteúdo offline está disponível na aplicação. Neste caso, os utilizadores podem decidir quando pretendem iniciar sessão para aceder ao recurso protegido ou Atualize as informações Desatualizadas. Ou, caso contrário a aplicação pode decidir repetir `AcquireTokenSilentAsync` quando a rede é restaurada após foi temporariamente indisponível.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Chamar Microsoft Graph API utilizando o token que apenas obtido

* Adicione o seguinte método novo para **MainPage.xaml.cs**. Este método é utilizado para efetuar um `GET` pedido em relação a Graph API através da utilização de um cabeçalho [autorizar]:

    ```csharp
    /// <summary>
    /// Perform an HTTP GET request to a URL using an HTTP Authorization header
    /// </summary>
    /// <param name="url">The URL</param>
    /// <param name="token">The token</param>
    /// <returns>String containing the results of the GET operation</returns>
    public async Task<string> GetHttpContentWithToken(string url, string token)
    {
        var httpClient = new System.Net.Http.HttpClient();
        System.Net.Http.HttpResponseMessage response;
        try
        {
            var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
            // Add the token in Authorization header
            request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
            response = await httpClient.SendAsync(request);
            var content = await response.Content.ReadAsStringAsync();
            return content;
        }
        catch (Exception ex)
        {
            return ex.ToString();
        }
    }
    ```

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Obter mais informações sobre como efetuar uma chamada REST em relação a uma API protegida

Nesta aplicação de exemplo, o `GetHttpContentWithToken` método é utilizado para efetuar um HTTP `GET` pedido contra um recurso protegido que necessita de um token. Em seguida, o método devolve o conteúdo para o autor da chamada. Este método adiciona o token adquirido no **HTTP autorização** cabeçalho. Para este exemplo, o recurso é o Microsoft Graph API **-me** ponto final, que apresenta as informações do perfil do utilizador.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Adicione um método para terminar sessão de utilizador

* Para terminar sessão de utilizador, adicione o seguinte método para **MainPage.xaml.cs**:

    ```csharp
    /// <summary>
    /// Sign out the current user
    /// </summary>
    private void SignOutButton_Click(object sender, RoutedEventArgs e)
    {
        if (App.PublicClientApp.Users.Any())
        {
            try
            {
                App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
                this.ResultText.Text = "User has signed-out";
                this.CallGraphButton.Visibility = Visibility.Visible;
                this.SignOutButton.Visibility = Visibility.Collapsed;
            }
            catch (MsalException ex)
            {
                ResultText.Text = $"Error signing-out user: {ex.Message}";
            }
        }
    }
    ```

### <a name="more-information-on-sign-out"></a>Obter mais informações sobre o fim de sessão

O `SignOutButton_Click` método Remove o utilizador da cache de utilizador MSAL. Este método de forma eficaz indica MSAL para se esqueça do utilizador atual. Em seguida, um pedido futuro para adquirir um token é concluída com êxito apenas se tiver efetuado ser interativo.
A aplicação neste exemplo suporta um único utilizador. Mas MSAL suporta cenários em que mais do que uma conta pode ter sessão iniciada ao mesmo tempo. Um exemplo é uma aplicação de e-mail em que um utilizador tem várias contas.

## <a name="display-basic-token-information"></a>Apresentar as informações do token básicas

* Adicione o seguinte método para **MainPage.xaml.cs** para apresentar informações básicas sobre o token:

    ```csharp
    /// <summary>
    /// Display basic information contained in the token
    /// </summary>
    private void DisplayBasicTokenInfo(AuthenticationResult authResult)
    {
        TokenInfoText.Text = "";
        if (authResult != null)
        {
            TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
            TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
            TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
            TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
        }
    }
    ```

### <a name="more-information"></a>Mais informações

Tokens de ID adquirido através de **OpenID Connect** também conter um pequeno subconjunto de informações relevantes para o utilizador. `DisplayBasicTokenInfo` Mostra informações básicas contidas no token. Os exemplos são nome a apresentar do utilizador e o ID, a data de expiração do token e a cadeia que representa o token de acesso si próprio. Se selecionar a **chamar Microsoft Graph API** botão várias vezes, verá que o mesmo token foi reutilizado em pedidos subsequentes. Também pode ver a data de expiração expandida quando MSAL decide está na altura de renovar o token.

## <a name="register-your-application"></a>Registar a sua aplicação

Agora tem de registar a sua aplicação no Portal de registo de aplicações da Microsoft:
1. Vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registar uma aplicação.
2. Introduza um nome para a sua aplicação.
3. Certifique-se de que a opção para **orientado configuração** é *não selecionada*.
4. Selecione **adicionar plataformas**, selecione **aplicação nativa**e, em seguida, selecione **guardar**.
5. Copiar o GUID no **ID da aplicação**, volte ao Visual Studio, abra **App.xaml.cs**e substitua `your_client_id_here` com o ID de aplicação que acabou de ser registado:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Ativar a autenticação integrada nos domínios federados (opcionais)

Para ativar a autenticação integrada do Windows quando é utilizado com um domínio federado do Azure Active Directory, o manifesto da aplicação tem de ativar capacidades adicionais:

1. Faça duplo clique em **Package. appxmanifest**.
2. Selecione o **capacidades** separador e certifique-se de que as seguintes definições estão ativadas:

    - Autenticação de empresa
    - Redes privadas (cliente e servidor)
    - Certificados de utilizador partilhados

3. Abra **App.xaml.cs** e adicione a seguinte linha no construtor de aplicação:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Autenticação integrada do Windows não está configurada por predefinição para este exemplo. As aplicações que pedem *autenticação* ou *certificados de utilizador partilhado* capacidades requerem um nível mais elevado de verificação pela loja Windows. Além disso, nem todos os programadores pretendem efetuar o nível mais elevado de verificação. Ative esta definição apenas se precisar de autenticação integrada do Windows com um domínio federado do Azure Active Directory.


## <a name="test-your-code"></a>Testar o seu código

Para testar a aplicação, selecione F5 para executar o projeto no Visual Studio. É apresentada a janela principal:

![Interface de utilizador da aplicação](media/active-directory-uwp-v2.md/testapp-ui.png)

Quando estiver pronto para testar, selecione **chamar Microsoft Graph API**. Em seguida, utilize uma conta institucional do Microsoft Azure Active Directory ou uma conta Microsoft, tais como live.com ou outlook.com, para iniciar sessão. Se for a primeira vez, verá uma janela solicitando ao utilizador para iniciar sessão:

![Página de início de sessão](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Consentimento
A primeira vez que iniciar sessão na sua aplicação, é apresentado um ecrã de consentimento semelhante ao seguinte. Selecione **Sim** explicitamente consentimento para o acesso:

![Ecrã de consentimento de acesso](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Resultados esperados
Ver informações de perfil de utilizador devolvidas pela chamada Microsoft Graph API no **resultados de chamada de API** ecrã:

![Ecrã de resultados de chamada de API](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

Consulte também informações básicas sobre o token adquirido através de `AcquireTokenAsync` ou `AcquireTokenSilentAsync` no **informações do Token** caixa:

|Propriedade  |Formato  |Descrição |
|---------|---------|---------|
|**Nome** |Nome completo do utilizador|O utilizador do primeiro e último nome.|
|**Nome de Utilizador** |<span>user@domain.com</span> |O nome de utilizador que identifica o utilizador.|
|**Token expira** |DateTime |A hora quando o token expira. MSAL expande a data de expiração renovando o token, conforme necessário.|
|**Token de acesso** |Cadeia |A cadeia do token que é enviada para HTTP pedidos que necessitam de um *cabeçalho de autorização*.|

#### <a name="see-whats-in-the-access-token-optional"></a>O que se encontra no token de acesso (opcional)
Opcionalmente, copie o valor no **Token de acesso** e cole-a no https://jwt.ms descodificá-lo e ver a lista de afirmações.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mais informações sobre âmbitos e as permissões delegadas

Requer o Microsoft Graph API do *user.read* âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todas as aplicações que está registada no Portal de registo de aplicação. Outras APIs para o Microsoft Graph e APIs personalizadas para o servidor de back-end poderá exigir âmbitos adicionais. Requer o Microsoft Graph API do *Calendars.Read* âmbito lista de calendários do utilizador.

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione o *Calendars.Read* delegado permissão para as informações de registo de aplicação. Em seguida, adicione o *Calendars.Read* âmbito para o `acquireTokenSilent` chamada. 

> [!NOTE]
> Os utilizadores poderão ser-lhe pedidos consents adicionais como aumentar o número de âmbitos.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="issue-1"></a>Problema 1
Receber uma das seguintes mensagens de erro ao iniciar sessão na sua aplicação num domínio federado do Azure Active Directory:
 - Não foi encontrado no pedido um certificado de cliente válido.
 - Não foram encontrados no arquivo de certificados do utilizador certificados válidos.
 - Tente novamente a escolher um método de autenticação diferentes.

**Causa:** capacidades de Enterprise e o certificado não estão ativadas.

**Solução:** siga os passos no [a autenticação integrada nos domínios federados](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Problema 2
Ativar [a autenticação integrada nos domínios federados](#enable-integrated-authentication-on-federated-domains-optional) e tente utilizar o Windows Hello num computador Windows 10 para iniciar sessão num ambiente com a autenticação multifator configurada. É apresentada a lista de certificados. No entanto, se optar por utilizar o seu PIN, a janela PIN nunca é apresentada.

**Causa:** este problema é uma limitação conhecida do Mediador de autenticação web em aplicações UWP que são executadas no ambiente de trabalho do Windows 10. Ajustar funciona no Windows 10 Mobile.

**Solução:** selecione **iniciar sessão com outras opções**. Em seguida, selecione **iniciar sessão com um nome de utilizador e palavra-passe**. Selecione **forneça a palavra-passe**. Em seguida, seguir o processo de autenticação de telefone.
