---
title: Implementar modelos como serviços da web
titleSuffix: Azure Machine Learning service
description: 'Saiba como e onde implementar os seus modelos de serviço do Azure Machine Learning, incluindo: Matrizes de porta do Container Instances, serviço Kubernetes do Azure, Azure IoT Edge e campos programáveis do Azure.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: e7840bb3ac6449009b843bb74cc19b960b492205
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310152"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementar modelos com o serviço Azure Machine Learning

O serviço Azure Machine Learning fornece várias formas que pode implementar o modelo preparado com o SDK. Neste documento, saiba como implementar o seu modelo como um serviço web na cloud do Azure ou para dispositivos do IoT edge.

Pode implementar modelos para os seguintes destinos de computação:

| Destino de computação | Tipo de implementação | Descrição |
| ----- | ----- | ----- |
| [Azure Container Instances (ACI)](#aci) | Serviço Web | Implementação rápida. Bom para desenvolvimento ou teste. |
| [Serviço Kubernetes do Azure (AKS)](#aks) | Serviço Web | Ideal para implementações de produção de grande escala. Fornece o dimensionamento automático e tempos de resposta rápidos. |
| [Azure IoT Edge](#iotedge) | Módulo de IoT | Implemente modelos em dispositivos IoT. Inferência acontece no dispositivo. |
| [Matriz de porta de campos programáveis (FPGA)](#fpga) | Serviço Web | Latência ultrabaixa para inferência em tempo real. |

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]

## <a name="prerequisites"></a>Pré-requisitos

- Uma área de trabalho do serviço do Azure Machine Learning e o Azure Machine Learning SDK para Python instalada. Saiba como obter estes pré-requisitos com o [introdução ao guia de início rápido do Azure Machine Learning](quickstart-get-started.md).

- Um modelo preparado em qualquer um dos pickle (`.pkl`) ou ONNX (`.onnx`) formato. Se não tiver um modelo preparado, utilize os passos no [formar modelos](tutorial-train-models-with-aml.md) tutorial para treinar e registar um com o serviço Azure Machine Learning.

- As seções de código partem do princípio de que `ws` faz referência a sua área de trabalho do machine learning. Por exemplo, `ws = Workspace.from_config()`.

## <a name="deployment-workflow"></a>Fluxo de trabalho de implantação

O processo de implantação de um modelo é semelhante para todos os destinos de computação:

1. Prepare um modelo.
1. Registe o modelo.
1. Crie uma configuração de imagem.
1. Crie a imagem.
1. Implantar a imagem num destino de computação.
1. Testar a implementação
1. (Opcional) Limpe os artefactos.

    * Quando **implementar como um serviço web**, existem três opções de implementação:

        * [Implementar](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-): Ao utilizar este método, não é necessário registar o modelo ou criar a imagem. No entanto não é possível controlar o nome do modelo ou imagem, ou etiquetas e descrições associados.
        * [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-): Ao utilizar este método, não é necessário criar uma imagem. Mas não tem controlo sobre o nome da imagem que é criado.
        * [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-): Registe o modelo e criar uma imagem antes de utilizar este método.

        Os exemplos nesta documentam utilização `deploy_from_image`.

    * Quando **implantação como um módulo do IoT Edge**, tem de registar o modelo e criar a imagem.

## <a name="register-a-model"></a>Registar um modelo

Apenas os modelos de formação podem ser implementados. O modelo pode ser treinado com o Azure Machine Learning ou outro serviço. Para registar um modelo do ficheiro, utilize o seguinte código:

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = ["0.1"],
                       description = "test",
                       workspace = ws)
```

> [!NOTE]
> Embora o exemplo mostra a utilizar um modelo armazenado como um ficheiro pickle, também pode modelos ONNX a utilizados. Para obter mais informações sobre como utilizar modelos ONNX, consulte a [ONNX e o Azure Machine Learning](how-to-build-deploy-onnx.md) documento.

Para obter mais informações, consulte a documentação de referência para o [classe de modelo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Criar uma configuração de imagem

Modelos implementados são empacotados como uma imagem. A imagem contém as dependências necessárias para executar o modelo.

Para **instância de contentor do Azure**, **Azure Kubernetes Service**, e **Azure IoT Edge** implementações, a `azureml.core.image.ContainerImage` classe é utilizada para criar uma configuração de imagem. A configuração de imagem, em seguida, é utilizada para criar uma nova imagem do Docker. 

O código a seguir demonstra como criar uma nova configuração de imagem:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml",
                                                 description = "Image with ridge regression model",
                                                 tags = {"data": "diabetes", "type": "regression"}
                                                 )
```

Esta configuração utiliza um `score.py` ficheiro passar pedidos para o modelo. Este ficheiro contém duas funções:

* `init()`: Normalmente, esta função carrega o modelo de um objeto global. Esta função só é executada uma vez, quando o contentor do Docker é iniciado. 

* `run(input_data)`: Essa função usa o modelo para prever um valor com base nos dados de entrada. Regra geral, as entradas e as saídas da execução utilizam JSON para a serialização e a desserialização, mas são suportados outros formatos.

Para obter um exemplo `score.py` de ficheiros, consulte a [tutorial de classificação de imagem](tutorial-deploy-models-with-aml.md#make-script). Para obter um exemplo que utiliza um modelo ONNX, consulte a [ONNX e o Azure Machine Learning](how-to-build-deploy-onnx.md) documento.

O `conda_file` parâmetro é utilizado para fornecer um ficheiro de ambiente de conda. Esse arquivo define o ambiente de conda para o modelo implementado. Para obter mais informações sobre como criar este ficheiro, consulte [criar um ficheiro de ambiente (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file).

Para obter mais informações, consulte a documentação de referência para [ContainerImage classe](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)

## <a id="createimage"></a> Criar a imagem

Depois de criar a configuração de imagem, pode usá-lo para criar uma imagem. Esta imagem é armazenada no registo de contentor para a área de trabalho. Depois de criado, pode implementar a mesma imagem em vários serviços.

```python
# Create the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Estimativa de tempo**: Aproximadamente 3 minutos.

As imagens são automaticamente com a versão ao registar várias imagens com o mesmo nome. Por exemplo, a primeira imagem registado como `myimage` é atribuído um ID de `myimage:1`. Da próxima vez que Registre-se uma imagem como `myimage`, o ID da nova imagem é `myimage:2`.

Para obter mais informações, consulte a documentação de referência [ContainerImage classe](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a name="deploy-the-image"></a>Implementar a imagem

Quando chegar à implementação, o processo é ligeiramente diferente consoante o destino de computação que implementar. Utilize as informações nas secções seguintes para aprender a implementar em:

* [Azure Container Instances](#aci)
* [Serviços de Kubernetes do Azure](#aks)
* [Project Brainwave (matrizes de porta de campos programáveis)](#fpga)
* [Dispositivos do IoT Edge do Azure](#iot)

### <a id="aci"></a> Implementar no Azure Container Instances

Utilização do Azure Container Instances para implementar os seus modelos como um serviço da web se um ou mais das seguintes condições for verdadeiro:

- Precisa implementar e validar o seu modelo rapidamente. Conclusão da implementação do ACI em menos de 5 minutos.
- Está a testar um modelo que está em desenvolvimento. Para ver a disponibilidade de região e quota para o ACI, consulte a [Quotas e disponibilidade das regiões do Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) documento.

Para implementar no Azure Container Instances, utilize os seguintes passos:

1. Defina a configuração de implementação. O exemplo seguinte define uma configuração que utiliza um núcleo de CPU e 1 GB de memória:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Para implementar a imagem criada no [criar a imagem](#createimage) secção deste documento, utilize o seguinte código:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Estimativa de tempo**: Aproximadamente 3 minutos.

    > [!TIP]
    > Se houver erros durante a implementação, utilize `service.get_logs()` para ver os registos de serviço do AKS. As informações com sessão iniciada podem indicar a causa do erro.

Para obter mais informações, consulte a documentação de referência para o [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) e [Webservice](https://docs.microsoft.comS/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py) classes.

### <a id="aks"></a> Implementar no serviço Kubernetes do Azure

Para implementar o seu modelo como um serviço da web de produção de grande escala, utilize o Azure Kubernetes Service (AKS). Pode utilizar um cluster do AKS existente ou crie um novo com o SDK do Azure Machine Learning, a CLI ou o portal do Azure.

Criar um cluster do AKS é um processo de tempo para a área de trabalho. Pode reutilizar este cluster para várias implementações. Se eliminar o cluster, em seguida, tem de criar um novo cluster da próxima vez que precisa implantar.

Serviço Kubernetes do Azure fornece as seguintes capacidades:

* Dimensionamento automático
* Registo
* Recolha de dados do modelo
* Tempos de resposta rápidos para os seus serviços web

Para implementar no serviço Kubernetes do Azure, utilize os seguintes passos:

1. Para criar um cluster do AKS, utilize o seguinte código:

    > [!IMPORTANT]
    > Criar o cluster do AKS é um processo de tempo para a área de trabalho. Depois de criado, pode reutilizar este cluster para várias implementações. Se eliminar o cluster ou o grupo de recursos que o contém, em seguida, tem de criar um novo cluster da próxima vez que precisa implantar.

    ```python
    from azureml.core.compute import AksCompute, ComputeTarget

    # Use the default configuration (you can also provide parameters to customize this)
    prov_config = AksCompute.provisioning_configuration()

    aks_name = 'aml-aks-1' 
    # Create the cluster
    aks_target = ComputeTarget.create(workspace = ws, 
                                        name = aks_name, 
                                        provisioning_configuration = prov_config)

    # Wait for the create process to complete
    aks_target.wait_for_completion(show_output = True)
    print(aks_target.provisioning_state)
    print(aks_target.provisioning_errors)
    ```

    **Estimativa de tempo**: Cerca de 20 minutos.

    > [!TIP]
    > Se já tiver um cluster do AKS na sua subscrição do Azure e é a versão 1.11. *, pode usá-lo para implementar a imagem. O código a seguir demonstra como anexar um cluster existente para a área de trabalho:
    >
    > ```python
    > # Set the resource group that contains the AKS cluster and the cluster name
    > resource_group = 'myresourcegroup'
    > cluster_name = 'mycluster'
    > 
    > # Attatch the cluster to your workgroup
    > attach_config = AksCompute.attach_configuration(resource_group = resource_group,
    >                                          cluster_name = cluster_name)
    > compute = ComputeTarget.attach(ws, 'mycompute', attach_config)
    > 
    > # Wait for the operation to complete
    > aks_target.wait_for_completion(True)
    > ```

2. Para implementar a imagem criada no [criar a imagem](#createimage) secção deste documento, utilize o seguinte código:

    ```python
    from azureml.core.webservice import Webservice, AksWebservice

    # Set configuration and service name
    aks_config = AksWebservice.deploy_configuration()
    aks_service_name ='aks-service-1'
    # Deploy from image
    service = Webservice.deploy_from_image(workspace = ws, 
                                                name = aks_service_name,
                                                image = image,
                                                deployment_config = aks_config,
                                                deployment_target = aks_target)
    # Wait for the deployment to complete
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

    > [!TIP]
    > Se houver erros durante a implementação, utilize `service.get_logs()` para ver os registos de serviço do AKS. As informações com sessão iniciada podem indicar a causa do erro.

Para obter mais informações, consulte a documentação de referência para o [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) e [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py) classes.

### <a id="fpga"></a> Implementar a matrizes de porta de campos programáveis (FPGA)

Project Brainwave torna possível alcançar a latência ultrabaixa para pedidos de inferência em tempo real. Project Brainwave acelera redes neurais profundas (DNN) implementadas nas matrizes de porta de campos programáveis na cloud do Azure. Normalmente utilizado o DNNs estão disponíveis como featurizers para aprendizagem de transferência ou personalizável com pesos preparado a partir de seus próprios dados.

Para obter instruções de implantação de um modelo com o Project Brainwave, consulte a [implementar para um FPGA](how-to-deploy-fpga-web-service.md) documento.

### <a id="iotedge"></a> Implementar o Azure IoT Edge

Um dispositivo Azure IoT Edge é um Linux ou um dispositivo baseado em Windows que executa o tempo de execução do Azure IoT Edge. Modelos de aprendizagem automática podem ser implementados para estes dispositivos, como módulos do IoT Edge. Implementar um modelo para um dispositivo IoT Edge permite ao dispositivo utilizar o modelo diretamente, em vez de precisar enviar dados para a cloud para processamento. Obtém os tempos de resposta mais rápidos e menos transferência de dados.

Módulos do IoT Edge do Azure são implementados para o seu dispositivo a partir de um registo de contentor. Quando cria uma imagem do seu modelo, são armazenado no registo de contentor para a área de trabalho.

Utilize os seguintes passos para obter as credenciais do registo de contentor para sua área de trabalho do serviço do Azure Machine Learning:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/signin/index).

1. Aceda à sua área de trabalho do serviço do Azure Machine Learning e selecione __descrição geral__. Para ir para as definições de registo de contentor, selecione o __Registro__ ligação.

    ![Uma imagem da entrada de registo de contentor](./media/how-to-deploy-and-where/findregisteredcontainer.png)

1. Uma vez no registo de contentor, selecione **chaves de acesso** e, em seguida, ative o utilizador de administrador.

    ![Uma imagem do ecrã de chaves de acesso](./media/how-to-deploy-and-where/findaccesskey.png)

1. Guarde os valores para **servidor de início de sessão**, **nome de utilizador**, e **palavra-passe**. 

Assim que tiver as credenciais, utilize os passos no [módulos de implementar o Azure IoT Edge do portal do Azure](../../iot-edge/how-to-deploy-modules-portal.md) documento para implementar a imagem no seu dispositivo. Quando configurar o __definições de registo__ para o dispositivo, utilize o __servidor de início de sessão__, __nome de utilizador__, e __palavra-passe__ sua área de trabalho registo de contentor.

> [!NOTE]
> Se não estiver familiarizado com o Azure IoT, consulte os seguintes documentos para obter informações sobre como começar com o serviço:
>
> * [Início rápido: Implementar o seu primeiro módulo do IoT Edge num dispositivo de Linux](../../iot-edge/quickstart-linux.md)
> * [Início rápido: Implementar o seu primeiro módulo do IoT Edge num dispositivo Windows](../../iot-edge/quickstart.md)

## <a name="testing-web-service-deployments"></a>Implementações de serviço web de teste

Para testar uma implementação do serviço web, pode usar o `run` método do objeto de serviço Web. No exemplo a seguir, um documento JSON está definido como um serviço da web e o resultado é exibido. Os dados enviados tem de corresponder ao que o modelo de espera. Neste exemplo, o formato de dados corresponde a entrada esperada pelo modelo diabetes.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

## <a name="update-the-web-service"></a>Atualizar o serviço web

Para atualizar o serviço web, utilize o `update` método. O código a seguir demonstra como atualizar o serviço web para utilizar uma nova imagem:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)
# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Quando atualizar uma imagem, o serviço web não é atualizado automaticamente. Tem de atualizar manualmente cada serviço que pretende utilizar a nova imagem.

## <a name="clean-up"></a>Limpeza

Para eliminar um serviço web implementado, utilize `service.delete()`.

Para eliminar uma imagem, utilize `image.delete()`.

Para eliminar um modelo registado, utilize `model.delete()`.

## <a name="next-steps"></a>Passos Seguintes

* [Proteger serviços da web do Azure Machine Learning com SSL](how-to-secure-web-service.md)
* [Consumir um modelo de ML implementado como um serviço web](how-to-consume-web-service.md)
* [Como executar previsões de batch](how-to-run-batch-predictions.md)
