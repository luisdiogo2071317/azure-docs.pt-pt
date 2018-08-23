---
title: Autenticação serviço a serviço para o Azure Key Vault com o .NET
description: Utilize a biblioteca de Microsoft.Azure.Services.AppAuthentication para autenticar para o Azure Key Vault com o .NET.
keywords: credenciais de locais de autenticação de Cofre de chaves do Azure
author: bryanla
manager: mbaldwin
services: key-vault
ms.author: bryanla
ms.date: 11/15/2017
ms.topic: article
ms.prod: ''
ms.service: key-vault
ms.technology: ''
ms.assetid: 4be434c4-0c99-4800-b775-c9713c973ee9
ms.openlocfilehash: b158414e7a2954981534fe6fb26c987eb2f4ce67
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42059561"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Autenticação serviço a serviço para o Azure Key Vault com o .NET

Para autenticar para o Azure Key Vault, precisa de uma credencial do Azure Active Directory (AD), um segredo partilhado ou um certificado. Gerir essas credenciais pode ser difícil e é tentador para reunir as credenciais para uma aplicação ao incluí-los nos arquivos de origem ou de configuração.

O `Microsoft.Azure.Services.AppAuthentication` para a biblioteca .NET simplifica este problema. Utiliza credenciais do programador para autenticar durante o desenvolvimento local. Quando a solução for implementada posteriormente para o Azure, a biblioteca muda automaticamente para as credenciais do aplicativo.  

Com as credenciais de desenvolvedor durante o desenvolvimento local é mais seguro porque não é necessário criar as credenciais do Azure AD ou partilhar as credenciais entre os desenvolvedores.

O `Microsoft.Azure.Services.AppAuthentication` biblioteca gere a autenticação automaticamente, que por sua vez, pode se concentrar em sua solução, em vez das suas credenciais.

O `Microsoft.Azure.Services.AppAuthentication` biblioteca oferece suporte a desenvolvimento local com o Microsoft Visual Studio, a CLI do Azure ou a autenticação integrada do Azure AD. Quando implementado nos serviços de aplicações do Azure ou de uma Máquina Virtual do Azure (VM), a biblioteca utiliza automaticamente [identidade do serviço gerido](/azure/active-directory/msi-overview) (MSI). Sem alterações de configuração ou de código são necessárias. A biblioteca também suporta a utilização direta do Azure AD [credenciais de cliente](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) quando o MSI não está disponível ou quando o contexto de segurança do desenvolvedor não é possível determinar durante o desenvolvimento local.

<a name="asal"></a>
## <a name="using-the-library"></a>Usando a biblioteca

Para aplicativos .NET, a maneira mais simples de trabalhar com uma identidade de serviço gerida (MSI) é por meio do `Microsoft.Azure.Services.AppAuthentication` pacote. Eis como começar a utilizar:

1. Adicionar uma referência para o [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) pacote NuGet ao seu aplicativo.

2. Adicione o seguinte código:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // ...

    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(
    azureServiceTokenProvider.KeyVaultTokenCallback));

    // or

    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync(
       "https://management.azure.com/").ConfigureAwait(false);
    ```

O `AzureServiceTokenProvider` classe armazena em cache o token na memória e obtém-lo a partir do Azure AD apenas antes da expiração. Conseqüentemente, já não tem de verificar a expiração antes de chamar o `GetAccessTokenAsync` método. Simplesmente chame o método quando pretender utilizar o token. 

O `GetAccessTokenAsync` método requer um identificador de recurso. Para obter mais informações, consulte [dos serviços do Azure que suportam a identidade do serviço gerido](https://docs.microsoft.com/azure/active-directory/msi-overview#which-azure-services-support-managed-service-identity).


<a name="samples"></a>
## <a name="samples"></a>Amostras

Os seguintes exemplos mostram a `Microsoft.Azure.Services.AppAuthentication` biblioteca em ação:

1. [Utilizar uma identidade de serviço gerida (MSI) para obter um segredo a partir do Azure Key Vault em tempo de execução](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Por meio de programação implementar um modelo do Azure Resource Manager a partir de uma VM do Azure com um MSI](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Utilize o exemplo de .NET Core e o MSI para chamar serviços do Azure a partir de uma VM do Linux do Azure](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).


<a name="local"></a>
## <a name="local-development-authentication"></a>Autenticação de desenvolvimento local

Para o desenvolvimento local, existem dois cenários de autenticação primária:

- [Autenticar-se aos serviços do Azure](#authenticating-to-azure-services)
- [Autenticar para serviços personalizados](#authenticating-to-custom-services)

Aqui aprenderá os requisitos para cada cenário e as ferramentas suportadas.


### <a name="authenticating-to-azure-services"></a>Autenticar-se aos serviços do Azure

Máquinas locais não suportam a identidade de serviço gerida (MSI).  Como resultado, o `Microsoft.Azure.Services.AppAuthentication` biblioteca utiliza as suas credenciais de desenvolvedor para executar no seu ambiente de desenvolvimento local. Quando a solução é implementada no Azure, a biblioteca usa MSI para mudar para um fluxo de concessão de credenciais de cliente OAuth 2.0.  Isso significa que pode testar o mesmo código localmente e remotamente sem se preocupar.

Para o desenvolvimento local, `AzureServiceTokenProvider` obtém os tokens utilizando **Visual Studio**, **interface de linha de comandos do Azure** (CLI), ou **autenticação integrada do Azure AD**. Cada opção é experimentada sequencialmente e a biblioteca usa a primeira opção for concluída com êxito. Se nenhuma opção funcionar, uma `AzureServiceTokenProviderException` exceção é lançada com informações detalhadas.

### <a name="authenticating-with-visual-studio"></a>Autenticar com o Visual Studio

Para utilizar o Visual Studio, verifique se:

1. Instalou [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) ou posterior.

2. O [extensão de autenticação da aplicação para o Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354) está instalado.
 
3. Iniciou sessão no Visual Studio e selecionou uma conta a utilizar para o desenvolvimento local. Uso **ferramentas**&nbsp;>&nbsp;**opções**&nbsp;>&nbsp;**autenticação de serviço do Azure**escolher uma conta de desenvolvimento local. 

Caso se depare com problemas com o Visual Studio, como erros relacionados com o arquivo do fornecedor do token, reveja com atenção estes passos. 

Também poderá ser necessário autenticar o token de desenvolvedor.  Para tal, aceda a **ferramentas**&nbsp;>&nbsp;**opções**>**Azure&nbsp;serviço&nbsp;autenticação**  e procure um **novamente à autenticação** ligação sob a conta selecionada.  Selecione-a para autenticar. 

### <a name="authenticating-with-azure-cli"></a>Autenticação com a CLI do Azure

Utilizar a CLI do Azure para desenvolvimento local:

1. Instale [CLI do Azure v2.0.12](/cli/azure/install-azure-cli) ou posterior. Atualize versões anteriores. 

2. Uso **início de sessão az** para iniciar sessão no Azure.

Utilize `az account get-access-token` para verificar o acesso.  Se receber um erro, certifique-se de que o passo 1 foi concluída com êxito. 

Se a CLI do Azure não está instalada no diretório padrão, poderá receber um erro de relatório que `AzureServiceTokenProvider` não é possível localizar o caminho para a CLI do Azure.  Utilize o **AzureCLIPath**variável de ambiente para definir a pasta de instalação da CLI do Azure. `AzureServiceTokenProvider` Adiciona o diretório especificado na **AzureCLIPath** variável de ambiente para o **caminho** variável de ambiente quando necessário.

Se tiver entrado CLI do Azure com várias contas ou a sua conta tem acesso a várias subscrições, tem de especificar a subscrição específica a ser utilizado.  Para tal, utilize:

```
az account set --subscription <subscription-id>
```

Este comando gera saída só em caso de falha.  Para verificar as definições de conta atual, utilize:

```
az account list
```

### <a name="authenticating-with-azure-ad-integrate-authentication"></a>Autenticação com a autenticação do Azure AD integrar

Para utilizar autenticação do Azure AD, certifique-se de que:

- O active directory no local [sincroniza com o Azure AD](/azure/active-directory/connect/active-directory-aadconnect).

- O código é executado num computador associado a um domínio.


### <a name="authenticating-to-custom-services"></a>Autenticar para serviços personalizados

Quando uma chamada de serviço, serviços do Azure, os passos anteriores funcionarem porque os serviços do Azure permitem o acesso a utilizadores e aplicações.  

Ao criar um serviço que chama um serviço personalizado, utilize credenciais de cliente do Azure AD para autenticação de desenvolvimento local.  Existem duas opções: 

1.  Utilize um principal de serviço para iniciar sessão no Azure:

    1.  [Criar um principal de serviço](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Utilize a CLI do Azure para iniciar sessão:

        ```
        az login --service-principal -u <principal-id> --password <password>
           --tenant <tenant-id> --allow-no-subscriptions
        ```

        Uma vez que o principal de serviço pode não ter acesso a uma subscrição, utilize o `--allow-no-subscriptions` argumento.

2.  Utilize variáveis de ambiente para especificar detalhes de principal de serviço.  Para obter detalhes, consulte [executando o aplicativo com um principal de serviço](#running-the-application-using-a-service-principal).

Depois de iniciar sessão para o Azure, `AzureServiceTokenProvider` utiliza o principal de serviço para obter um token para o desenvolvimento local.

Isto aplica-se apenas a desenvolvimento local. Quando a solução é implementada no Azure, a biblioteca muda para autenticação de MSI.

<a name="msi"></a>
## <a name="running-the-application-using-a-managed-service-identity"></a>Executar a aplicação com uma identidade do serviço gerido 

Quando executa seu código num serviço de aplicações do Azure ou uma VM do Azure com o MSI ativada, a biblioteca utiliza automaticamente a identidade do serviço gerido. Sem alterações de código são necessárias. 


<a name="sp"></a>
## <a name="running-the-application-using-a-service-principal"></a>Executar a aplicação com um Principal de serviço 

Poderá ser necessário criar uma credencial de cliente do Azure AD para autenticar. Exemplos comuns incluem:

1. O código é executado num ambiente de desenvolvimento local, mas não sob a identidade do desenvolvedor.  Recursos de infraestrutura do serviço, por exemplo, utiliza a [conta NetworkService](/azure/service-fabric/service-fabric-application-secret-management) para desenvolvimento local.
 
2. O código é executado num ambiente de desenvolvimento local e autenticar para um serviço personalizado, para que não é possível utilizar a sua identidade de desenvolvedor. 
 
3. O código é executado num recurso de computação do Azure que ainda não oferece suporte a identidade do serviço gerido, como o Azure Batch.

Para utilizar um certificado para iniciar sessão no Azure AD:

1. Criar uma [certificado de serviço principal](/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

2. Implementar o certificado ou o _LocalMachine_ ou _CurrentUser_ armazenar. 

3. Definir uma variável de ambiente com o nome **AzureServicesAuthConnectionString** para:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={LocalMachine or CurrentUser}
    ```
 
    Substitua _{AppId}_, _{TenantId}_, e _{Thumbprint}_ com valores geradas no passo 1.

    **CertificateStoreLocation** tem de ser _CurrentUser_ ou _LocalMachine_, com base no seu plano de implementação.

4. Execute a aplicação. 

Para iniciar sessão com um Azure AD partilhado credencial secreta:

1. Criar uma [principal de serviço com uma palavra-passe](/azure/azure-resource-manager/resource-group-authenticate-service-principal) e conceder acesso ao Key Vault. 

2. Definir uma variável de ambiente com o nome **AzureServicesAuthConnectionString** para:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    Substitua _{AppId}_, _{TenantId}_, e _{ClientSecret}_ com valores geradas no passo 1.

3. Execute a aplicação. 

Uma vez que tudo está configurado corretamente, sem alterações de código adicionais são necessários.  `AzureServiceTokenProvider` utiliza a variável de ambiente e o certificado para autenticar para o Azure AD. 

<a name="connectionstrings"></a>
## <a name="connection-string-support"></a>Suporte de cadeia de ligação

Por predefinição, `AzureServiceTokenProvider` usa vários métodos para obter um token. 

Para controlar o processo, use uma cadeia de ligação passada para o `AzureServiceTokenProvider` construtor ou foi especificado no *AzureServicesAuthConnectionString* variável de ambiente. 

São suportadas as seguintes opções:

| Ligação&nbsp;cadeia de caracteres&nbsp;opção | Cenário | Comentários|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Desenvolvimento local | AzureServiceTokenProvider utiliza AzureCli ao obter o token. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Desenvolvimento local | AzureServiceTokenProvider usa o Visual Studio para obter o token. |
| `RunAs=CurrentUser;` | Desenvolvimento local | AzureServiceTokenProvider utiliza autenticação integrada do Azure AD ao obter o token. |
| `RunAs=App;` | Identidade do Serviço Gerido | AzureServiceTokenProvider utiliza a identidade do serviço gerido para obter o token. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint`<br>`   ={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`  | Principal de serviço | `AzureServiceTokenProvider` utiliza o certificado ao obter o token do Azure AD. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};`<br>`   CertificateSubjectName={Subject};CertificateStoreLocation=`<br>`   {LocalMachine or CurrentUser}` | Principal de serviço | `AzureServiceTokenProvider` utiliza o certificado ao obter o token do Azure AD|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Principal de serviço |`AzureServiceTokenProvider` utiliza o segredo para obter o token do Azure AD. |


## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [identidade do serviço gerido](/azure/app-service/app-service-managed-service-identity).

- Conheça formas diferentes [autenticar e autorizar aplicações](/azure/app-service/app-service-authentication-overview).

- Saiba mais sobre o Azure AD [cenários de autenticação](/azure/active-directory/develop/active-directory-authentication-scenarios#web-browser-to-web-application).
