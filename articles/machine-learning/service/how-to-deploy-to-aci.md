---
title: Implementar serviços web ao Azure Container Instances (ACI) - Azure Machine Learning
description: Saiba como implementar um modelo preparado como um serviço web no Azure Container Instances (ACI) com o serviço Azure Machine Learning. Este artigo mostra três formas diferentes de implementar um modelo no ACI. Eles diferem no número de linhas de código e o controlo que tem na nomenclatura partes da implementação.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 8a736516a598eee051b416834d2b737211e66b96
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429468"
---
# <a name="deploy-web-services-to-azure-container-instances"></a>Implementar serviços web no Azure Container Instances 

Pode implementar o modelo treinado como um serviço web no [Azure Container Instances](https://azure.microsoft.com/services/container-instances/) (ACI), [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) (AKS), o IoT edge o dispositivo, ou [campo matrizes de porta programável por (FPGAs)](concept-accelerate-with-fpgas.md) 

ACI é geralmente mais barato do que o AKS e pode ser configurado nas linhas 4 a 6 de código. ACI é a opção perfeita para as implementações de teste. Mais tarde, quando estiver pronto para utilizar os seus modelos e serviços da web para utilização em produção, de grande escala, pode [implementá-las no AKS](how-to-deploy-to-aks.md).

Este artigo mostra três formas diferentes de implementar um modelo no ACI. Eles diferem no número de linhas de código e o controlo que tem na nomenclatura partes da implementação. Do método com o mínimo de código e o controle para o método com a maior parte do código e o controle, as opções de ACI são:

* Implementar a utilização do ficheiro de modelo `Webservice.deploy()` 
* Implementar a partir de utilizar o modelo registado `Webservice.deploy_from_model()`
* Implementar o modelo registado de imagem com `Webservice.deploy_from_image()`

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

- Uma área de trabalho do serviço do Azure Machine Learning e o Azure Machine Learning SDK para Python instalada. Saiba como obter estes pré-requisitos com o [introdução ao guia de início rápido do Azure Machine Learning](quickstart-get-started.md).

- O objeto de área de trabalho de serviço do Azure Machine Learning

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

- Um modelo para implementar. Os exemplos neste documento utilizam o modelo criado quando seguir a "[preparar um modelo](tutorial-train-models-with-aml.md)" tutorial. Se não utilizar este modelo, modificar os passos para fazer referência para o seu nome de modelo.  Também terá de escrever seu próprio script de classificação para executar o seu modelo.


## <a name="configure-an-image"></a>Configurar uma imagem

Configure a imagem do Docker que é utilizada para armazenar todos os arquivos de modelo.
1. Criar um script de classificação (score.py) [a utilizar estas instruções](tutorial-deploy-models-with-aml.md#create-scoring-script)

1. Crie um ficheiro de ambiente (myenv.yml) [a utilizar estas instruções](tutorial-deploy-models-with-aml.md#create-environment-file) 

1. Utilize esses dois arquivos para configurar a imagem do Docker no Python com o SDK da seguinte forma:

    ```python
    from azureml.core.image import ContainerImage

    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                      runtime = "python",
                                                      conda_file = "myenv.yml",
                                                      description = "Image with mnist model",
                                                      tags = {"data": "mnist", "type": "classification"}
                                                     )
    ```

## <a name="configure-the-aci-container"></a>Configure o contentor do ACI

Configure o contentor do ACI ao especificar o número de CPUs e gigabyte de RAM necessário para o contentor do ACI. A predefinição de um núcleo e 1 gigabyte de RAM são suficientes para o número de modelos. Se achar que precisa de mais licenças posteriormente, recriar a imagem e reimplemente o serviço.  

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores = 1, 
                                               memory_gb = 1, 
                                               tags = {"data": "mnist", "type": "classification"},
                                               description = 'Handwriting recognition')
```

## <a name="register-a-model"></a>Registar um modelo

> Ignorar este pré-requisito se estiver [implementar a partir de um ficheiro de modelo](#deploy-from-model-file) (`Webservice.deploy()`).

Registar um modelo a utilizar [Webservice.deploy_from_model](#deploy-from-registered-model) ou [Webservice.deploy_from_image](#deploy-from-image). Ou, se já tiver um modelo registado, recuperá-lo agora.

### <a name="retrieve-a-registered-model"></a>Obter um modelo registado
Se utilizar o Azure Machine Learning para preparar o seu modelo, o modelo já pode ser registado na sua área de trabalho.  Por exemplo, a última etapa do [treinar um tutorial de modelo](tutorial-train-models-with-aml.md) registado o modelo.  Em seguida, a obter o modelo registado para implementar.

```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model=Model(ws, model_name)
```
  
### <a name="register-a-model-file"></a>Registe-se de um ficheiro de modelo

Se o seu modelo foi criado em outro lugar, ainda pode registá-lo na área de trabalho.  Para registar um modelo, o ficheiro de modelo (`sklearn_mnist_model.pkl` neste exemplo) tem de estar no diretório de trabalho atual. Em seguida, registe-se esse arquivo como um modelo chamado `sklearn_mnist` na área de trabalho com `Model.register()`.
    
```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model = Model.register(model_path = "sklearn_mnist_model.pkl",
                        model_name = model_name,
                        tags = {"data": "mnist", "type": "classification"},
                        description = "Mnist handwriting recognition",
                        workspace = ws)
```

<a name='deploy-from-model-file'/>
## <a name="option-1-deploy-from-model-file"></a>Opção 1: Implementar a partir de ficheiro de modelo

A opção de implementar a partir de um ficheiro de modelo requer a menor quantidade de código para escrever, mas também oferece a menor quantidade de controle sobre a nomenclatura dos componentes. Esta opção começa com um ficheiro de modelo e regista-o para a área de trabalho para.  No entanto, não é possível nomear o modelo ou associar etiquetas ou uma descrição para ele.  

Esta opção utiliza o método SDK, Webservice.deploy().  

**Estimativa de tempo**: implementação demora cerca de 6 a 7 minutos.

1. Certifique-se de que o ficheiro de modelo está no diretório de trabalho local.

1. Abra o ficheiro de modelo de pré-requisitos, score.py e alterar o `init()` secção para:

    ```python
    def init():
        global model
        # retreive the local path to the model using the model name
        model_path = Model.get_model_path('sklearn_mnist_model.pkl')
        model = joblib.load(model_path)
    ```

1. Implemente o ficheiro de modelo.

    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-1'
    service = Webservice.deploy(deployment_config = aciconfig,
                                    image_config = image_config,
                                    model_paths = ['sklearn_mnist_model.pkl'],
                                    name = service_name,
                                    workspace = ws)
    
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Agora, pode [testar o serviço web](#test-web-service).

<a name='deploy-from-registered-model'/>
## <a name="option-2-deploy-from-registered-model"></a>Opção 2: Implementar a partir de modelo registado

A opção de implementar um ficheiro de modelo registado demora mais alguns linhas de código e permite que algum controle sobre a nomenclatura de saídas. Esta opção é um meio cómodo para implementar um modelo registado que já tem.  No entanto, não é possível nomear a imagem do Docker.  

Esta opção utiliza o método SDK, Webservice.deploy_from_model().

**Estimativa de tempo**: implementação com esta opção a demora cerca de 8 minutos.

1. Execute o código para configurar o contentor do Docker e o contentor do ACI e especificar o modelo registado.

    ```python
    from azureml.core.webservice import Webservice

    service_name = 'aci-mnist-2'
    service = Webservice.deploy_from_model(deployment_config = aciconfig,
                                           image_config = image_config,
                                           models = [model], # this is the registered model object
                                           name = service_name,
                                           workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Agora, pode [testar o serviço web](#test-web-service).

<a name='deploy-from-image'/>
## <a name="option-3-deploy-from-image"></a>Opção 3: Implementar a partir da imagem

Implementar um modelo registado (`model`) com `Webservice.deploy_from_image()`. Este método permite-lhe criar a imagem do Docker separadamente e, em seguida, implementar a partir dessa imagem.

1. Crie e registe-se a imagem do Docker com a área de trabalho `ContainerImage.create()`

    Este método fornece mais controle sobre a imagem através da criação de um passo separado.  O modelo registado (`model`) está incluído na imagem.
    
    ```python
    from azureml.core.image import ContainerImage
    
    image = ContainerImage.create(name = "myimage1",
                                  models = [model], # this is the registered model object
                                  image_config = image_config,
                                  workspace = ws)
    
    image.wait_for_creation(show_output = True)
    ```
**Estimativa de tempo**: aproximadamente 3 minutos.

1. Implementar a imagem do Docker como um serviço a utilizar `Webservice.deploy_from_image()`

    Agora, implemente a imagem no ACI.  
    
    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-3'
    service = Webservice.deploy_from_image(deployment_config = aciconfig,
                                                image = image,
                                                name = service_name,
                                                workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```   
 
**Estimativa de tempo**: aproximadamente 3 minutos.

Este método fornece mais controle sobre a criação e nomeação de componentes na implementação.

Agora pode testar o serviço web.

<a name='test-web-service'/>
## <a name="test-the-web-service"></a>Testar o serviço web

O serviço web é o mesmo, independentemente da que foi utilizado o método.  Para obter previsões de indisponibilidade, utilize o `run` método do serviço.  

```python
# Load Data
import os
import urllib

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/test-images.gz')

from utils import load_data
X_test = load_data('./data/test-images.gz', False) / 255.0

from sklearn import datasets
import numpy as np
import json

# find 5 random samples from test set
n = 5
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
prediction = service.run(input_data = test_samples)
print(prediction)
```


## <a name="clean-up-resources"></a>Limpar recursos

Se não pretende utilizar este serviço web, elimine-o para que não existem custos.

```python
service.delete()
```

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar no serviço Kubernetes do Azure](how-to-deploy-to-aks.md) para uma implementação de escala maior. 
