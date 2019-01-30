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
ms.date: 12/07/2018
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 12/07/2018
ms.openlocfilehash: e335f4cb112c0f029768ccf050f888e00a6efdc1
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250627"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>Utilizar perfis de versão de API com .NET no Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O SDK do .NET para o Azure Stack Resource Manager fornece ferramentas para ajudar a criar e gerir a sua infraestrutura. Fornecedores de recursos no SDK incluem computação, rede, armazenamento, serviços de aplicações, e [KeyVault](../../key-vault/key-vault-whatis.md). O SDK de .NET inclui 14 pacotes de NuGet. Esses pacotes deverão ser transferidos para a sua solução de projeto sempre que incorpora as informações de perfil. No entanto, pode baixar especificamente qual o fornecedor de recursos que irá utilizar para o 2018-03-01-híbrido ou 2017-03-09-perfil para otimizar a memória para a sua aplicação. Cada pacote é composta por um fornecedor de recursos, a respetiva versão de API e o perfil de API à qual ele pertence. Perfis de API do SDK do .NET permitem o desenvolvimento de cloud híbrida com a ajuda para alternar entre recursos do Azure global e recursos no Azure Stack.

## <a name="net-and-api-version-profiles"></a>Perfis de versão do .NET e de API

Um perfil de API é uma combinação de fornecedores de recursos e as versões de API. Pode utilizar um perfil de API para obter a versão mais recente, mais estável de cada tipo de recurso num pacote de fornecedor de recursos.

-   Para usar as versões mais recentes de todos os serviços, utilize o **mais recente** perfil dos pacotes. Este perfil faz parte do **Microsoft.Azure.Management** pacote NuGet.

-   Para utilizar os serviços compatíveis com o Azure Stack, utilize o **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg** ou **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg** pacotes.

    -   Existem dois pacotes para cada fornecedor de recursos para cada perfil.

    -   Certifique-se de que o **ResourceProvider** parte do pacote NuGet acima é alterado para o fornecedor correto.

-   Para utilizar a API-version mais recente de um serviço, utilize o **mais recente** perfil do pacote NuGet específico. Por exemplo, se pretender utilizar o **API mais recente** versão o serviço de computação autónomo, utilize o **mais recente** perfil dos **computação** pacote. O **mais recente** perfil faz parte do **Microsoft.Azure.Management** pacote NuGet.

-   Para utilizar versões de API específicas para um tipo de recurso de um fornecedor de recursos específico, utilize as versões de API específicas definidas dentro do pacote.

Pode combinar todas as opções no mesmo aplicativo.

## <a name="install-the-azure-net-sdk"></a>Instalar o SDK .NET do Azure

1.  Instale o Git. Para obter instruções, consulte [introdução - instalar o Git][].

2.  Para instalar os pacotes de NuGet corretos, consulte [Localizar e instalar um pacote][].

3.  Os pacotes que devem ser instaladas depende da versão de perfil que pretende utilizar. Os nomes de pacote para as versões de perfil são:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**

4.  Para instalar os pacotes de NuGet corretos para o Visual Studio Code, veja a seguinte hiperligação para transferir os [Instruções do Gestor de pacotes NuGet][].

5.  Se não está disponível, criar uma subscrição e guarde o ID de subscrição a ser utilizado mais tarde. Para obter instruções criar uma subscrição, veja [criar subscrições de ofertas no Azure Stack][].

6.  Criar um principal de serviço e guarde o ID de cliente e o segredo do cliente. Para obter instruções sobre como criar um principal de serviço para o Azure Stack, veja [fornecer acesso de aplicações para o Azure Stack][]. O ID de cliente é também conhecido como o ID da aplicação ao criar um serviço principal.

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
| Localização                  | RESOURCE_LOCATION     | Localização para o Azure Stack.

Para localizar o ID de inquilino para o Azure Stack, siga as instruções presentes [aqui](../azure-stack-csp-ref-operations.md). Para definir as variáveis de ambiente, siga os passos abaixo:

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

- O **ResourceManagerUrl** em sistemas integrados é: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` Para obter os metadados necessários: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

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

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg**: Perfil mais recentes, criado para o Azure Stack. Utilize este perfil para os serviços para ser mais compatível com o Azure Stack, desde que estiver a utilizar o carimbo de data / 1808 ou mais.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**: Se tiver um carimbo de data / menor do que a compilação de 1808, utilize este perfil.

3.  **Mais recente**: Perfil consistindo em versões mais recentes de todos os serviços. Utilize as versões mais recentes de todos os serviços. Este perfil faz parte do **Microsoft.Azure.Management** pacote NuGet.

Para obter mais informações sobre os perfis do Azure Stack e a API, consulte um [Resumo dos perfis de API][].

## <a name="azure-net-sdk-api-profile-usage"></a>Utilização de perfil de API do SDK .NET do Azure

O código a seguir deve ser usado para criar uma instância de um cliente de gestão de recursos. Um código semelhante pode ser utilizado para criar uma instância de outro fornecedor de recursos (por exemplo, computação, rede e armazenamento) os clientes. 

```csharp
var client = new ResourceManagementClient(armEndpoint, credentials)
{
    SubscriptionId = subscriptionId
};
```

O `credentials` parâmetro no código acima é necessário para criar uma instância de um cliente. O código a seguir gera um token de autenticação, o ID de inquilino e o principal de serviço.

```csharp
var azureStackSettings = getActiveDirectoryServiceSettings(armEndpoint);
var credentials = ApplicationTokenProvider.LoginSilentAsync(tenantId, servicePrincipalId, servicePrincipalSecret, azureStackSettings).GetAwaiter().GetResult();
```
O `getActiveDirectoryServiceSettings` chamada no código obtém pontos finais de pilha do Azure, do ponto de extremidade de metadados. Ele declara as variáveis de ambiente da chamada que é efetuada: 

```csharp
public static ActiveDirectoryServiceSettings getActiveDirectoryServiceSettings(string armEndpoint)
{
    var settings = new ActiveDirectoryServiceSettings();
    try
    {
        var request = (HttpWebRequest)HttpWebRequest.Create(string.Format("{0}/metadata/endpoints?api-version=1.0", armEndpoint));
        request.Method = "GET";
        request.UserAgent = ComponentName;
        request.Accept = "application/xml";
        using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
        {
            using (StreamReader sr = new StreamReader(response.GetResponseStream()))
            {
                var rawResponse = sr.ReadToEnd();
                var deserialized = JObject.Parse(rawResponse);
                var authenticationObj = deserialized.GetValue("authentication").Value<JObject>();
                var loginEndpoint = authenticationObj.GetValue("loginEndpoint").Value<string>();
                var audiencesObj = authenticationObj.GetValue("audiences").Value<JArray>();
                settings.AuthenticationEndpoint = new Uri(loginEndpoint);
                settings.TokenAudience = new Uri(audiencesObj[0].Value<string>());
                settings.ValidateAuthority = loginEndpoint.TrimEnd('/').EndsWith("/adfs", StringComparison.OrdinalIgnoreCase) ? false : true;
            }
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine(String.Format("Could not get AD service settings. Exception: {0}", ex.Message));
    }
    return settings;
}
```
Isso permitirá que usar os pacotes de NuGet de perfil de API para implementar a sua aplicação com êxito para o Azure Stack.

## <a name="samples-using-api-profiles"></a>Exemplos de utilização de perfis de API

Os exemplos seguintes podem ser utilizados como referência para a criação de soluções com perfis .NET e a API do Azure Stack.
- [Gerir grupos de recursos](https://github.com/Azure-Samples/hybrid-resources-dotnet-manage-resource-group)
- [Gerir contas de armazenamento](https://github.com/Azure-Samples/hybird-storage-dotnet-manage-storage-accounts)
- [Gerir uma Máquina Virtual](https://github.com/Azure-Samples/hybrid-compute-dotnet-manage-vm)

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
  [* o Azure Stack Resource Manager endpoint *]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [Resumo dos perfis de API]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Test Project to Virtual Machine, vNet, resource groups, and storage account]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Use Azure PowerShell to create a service principal with a certificate]: ../azure-stack-create-service-principals.md
  [Run unit tests with Test Explorer.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
