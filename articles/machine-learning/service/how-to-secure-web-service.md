---
title: Proteger serviços da web do Azure Machine Learning com SSL
description: Saiba como proteger um serviço web implementado com o serviço Azure Machine Learning. Pode restringir o acesso a serviços da web e proteger os dados submetidos por clientes utilizando as camadas de soquete seguro (SSL) e a autenticação baseada em chave.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.openlocfilehash: 8a26491acc7215598e57ce6074fffe26a4374a96
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48251086"
---
# <a name="secure-azure-machine-learning-web-services-with-ssl"></a>Proteger serviços da web do Azure Machine Learning com SSL

Neste artigo, ficará a saber como proteger um serviço web implementado com o serviço Azure Machine Learning. Pode restringir o acesso a serviços da web e proteger os dados submetidos por clientes utilizando as camadas de soquete seguro (SSL) e a autenticação baseada em chave.

> [!Warning]
> Se não ativar SSL, qualquer utilizador na internet será capaz de fazer chamadas para o serviço web.

O SSL criptografa dados enviados entre o cliente e o serviço web. Ele é também utilizado pelo cliente para verificar a identidade do servidor. Autenticação apenas está ativada para os serviços que forneceram um certificado SSL e a chave.  Se ativar o SSL, é necessária uma chave de autenticação quando aceder ao serviço web.

Se implementar um serviço web habilitado com SSL ou ativar SSL para o serviço web implementado existente, os passos são os mesmos:

1. Obtenha um nome de domínio.

2. Obtenha um certificado SSL.

3. Implementar ou atualizar o serviço web com a definição de SSL ativada.

4. Atualize o DNS para apontar para o serviço web.

Existem pequenas diferenças quando protegendo serviços da web entre as [destinos de implementação](how-to-deploy-and-where.md). 

## <a name="get-a-domain-name"></a>Obter um nome de domínio

Se já não possui um nome de domínio, pode comprar um de uma __entidade de registo de nome de domínio__. O processo é diferente entre entidades de registo, como faz o custo. A entidade de registo também fornece ferramentas para gerenciar o nome de domínio. Estas ferramentas são utilizadas para mapear um nome de domínio completamente qualificado (como www.contoso.com) para o endereço IP que aloja o serviço da web.

## <a name="get-an-ssl-certificate"></a>Obter um certificado SSL

Existem várias formas de obter um certificado SSL. A mais comum é comprar um de uma __autoridade de certificação__ (AC). Independentemente de onde obter o certificado, terá dos seguintes ficheiros:

* R __certificado__. O certificado tem de conter a cadeia de certificados completa e tem de ser PEM codificado.
* R __chave__. A chave tem de ser PEM codificado.

Quando pedir um certificado, tem de fornecer o nome de domínio completamente qualificado (FQDN) do endereço que pretende utilizar para o serviço web. Por exemplo, www.contoso.com. O endereço marcados para o certificado e o endereço utilizado pelos clientes são comparados ao validar a identidade do serviço web. Se os endereços não corresponderem, os clientes irão receber um erro. 

> [!TIP]
> Se a autoridade de certificação não é possível fornecer o certificado e chave, como ficheiros PEM codificado, pode utilizar um utilitário como [OpenSSL](https://www.openssl.org/) para alterar o formato.

> [!IMPORTANT]
> Certificados autoassinados devem ser usados apenas para desenvolvimento. Eles não devem ser usados na produção. Se utilizar um certificado autoassinado, consulte a [consumindo serviços da web com certificados autoassinados](#self-signed) secção para obter instruções específicas.


## <a name="enable-ssl-and-deploy"></a>Ativar o SSL e implementar

Para implementar (ou voltar a implementar) o serviço com SSL ativado, defina o `ssl_enabled` parâmetro `True`, sempre que aplicável. Definir o `ssl_certificate` parâmetro para o valor da __certificado__ ficheiro e o `ssl_key` para o valor da __chave__ ficheiro. 

+ **Implementar no serviço Kubernetes do Azure (AKS)**
  
  Durante o aprovisionamento do cluster do AKS, fornece valores para parâmetros relacionados com SSL, conforme mostrado no fragmento de código:

    ```python
    from azureml.core.compute import AksCompute
    
    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementar no Azure Container Instances (ACI)**
 
  Durante a implementação no ACI, fornece valores para parâmetros relacionados com SSL, conforme mostrado no fragmento de código:

    ```python
    from azureml.core.webservice import AciWebservice
    
    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementar nas matrizes de gama programável de campo (FPGAs)**

  A resposta do `create_service` operação contém o endereço IP do serviço. O endereço IP é utilizado quando for mapear o nome DNS para o endereço IP do serviço. A resposta também contém um __chave primária__ e __chave secundária__ que são utilizados para consumir o serviço. Fornece valores para parâmetros relacionados com SSL, conforme mostrado no fragmento de código:

    ```python
    from amlrealtimeai import DeploymentClient
    
    subscription_id = "<Your Azure Subscription ID>"
    resource_group = "<Your Azure Resource Group Name>"
    model_management_account = "<Your AzureML Model Management Account Name>"
    location = "eastus2"
    
    model_name = "resnet50-model"
    service_name = "quickstart-service"
    
    deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)
    
    with open('cert.pem','r') as cert_file:
        with open('key.pem','r') as key_file:
            cert = cert_file.read()
            key = key_file.read()
            service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
    ```

## <a name="update-your-dns"></a>Atualizar o seu DNS

Em seguida, tem de atualizar o DNS para apontar para o serviço web.

+ **Para o ACI e FPGA**:  

  Utilize as ferramentas fornecidas pelo sua entidade de registo de nome de domínio para atualizar o registo DNS para o seu nome de domínio. O registo tem de apontar para o endereço IP do serviço.  

  Consoante a entidade de registo e a hora para live (TTL) configurada para o nome de domínio, pode demorar vários minutos a várias horas para que os clientes podem resolver o nome de domínio.

+ **Para o AKS**: 

  Atualize o DNS no separador "Configuração" das "Endereço IP público" do cluster AKS conforme mostrado na imagem. Pode encontrar o endereço IP público como um dos tipos de recursos criados no grupo de recursos que contém os nós de agente do AKS e outros recursos de rede.

  [ ![O serviço do Azure Machine Learning: protegendo serviços da web com SSL](./media/how-to-secure-web-service/aks-public-ip-address.png) ] ((.media/how-to-secure-web-service/aks-public-ip-address.png#lightbox)

## <a name="consume-authenticated-services"></a>Consumir serviços autenticados

### <a name="how-to-consume"></a>Como consumir 
+ **Para o ACI e o AKS**: 

  Para serviços da web ACI e o AKS, saiba como consumir os serviços da web nestes artigos:
  + [Como implementar no ACI](how-to-deploy-to-aci.md)

  + [Como implementar no AKS](how-to-deploy-to-aks.md)

+ **Para o ACI e FPGA**:  

  Os exemplos seguintes demonstram como consumir um serviço FPGA autenticado em Python e c#.
  Substitua `authkey` com a chave primária ou secundária que foi retornada quando o serviço foi implementado.

  Exemplo de Python:
    ```python
    from amlrealtimeai import PredictionClient
    client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
    image_file = R'C:\path_to_file\image.jpg'
    results = client.score_image(image_file)
    ```

  Exemplo do c#:
    ```csharp
    var client = new ScoringClient(host, 50051, useSSL, "authKey");
    float[,] result;
    using (var content = File.OpenRead(image))
        {
            IScoringRequest request = new ImageRequest(content);
            result = client.Score<float[,]>(request);
        }
    ```

### <a name="set-the-authorization-header"></a>Definir o cabeçalho de autorização
Outros clientes gRPC podem autenticar pedidos ao definir um cabeçalho de autorização. A abordagem geral é criar um `ChannelCredentials` objeto que combina `SslCredentials` com `CallCredentials`. Isso é adicionado ao cabeçalho de autorização do pedido. Para obter mais informações sobre como implementar o suporte para os cabeçalhos de específicos, consulte [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

Os exemplos seguintes demonstram como definir o cabeçalho em c# e Go:

+ Utilizar c# para definir o cabeçalho:
    ```csharp
    creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                          async (context, metadata) =>
                          {
                              metadata.Add(new Metadata.Entry("authorization", "authKey"));
                              await Task.CompletedTask;
                          }));
    
    ```

+ Utilize o Go para definir o cabeçalho:
    ```go
    conn, err := grpc.Dial(serverAddr, 
        grpc.WithTransportCredentials(credentials.NewClientTLSFromCert(nil, "")),
        grpc.WithPerRPCCredentials(&authCreds{
        Key: "authKey"}))
    
    type authCreds struct {
        Key string
    }
    
    func (c *authCreds) GetRequestMetadata(context.Context, uri ...string) (map[string]string, error) {
        return map[string]string{
            "authorization": c.Key,
        }, nil
    }
    
    func (c *authCreds) RequireTransportSecurity() bool {
        return true
    }
    ```

<a id="self-signed"></a>

## <a name="consume-services-with-self-signed-certificates"></a>Consumir serviços com certificados autoassinados

Existem duas formas de ativar o cliente para autenticação num servidor protegido por um certificado autoassinado:

* No sistema cliente, defina o `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` variável de ambiente no sistema de cliente para apontar para o ficheiro de certificado.

* Ao construir um `SslCredentials` de objeto, transmita os conteúdos do ficheiro de certificado para o construtor.

Utilizando um dos métodos faz com que o gRPC utilizar o certificado como o certificado de raiz.

> [!IMPORTANT]
> gRPC não aceita certificados não fidedignos. Utilizando um certificado não fidedigno irá falhar com um `Unavailable` código de estado. Os detalhes da falha contêm `Connection Failed`.
