---
title: Configurar uma aplicação web do Azure para ler um segredo do Cofre de chaves | Microsoft Docs
description: Tutorial de configurar uma aplicação ASP.Net core ler um segredo do Cofre de chaves
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 04/16/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 4a765b314b9879877bb6ff926e4a6584456b7823
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Tutorial: Configurar uma aplicação web do Azure para ler um segredo do Cofre de chaves

Neste tutorial, passam os passos necessários para obter uma aplicação web do Azure para ler as informações a partir do Cofre de chaves com identidades de serviço geridas. Saiba como:

> [!div class="checklist"]
> * Crie um cofre de chaves.
> * Armazene um segredo no Cofre de chaves.
> * Crie uma aplicação Web do Azure.
> * Ativar identidades de serviço geridas
> * Conceda as permissões necessárias para a aplicação para ler dados a partir do Cofre de chaves.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

Para iniciar sessão Azure utilizando a CLI, escreva:

```azurecli
az login
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli
az group create --name ContosoResourceGroup --location eastus
```

O grupo de recursos que acabou de criar é utilizado ao longo deste tutorial.

## <a name="create-an-azure-key-vault"></a>Criar um Azure Key Vault

Em seguida, criar um cofre de chaves no grupo de recursos criado no passo anterior. Tem de fornecer algumas informações:

>[!NOTE]
> Embora "ContosoKeyVault" for utilizado como o nome do nosso Cofre de chaves ao longo deste tutorial, tem de utilizar um nome exclusivo.

* Nome do cofre **ContosoKeyVault**.
* Nome do grupo de recursos **ContosoResourceGroup**.
* A localização **EUA Leste**.

```azurecli
az keyvault create --name '<YourKeyVaultName>' --resource-group ContosoResourceGroup --location eastus
```

O resultado deste comando mostra as propriedades do Cofre de chaves recentemente criado. Tome nota das duas propriedades listados abaixo:

* **Nome do Cofre**: No exemplo, o nome é **ContosoKeyVault**. Irá utilizar o nome do seu Cofre de chaves para todos os comandos do Cofre de chaves.
* **URI do cofre**: no exemplo, isto é https://<YourKeyVaultName>.vault.azure.net/. As aplicações que utilizam o cofre através da respetiva API têm de utilizar este URI.

>[!IMPORTANT]
> Se receber o erro do parâmetro 'vault_name' tem de estar em conformidade com o padrão do seguinte: ' ^ [uma-zA-Z0 - 9-] {3,24} $' param-nome valor não estava exclusivo ou não cumpria numa cadeia composto por carateres alfanuméricos de 3 a 24 longo.

Neste momento, a conta do Azure é a única autorizada para efetuar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-key-vault"></a>Adicionar um segredo ao Cofre de chaves

Que estamos a adicionar um segredo para ajudar a ilustrar a forma como isto funciona. Pode ser a armazenar uma cadeia de ligação do SQL Server ou outras informações que tem de manter de forma segura, mas tornar disponível para a aplicação. Neste tutorial a palavra-passe será chamada **AppSecret** e irá armazenar o valor de **MySecret** no mesmo.

Escreva os comandos abaixo para criar um segredo no Cofre de chaves chamado **AppSecret** que irá armazenar o valor **MySecret**:

```azurecli
az keyvault secret set --vault-name '<YourKeyVaultName>' --name 'AppSecret' --value 'MySecret'
```

Para ver o valor contido no segredo como texto simples:

```azurecli
az keyvault secret show --name 'AppSecret' --vault-name '<YourKeyVaultName>'
```

Este comando apresenta informações secretas, incluindo o URI. Depois de concluir estes passos deve ter um URI para um segredo um cofre de chaves do Azure. Tome nota destas informações. Terá num passo posterior.

## <a name="create-a-web-app"></a>Criar uma aplicação Web

Nesta secção pode criar uma aplicação ASP.NET MVC e implementá-la no Azure como uma aplicação Web. Para obter mais informações sobre as aplicações Web do Azure, consulte [descrição geral das aplicações Web](../app-service/app-service-web-overview.md).

1. No Visual Studio, crie um projeto ao selecionar **Ficheiro > Novo > Projeto**. 

2. Na caixa de diálogo **Novo projeto**, clique em **Visual C# > Web > Aplicação Web ASP.NET Core**.

3. Atribua um nome de aplicação **WebKeyVault**e, em seguida, selecione **OK**.
   >[!IMPORTANT]
   > Tem de nome de aplicação WebKeyVault para que o código de copiar e colar corresponderá o espaço de nomes. Se a o nome do site há mais alguma coisa, terá de modificar o código de corresponder ao nome do site.

    ![Caixa de diálogo Novo Projeto ASP.NET](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. Pode implementar qualquer tipo de aplicação Web ASP.NET Core no Azure. Para este tutorial, selecione o **aplicação Web** modelo e certifique-se a autenticação está definida como **sem autenticação**.

    ![ASPNET nenhuma caixa de diálogo de autenticação](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. Selecione **OK**.

6. Depois de criado o projeto do ASP.NET Core, a página de boas-vindas do ASP.NET Core é apresentada, com várias ligações para recursos que o podem ajudar a começar.

7. No menu, selecione **Depurar > Iniciar sem depuração** para executar a aplicação Web localmente.

## <a name="modify-the-web-app"></a>Modificar a aplicação web

Existem dois pacotes de NuGet que a aplicação web tem de ter instalado. Para instalá-los siga os passos abaixo:

1. Na solução explorer com o botão direito no seu nome de Web site.
2. Selecione **pacotes gerir NuGet para solução...**
3. Selecione a caixa de verificação junto à caixa de pesquisa. **Incluir pré-lançamento**
4. Procure os dois pacotes de NuGet listados abaixo e aceitar-lhes a ser adicionado à sua solução:

    * [Microsoft.Azure.Services.AppAuthentication (pré-visualização)](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) -torna mais fácil de obter os tokens de acesso para cenários de autenticação de-de-Azure-serviços. 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/2.4.0-preview) -contém métodos para interagir com o Cofre de chaves.

5. Utilize o Explorador de soluções para abrir `Program.cs` e substitua os conteúdos do ficheiro Program.cs pelo seguinte código. Substitute ```<YourKeyVaultName>``` com o nome do seu Cofre de chaves:

    ```csharp
    
    using Microsoft.AspNetCore;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureKeyVault;
    
        namespace WebKeyVault
        {
        public class Program
        {
        public static void Main(string[] args)
        {
        BuildWebHost(args).Run();
        }
    
            public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((ctx, builder) =>
                {
                    var keyVaultEndpoint = GetKeyVaultEndpoint();
                    if (!string.IsNullOrEmpty(keyVaultEndpoint))
                    {
                        var azureServiceTokenProvider = new AzureServiceTokenProvider();
                        var keyVaultClient = new KeyVaultClient(
                            new KeyVaultClient.AuthenticationCallback(
                                azureServiceTokenProvider.KeyVaultTokenCallback));
                        builder.AddAzureKeyVault(
                            keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                    }
                }
             )
                .UseStartup<Startup>()
                .Build();
    
            private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
        }
        }
    ```

6. Utilize o Explorador de soluções para navegar para o **páginas** secção e abra `About.cshtml`. Substitua os conteúdos de **About.cshtml.cs** com o código abaixo:

    ```csharp
    
    using Microsoft.AspNetCore.Mvc.RazorPages;
    using Microsoft.Extensions.Configuration;
    
    namespace WebKeyVault.Pages
    {
        public class AboutModel : PageModel
        {
            public AboutModel(IConfiguration configuration)
            {
                _configuration = configuration;
            }
    
            private readonly IConfiguration _configuration = null;
            public string Message { get; set; }
    
            public void OnGet()
            {
                Message = "My key val = " +  _configuration["AppSecret"];
            }
        }
    }
    
    ```

7. No menu principal, escolha **depurar** > **iniciar sem a depuração**. Quando for apresentada no browser, navegue para o **sobre** página. O valor para o AppSecret é apresentado.

>[!IMPORTANT]
> Se obtiver um 502.5 de erro HTTP - mensagem de falha de processo Verifique o nome do Cofre de chaves especificado na `Program.cs`

## <a name="publish-the-web-application-to-azure"></a>Publicar a aplicação web no Azure

1. Acima do editor selecione **WebKeyVault**.
2. Selecione **publicar**.
3. Selecione **publicar** novamente.
4. Selecione **criar**.

>[!IMPORTANT]
> Abre uma janela do browser e verá uma mensagem de falha de processo 502.5. Isto é esperado. Terá de conceder os direitos de identidade da aplicação para ler os segredos do Cofre de chaves.

## <a name="enable-managed-service-identity"></a>Ativar a identidade de serviço geridas

O Cofre de chaves do Azure fornece uma forma de armazenar com segurança as credenciais e outras chaves e segredos, mas o seu código tem de autenticar para o Cofre de chaves para recuperar. Identidade de serviço geridas (MSI) permite a resolver este problema mais simples, conferindo aos serviços do Azure uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, incluindo o Cofre de chaves, sem ter as credenciais no seu código.

1. Voltar para a CLI do Azure
2. Execute o comando de atribuir-identity para criar a identidade para esta aplicação:

```azurecli
az webapp assign-identity --name WebKeyVault --resource-group ContosoResourcegroup
```

>[!NOTE]
>Este é o equivalente a ir para o portal e mudança **identidade do serviço gerido** para **no** nas propriedades de aplicação web.

## <a name="grant-rights-to-the-application-identity"></a>Conceder direitos para a identidade da aplicação

No portal do Azure, aceda às políticas de acesso do Cofre de chaves e conceder si próprio o acesso de gestão do segredo ao Cofre de chaves. Isto permitirá executar a aplicação no seu computador de desenvolvimento local.

1. Procure o seu Cofre de chaves no **pesquisar recursos** caixa de diálogo no portal do Azure.
2. Selecione **políticas de acesso**.
3. Selecione **adicionar novo**, no **permissões secretas** secção selecione **obter** e **lista**.
4. Selecione **selecione Principal**e adicione a identidade da aplicação. Terá o mesmo nome que a aplicação.
5. Escolha **Ok**

Agora a sua conta no Azure e a identidade da aplicação tem direitos para ler as informações do Cofre de chaves. Se atualizar a página deverá ver a página de destino do site. Se selecionar **sobre**. Pode ver o valor armazenado no Cofre de chaves.

## <a name="clean-up-resources"></a>Limpar recursos

Para eliminar um grupo de recursos e todos os respetivos recursos, utilize o **eliminação do grupo de az** comando.

  ```azurecli
  az group delete -n ContosoResourceGroup
  ```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Guia para programadores do Cofre de chaves do Azure](key-vault-developers-guide.md)
