---
title: Através de perfis de versão de API com ACEDA na pilha do Azure | Microsoft Docs
description: Saiba mais sobre como utilizar perfis de versão de API com ACEDA na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 5b881c17b6ad1c9a7e46492f8549f563cfd6d796
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>Utilize perfis de versão de API com aceda na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

## <a name="go-and-version-profiles"></a>Perfis aceda e versão

Um perfil é uma combinação de tipos de recursos diferente com diferentes versões de diferentes serviços. Utilizar um perfil irá ajudá-lo combinar e misturar entre tipos de recursos diferente. Podem fornecer perfis de:

 - Estabilidade da sua aplicação, bloqueando a versões de API específicas.
 - Compatibilidade para a sua aplicação com a pilha do Azure e regionais centros de dados do Azure.

SDK aceda, estão disponíveis nos perfis de perfis / caminho, com a respetiva versão no **aaaa-MM-DD** formato. Agora, o mais recente pilha do Azure é a versão de perfil **2017-03-09**. Para importar um determinado serviço a partir de um perfil, terá de importar o módulo correspondente do perfil. Por exemplo, para importar **computação** serviço de **2017-03-09** perfil:

````go
import "github.com/Azure/azure-sdk-for-go/profi1es/2e17-e3-eg/compute/mgmt/compute" 
````

## <a name="install-azure-sdk-for-go"></a>Instalar o SDK do Azure para ir

  1. Instale o Git. Para obter instruções, consulte [introdução - instalar o Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  2. Instalar o [aceda a linguagem de programação](https://golang.org/dl).  
  Perfis de API do Azure irão necessitar aceda versão 1.9 ou mais recente.
  3. Instale o SDK do Azure aceda e as respetivas dependências, executando o seguinte comando bash:
  ```
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ```

### <a name="the-go-sdk"></a>O SDK ACEDO

Pode encontrar mais informações sobre o SDK do Azure ACEDA ao:
- O Azure aceda SDK em [instalar o Azure SDK para ir](https://docs.microsoft.com/go/azure/azure-sdk-go-install).
- O SDK do Azure aceda está publicamente disponível no GitHub em [azure sdk para ir](https://github.com/Azure/azure-sdk-for-go).

### <a name="go-autorest-dependencies"></a>Aceda AutoRest dependências

O SDK ACEDA depende os módulos do Azure aceda-AutoRest para enviar pedidos REST para pontos finais do Azure Resource Manager. Terá de importar as dependências de módulo do Azure aceda-AutoRest de [do Azure aceda-AutoRest no GitHub](https://github.com/Azure/go-autorest). Pode encontrar os comandos de bash de instalação no **instalar** secção.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Como utilizar perfis de SDK ACEDA na pilha do Azure

Para executar um exemplo de código aceda na pilha do Azure:
  1. Instale o Azure SDK para aceda e as respetivas dependências. Para instruções, consulte a secção anterior, [instalar o Azure SDK para ir](#install-azure-sdk-for-go).
  2. Obter as informações de metadados do ponto final do Gestor de recursos. O ponto final devolve um ficheiro JSON com as informações necessárias para executar o código de ir.
  > [!note]  
  > O **ResourceManagerUrl** no Azure pilha Development Kit (ASDK) é: `https://management.local.azurestack.external/`  
  > O **ResourceManagerUrl** nos sistemas integrados é: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Para obter os metadados necessários: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  Ficheiro JSON de exemplo:

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

  3. Se não estiver disponível, crie uma subscrição e guarde o ID de subscrição a utilizar mais tarde. Para obter informações sobre como criar uma subscrição, consulte [criar subscrições de ofertas na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
  4. Criar um principal de serviço com o âmbito de "Subscrição" e **proprietário** função. Guarde os principais de serviço ID e o segredo. Para obter informações sobre como criar um principal de serviço para a pilha do Azure, consulte [criar serviço principal](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-azure-ad). O ambiente de pilha do Azure está definido.
  5. Importe um módulo do serviço de perfil aceda SDK no seu código. A versão atual do perfil de pilha do Azure é **2017-03-09**. Por exemplo, para importar o módulo de rede de **2017-03-09** tipo de perfil: 

  ````go
    package main 
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
  ````
  
  6. Na sua função, criar e autenticar-se um cliente com um **novo** chamada de função de cliente. Para criar um cliente de rede virtual, pode utilizar o seguinte código:  

  ````go
  package main 

  import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 

  func main() { 
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
  ````

  Definir `<baseURI>` para o **ResourceManagerUrl** utilizado no passo dois do valor.
  Definir `<subscriptionID>` para o **SubscriptionID** valor guardado do passo três.
  Para criar token, consulte autenticação secção abaixo.  

  7. Invocar métodos API utilizando o cliente que criou no passo anterior. Por exemplo, para criar uma rede virtual através do nosso cliente a partir do passo anterior: 
  
````go
package main

import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 
func main() { 
  vnetC1ient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
  vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

  vnetClient .CreateOrUpdate( ) 
````
  
  Para um exemplo completo de como criar uma rede virtual na pilha do Azure utilizando o perfil de aceda SDK, consulte [exemplo](#example).

## <a name="authentication"></a>Autenticação

Para obter a propriedade Authorizer do Azure Active Directory utilizando o SDK aceda, instale os módulos aceda AutoRest. Estes módulos devem ter sido previamente instalados com a instalação de "Aceda SDK"; Se não, instale o [pacote de autenticação no GitHub](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

O Authorizer tem de ser definido como authorizer para o cliente do recurso. Existem vários métodos para obter um Authorizer; para uma lista completa, consulte aqui.

Esta secção apresenta uma forma comum para obter os tokens de authorizer na pilha do Azure, utilizando as credenciais do cliente:

  1. Se estiver disponível um principal de serviço com a função de proprietário da subscrição, ignore este passo. Caso contrário, crie um principal de serviço [instruções]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) e atribua-lhe uma função de "proprietário" confinada à sua subscrição [instruções]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal). Guarde o ID de aplicação principal de serviço e o segredo. 

  2. Importar **adal** pacote a partir da aceda AutoRest no seu código. 
  
  ````go
  package main
  import "github.com/Azure/go-autorest/autorest/adal" 
  ````

  3. Criar um oauthConfig utilizando o método NewOAuthConfig **adal** módulo. 
  
  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/ada1" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
  ````
   
  Definir `<activeDirectoryEndpoint>` para o valor de "loginEndpoint" propriedade dos metadados ResourceManagerUrl obtido na secção anterior deste documento.
  Definir `<tenantID>` valor para o seu ID de inquilino de pilha do Azure. 

  4. Por fim, crie um token de principal de serviço utilizando o método de NewServicePrincipalToken do módulo adal. 

  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/adal" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  ````
  
  Definir `<activeDirectoryResourceID>` para um dos valores existentes na "audience" lista dos metadados ResourceManagerUrl obtido na secção anterior deste documento.  
  Definir `<clientID>` para a aplicação principal de serviço ID guardados quando principal de serviço foi criado na secção anterior deste documento.  
  Definir `<clientSecret>` para a aplicação principal de serviço segredo guardados quando principal de serviço foi criado na secção anterior deste documento.  

## <a name="example"></a>Exemplo

Esta secção mostra um exemplo de código de ir para criar rede virtual na pilha do Azure. Para exemplos completos do SDK aceda consulte [repositório de exemplos do Azure aceda SDk](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Exemplos de pilha do Azure estão disponíveis em híbrida / caminho dentro de pastas de serviço do repositório.
> [!note]  
> Para executar o código neste exemplo, certifique-se de que a subscrição utilizada tem **rede** fornecedor de recursos listado como **registada**. A verificar, procure a subscrição no portal do Azure pilha e clique em **fornecedores de recursos.**

1. Importe pacotes necessários no seu código. Deve utilizar o perfil disponível mais recente na pilha do Azure para importar o módulo de rede. 
  
  ````go
  package main

  import (
      "context"
      "fmt"
      "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
      "github.com/Azure/go-autorest/autorest"
      "github.com/Azure/go-autorest/autorest/adal"
      "github.com/Azure/go-autorest/autorest/to"
  )
  ````

2. Defina as variáveis de ambiente. Tenha em atenção que para criar uma rede virtual tem de ter um grupo de recursos. 

  ````go
  var (
      activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
      tenantID = "yourAzureStackTenantID"
      clientID = "yourServicePrincipalApplicationID"
      clientSecret = "yourServicePrincipalSecret"
      activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
      subscriptionID = "yourSubscriptionID"
      baseURI = "yourResourceManagerURL"
      resourceGroupName = "existingResourceGroupName"
  )
  ````

3. Agora que definiu as variáveis de ambiente, adicione um método para criar o token de autenticação utilizando **adal** pacote. Ver detalhes sobre a autenticação na secção anterior.
  
  ````go
  //CreateToken creates a service principal token
  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  }
  ````

4. Adicione o método principal. O método principal primeiro obtém um token utilizando o método que é definido no passo anterior. Em seguida, cria um cliente utilizando o módulo de rede do perfil. Por fim, cria uma rede virtual. 
  
````go
package main

import (
    "context"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
    "github.com/Azure/go-autorest/autorest"
    "github.com/Azure/go-autorest/autorest/adal"
    "github.com/Azure/go-autorest/autorest/to"
)

var (
    activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
    tenantID = "yourAzureStackTenantID"
    clientID = "yourServicePrincipalApplicationID"
    clientSecret = "yourServicePrincipalSecret"
    activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
    subscriptionID = "yourSubscriptionID"
    baseURI = "yourResourceManagerURL"
    resourceGroupName = "existingResourceGroupName"
)

//CreateToken creates a service principal token
func CreateToken() (adal.OAuthTokenProvider, error) {
    var token adal.OAuthTokenProvider
    oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        clientID,
        clientSecret,
        activeDirectoryResourceID)
    return token, err
}

func main() {
    token, _ := CreateToken()
    vnetClient := network.NewVirtualNetworksClientWithBaseURI(baseURI, subscriptionID)
    vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
    future, _ := vnetClient.CreateOrUpdate(
        context.Background(),
        resourceGroupName,
        "sampleVnetName",
        network.VirtualNetwork{
            Location: to.StringPtr("local"),
            VirtualNetworkPropertiesFormat: &network.VirtualNetworkPropertiesFormat{
                AddressSpace: &network.AddressSpace{
                    AddressPrefixes: &[]string{"10.0.0.0/8"},
                },
                Subnets: &[]network.Subnet{
                    {
                        Name: to.StringPtr("subnetName"),
                        SubnetPropertiesFormat: &network.SubnetPropertiesFormat{
                            AddressPrefix: to.StringPtr("10.0.0.0/16"),
                        },
                    },
                },
            },
        })
    err := future.WaitForCompletion(context.Background(), vnetClient.Client)
    if err != nil {
        fmt.Printf(err.Error())
        return
    }
}

````

## <a name="next-steps"></a>Passos Seguintes
* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)  
