---
title: Implementar um modelo como um serviço web num FPGA com o Azure Machine Learning
description: Saiba como implementar um serviço web com um modelo em execução num FPGA com o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 10/01/2018
ms.openlocfilehash: df6637f1a52b679ba9ad0a49fb37d4e4b72f35e4
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237828"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Implementar um modelo como um serviço web num FPGA com o Azure Machine Learning

Pode implementar um modelo como um serviço web no [campo matrizes de porta programável por (FPGAs)](concept-accelerate-with-fpgas.md).  A utilização de FPGAs fornece a inferência de latência Ultra baixas, mesmo com um tamanho de lote única.   

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

- Tem de pedir e ser aprovado para a quota FPGA. Para pedir acesso, preencha o formulário de pedido de quota: https://aka.ms/aml-real-time-ai

- Uma área de trabalho do serviço do Azure Machine Learning e o Azure Machine Learning SDK para Python instalada. Saiba como obter estes pré-requisitos com o [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md) documento.
 
  - Deverá estar na sua área de trabalho do *E.U.A. Leste 2* região.

  - Instale os extras contrib:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Criar e implementar o seu modelo
Criar um pipeline para pré-processar a imagem de entrada, caracterização usando 50 de utilizar o ResNet num FPGA e, em seguida, execute os recursos através de um classifer com base em com o conjunto de dados ImageNet.

Siga as instruções para:

* Definir o pipeline de modelo
* Implementar o modelo
* Consumir o modelo implementado
* Eliminar serviços implementados

> [!IMPORTANT]
> Para otimizar o débito e latência, o cliente deve ser na mesma região do Azure como o ponto final.  Atualmente as APIs são criadas na região do Azure do Leste-nos.



### <a name="preprocess-image"></a>Pré-processar a imagem
O primeiro estágio do pipeline é pré-processar as imagens.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Adicionar Featurizer
Inicializar o modelo e transfira um ponto de verificação do TensorFlow da versão quantificada do ResNet50 para ser utilizado como um featurizer.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Adicionar o classificador
Este classificador foi treinado no conjunto de dados ImageNet.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>Criar definição de serviço
Agora que tem definidas, o processamento prévio de imagem, featurizer e classificador que executa o serviço, pode criar uma definição de serviço. A definição de serviço é um conjunto de arquivos gerados a partir do modelo que é implementado para o serviço FPGA. A definição de serviço é constituído por um pipeline. O pipeline é uma série de fases que são executados na ordem.  Fases do TensorFlow, Keras fases e fases BrainWave são suportadas.  As fases são executadas na ordem no serviço, com a saída de cada entrada de fase para a fase subsequente.

Para criar um estágio de TensorFlow, especifique uma sessão que contém o gráfico (neste caso é usado gráfico do padrão) e a entrada e saída tensors para esta fase.  Estas informações são utilizadas para guardar o gráfico para que ele pode ser executado no serviço.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Implementar modelo
Crie um serviço da definição de serviço.  A área de trabalho tem de estar na localização E.U.A. Leste 2.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Testar o serviço
Para enviar uma imagem para a API e testar a resposta, adicione um mapeamento de ID de classe de saída para o nome da classe ImageNet.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Chamar o seu serviço e substitua o nome do ficheiro de "your jpg" abaixo de uma imagem a partir do seu computador. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Limpar o serviço
Elimine o serviço.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Proteger serviços da web FPGA

Modelos de Machine Learning do Azure em execução no FPGAs fornecem suporte SSL e autenticação baseada em chave. Isto permite-lhe restringir o acesso ao seu serviço e a proteger os dados submetidos por clientes.

> [!IMPORTANT]
> Autenticação apenas está ativada para os serviços que forneceram um certificado SSL e a chave. 
>
> Se não ativar SSL, qualquer utilizador na internet será capaz de fazer chamadas para o serviço.
>
> Se ativar o SSL e a chave de autenticação é necessária ao acessar o serviço.

O SSL criptografa dados enviados entre o cliente e o serviço. Ele é também utilizado pelo cliente para verificar a identidade do servidor.

Pode implementar um serviço com SSL ativado, ou atualizar um serviço já implementado para ativá-la. Os passos são os mesmos:

1. Adquira um nome de domínio.

2. Adquira um certificado SSL.

3. Implementar ou atualizar o serviço com SSL ativado.

4. Atualize o DNS para apontar para o serviço.

### <a name="acquire-a-domain-name"></a>Adquirir um nome de domínio

Se já não possui um nome de domínio, pode comprar um de uma __entidade de registo de nome de domínio__. O processo é diferente entre entidades de registo, como faz o custo. A entidade de registo também fornece ferramentas para gerenciar o nome de domínio. Essas ferramentas são utilizadas para mapear um nome de domínio completamente qualificado (como www.contoso.com) para o endereço IP que está alojado o serviço em.

### <a name="acquire-an-ssl-certificate"></a>Adquirir um certificado SSL

Existem várias formas de obter um certificado SSL. A mais comum é comprar um de uma __autoridade de certificação__ (AC). Independentemente de onde obter o certificado, terá dos seguintes ficheiros:

* R __certificado__. O certificado tem de conter a cadeia de certificados completa e tem de ser PEM codificado.
* R __chave__. A chave tem de ser PEM codificado.

> [!TIP]
> Se a autoridade de certificação não é possível fornecer o certificado e chave, como ficheiros PEM codificado, pode utilizar um utilitário como [OpenSSL](https://www.openssl.org/) para alterar o formato.

> [!IMPORTANT]
> Certificados autoassinados devem ser usados apenas para desenvolvimento. Eles não devem ser usados na produção.
>
> Se utilizar um certificado autoassinado, consulte a [uso de serviços com certificados autoassinados](#self-signed) secção para obter instruções específicas.

> [!WARNING]
> Quando pedir um certificado, tem de fornecer o nome de domínio completamente qualificado (FQDN) do endereço que pretende utilizar para o serviço. Por exemplo, www.contoso.com. O endereço marcados para o certificado e o endereço utilizado pelos clientes são comparados ao validar a identidade do serviço.
>
> Se os endereços não corresponderem, os clientes irão receber um erro. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Implementar ou atualizar o serviço com SSL ativado

Para implementar o serviço com SSL ativado, defina o `ssl_enabled` parâmetro `True`. Definir o `ssl_certificate` parâmetro para o valor da __certificado__ ficheiro e o `ssl_key` para o valor da __chave__ ficheiro. O exemplo seguinte demonstra a implementar um serviço com SSL ativado:

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

A resposta do `create_service` operação contém o endereço IP do serviço. O endereço IP é utilizado quando for mapear o nome DNS para o endereço IP do serviço.

A resposta também contém um __chave primária__ e __chave secundária__ que são utilizados para consumir o serviço.

### <a name="update-your-dns-to-point-to-the-service"></a>Atualizar o DNS para apontar para o serviço

Utilize as ferramentas fornecidas pelo sua entidade de registo de nome de domínio para atualizar o registo DNS para o seu nome de domínio. O registo tem de apontar para o endereço IP do serviço.

> [!NOTE]
> Consoante a entidade de registo e a hora para live (TTL) configurada para o nome de domínio, pode demorar vários minutos a várias horas para que os clientes podem resolver o nome de domínio.

### <a name="consume-authenticated-services"></a>Consumir serviços autenticados

Os exemplos seguintes demonstram como consumir um serviço autenticado com Python e c#:

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

Outros clientes gRPC podem autenticar pedidos ao definir um cabeçalho de autorização. A abordagem geral é criar um `ChannelCredentials` objeto que combina `SslCredentials` com `CallCredentials`. Isso é adicionado ao cabeçalho de autorização do pedido. Para obter mais informações sobre como implementar o suporte para os cabeçalhos de específicos, consulte [ https://grpc.io/docs/guides/auth.html ](https://grpc.io/docs/guides/auth.html).

Os exemplos seguintes demonstram como definir o cabeçalho em c# e Go:

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

### <a id="self-signed"></a>Uso de serviços com certificados autoassinados

Existem duas formas de ativar o cliente para autenticação num servidor protegido por um certificado autoassinado:

* No sistema cliente, defina o `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` variável de ambiente no sistema de cliente para apontar para o ficheiro de certificado.

* Ao construir um `SslCredentials` de objeto, transmita os conteúdos do ficheiro de certificado para o construtor.

Utilizando um dos métodos faz com que o gRPC utilizar o certificado como o certificado de raiz.

> [!IMPORTANT]
> gRPC não aceita certificados não fidedignos. Utilizando um certificado não fidedigno irá falhar com um `Unavailable` código de estado. Os detalhes da falha contêm `Connection Failed`.

## <a name="sample-notebook"></a>Bloco de notas de exemplo

Conceitos neste artigo são demonstrados a `project-brainwave/project-brainwave-quickstart.ipynb` bloco de notas.

Obter este bloco de notas:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
