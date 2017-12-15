
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Utilizar MSAL para obter um token para a Microsoft Graph API

Nesta secção, utilize MSAL para obter um token para a Microsoft Graph API.

1.  No *MainWindow.xaml.cs* ficheiro, adicione a referência para MSAL à classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Substitua o `MainWindow` classe código com o seguinte:

    ```csharp
    public partial class MainWindow : Window
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] _scopes = new string[] { "user.read" };

        public MainWindow()
        {
            InitializeComponent();
        }

        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
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
                ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Mais informações
#### <a name="get-a-user-token-interactively"></a>Obter um utilizador token interativamente
Chamar o `AcquireTokenAsync` método resulta numa janela que solicita aos utilizadores iniciar sessão. As aplicações necessitam normalmente aos utilizadores iniciar sessão interativamente na primeira vez que necessitam para aceder a recursos protegidos. Também poderá ter de iniciar sessão quando uma operação automática para obter um token de falha (por exemplo, quando palavra-passe um utilizador está expirado).

#### <a name="get-a-user-token-silently"></a>Obter um utilizador token automaticamente
O `AcquireTokenSilentAsync` método processa aquisições token e renovações sem qualquer interação do utilizador. Depois de `AcquireTokenAsync` é executado pela primeira vez, `AcquireTokenSilentAsync` é o habitual método a utilizar para obter os tokens que acedem a recursos protegidos para as chamadas subsequentes, porque as chamadas para pedir ou renovar tokens são efetuadas automaticamente.

Eventualmente, o `AcquireTokenSilentAsync` método irá falhar. Motivos da falha podem ser que o utilizador tem sessão ou alterar a palavra-passe noutro dispositivo. Quando MSAL Deteta que é possível resolver o problema, exigindo que uma ação interativa, é acionado um `MsalUiRequiredException` exceção. A aplicação pode processar esta exceção de duas formas:

* Pode efetuar uma chamada contra `AcquireTokenAsync` imediatamente. Esta chamada resulta numa pedir ao utilizador para iniciar sessão. Este padrão é normalmente utilizado nas aplicações online em que não existe conteúdo disponível offline para o utilizador. O exemplo gerado por esta configuração orientada segue neste padrão, que pode ver no tempo de ação primeiro que executar o exemplo. 
    * Porque nenhum utilizador tiver utilizado a aplicação, `PublicClientApp.Users.FirstOrDefault()` contém um valor nulo e um `MsalUiRequiredException` exceção é emitida. 
    * O código no exemplo, em seguida, processa a exceção ao chamar `AcquireTokenAsync`, que resulta numa pedir ao utilizador para iniciar sessão.

* Em vez disso, pode apresentar uma indicação de visual aos utilizadores um interativa início de sessão é necessário, para que o utilizador podem selecionar no momento certo para iniciar sessão. Ou pode repetir a aplicação `AcquireTokenSilentAsync` mais tarde. Este padrão é frequentemente utilizado quando os utilizadores podem utilizar outras funcionalidades de aplicação sem interrupção – por exemplo, quando o conteúdo offline está disponível na aplicação. Neste caso, os utilizadores podem decidir quando pretendem iniciar sessão para aceder ao recurso protegido ou Atualize as informações Desatualizadas. Em alternativa, a aplicação pode optar por repita `AcquireTokenSilentAsync` quando a rede for restaurada após ter sido temporariamente indisponível.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Chamar o Microsoft Graph API utilizando o token que apenas obtido

Adicione o seguinte método novo à sua `MainWindow.xaml.cs`. O método é utilizado para efetuar um `GET` pedido em relação a Graph API através da utilização de um cabeçalho de autorizar:

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
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Mais informações sobre como efetuar uma chamada REST em relação a uma API protegida

Nesta aplicação de exemplo, utilize o `GetHttpContentWithToken` método para tornar um HTTP `GET` contra um recurso protegido que necessita de um token de pedido e, em seguida, devolver o conteúdo para o autor da chamada. Este método adiciona o token adquirido no cabeçalho de autorização de HTTP. Para este exemplo, o recurso é o Microsoft Graph API *-me* ponto final, que apresenta as informações do perfil do utilizador.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Adicione um método para terminar sessão um utilizador

Para terminar sessão um utilizador, adicione o método seguinte à sua `MainWindow.xaml.cs` ficheiro:

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
<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Mais informações sobre a fim de sessão do utilizador

O `SignOutButton_Click` método Remove os utilizadores da cache de utilizador MSAL, que indica eficazmente MSAL para se esqueça do utilizador atual para que um pedido futuro para adquirir um token apenas terá êxito se é efetuada para ser interativo.

Apesar da aplicação neste exemplo suporta utilizadores único, MSAL suporta cenários em que várias contas podem ter sessão iniciadas ao mesmo tempo. Um exemplo é uma aplicação de e-mail em que um utilizador tem várias contas.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Apresentar as informações do token básicas

Para apresentar informações básicas sobre o token, adicione o método seguinte à sua *MainWindow.xaml.cs* ficheiro:

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
<!--start-collapse-->
### <a name="more-information"></a>Mais informações

Para além de token de acesso que é utilizado para chamar o Microsoft Graph API, depois do utilizador iniciar sessão no, MSAL também obtém um token de ID. Este token contém um pequeno subconjunto de informações que são relevantes para os utilizadores. O `DisplayBasicTokenInfo` método apresenta as informações básicas que estão contidas no token. Por exemplo, apresenta o nome a apresentar do utilizador e ID, bem como a data de expiração do token e a cadeia que representa o token de acesso si próprio. Pode selecionar o *chamar Microsoft Graph API* botão várias vezes e ver que o mesmo token foi reutilizado em pedidos subsequentes. Também pode ver a data de expiração a ser expandida quando MSAL decide está na altura de renovar o token.
<!--end-collapse-->

