---
title: Tutorial – utilizar o Azure Key Vault com uma aplicação web do Azure em .NET | Documentos da Microsoft
description: Tutorial - configurar uma aplicação ASP.NET core para ler um segredo do Key vault
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: defe1a109381c7ee44c6fc5e5db4c6f6ecc5ac6f
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706845"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Tutorial: Utilização do Azure Key Vault com uma aplicação web do Azure no .NET

O Azure Key Vault ajuda a proteger segredos, como chaves de API e as cadeias de ligação de base de dados. Ele fornece acesso às suas aplicações, serviços e recursos de TI.

Neste tutorial, saiba como criar uma aplicação web do Azure que pode ler as informações de um cofre de chave do Azure. O processo utiliza identidades geridas para recursos do Azure. Para obter mais informações sobre aplicações web do Azure, consulte [aplicações Web do Azure](../app-service/app-service-web-overview.md).

O artigo mostra-lhe como para:

> [!div class="checklist"]
> * Criar um cofre de chaves.
> * Armazene um segredo no cofre de chaves.
> * Obter um segredo do cofre de chaves.
> * Criar uma aplicação Web do Azure.
> * Ative uma [identidade gerida](../active-directory/managed-identities-azure-resources/overview.md) para a aplicação Web.
> * Conceda as permissões exigidas para a aplicação ler dados do cofre de chaves.
> * Execute a aplicação web no Azure.

Antes de continuar, leia [conceitos básicos do Key Vault](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Pré-requisitos

* No Windows:
  * [SDK .NET Core 2.1 ou posterior](https://www.microsoft.com/net/download/windows)

* No Mac:
  * [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/)

* Todas as plataformas:
  * [Git](https://git-scm.com/downloads)
  * Uma subscrição do Azure <br />(Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.)
  * [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versão 2.0.4 ou posterior, disponível para Windows, Mac e Linux
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="managed-service-identity-and-how-it-works"></a>Identidade de serviço gerida e como funciona

O Azure Key Vault armazena com segurança as credenciais para que eles não estão no seu código. No entanto, tem de autenticar para o Azure Key Vault para recuperar as suas chaves. Para autenticar para o Key Vault, precisa de uma credencial. É um dilema bootstrap clássico. Identidade de serviço gerida (MSI) resolve este problema, fornecendo uma _inicializar identidade_ que simplifica o processo.

Quando ativar o MSI para um serviço do Azure (por exemplo: máquinas virtuais, o serviço de aplicações ou funções), o Azure cria um [Principal de serviço](key-vault-whatis.md#basic-concepts). MSI faz isso para a instância do serviço no Azure Active Directory (Azure AD) e injeta as credenciais para o Principal de serviço nessa instância.

![Diagrama MSI](media/MSI.png)

Em seguida, o seu código chama um serviço de metadados locais disponível no recurso do Azure para obter um token de acesso. O código utiliza o token de acesso que obtém do MSI_ENDPOINT local para se autenticar no serviço Azure Key Vault.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Para iniciar sessão no Azure com a CLI do Azure, introduza:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

1. Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create).
1. Selecione um nome do grupo de recursos e preencha o marcador de posição. O exemplo seguinte cria um grupo de recursos na localização E.U.A Oeste:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Utilize este grupo de recursos ao longo deste tutorial.

## <a name="create-a-key-vault"></a>Criar um cofre de chaves

Para criar um cofre de chaves no seu grupo de recursos, forneça as seguintes informações:

* Nome do Cofre de chaves: uma cadeia de caracteres de 3 a 24 carateres que pode conter apenas números, letras e hífenes (por exemplo: 0 a 9, a-z, A-Z e -)
* Nome do grupo de recursos
* Localização: **E.U.A. oeste**

Introduza o seguinte comando na CLI do Azure:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-the-key-vault"></a>Adicionar um segredo ao cofre de chaves

Agora pode adicionar um segredo. Pode ser uma cadeia de ligação de SQL ou outras informações que precisa para manter seguros e disponíveis para a aplicação.

Tipo chamado do seguinte comando para criar um segredo no Cofre de chaves **AppSecret**. Este segredo armazena o valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver o valor contido no segredo como texto sem formatação, introduza o seguinte comando:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando apresenta informações do segredo, incluindo o URI. Depois de concluir estes passos, deve ter um URI para um segredo num cofre de chaves. Tome nota destas informações. Irá precisar deles noutro passo.

## <a name="create-a-net-core-web-app"></a>Criar uma aplicação web .NET Core

Siga este [tutorial](../app-service/app-service-web-get-started-dotnet.md) para criar uma aplicação web .NET Core e **publicar** -lo para o Azure. Também pode ver o vídeo seguinte:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Abrir e editar a solução

1. Navegue para o **páginas** > **About.cshtml.cs** ficheiro.
2. Instale estes pacotes de NuGet:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importe o seguinte código no ficheiro About.cshtml.cs:

   ```
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

4. O código na classe AboutModel deve assim:

   ```
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;

                /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
                do
                {
                    long waitTime = Math.Min(getWaitTime(retries), 2000000);
                    secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                    retry = false;
                } 
                while(retry && (retries++ < 10));
            }
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
                if((int)keyVaultException.Response.StatusCode == 429)
                    retry = true;
            }
        }

        // This method implements exponential backoff incase of 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-app"></a>Executar a aplicação

1. No menu principal do Visual Studio 2017, selecione **depurar** > **iniciar** com ou sem depuração. 
1. Quando o browser é apresentado, aceda à página **Sobre**.
1. O valor para o **AppSecret** é apresentado.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Ativar uma identidade gerida para a aplicação Web

O Azure Key Vault oferece uma forma de armazenar em segurança as credenciais e outros segredos, mas o seu código precisa para autenticar para o Key Vault para recuperá-los. [Gerido identidades para descrição geral de recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md) ajuda a resolver esse problema fornecendo Azure dos serviços de uma identidade gerida automaticamente no Azure AD. Pode utilizar esta identidade para autenticar em qualquer serviço que suporte a autenticação do Azure AD, incluindo o Key Vault, sem ser necessário ter credenciais no seu código.

1. Na CLI do Azure, execute o comando de atribuir identidade para criar a identidade para esta aplicação:

   ```azurecli

   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"

   ```

   >[!NOTE]
   >Terá de substituir \<NomeAplic\> com o nome da aplicação publicada no Azure. Por exemplo, se o nome da sua aplicação publicada foi **MyAwesomeapp.azurewebsites.net**, substitua \<NomeAplic\> com **MyAwesomeapp**.

1. Anote o `PrincipalId` ao publicar a aplicação no Azure. A saída do comando no passo 1 deve estar no seguinte formato:

   ```
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
   ```

>[!NOTE]
>O comando neste procedimento é o equivalente a aceder ao [portal](https://portal.azure.com) e mudar a definição **Identidade/Sistema atribuído** para **Ativado**, nas propriedades da aplicação Web.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Atribuir permissões à sua aplicação para ler segredos do Cofre de Chaves

Substituir \<YourKeyVaultName\> com o nome do seu Cofre de chaves e \<PrincipalId\> com o valor de **PrincipalId** no comando seguinte:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Este comando apresenta a identidade (MSI) da aplicação permissões de serviço para o fazer **Obtenha** e **lista** operações no seu Cofre de chaves.

## <a name="publish-the-web-application-to-azure"></a>Publicar a aplicação Web no Azure

Publicar a aplicação web para o Azure novamente para ver que a sua aplicação web em direto pode obter o valor secreto.

1. No Visual Studio, selecione o projeto **key-vault-dotnet-core-quickstart**.
2. Selecione **Publicar** > **Iniciar**.
3. Selecione **Criar**.

Quando executar o aplicativo, verá que este possa obter seu valor secreto.

Agora, que agora com êxito criou uma aplicação web no .NET que armazena e obtém os respetivos segredos do Key Vault.

## <a name="next-steps"></a>Passos Seguintes

>[!div class="nextstepaction"]
>[Guia do Programador do Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-developers-guide)
