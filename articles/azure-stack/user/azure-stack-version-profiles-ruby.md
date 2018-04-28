---
title: Através de perfis de versão de API com Ruby na pilha do Azure | Microsoft Docs
description: Saiba mais sobre como utilizar perfis de versão de API com Ruby na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 98d285bf8ec92e7b6baf709d4796ce2f4420c4f2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>Utilize perfis de versão de API com Ruby na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

## <a name="ruby-and-api-version-profiles"></a>Ruby e API perfis de versão

O SDK de Ruby para o Gestor de recursos de pilha do Azure fornece ferramentas para ajudar a criar e gerir a sua infraestrutura. Fornecedores de recursos no SDK incluem computação, redes virtuais e armazenamento com o idioma Ruby. Perfis de API no Ruby SDK ativam desenvolvimento de nuvem híbrida, ajudando-o a alternar entre os recursos do Azure globais e de recursos na pilha do Azure.

Um perfil de API é uma combinação de fornecedores de recursos e as versões do serviço. Pode utilizar um perfil de API para combinar os tipos de recursos diferentes.

 - Para tornar a utilização de versões mais recentes de todos os serviços, utilize o **mais recente** perfil gem de rollup do Azure SDK.
 - Para utilizar os serviços compatíveis com a pilha do Azure, utilize o **V2017_03_09** perfil gem de rollup do Azure SDK.
 - Para utilizar a api-version mais recente de um serviço, utilize o **mais recente** perfil a gem específica. Por exemplo, se gostaria de utilizar a última versão de api do serviço de computação por si só, utilize o **mais recente** perfil o **computação** gem.
 - Para utilizar a versão de api específica para um serviço, utilize as versões de API específicas definidas no interior da gem.

> [!note] 
> Pode combinar todas as opções na mesma aplicação.

## <a name="install-the-azure-ruby-sdk"></a>Instale o Azure SDK Ruby

 - Siga as instruções oficiais de instalação [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
 - Siga as instruções oficiais de instalação [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
    - Ao instalar escolha **Ruby adicionar à variável de caminho**
    - Instale o kit de desenvolvimento durante a instalação Ruby quando lhe for pedido.
    - Em seguida, instale bundler utilizando o seguinte comando:  
      `Gem install bundler`
 - Se não estiver disponível, crie uma subscrição e guarde o ID de subscrição a utilizar mais tarde. Instruções para criar uma subscrição são [aqui](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
 - Criar um principal de serviço e guarde o respetivo ID e o segredo. Instruções para criar um principal de serviço para a pilha do Azure são [aqui](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals). 
 - Certifique-se a que sua principal de serviço tem a função de contribuinte/proprietário na sua subscrição. Instruções sobre como atribuir função principal de serviço são [aqui](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

## <a name="install-the-rubygem-packages"></a>Instalar os pacotes de rubygem

Pode instalar os pacotes de azure rubygem diretamente.

````Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
````

Lembre-se de que o SDK do Azure Resource Manager Ruby está em pré-visualização e terão, provavelmente, interrompendo as alterações de interface em versões futuras. Um aumento do número na versão secundária pode indicar a alterações.

## <a name="usage-of-the-azuresdk-gem"></a>Utilização da gem azure_sdk

Gem, azure_sdk, é um rollup de todos os gems suportados no Ruby SDK. Este gem é composta por um **mais recente** perfil, que suporta a versão mais recente de todos os serviços. Introduz um perfil com a versão **V2017_03_09** perfil, o que é criada para a pilha do Azure.

Pode instalar a gem de rollup azure_sdk com o seguinte comando:  

````Ruby  
  gem install 'azure_sdk
````

## <a name="prerequisite"></a>Pré-requisito

Para utilizar o SDK do Azure Ruby com pilha do Azure, tem de indicar os valores seguintes e, em seguida, definir valores de variáveis de ambiente. Consulte as instruções depois da tabela para o seu sistema operativo em definir as variáveis de ambientais. 

| Valor | Variáveis de ambiente | Descrição | 
| --- | --- | --- | --- |
| ID do inquilino | AZURE_TENANT_ID | O valor da pilha do Azure [ID de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview). |
| ID do Cliente | AZURE_CLIENT_ID | O serviço de ID da aplicação principal guardado quando principal de serviço foi criado na secção anterior deste documento.  |
| ID da subscrição | AZURE_SUBSCRIPTION_ID | O [ID de subscrição](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) é como aceder aos ofertas na pilha do Azure. |
| Segredo do Cliente | AZURE_CLIENT_SECRET | A aplicação principal de serviço segredo guardados quando principal de serviço foi criado. |
| Ponto final do Gestor de recursos | ARM_ENDPOINT | Consulte [a pilha do Azure resource manager endpoin](#The-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>O ponto final da pilha do Azure resource manager

O Microsoft Azure Resource Manager é uma arquitetura de gestão que permite aos administradores implementar, gerir e monitorizar os recursos do Azure. O Azure Resource Manager pode processar estas tarefas, como um grupo, em vez de individualmente, numa única operação.

Pode obter as informações de metadados do ponto final do Gestor de recursos. O ponto final devolve um ficheiro JSON com as informações necessárias para executar o código.
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

### <a name="set-environmental-variables"></a>Variáveis de ambientais de conjunto

**Microsoft Windows**  
Para definir as variáveis de ambiente, na linha de comandos do Windows, utilize o seguinte formato:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**macOS, Linux e os sistemas baseados em Unix**  
Nos sistemas Unix com base, pode utilizar o comando, tal como:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Perfis de API existentes

Gem de rollup azure_sdk tem os perfis de dois seguintes:

1. **V2017_03_09**  
  Perfil criada para a pilha do Azure. Utilize este perfil para os serviços para ser mais compatível com a pilha do Azure.
2. **Latest**  
  Perfil é constituído por versões mais recentes de todos os serviços. Utilize as versões mais recentes de todos os serviços.

Para obter mais informações sobre perfis de pilha do Azure e a API, consulte um [perfis de resumo de API](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Utilização de perfil Ruby API do SDK do Azure

As seguintes linhas devem ser utilizadas para instanciar um cliente de perfil. Este parâmetro só é necessário para a pilha do Azure ou de outras nuvens privadas. Global Azure já tem estas definições por predefinição.

````Ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
    ENV['AZURE_TENANT_ID'],
    ENV['AZURE_CLIENT_ID'],
    ENV['AZURE_CLIENT_SECRET'],
    active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack
client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
````

O cliente do perfil pode ser utilizado para aceder aos fornecedores de recursos individuais, tais como a computação, armazenamento e rede.

````Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2017_03_09::Compute::Mgmt::Models::PurchasePlan.new
````

## <a name="define-azurestack-environment-setting-functions"></a>Definir funções de definição de ambiente de AzureStack

Para autenticar o principal de serviço para o ambiente de pilha do Azure, definir os pontos finais utilizando **get_active_directory_settings()**. Este método utiliza o **ARM_Endpoint** variável de ambiente que definiu quando estabelecer as variáveis de ambientais.

````Ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
    settings = MsRestAzure::ActiveDirectoryServiceSettings.new
    response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
    status_code = response.code
    response_content = response.body
    unless status_code == "200"
        error_model = JSON.load(response_content)
        fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
    end
    result = JSON.load(response_content)
    settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
    settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
    settings
end
````

## <a name="samples-using-api-profiles"></a>Amostras através de perfis de API

Pode utilizar os exemplos seguintes encontrado no GitHub repositoreis como uma referência de criação de soluções com perfis de Ruby e a API de pilha do Azure:

 - [Gerir recursos do Azure e grupos de recursos com Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)
 - [Gerir máquinas virtuais utilizando Ruby](https://github.com/Azure-Samples/compute-ruby-manage-vm/tree/master/Hybrid)
 - [Implementar um SSH ativada VM com um modelo no Ruby](https://github.com/Azure-Samples/resource-manager-ruby-template-deployment/tree/master/Hybrid)

### <a name="sample-resource-manager-and-groups"></a>Gestor de recursos de exemplo e grupos

Para executar o exemplo, certifique-se de que tem instalado Ruby. Se estiver a utilizar o Visual Studio Code, transferi o SDK de Ruby como uma extensão bem. 

> [!note]  
> Pode obter o repositório para o exemplo em "[recursos de gerir o Azure e os grupos de recursos com Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)".

1. Clone o repositório.

    ````Bash
    git clone https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups.git
    ````

2. Instale as dependências de pacote a utilizar.

    ````Bash
    cd resource-manager-ruby-resources-and-groups\Hybrid\
    bundle install
    ````

3. Criar um Principal de serviço do Azure com o PowerShell e obter os valores necessários. 

  Para obter instruções sobre como criar um principal de serviço, consulte [utilize o Azure PowerShell para criar um principal de serviço com um certificado](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

  Os valores necessários são:
  - ID do inquilino
  - ID do Cliente
  - Segredo do Cliente
  - ID da subscrição
  - Ponto final do Gestor de recursos

  Defina as seguintes variáveis de ambiente utilizando as informações obtidas a partir do Principal de serviço que criou.

  - Exportar AZURE_TENANT_ID = {o id de inquilino}
  - Exportar AZURE_CLIENT_ID = {seu id de cliente}
  - Exportar AZURE_CLIENT_SECRET = {seu segredo do cliente}
  - Exportar AZURE_SUBSCRIPTION_ID = {o id de subscrição}
  - Exportar ARM_ENDPOINT = {o url do Gestor de recursos de AzureStack}

  > [!note]  
  > No Windows, utilize o conjunto em vez de exportar.

4. Certifique-se de que a variável de localização é definida para a localização de AzureStack. Por exemplo LOCAL = "local"

5. Adicione a seguinte linha de código, se estiver a utilizar a pilha do Azure ou de outras nuvens privadas para os pontos finais do direito Active Directory de destino.

  ````Ruby  
  active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
  ````

6. Dentro da variável de opções, adicione as definições do Active Directory e o URL de base para trabalhar com a pilha do Azure. 

  ````Ruby  
  options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
  }
  ````

7. Crie perfil de cliente que tenha como destino o perfil de pilha do Azure:

  ````Ruby  
    client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
  ````

8. Para autenticar o principal de serviço com a pilha do Azure, os pontos finais deverá ser definidos com **get_active_directory_settings()**. Este método utiliza o **ARM_Endpoint** variável de ambiente que definiu quando estabelecer as variáveis de ambientais.

  ````Ruby  
  def get_active_directory_settings(armEndpoint)
    settings = MsRestAzure::ActiveDirectoryServiceSettings.new
    response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
    status_code = response.code
    response_content = response.body
    unless status_code == "200"
      error_model = JSON.load(response_content)
      fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
    end
    result = JSON.load(response_content)
    settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
    settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
    settings
  end
  ````

9. Execute o exemplo.

  ````Ruby
    bundle exec ruby example.rb
  ````

## 

## <a name="next-steps"></a>Passos Seguintes

* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)  
