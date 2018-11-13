---
title: Tutorial - Como utilizar o Azure Key Vault com a Aplicação Web do Azure em .NET | Microsoft Docs
description: Tutorial para configurar uma aplicação ASP.NET Core para ler um segredo do Key Vault
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
ms.openlocfilehash: 9cc22e158a9473b7b60f7e8bcb57174abc1fb8cc
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218557"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-web-app-in-net"></a>Tutorial: Como utilizar o Azure Key Vault com a Aplicação Web do Azure em .NET

O Azure Key Vault ajuda-o a proteger segredos, como chaves de APIs, cadeias de ligação de bases de dados necessárias para aceder às suas aplicações, serviços e recursos de TI.

Neste tutorial, vai seguir os passos necessários para que uma aplicação Web do Azure leia as informações do Azure Key Vault mediante a utilização de identidades geridas para os recursos do Azure. Este tutorial baseia-se nas [Aplicações Web do Azure](../app-service/app-service-web-overview.md). Vai aprender a:

> [!div class="checklist"]
> * Criar um cofre de chaves.
> * Armazene um segredo no cofre de chaves.
> * Obter um segredo do cofre de chaves.
> * Criar uma aplicação Web do Azure.
> * Ative uma [identidade gerida](../active-directory/managed-identities-azure-resources/overview.md) para a aplicação Web.
> * Conceda as permissões exigidas para a aplicação ler dados do cofre de chaves.
> * Executar a aplicação Web no Azure

Antes de continuarmos, leia os [conceitos básicos](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Pré-requisitos

* No Windows:
  * [SDK .NET Core 2.1 ou posterior](https://www.microsoft.com/net/download/windows)

* No Mac:
  * Veja [Novidades do Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).

* Todas as plataformas:
  * Git ([transferir](https://git-scm.com/downloads)).
  * Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
  * [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versão 2.0.4 ou posterior. Está disponível para Windows, Mac e Linux.
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>O que é a Identidade de Serviço Gerida e como funciona?
 Antes de avançarmos, vamos compreender o que é o MSI. O Azure Key Vault pode armazenar credenciais em segurança, para que não estejam no seu código. Contudo, para as obter, tem de se autenticar no serviço. Para se autenticar no Key Vault, precisa de uma credencial! Trata-se de um problema clássico de arranque do sistema. Com a magia do Azure e do Azure AD, o MSI disponibiliza uma “identidade de arranque de sistema” que permite fazer tudo de forma muito mais simples.

Eis como funciona: Quando ativa o MSI para um serviço do Azure, como as Máquinas Virtuais, o Serviço de Aplicações ou as Funções do Azure, o Azure cria um [Principal de Serviço](key-vault-whatis.md#basic-concepts) para a instância do serviço no Azure Active Directory e injeta as credenciais desse Principal de Serviço na instância. 

![MSI](media/MSI.png)

Depois, o código chama um serviço de metadados local que está disponível no recurso do Azure, para obter um token.
O código utiliza o token de acesso que obtém do MSI_ENDPOINT local para se autenticar no serviço Azure Key Vault. 

Vamos começar então o tutorial.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Para iniciar sessão no Azure com a CLI do Azure, introduza:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

Selecione um nome do grupo de recursos e preencha o marcador de posição.
O exemplo seguinte cria um grupo de recursos na localização E.U.A Oeste:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

O grupo de recursos que acabou de criar é utilizado ao longo deste artigo.

## <a name="create-a-key-vault"></a>Criar um cofre de chaves

A seguir, vai criar um cofre de chaves no grupo de recursos criado no passo anterior. Forneça as seguintes informações:

* Nome do cofre de chaves: O nome tem de ser uma cadeia de carateres de 3 a 24 carateres e tem de conter apenas (0 a 9, a-z, A-Z e -).
* Nome do grupo de recursos.
* Localização: **EUA Oeste**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-the-key-vault"></a>Adicionar um segredo ao cofre de chaves

Estamos a adicionar um segredo para ajudar a ilustrar a forma como isto funciona. Pode armazenar uma cadeia de ligação do SQL ou outras informações que precise de manter em segurança, mas mantenha disponível para a sua aplicação.

Escreva os seguintes comandos para criar um segredo no cofre de chaves designado **AppSecret**. Este segredo irá armazenar o valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver o valor contido no segredo como texto simples:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando apresenta informações do segredo, incluindo o URI. Depois de concluir estes passos, deve ter um URI para um segredo num cofre de chaves. Tome nota destas informações. Irá precisar deles noutro passo.

## <a name="create-a-net-core-web-app"></a>Criar uma aplicação Web .NET Core

Siga este [tutorial](../app-service/app-service-web-get-started-dotnet.md) para criar uma Aplicação Web .NET Core e **publicá-la** no Azure **OU** veja o vídeo abaixo.
> [!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Abrir e editar a solução

1. Navegue para Páginas > ficheiro About.cshtml.cs.
2. Instale estes dois pacotes NuGet
    - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
    - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importe o seguinte no ficheiro About.cshtml.cs

    ```
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
    ```
4. O seu código na classe AboutModel deve ser semelhante ao seguinte
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

No menu principal do Visual Studio 2017, selecione **Debug** (Depurar) > **Start** with/without debugging (Começar com/sem depuração). Quando o browser é apresentado, aceda à página **Sobre**. O valor para o **AppSecret** é apresentado.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Ativar uma identidade gerida para a aplicação Web

O Azure Key Vault oferece uma forma de armazenar credenciais e outras chaves e segredos em segurança, mas o código tem de se autenticar no Key Vault para poder obtê-los. A [descrição geral das identidades geridas para os recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md) simplifica a resolução deste problema ao conferir aos serviços do Azure uma identidade gerida automaticamente no Microsoft Azure Active Directory (Microsoft Azure AD). Pode utilizar esta identidade para autenticar em qualquer serviço que suporte a autenticação do Azure AD, incluindo o Key Vault, sem ser necessário ter credenciais no seu código.

1. Voltar à CLI do Azure.
2. Execute o comando assign-identity para criar a identidade para esta aplicação: 

   ```azurecli
   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
   ```
   Tenha em conta que tem de substituir <YourAppName> pelo nome da aplicação publicada no Azure, ou seja, se o nome da aplicação publicada for MyAwesomeapp.azurewebsites.net, substitua <YourAppName> por MyAwesomeapp
 
 A saída do comando anterior é semelhante a isto. Quando publicar a aplicação no Azure, aponte o PrincipalId. Deverá estar no formato:
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
        
Em seguida, execute este comando com o nome do seu cofre de chaves e o valor do **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

## <a name="publish-the-web-application-to-azure"></a>Publicar a aplicação Web no Azure

Volte a publicar esta aplicação no Azure para a ver em direto como uma aplicação Web e para ver se também consegue obter o valor do segredo.

1. No Visual Studio, selecione o projeto **key-vault-dotnet-core-quickstart**.
2. Selecione **Publicar** > **Iniciar**.
3. Selecione **Criar**.

No comando acima, está a dar a Identidade (MSI) das permissões do Serviço de Aplicações para realizar operações **get** e **list** no Key Vault. <br />
Agora quando executar a aplicação, deverá ver o seu valor secreto obtido. 

E já está! Criou uma aplicação Web em .NET que armazena e obtém os respetivos segredos no Key Vault.
