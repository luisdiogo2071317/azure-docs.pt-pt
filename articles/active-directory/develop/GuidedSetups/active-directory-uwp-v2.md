---
title: Azure AD v2 UWP introdução | Microsoft Docs
description: Como as aplicações da plataforma de Windows Universal (XAML) podem chamar uma API que necessitam de tokens de acesso ao ponto final do Azure Active Directory v2
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
ms.openlocfilehash: 4db14bc250bf9d6740380f3c4376f43d6f315b01
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Chamar o Microsoft Graph API a partir de uma aplicação plataforma Universal do Windows (UWP)

Este guia demonstra como uma aplicação Universal Windows plataforma (XAML) nativo pode obter um token de acesso e, em seguida, utilizar este toke de acesso para chamar o Microsoft Graph API ou outras APIs que necessitam de tokens de acesso a partir do ponto final do Azure Active Directory v2.

No final deste guia, a aplicação será possível chamar uma API protegida com contas pessoais (incluindo outlook.com, live.com e outros), bem como o trabalho contas escolares ou profissionais de qualquer da empresa ou organização que tenha o Azure Active Directory.  

> Este guia requer 2017 do Visual Studio com o desenvolvimento de plataforma Universal do Windows instalado. Selecionar esta opção [artigo](https://docs.microsoft.com/windows/uwp/get-started/get-set-up "configurar o Visual Studio para UWP") para obter instruções sobre como transferir e configurar o Visual Studio desenvolver aplicações da plataforma Windows Universal.

### <a name="how-this-guide-works"></a>Como funciona este guia

![Como funciona este guia](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

A aplicação de exemplo criada por este guia permite que uma aplicação plataforma Universal do Windows consultar o Microsoft Graph API ou uma API Web que aceita tokens de ponto final do Azure Active Directory v2. Neste cenário, um token é adicionado a pedidos de HTTP através do cabeçalho de autorização. Aquisição do token e renovação são processadas pela biblioteca de autenticação do Microsoft (MSAL).

### <a name="nuget-packages"></a>Pacotes NuGet

Este guia utiliza os seguintes pacotes de NuGet:

|Biblioteca|Descrição|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticação da Microsoft (MSAL)|


## <a name="set-up-your-project"></a>Configurar o projeto

Esta secção fornece instruções passo a passo para saber como criar um novo projeto para demonstrar como integrar uma aplicação .NET de ambiente de trabalho do Windows (XAML) com *início de sessão com a Microsoft* que possa consultar APIs da Web que necessitam de um token.

A aplicação criada por este guia expõe um botão para graph e mostrar os resultados no ecrã e botão de início de sessão.

> Prefere transferir o projeto do Visual Studio este exemplo em vez disso? [Transferir um projeto](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) e avance para o [registo de aplicação](#register-your-application "passo de registo de aplicação") passo para configurar o exemplo de código antes de executar.


### <a name="create-your-application"></a>Criar a sua aplicação
1. No Visual Studio: **ficheiro** > **novo** > **projeto**<br/>
2. Em *modelos*, selecione **Visual c#**
3. Selecione **em branco aplicação (Universal Windows)**
4. Atribua um nome e clique em 'Ok'.
5. Se lhe for solicitado, desceu livre para selecionar qualquer versão para *destino* e *mínimo* versão e clique em 'Ok':<br/><br/>![Versões mínimo e de destino](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Adicionar a biblioteca de autenticação da Microsoft (MSAL) ao seu projeto
1. No Visual Studio: **ferramentas** > **Gestor de pacotes Nuget** > **consola do Gestor de pacotes**
2. Copie/cole o seguinte comando na janela da consola do Gestor de pacotes:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> O pacote acima instala o [biblioteca de autenticação da Microsoft (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL processa a aquisição, colocação em cache e atualizar os tokens de utilizador utilizados para acesso a APIs protegidas pelo Azure Active Directory v2.

## <a name="initialize-msal"></a>Inicializar MSAL
Este passo ajuda a criar uma classe para processar interação com MSAL biblioteca, tais como o processamento de tokens.

1. Abra o **App.xaml.cs** de ficheiros e adicionar a referência da biblioteca MSAL para a classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Adicione as duas linhas seguintes à classe da aplicação (dentro <code>sealed partial class App : Application</code> blocos):

    ```csharp
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>Criar a IU da aplicação

A **MainPage.xaml** ficheiro deve ser criado automaticamente como parte do seu modelo de projeto. Abrir este ficheiro e, em seguida, siga as instruções:

1.  Substitua a sua aplicação **<Grid>** nós com:

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
    
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Utilizar a biblioteca de autenticação da Microsoft (MSAL) para obter um token para a Microsoft Graph API

Esta secção mostra como utilizar MSAL para obter um token para a Microsoft Graph API.

1.  No **MainPage.xaml.cs**, adicione a referência da biblioteca MSAL à classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Substitua o código do seu <code>MainPage</code> classe com o seguinte:

    ```csharp
    public sealed partial class MainPage : Page
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";
    
        //Set the scope for API call to user.read
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

### <a name="more-information"></a>Mais Informações
#### <a name="get-a-user-token-interactively"></a>Obter um utilizador token interativamente
Chamar o `AcquireTokenAsync` método resulta numa janela que solicita aos utilizadores iniciar sessão. As aplicações necessitam normalmente aos utilizadores iniciar sessão interativamente na primeira vez que necessitam para aceder a recursos protegidos. Também poderá ter de iniciar sessão quando uma operação automática para obter um token de falha (por exemplo, quando palavra-passe um utilizador está expirado).

#### <a name="get-a-user-token-silently"></a>Obter um utilizador token automaticamente
O `AcquireTokenSilentAsync` método processa aquisições token e renovações sem qualquer interação do utilizador. Depois de `AcquireTokenAsync` é executado pela primeira vez, `AcquireTokenSilentAsync` é o habitual método a utilizar para obter os tokens que acedem a recursos protegidos para as chamadas subsequentes, porque as chamadas para pedir ou renovar tokens são efetuadas automaticamente.

Eventualmente, o `AcquireTokenSilentAsync` método irá falhar. Motivos da falha podem ser que o utilizador tem sessão ou alterar a palavra-passe noutro dispositivo. Quando MSAL Deteta que é possível resolver o problema, exigindo que uma ação interativa, é acionado um `MsalUiRequiredException` exceção. A aplicação pode processar esta exceção de duas formas:

* Pode efetuar uma chamada contra `AcquireTokenAsync` imediatamente. Esta chamada resulta numa pedir ao utilizador para iniciar sessão. Este padrão é normalmente utilizado nas aplicações online em que não existe conteúdo disponível offline para o utilizador. O exemplo gerado por esta configuração orientada segue neste padrão, que pode ver no tempo de ação primeiro que executar o exemplo. 
    * Porque nenhum utilizador tiver utilizado a aplicação, `PublicClientApp.Users.FirstOrDefault()` contém um valor nulo e um `MsalUiRequiredException` exceção é emitida. 
    * O código no exemplo, em seguida, processa a exceção ao chamar `AcquireTokenAsync`, que resulta numa pedir ao utilizador para iniciar sessão.

* Em vez disso, pode apresentar uma indicação de visual aos utilizadores um interativa início de sessão é necessário, para que o utilizador podem selecionar no momento certo para iniciar sessão. Ou pode repetir a aplicação `AcquireTokenSilentAsync` mais tarde. Este padrão é frequentemente utilizado quando os utilizadores podem utilizar outras funcionalidades de aplicação sem interrupção – por exemplo, quando o conteúdo offline está disponível na aplicação. Neste caso, os utilizadores podem decidir quando pretendem iniciar sessão para aceder ao recurso protegido ou Atualize as informações Desatualizadas. Em alternativa, a aplicação pode optar por repita `AcquireTokenSilentAsync` quando a rede for restaurada após ter sido temporariamente indisponível.

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Chamar o Microsoft Graph API utilizando o token obtido apenas

1. Adicione o seguinte método novo à sua **MainPage.xaml.cs**. O método é utilizado para efetuar um `GET` pedido em relação a Graph API utilizando um cabeçalho de autorizar:

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
            //Add the token in Authorization header
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

Nesta aplicação de exemplo, o `GetHttpContentWithToken` método é utilizado para efetuar um HTTP `GET` contra um recurso protegido que necessita de um token de pedido e, em seguida, devolver o conteúdo para o autor da chamada. Este método adiciona o token adquirido no *cabeçalho de autorização de HTTP*. Para este exemplo, o recurso é o Microsoft Graph API *-me* ponto final – apresenta informações de perfil do utilizador.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Adicione um método para terminar sessão de utilizador

1. Para terminar sessão de utilizador, adicione o seguinte método para **MainPage.xaml.cs**:

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

### <a name="more-info-on-sign-out"></a>Obter mais informações sobre o fim de sessão

O método `SignOutButton_Click` remove o utilizador da cache de utilizador MSAL – Isto indica eficazmente MSAL, para se esqueça do utilizador atual para um pedido futuro para adquirir um token só pode ter êxito se for efetuada para ser interativo.
Apesar da aplicação neste exemplo suporta um único utilizador, MSAL suporta cenários em que várias contas podem ser com sessão iniciada em simultâneo – um exemplo é uma aplicação de e-mail em que um utilizador tem várias contas.

## <a name="display-basic-token-information"></a>Apresentar as informações do Token básicas

1. Adicione o método seguinte à sua **MainPage.xaml.cs** para apresentar informações básicas sobre o token:

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

### <a name="more-information"></a>Mais Informações

Tokens de ID adquirido através de *OpenID Connect* também conter um pequeno subconjunto de informações relevantes para o utilizador. `DisplayBasicTokenInfo` Mostra informações básicas contidas no token: por exemplo, nome a apresentar do utilizador e ID, bem como a data de expiração do token e a cadeia que representa o acesso do token próprio. Estas informações são apresentadas para ver. Pode clicar no **chamar Microsoft Graph API** botão várias vezes e ver que o mesmo token foi reutilizado em pedidos subsequentes. Também pode ver a data de expiração a ser expandida quando MSAL decide está na altura de renovar o token.

## <a name="register-your-application"></a>Registar a sua aplicação

Agora tem de registar a sua aplicação no *Portal de registo de aplicações do Microsoft*:
1. Vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registar uma aplicação
2. Introduza um nome para a sua aplicação 
3. Certifique-se que a opção para a configuração orientado está desmarcada
4. Clique em **adicionar plataformas**, em seguida, selecione **aplicação nativa** e clique em Guardar
5. Copie o GUID no ID da aplicação, volte atrás para Visual Studio, abra **App.xaml.cs** e substitua `your_client_id_here` com o ID de aplicação que acabou de ser registado:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Ativar a autenticação integrada nos domínios federados (opcionais)

Para ativar a autenticação integrada do Windows quando utilizado com um domínio federado do Azure Active Directory, o manifesto da aplicação tem de ativar capacidades adicionais:

1. Faça duplo clique **Package. appxmanifest**
2. Selecione **capacidades** separador e certifique-se de que as seguintes definições estão ativadas:

    - Autenticação de empresa
    - Redes privadas (cliente e servidor)
    - Certificados de utilizador partilhados 

3. Em seguida, abra **App.xaml.cs**e adicione o seguinte no construtor de aplicação:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Autenticação integrada do Windows não está configurada por predefinição para este exemplo porque aplicações pedir a *autenticação* ou *certificados de utilizador partilhado* capacidades requerem um nível mais elevado de verificationby a loja Windows e nem todos os programadores interessados em realizar o nível mais elevado de verificação. Ative esta definição apenas se precisar de autenticação integrada do Windows com um domínio federado do Azure Active Directory.


## <a name="test-your-code"></a>Testar o seu código

Para testar a aplicação, prima `F5` para executar o projeto no Visual Studio. Deverá aparecer a janela principal:

![Interface de utilizador da aplicação](media/active-directory-uwp-v2.md/testapp-ui.png)

Quando estiver pronto para testar, clique em *chamar Microsoft Graph API* e utilizar (conta institucional) um Microsoft Azure Active Directory ou uma conta de Account Microsoft (live.com, outlook.com) para iniciar sessão. Se for a primeira vez, verá uma janela solicitando utilizador iniciar sessão:

![Página de início de sessão](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Consentimento
A primeira vez que iniciar sessão na sua aplicação, é-lhe apresentado um ecrã de consentimento semelhante ao seguinte, onde tem de aceitar explicitamente:

![Ecrã de consentimento](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Resultados esperados
Deverá ver informações de perfil de utilizador devolvidas pela chamada Microsoft Graph API no ecrã de resultados de chamada de API:

![Ecrã de resultados](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

Também deverá ver informações básicas sobre o token adquirido através de `AcquireTokenAsync` ou `AcquireTokenSilentAsync` na caixa de informações de Token:

|Propriedade  |Formato  |Descrição |
|---------|---------|---------|
|**Nome** |Nome completo do utilizador |O utilizador do primeiro e último nome.|
|**Nome de Utilizador** |<span>user@domain.com</span> |O nome de utilizador que é utilizado para identificar o utilizador.|
|**Token expira** |DateTime |A hora em que o token expira. MSAL expande a data de expiração renovando o token, conforme necessário.|
|**Token de acesso** |Cadeia |A cadeia do token que é enviada para HTTP pedidos que necessitam de um *cabeçalho de autorização*.|

#### <a name="see-what-is-in-the-access-token-optional"></a>O que se encontra no token de acesso (opcional)
Opcionalmente, pode copiar o valor na 'Acesso Token' e cole-a no https://jwt.ms descodificá-lo e ver a lista de afirmações.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mais informações sobre âmbitos e as permissões delegadas

Requer o Microsoft Graph API do *user.read* âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todas as aplicações que está registada no Portal de registo de aplicação. Outras APIs para o Microsoft Graph, bem como as APIs personalizadas para o servidor de back-end, poderá necessitar de âmbitos adicionais. Requer o Microsoft Graph API do *Calendars.Read* âmbito lista de calendários do utilizador.

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione o *Calendars.Read* delegado permissão para as informações de registo de aplicação. Em seguida, adicione o *Calendars.Read* âmbito para o `acquireTokenSilent` chamada. 

> [!NOTE]
> Poderá ser solicitado ao utilizador consents adicionais como aumentar o número de âmbitos.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="issue-1"></a>Problema 1:
Poderá receber um dos seguintes erros ao início de sessão na sua aplicação num domínio federado para o Azure Active Directory:
 - ' Encontrado nenhum certificado de cliente válido no pedido.
 - Não foram encontrados no arquivo de certificados do utilizador certificados válidos.
 - Volte a tentar novamente escolher um método de autenticação diferente.'

**Causa:** capacidades de Enterprise e certificados não estão ativadas

**Solução:** siga os passos no [a autenticação integrada no domínio federado](#enable-integrated-authentication-on-federated-domains-optional)

### <a name="issue-2"></a>Problema de 2:
Ater ativar [a autenticação integrada nos domínios federados](#enable-integrated-authentication-on-federated-domains-optional) e tente utilizar o Windows Hello num computador Windows 10 para início de sessão um ambiente com várias-factor-autenticação configurada, a lista de certificados é apresentada , no entanto se optar por utilizar o seu PIN, a janela PIN é nunca apresentada.

**Causa:** esta é uma limitação conhecida com Mediador de autenticação Web em aplicações UWP em execução no ambiente de trabalho do Windows 10 (funciona bem no Windows 10 Mobile)

**Solução:** como solução, os utilizadores tem de selecionar para iniciar sessão com outras opções e, em seguida, escolha *início de sessão com um nome de utilizador e palavra-passe* em vez disso, selecione forneça a palavra-passe e, em seguida, percorra a autenticação de telefone.

