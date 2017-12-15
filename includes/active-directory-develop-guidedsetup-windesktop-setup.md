
## <a name="set-up-your-project"></a>Configurar o projeto

Nesta secção criar um novo projeto para demonstrar como integrar uma aplicação .NET de ambiente de trabalho do Windows (XAML) com *início de sessão com a Microsoft* para que a aplicação possa consultar a APIs da Web que necessitam de um token.

A aplicação que criar com este guia mostra um botão que é utilizado para chamar um gráfico, uma área para mostrar os resultados no ecrã e botão de início de sessão.

> [!NOTE]
> Prefere transferir o projeto do Visual Studio este exemplo em vez disso? [Transferir um projeto de](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip)e avance para o [passo da configuração](#create-an-application-express) para configurar o exemplo de código antes de executá-lo.
>

Para criar a sua aplicação, efetue o seguinte:
1. No Visual Studio, selecione **ficheiro** > **novo** > **projeto**.
2. Em **modelos**, selecione **Visual c#**.
3. Selecione **aplicação WPF** ou **aplicação WPF**, consoante a versão da versão do Visual Studio que está a utilizar.

## <a name="add-msal-to-your-project"></a>Adicionar MSAL ao seu projeto
1. No Visual Studio, selecione **ferramentas** > **Gestor de pacotes NuGet**> **consola do Gestor de pacotes**.
2. Na janela da consola do Gestor de pacotes, cole o seguinte comando do PowerShell do Azure:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Este comando instala a biblioteca de autenticação da Microsoft. MSAL processa a aquisição, colocação em cache e atualizar os tokens de utilizador que são utilizados para aceder o APIs que estão protegidos pelo Azure Active Directory v2.
    >

## <a name="add-the-code-to-initialize-msal"></a>Adicione o código para inicializar MSAL
Neste passo, crie uma classe para processar a interação com MSAL, por exemplo, o processamento de tokens.

1. Abra o *App.xaml.cs* de ficheiros e, em seguida, adicione a referência para MSAL à classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Atualize a classe de aplicação para o seguinte:

    ```csharp
    public partial class App : Application
    {
        //Below is the clientId of your app registration. 
        //You have to replace the below with the Application Id for your app registration
        private static string ClientId = "your_client_id_here";

        public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

    }
    ```

## <a name="create-the-application-ui"></a>Criar a IU da aplicação
Esta secção mostra como uma aplicação pode consultar um servidor de back-end protegido, tais como o Microsoft Graph. 

A *MainWindow.xaml* ficheiro deve ser criado automaticamente como parte do seu modelo de projeto. Abrir este ficheiro e, em seguida, substitua a aplicação  *\<grelha >* nó com o seguinte código:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

