---
title: Como implementar um modelo como um serviço web num FPGA com o Azure Machine Learning
description: Saiba como implementar um serviço web com um modelo em execução num FPGA com o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: f3237980a1ad1969b5cf8d42d547ddf96608dd97
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Implementar um modelo como um serviço web num FPGA com o Azure Machine Learning

Neste documento, irá aprender a configurar o ambiente de estação de trabalho e implementar um modelo como um serviço web em [campo matrizes de porta programável (FPGA)](concept-accelerate-with-fpgas.md). O serviço web utiliza Brainwave de projeto para executar o modelo no FPGA.

Se utilizar FPGAs fornece inferencing ultra-baixa latência, mesmo com um tamanho de lote única.

## <a name="create-an-azure-machine-learning-model-management-account"></a>Criar uma conta de gestão de modelo do Azure Machine Learning

1. Avance para a página de criação de conta de gestão de modelo de [Portal do Azure](https://aka.ms/aml-create-mma).

2. No portal, crie uma conta de gestão de modelo no **EUA Leste 2** região.

   ![Imagem do ecrã criar conta de gestão de modelo](media/how-to-deploy-fpga-web-service/azure-portal-create-mma.PNG)

3. Dê um nome a sua conta de gestão de modelo, escolha uma subscrição e escolha um grupo de recursos.

   >[!IMPORTANT]
   >Para a localização, tem de escolher **EUA Leste 2** como a região.  Não existem outras regiões estão atualmente disponíveis.

4. Escolha um escalão de preço (S1 é suficiente, mas também funcionam S2 e S3).  O escalão de DevTest não é suportado.  

5. Clique em **selecione** para confirmar o escalão de preço.

6. Clique em **criar** na gestão de modelo de ML à esquerda.

## <a name="get-model-management-account-information"></a>Obter informações de conta de gestão de modelo

Para obter informações sobre a sua conta de gestão de modelo (MMA), clique em de __conta de gestão de modelo__ no portal do Azure.

Copie os valores dos seguintes itens:

+ Nome da conta de gestão de modelo (no canto superior esquerdo)
+ Nome do grupo de recursos
+ ID da subscrição
+ Localização (utilize "eastus2")

![Informações da conta de gestão modelo](media/how-to-deploy-fpga-web-service/azure-portal-mma-info.PNG)

## <a name="set-up-your-machine"></a>Configurar o seu computador

Para configurar a estação de trabalho para a implementação de FPGA, utilize estes passos:

1. Transfira e instale a versão mais recente do [Git](https://git-scm.com/downloads).

2. Instalar [Anaconda (Python 3.6)](https://conda.io/miniconda.html).

3. Para transferir o ambiente de Anaconda, utilize o comando seguinte a partir de uma linha de comandos de Git:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```

4. Para criar o ambiente, abra uma **Anaconda linha** (e não uma linha do Workbench do Azure Machine Learning) e execute o seguinte comando:

    > [!IMPORTANT]
    > O `environment.yml` ficheiro está no repositório de git clonado no passo anterior. Altere o caminho conforme necessário para apontar para o ficheiro na sua estação de trabalho.

    ```
    conda env create -f environment.yml
    ```

5. Para ativar o ambiente, utilize o seguinte comando:

    ```
    conda activate amlrealtimeai
    ```

6. Para iniciar o servidor de notas do Jupyter, utilize o seguinte comando:

    ```
    jupyter notebook
    ```

    O resultado deste comando é semelhante ao seguinte texto:

    ```text
    Copy/paste this URL into your browser when you connect for the first time, to login with a token:
        http://localhost:8888/?token=bb2ce89cc8ae931f5df50f96e3a6badfc826ff4100e78075
    ```

    > [!TIP]
    > Irá obter um token diferente sempre que executar o comando.

    Se o seu browser não abrir automaticamente para o bloco de notas do Jupyter, utilize o URL de HTTP devolvido pelo comando anterior para abrir a página.

    ![Imagem da página web de notas do Jupyter](./media/how-to-deploy-fpga-web-service/jupyter-notebook.png)

## <a name="deploy-your-model"></a>Implementar o seu modelo

A partir do bloco de notas do Jupyter, abra o `00_QuickStart.ipynb` bloco de notas do `notebooks/resnet50` diretório. Siga as instruções no bloco de notas para:

* Definir o serviço
* Implementar o modelo
* Consumir o modelo de implementada
* Eliminar os serviços implementados

> [!IMPORTANT]
> Para otimizar a latência e débito, a estação de trabalho deve estar na mesma região do Azure como o ponto final.  Atualmente, as APIs são criadas na região do Azure do Leste-nos.

## <a name="ssltls-and-authentication"></a>SSL/TLS e autenticação

O Azure Machine Learning fornece suporte SSL e autenticação baseada em chave. Isto permite-lhe restringir o acesso ao seu serviço e proteger os dados submetidos por clientes.

> [!NOTE]
> Os passos nesta secção aplicam-se apenas a modelos de acelerados de Hardware do Azure Machine Learning. Para serviços padrão do Azure Machine Learning, consulte o [como configurar o SSL no Azure Machine Learning computação](https://docs.microsoft.com/azure/machine-learning/preview/how-to-setup-ssl-on-mlc) documento.

> [!IMPORTANT]
> Autenticação só é ativada para os serviços que forneceram um certificado SSL e a chave. 
>
> Se não ativar o SSL, qualquer utilizador na internet conseguirá efetuar chamadas para o serviço.
>
> Se ativar o SSL e chave de autenticação é necessária quando o acesso ao serviço.

SSL encripta os dados enviados entre o cliente e o serviço. Se também utilizado pelo cliente para verificar a identidade do servidor.

Pode implementar um serviço com SSL ativado, ou atualizar um serviço já implementado para ativá-la. Os passos são os mesmos:

1. Adquirir um nome de domínio.

2. Adquirir um certificado SSL.

3. Implementar ou atualizar o serviço com SSL ativado.

4. Atualize o DNS para apontar para o serviço.

### <a name="acquire-a-domain-name"></a>Adquirir um nome de domínio

Se já não possui um nome de domínio, pode comprar um um __entidade de registo de nome de domínio__. O processo é diferente entre registrars, como sucede o custo. A entidade de registo também fornece ferramentas para gerir o nome de domínio. Estas ferramentas são utilizadas para mapear um nome de domínio completamente qualificado (tal como www.contoso.com) para o endereço IP que o serviço está alojado no.

### <a name="acquire-an-ssl-certificate"></a>Adquirir um certificado SSL

Existem várias formas de obter um certificado SSL. As mais comuns é a adquirir um um __autoridade de certificação__ (AC). Independentemente de onde obter o certificado, terá dos seguintes ficheiros:

* A __certificado__. O certificado tem de conter a cadeia de certificados completa e tem de ser codificados PEM.
* A __chave__. A chave tem de ser codificados PEM.

> [!TIP]
> Se a autoridade de certificação não é possível fornecer o certificado e a chave, como os ficheiros com codificação PEM, pode utilizar um utilitário como [OpenSSL](https://www.openssl.org/) para alterar o formato.

> [!IMPORTANT]
> Certificados autoassinados devem ser utilizados apenas para o desenvolvimento. Não deve ser utilizadas na produção.
>
> Se utilizar um certificado autoassinado, consulte o [consumir os serviços de certificados autoassinados](#self-signed) secção para obter instruções específicas.

> [!WARNING]
> Quando solicitar um certificado, tem de fornecer o nome de domínio completamente qualificado (FQDN) do endereço que pretende utilizar para o serviço. Por exemplo, www.contoso.com. O endereço de carimbo de data / para o certificado e o endereço utilizado pelos clientes são comparadas ao validar a identidade do serviço.
>
> Se os endereços não corresponderem, os clientes irão receber um erro. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Implementar ou atualizar o serviço com SSL ativado

Para implementar o serviço com SSL ativado, defina o `ssl_enabled` parâmetro `True`. Definir o `ssl_certificate` parâmetro para o valor da __certificado__ ficheiros e o `ssl_key` para o valor da __chave__ ficheiro. O exemplo seguinte mostra como implementar um serviço com SSL ativado:

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

A resposta do `create_service` operação contém o endereço IP do serviço. O endereço IP é utilizado ao mapear o nome DNS para o endereço IP do serviço.

A resposta também contém um __chave primária__ e __chave secundária__ que são utilizados para consumir o serviço.

### <a name="update-your-dns-to-point-to-the-service"></a>Atualizar o DNS para apontar para o serviço

Utilize as ferramentas fornecidas pelo sua entidade de registo de nome de domínio para atualizar o registo DNS para o seu nome de domínio. O registo tem de apontar para o endereço IP do serviço.

> [!NOTE]
> Consoante a entidade de registo e o tempo TTL (TTL) configurada para o nome de domínio, pode demorar vários minutos a várias horas antes dos clientes podem resolver o nome de domínio.

### <a name="consuming-authenticated-services"></a>Consumir serviços autenticados

Os exemplos seguintes demonstram como consumir um serviço autenticado utilizando Python e c#:

> [!NOTE]
> Substitua `authkey` com a chave primária ou secundária devolvido ao criar o serviço.

```python
from amlrealtimeai import PredictionClient
client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
image_file = R'C:\path_to_file\image.jpg'
results = client.score_image(image_file)
```

```csharp
var client = new ScoringClient(host, 50051, useSSL, "authKey");
float[,] result;
using (var content = File.OpenRead(image))
    {
        IScoringRequest request = new ImageRequest(content);
        result = client.Score<float[,]>(request);
    }
```

Outros clientes gRPC podem autenticar pedidos, definindo um cabeçalho de autorização. A abordagem geral consiste em criar um `ChannelCredentials` objeto que combina `SslCredentials` com `CallCredentials`. Isto é adicionado ao cabeçalho de autorização do pedido. Para mais informações sobre como implementar o suporte para os cabeçalhos específicos, consulte [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

Os exemplos seguintes demonstram como definir o cabeçalho em c# e aceda:

```csharp
creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                      async (context, metadata) =>
                      {
                          metadata.Add(new Metadata.Entry("authorization", "authKey"));
                          await Task.CompletedTask;
                      }));

```

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

### <a id="self-signed"></a>Consumir os serviços de certificados autoassinados

Existem duas formas de ativar o cliente autenticar para um servidor protegido com um certificado autoassinado:

* No sistema cliente, defina o `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` variável de ambiente no sistema de cliente para apontar para o ficheiro de certificado.

* Quando construir um `SslCredentials` objeto, transmitir o conteúdo do ficheiro de certificado para o construtor.

Utilizando um dos métodos faz com que gRPC utilizar o certificado como o certificado de raiz.

> [!IMPORTANT]
> gRPC não aceitará certificados não fidedignos. Utilizar um certificado não fidedigno irá falhar com um `Unavailable` código de estado. Os detalhes da falha contêm `Connection Failed`.
