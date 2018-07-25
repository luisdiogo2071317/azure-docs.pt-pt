---
title: Configure an Azure web application to read a secret from Key vault tutorial| Microsoft Docs (Tutorial para configurar uma aplicação Web do Azure para ler segredos em cofres de segurança | Microsoft Docs)
description: Tutorial para configurar uma aplicação ASP.Net do Azure para ler um segredo do Key Vault
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 05/17/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 747a0fc7f66edbae8d4a99eeaf0ea45f844d6465
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125950"
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Tutorial: Configurar uma aplicação Web do Azure para ler um segredo do Key Vault

Neste tutorial, irá conhecer os passos necessários para que uma aplicação Web do Azure leia as informações do Cofre de chaves com identidades de serviço geridas. Saiba como:

> [!div class="checklist"]
> * Criar um Key Vault.
> * Armazene um segredo no Key Vault.
> * Criar uma aplicação Web do Azure.
> * Ativar as identidades de serviço geridas
> * Conceda as permissões necessárias para a aplicação ler dados do Cofre de chaves.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

Para iniciar sessão no Azure através da CLI, pode escrever:

```azurecli
az login
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome *ContosoResourceGroup* na localização *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "ContosoResourceGroup" --location "East US"
```

O grupo de recursos que acabou de criar é utilizado ao longo deste tutorial.

## <a name="create-an-azure-key-vault"></a>Criar um Azure Key Vault

A seguir, vai criar um Key Vault no grupo de recursos criado no passo anterior. Embora "ContosoKeyVault" seja utilizado como o nome para o Key Vault ao longo deste tutorial, tem de utilizar um nome exclusivo. Forneça as seguintes informações:

* Nome do cofre **ContosoKeyVault**.
* **ContosoResourceGroup** como o nome do grupo de recursos.
* **E.U.A. Leste** como a localização.

```azurecli
az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "East US"
```

O resultado deste comando mostra as propriedades do Key Vault recém-criado. Tome nota das duas propriedades listadas abaixo:

* **Nome do Cofre**: No exemplo, o nome é **ContosoKeyVault**. Irá utilizar o nome do seu Key Vault para todos os comandos do mesmo.
* **URI do Cofre**: No exemplo, trata-se de https://<YourKeyVaultName>.vault.azure.net/. As aplicações que utilizam o cofre através da respetiva API têm de utilizar este URI.

>[!IMPORTANT]
> Se receber o erro do Parâmetro "vault_name" tem de estar em conformidade com o padrão seguinte: "^[a-zA-Z0-9-]{3,24}$" o valor de parâmetro -name não era exclusivo ou não cumpria uma cadeia composta por carateres alfanuméricos de 3 a 24 de comprimento.

Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-key-vault"></a>Adicionar um segredo ao Cofre de chaves

Estamos a adicionar um segredo para ajudar a ilustrar a forma como isto funciona. Pode armazenar uma cadeia de ligação do SQL ou outras informações que precise de manter em segurança, mas mantenha disponível para a sua aplicação. Neste tutorial, a palavra-passe será chamada de **AppSecret** e irá armazenar o valor de **MySecret** no mesmo.

Escreva os comandos abaixo para criar um segredo no Key Vault denominado **AppSecret** que irá armazenar o valor **MySecret**:

```azurecli
az keyvault secret set --vault-name "ContosoKeyVault" --name "AppSecret" --value "MySecret"
```

Para ver o valor contido no segredo como texto simples:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "ContosoKeyVault"
```

Este comando apresenta informações do segredo, incluindo o URI. Depois de concluir estes passos, deve ter um URI para um segredo num Azure Key Vault. Tome nota destas informações. Irá precisar delas noutro passo.

## <a name="create-a-web-app"></a>Criar uma aplicação Web

Nesta secção pode criar uma aplicação ASP.NET MVC e implementá-la no Azure como uma Aplicação Web. Para obter mais informações sobre as Aplicações Web do Azure, veja [Descrição geral das Aplicações Web](../app-service/app-service-web-overview.md).

1. No Visual Studio, crie um projeto ao selecionar **Ficheiro > Novo > Projeto**. 

2. Na caixa de diálogo **Novo projeto**, clique em **Visual C# > Web > Aplicação Web ASP.NET Core**.

3. Nomeie a aplicação de **WebKeyVault** e, em seguida, selecione **OK**.
   >[!IMPORTANT]
   > Tem de nomear a aplicação de WebKeyVault, para que o código que copia e cola corresponda ao espaço de nomes. Se deu outro nome ao site, terá de modificar o código para corresponder ao nome do site.

    ![Caixa de diálogo Novo Projeto ASP.NET](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. Pode implementar qualquer tipo de aplicação Web ASP.NET Core no Azure. Neste tutorial, selecione o modelo **Aplicação Web** e confirme se a autenticação está definida como **Sem Autenticação**.

    ![Caixa de diálogo sem autenticação ASPNET](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. Selecione **OK**.

6. Depois de criado o projeto do ASP.NET Core, a página de boas-vindas do ASP.NET Core é apresentada, com várias ligações para recursos que o podem ajudar a começar.

7. No menu, selecione **Depurar > Iniciar sem depuração** para executar a aplicação Web localmente.

## <a name="modify-the-web-app"></a>Modificar a aplicação Web

Existem dois pacotes NuGet que a aplicação Web tem de ter instalados. Para instalá-los, siga os passos abaixo:

1. Na explorador de soluções, clique com o botão direito do rato no nome do site.
2. Selecione **Gerir pacotes NuGet para a solução...**
3. Selecione a caixa de verificação junto à caixa de pesquisa. **Incluir pré-lançamento**
4. Procure os dois pacotes NuGet listados abaixo e aceite que sejam adicionados à sua solução:

    * [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) – facilita a obtenção de tokens de acesso para cenários de autenticação de "Serviço para o Serviço do Azure". 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) - contém métodos para interagir com o Key Vault.

5. Utilize o Explorador de Soluções para abrir `Program.cs` e substitua os conteúdos do ficheiro Program.cs pelo seguinte código. Substitua ```<YourKeyVaultName>``` pelo nome do seu cofre de chaves:

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
            ).UseStartup<Startup>()
             .Build();

           private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
         }
    }
    ```

6. Utilize o Explorador de Soluções para navegar para a secção **Páginas** e abra `About.cshtml`. Substitua os conteúdos de **About.cshtml.cs** pelo código abaixo:

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

7. No menu principal, escolha **Depurar** > **Iniciar sem a Depuração**. Quando o browser é apresentado, navegue para a página **Sobre**. O valor para o AppSecret é apresentado.

>[!IMPORTANT]
> Se receber uma mensagem de Erro de HTTP 502.5 – Falha de Processo
> > em seguida, verifique o nome do Cofre de Chaves especificado em `Program.cs`

## <a name="publish-the-web-application-to-azure"></a>Publicar a aplicação Web no Azure

1. Acima do editor, selecione **WebKeyVault**.
2. Selecione **Publicar** e, em seguida, **Iniciar**.
3. Crie um novo **Serviço de Aplicações**, selecione **Publicar**.
4. Selecione **Criar**.

>[!IMPORTANT]
> É aberta uma janela do browser e verá uma mensagem de Falha de Processo - 502.5. Isto era esperado. Terá de conceder os direitos de identidade da aplicação para ler os segredos do Key Vault.

## <a name="enable-managed-service-identity"></a>Ativar a Identidade de Serviço Gerida

O Azure Key Vault oferece uma forma de armazenar credenciais e outras chaves e segredos em segurança, mas o código tem de se autenticar no Key Vault para poder obtê-los. A Identidade de Serviço Gerida (MSI) simplifica a resolução deste problema ao dar aos serviços do Azure uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar em qualquer serviço que suporte a autenticação do Azure AD, incluindo o Key Vault, sem ser necessário ter credenciais no seu código.

1. Voltar à CLI do Azure
2. Execute o comando assign-identity para criar a identidade para esta aplicação:

```azurecli
az webapp identity assign --name "WebKeyVault" --resource-group "ContosoResourcegroup"
```

>[!NOTE]
>Este comando é o equivalente a ir para o portal e mudar a **Identidade do serviço gerido** para **Ativada** nas propriedades da aplicação Web.

## <a name="grant-rights-to-the-application-identity"></a>Conceder direitos para a identidade da aplicação

No portal do Azure, aceda às políticas de acesso do Key Vault e conceda acesso a si próprio à Gestão de Segredos para o Key Vault. Isto irá permitir executar a aplicação no seu computador de desenvolvimento local.

1. Procure o seu Key Vault na caixa de diálogo **Pesquisar Recursos** no portal do Azure.
2. Selecione **Políticas de acesso**.
3. Selecione **Adicionar Novo**, na secção **Permissões de segredos**, selecione **Obter** e **Listar**.
4. Selecione **Selecionar Principal** e adicione a identidade da aplicação. Terá o mesmo nome que a aplicação.
5. Escolha **Ok**.

Agora a sua conta no Azure e a identidade da aplicação têm direitos para ler as informações do Key Vault. Ao atualizar a página, deverá ver a página de destino do site. Se selecionar **Sobre**, verá o valor armazenado no Key Vault.

## <a name="clean-up-resources"></a>Limpar recursos

Para eliminar um grupo de recursos e todos os respetivos recursos, utilize o comando **az group delete**.

  ```azurecli
  az group delete -n "ContosoResourceGroup"
  ```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Guia do Programador do Azure Key Vault](key-vault-developers-guide.md)
