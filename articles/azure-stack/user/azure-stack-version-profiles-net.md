---
title: Através de perfis de versão de API com o SDK de .NET no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre como utilizar perfis de versão de API com .NET no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 35329468ee01d5b70d654c1eb4a908db9d3fcb5d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184907"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>Utilizar perfis de versão de API com .NET no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O SDK do .NET para o Azure Stack Resource Manager fornece ferramentas para ajudar a criar e gerir a sua infraestrutura. Fornecedores de recursos no SDK incluem computação, rede, armazenamento, serviços de aplicações, e [KeyVault](../../key-vault/key-vault-whatis.md). O SDK de .NET inclui 14 pacotes NuGet que deverão ser transferidos para a sua solução de projeto sempre que incorporam as informações de perfil. No entanto, pode baixar especificamente qual o fornecedor de recursos que irá utilizar para o 2018-03-01-híbrido ou 2017-03-09-perfil para otimizar a memória para a sua aplicação. Cada pacote é composta por um fornecedor de recursos, a respetiva versão de API e o perfil de API à qual ele pertence. Perfis de API do SDK do .NET permitem o desenvolvimento de cloud híbrida com a ajuda para alternar entre recursos do Azure global e recursos no Azure Stack.

## <a name="net-and-api-version-profiles"></a>Perfis de versão do .NET e de API

Um perfil de API é uma combinação de fornecedores de recursos e as versões de API. Pode utilizar um perfil de API para obter a versão mais recente, mais estável de cada tipo de recurso num pacote de fornecedor de recursos.

-   Para usar as versões mais recentes de todos os serviços, utilize o **mais recente** perfil dos pacotes. Este perfil faz parte do **Microsoft.Azure.Management** pacote NuGet.

-   Para utilizar os serviços compatíveis com o Azure Stack, utilize o **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg** ou **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg** pacotes.

    -   Existem dois pacotes para cada fornecedor de recursos para cada perfil.

    -   Certifique-se de que o **ResourceProvider** parte do pacote NuGet acima é alterado para o fornecedor correto.

-   Para utilizar a API-version mais recente de um serviço, utilize o **mais recente** perfil do pacote NuGet específico. Por exemplo, se pretender utilizar o **API mais recente** versão o serviço de computação autónomo, utilize o **mais recente** perfil dos **computação** pacote. O **mais recente** perfil faz parte do **Microsoft.Azure.Management** pacote NuGet.

-   Para utilizar versões de API específicas para um tipo de recurso de um fornecedor de recursos específico, utilize as versões de API específicas definidas dentro do pacote.

Tenha em atenção que pode combinar todas as opções no mesmo aplicativo.

## <a name="install-the-azure-net-sdk"></a>Instalar o SDK .NET do Azure

1.  Instale o Git. Para obter instruções, consulte [introdução - instalar o Git][].

2.  Para instalar os pacotes de NuGet corretos, consulte [Localizar e instalar um pacote][].

3.  Os pacotes que devem ser instaladas depende da versão de perfil que pretende utilizar. O nome do pacote para as versões de perfil são:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**

4.  Para instalar os pacotes de NuGet corretos para o Visual Studio Code, veja a seguinte hiperligação para transferir os [Instruções do Gestor de pacotes NuGet][].

5.  Se não está disponível, criar uma subscrição e guarde o ID de subscrição a ser utilizado mais tarde. Para obter instruções criar uma subscrição, veja [criar subscrições de ofertas no Azure Stack][].

6.  Criar um principal de serviço e guarde o ID de cliente e o segredo do cliente. Para obter instruções sobre como criar um principal de serviço para o Azure Stack, veja [fornecer acesso de aplicações para o Azure Stack][]. Tenha em atenção que o ID de cliente também é conhecido como o ID da aplicação ao criar um principal de serviço.

7.  Certifique-se de que o seu principal de serviço tem a função de proprietário/Contribuidor na sua subscrição. Para obter instruções sobre como atribuir uma função ao principal de serviço, consulte [fornecer acesso de aplicações para o Azure Stack][].

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o SDK do .NET com o Azure Stack, tem de indicar os valores seguintes e, em seguida, definir valores com variáveis de ambiente. Para definir as variáveis de ambiente, consulte as instruções a seguir à tabela para o seu sistema operativo.

| Valor                     | Variáveis de ambiente   | Descrição                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| ID do inquilino                 | AZURE_TENANT_ID       | O valor do seu Azure Stack [ *ID de inquilino*][].                                                                          |
| ID de Cliente                 | AZURE_CLIENT_ID       | O serviço de ID da aplicação principal guardado quando o principal de serviço foi criado na secção anterior deste artigo. |
| ID da subscrição           | AZURE_SUBSCRIPTION_ID | O [ *ID da subscrição* ][] é como acessa ofertas no Azure Stack.                                                      |
| Segredo do Cliente             | AZURE_CLIENT_SECRET   | O segredo de aplicação principal de serviço guardado quando o principal de serviço foi criado.                                      |
| Ponto final do Gestor de recursos | ARM_ENDPOINT           | Ver [ *o ponto de final do Gestor de recursos do Azure Stack*][].                                                                    |

Para localizar o ID de inquilino para o Azure Stack, siga as instruções presentes [aqui](../azure-stack-csp-ref-operations.md). Para definir as variáveis de ambiente, efetue o seguinte:

### <a name="microsoft-windows"></a>Microsoft Windows

Para definir as variáveis de ambiente num prompt de comando do Windows, utilize o seguinte formato:

```shell
Set Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="macos-linux-and-unix-based-systems"></a>sistemas baseados em Unix, Linux e macOS

Em sistemas Unix com base, pode usar o seguinte comando:

```shell
Export Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="the-azure-stack-resource-manager-endpoint"></a>O ponto de final do Gestor de recursos do Azure Stack

O Gestor de recursos do Microsoft Azure é uma estrutura de gestão que permite aos administradores implementar, gerir e monitorizar recursos do Azure. O Azure Resource Manager pode lidar com essas tarefas, como um grupo, em vez de individualmente, numa única operação.

Pode obter as informações de metadados do ponto final do Resource Manager. O ponto final devolve um ficheiro JSON com as informações necessárias para executar o seu código.

Tenha em atenção as seguintes considerações:

- O **ResourceManagerUrl** no Azure Stack Development Kit (ASDK) é: https://management.local.azurestack.external/

- O **ResourceManagerUrl** em sistemas integrados é: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` para obter os metadados necessários: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

Ficheiro JSON de exemplo:

```json
{ 
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Perfis de API existentes

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**: mais recente do perfil criado para o Azure Stack. Utilize este perfil para os serviços para ser mais compatível com o Azure Stack, desde que estiver a utilizar o carimbo de data / 1808 ou mais.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**: Se tiver um carimbo de data / menor do que a compilação de 1808, utilize este perfil.

3.  **Mais recente**: perfil consistindo em versões mais recentes de todos os serviços. Utilize as versões mais recentes de todos os serviços. Este perfil faz parte do **Microsoft.Azure.Management** pacote NuGet.

Para obter mais informações sobre os perfis do Azure Stack e a API, consulte um [Resumo dos perfis de API][].

## <a name="azure-net-sdk-api-profile-usage"></a>Utilização de perfil de API do SDK .NET do Azure

O código a seguir deve ser usado para criar uma instância de um cliente de perfil. Este parâmetro só é necessário para o Azure Stack ou noutras clouds privadas. Global Azure já tem estas definições por predefinição.

O código a seguir é necessário para autenticar o principal de serviço no Azure Stack. Ele cria um token pelo inquilino ID e a base de autenticação, o que é específica para o Azure Stack.

```csharp
public class CustomLoginCredentials : ServiceClientCredentials
{
    private string clientId;
    private string clientSecret;
    private string resourceId;
    private string tenantId;

    private const string authenticationBase = "https://login.windows.net/{0}";

    public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
    {
        clientId = servicePrincipalId;
        clientSecret = servicePrincipalSecret;
        resourceId = azureEnvironmentResourceId;
        tenantId = azureEnvironmentTenandId;
    }
```

Isso permitirá que usar os pacotes de NuGet de perfil de API para implementar a sua aplicação com êxito para o Azure Stack.

## <a name="define-azure-stack-environment-setting-functions"></a>Definir funções de definição de ambiente do Azure Stack

Para autenticar o principal de serviço para o ambiente do Azure Stack, utilize o seguinte código:

```csharp
private string AuthenticationToken { get; set; }
public override void InitializeServiceClient<T>(ServiceClient<T> client)
{
    var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
    var credential = new ClientCredential(clientId, clientSecret);
    var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
    clientCredential: credential).Result;
    if (result == null)
    {
        throw new InvalidOperationException("Failed to obtain the JWT token");
    }
    AuthenticationToken = result.AccessToken;
}
```

Esta ação substitui o cliente do serviço de inicialização para autenticar para o Azure Stack.

## <a name="samples-using-api-profiles"></a>Exemplos de utilização de perfis de API

Pode utilizar os exemplos seguintes encontrados nos repositórios do GitHub, como uma referência para a criação de soluções com perfis .NET e a API do Azure Stack.

-   [Projeto de teste para a Máquina Virtual, vNet, grupos de recursos e conta de armazenamento][]
-   Gerir máquinas virtuais com o .NET

### <a name="sample-unit-test-project"></a>Projeto de teste de unidade de exemplo 

1.  Clone o repositório utilizando o seguinte comando:

    `git clone <https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject>`

2.  Criar um Azure principal de serviço e atribuir uma função aceda à subscrição. Para obter instruções sobre como criar um principal de serviço, consulte [Utilizar o Azure PowerShell para criar um principal de serviço com um certificado][].

3.  Obter os valores necessários seguintes:

    1.  ID do inquilino
    2.  ID de Cliente
    3.  Segredo do Cliente
    4.  ID da subscrição
    5.  Ponto final do Gestor de recursos

4.  Defina as seguintes variáveis de ambiente com as informações obtidas a partir do principal de serviço que criou utilizando a linha de comandos:

    1.  Exportar AZURE_TENANT_ID = {o seu id de inquilino}
    2.  Exportar AZURE_CLIENT_ID = {o seu id de cliente}
    3.  Exportar AZURE_CLIENT_SECRET = {o seu segredo de cliente}
    4.  Exportar AZURE_SUBSCRIPTION_ID = {o seu id de subscrição}
    5.  Exportar ARM_ENDPOINT = {seu URL do Gestor de recursos do Azure Stack}

   No Windows, utilize **definir** em vez de **exportar**.

5.  Certifique-se de que a variável de localização é definida para a localização do Azure Stack. Por exemplo, LOCAL = "local".

6.  Defina as credenciais de início de sessão personalizado que permitirão a autenticação no Azure Stack. Tenha em atenção que esta parte do código está incluído neste exemplo, na pasta de autorização.

   ```csharp
   public class CustomLoginCredentials : ServiceClientCredentials
   {
       private string clientId;
       private string clientSecret;
       private string resourceId;
       private string tenantId;
       private const string authenticationBase = "https://login.windows.net/{0}";
       public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
       {
           clientId = servicePrincipalId;
           clientSecret = servicePrincipalSecret;
           resourceId = azureEnvironmentResourceId;
           tenantId = azureEnvironmentTenandId;
       }
   private string AuthenticationToken { get; set; }
   ```

7.  Adicione o seguinte código se estiver a utilizar o Azure Stack para substituir o cliente do serviço de inicialização para autenticar para o Azure Stack. Tenha em atenção que uma parte do código já está incluída neste exemplo, na pasta de autorização.

   ```csharp
   public override void InitializeServiceClient<T>(ServiceClient<T> client)
   {
      var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
      var credential = new ClientCredential(clientId, clientSecret);
      var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
                clientCredential: credential).Result;
      if (result == null)
      {
          throw new InvalidOperationException("Failed to obtain the JWT token");
      }
      AuthenticationToken = result.AccessToken;
   }
   ```
 
8.  Utilizar o Gestor de pacotes do NuGet, procure "2018-03-01-híbrida" e instalar os pacotes associados este perfil para os fornecedores de recursos de computação, rede, armazenamento, Cofre de chaves e dos serviços de aplicações.

2.  Dentro de cada tarefa no arquivo. CS, defina os parâmetros que são necessárias para trabalhar com o Azure Stack. Um exemplo é mostrado como a seguir para a tarefa `CreateResourceGroupTest`:

   ```csharp
   var location = Environment.GetEnvironmentVariable("AZURE_LOCATION");
   var baseUriString = Environment.GetEnvironmentVariable("AZURE_BASE_URL");
   var resourceGroupName = Environment.GetEnvironmentVariable("AZURE_RESOURCEGROUP");
   var servicePrincipalId = Environment.GetEnvironmentVariable("AZURE_CLIENT_ID");
   var servicePrincipalSecret = Environment.GetEnvironmentVariable("AZURE_CLIENT_SECRET");
   var azureResourceId = Environment.GetEnvironmentVariable("AZURE_RESOURCE_ID");
   var tenantId = Environment.GetEnvironmentVariable("AZURE_TENANT_ID");
   var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
   var credentials = new CustomLoginCredentials(servicePrincipalId, servicePrincipalSecret, azureResourceId, tenantId);
   ```

1.  Clique com o botão direito do rato em cada tarefa e selecione **executar teste**.

    1.  As marcas de verificação verde na janela Painel do lado alertá-lo se cada tarefa foi criada com êxito com base nos parâmetros de dado. Verifique a sua subscrição do Azure Stack para garantir que os recursos foram criados com êxito.

    2.  Para obter mais informações sobre como executar testes de unidade, consulte [Execute testes de unidade com o Gerenciador de testes.][]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os perfis de API, consulte:

- [Gerir perfis de versão de API no Azure Stack](azure-stack-version-profiles.md)
- [Versões de API do fornecedor de recursos suportadas por perfis](azure-stack-profiles-azure-resource-manager-versions.md)

  [Introdução - instalar o Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [Localizar e instalar um pacote]: /nuget/tools/package-manager-ui
  [Instruções do Gestor de pacotes NuGet]: https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager
  [Criar subscrições de ofertas no Azure Stack]: ../azure-stack-subscribe-plan-provision-vm.md
  [Fornecer acesso de aplicações para o Azure Stack]: ../azure-stack-create-service-principals.md
  [* inquilino ID *]: ../azure-stack-identity-overview.md
  [* subscrição ID *]: ../azure-stack-plan-offer-quota-overview.md#subscriptions
  [* o Azure Stack resource manager endpoint *]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [Resumo dos perfis de API]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Projeto de teste para a Máquina Virtual, vNet, grupos de recursos e conta de armazenamento]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Utilizar o Azure PowerShell para criar um principal de serviço com um certificado]: ../azure-stack-create-service-principals.md
  [Execute testes de unidade com o Gerenciador de testes.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
