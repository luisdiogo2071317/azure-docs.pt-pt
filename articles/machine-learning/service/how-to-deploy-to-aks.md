---
title: Implementar modelos para Kubernetes do serviço Azure Machine Learning | Documentos da Microsoft
description: Saiba como implementar um modelo do serviço Azure Machine Learning no serviço Kubernetes do Azure. O modelo é implementado como um serviço web. Serviço Kubernetes do Azure é bom para cargas de trabalho de produção de grande escala.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 4e2529e760e23ba9837137cf91fd6ca7f52253b2
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237845"
---
# <a name="how-to-deploy-models-from-azure-machine-learning-service-to-azure-kubernetes-service"></a>Como implementar modelos de serviço do Azure Machine Learning no serviço Kubernetes do Azure

Para cenários de produção de grande escala, pode implementar o seu modelo para o Azure Kubernetes Service (AKS). O Azure Machine Learning pode utilizar um cluster do AKS existente ou um novo cluster criado durante a implementação. O modelo é implementado como um serviço web ASK.

Implementar no AKS fornece o dimensionamento automático, registo, a recolha de dados do modelo e tempos de resposta rápidos para o seu serviço web.

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

- Uma Azure Machine Learning serviço área de trabalho, um diretório local que contém os scripts e o SDK do Azure Machine Learning para Python instalada. Saiba como obter estes pré-requisitos com o [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md) documento.

- Um modelo de aprendizagem de máquina treinado. Se não tiver uma, veja a [preparar o modelo de classificação de imagem](tutorial-train-models-with-aml.md) tutorial.

## <a name="initialize-the-workspace"></a>Inicializar a área de trabalho

Para inicializar a área de trabalho, carregue o `config.json` ficheiro que contém as informações da sua área de trabalho.

```python
from azureml.coreazureml import Workspace

# Load existing workspace from the config file info.
ws = Workspace.from_config()
```

## <a name="register-the-model"></a>Registe o modelo

Para registar um modelo existente, especifica o caminho de modelo, descrição e marcas.

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl", # this points to a local file
                        model_name = "best_model", # this is the name the model is registered as
                        tags = {"data": "diabetes", "type": "regression"},
                        description = "Ridge regression model to predict diabetes",
                        workspace = ws)
```

## <a name="create-a-docker-image"></a>Crie uma imagem do Docker.

Serviço Kubernetes do Azure utiliza imagens do Docker. Para criar a imagem, utilize os seguintes passos:

1. Para configurar a imagem, tem de criar um script de classificação e o ficheiro de ambiente. Para obter um exemplo de criar o ficheiro de script e de ambiente, consulte as secções seguintes do exemplo de classificação de imagem:

    * [Criar um script de classificação (score.py)](tutorial-deploy-models-with-aml.md#create-scoring-script)

    * [Crie um ficheiro de ambiente (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file) 

   O exemplo seguinte utiliza estes ficheiros para configurar a imagem:

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

1. Para criar a imagem, utilize a configuração de modelo e imagem. Esta operação poderá demorar cerca de 5 minutos para concluir:

    ```python
    image = ContainerImage.create(name = "myimage1",
                                    # this is the model object
                                    models = [model],
                                    image_config = image_config,
                                    workspace = ws)

    # Wait for the create process to complete
    image.wait_for_creation(show_output = True)
    ```

## <a name="create-the-aks-cluster"></a>Criar o Cluster do AKS

O fragmento de código seguinte demonstra como criar o cluster do AKS. Este processo demora cerca de 20 minutos para concluir:

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

### <a name="attach-existing-aks-cluster-optional"></a>Anexar existente cluster do AKS (opcional)

Se tiver um cluster do AKS existente na sua subscrição do Azure, pode utilizá-lo para implementar a imagem. O fragmento de código seguinte demonstra como anexar um cluster a sua área de trabalho. 

> [!IMPORTANT]
> Apenas o AKS versão 1.8.7 é suportado.

```python
# Get the resource id from https://porta..azure.com -> Find your resource group -> click on the Kubernetes service -> Properties
resource_id = '/subscriptions/<your subscription id>/resourcegroups/<your resource group>/providers/Microsoft.ContainerService/managedClusters/<your aks service name>'

# Set to the name of the cluster
cluster_name='my-existing-aks' 

# Attatch the cluster to your workgroup
aks_target = AksCompute.attach(workspace=ws, name=cluster_name, resource_id=resource_id)

# Wait for the operation to complete
aks_target.wait_for_provisioning(True)
```

## <a name="deploy-your-web-service"></a>Implementar o seu serviço web

O fragmento de código seguinte demonstra como implementar a imagem para o cluster do AKS:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
aks_service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!TIP]
> Se houver erros durante a implementação, utilize `aks_service.get_logs()` para ver os registos de serviço do AKS. As informações com sessão iniciada podem indicar a causa do erro.

## <a name="test-the-web-service"></a>Testar o serviço web

Utilize `aks_service.run()` para testar o serviço web. O fragmento de código seguinte demonstra como transmitir dados para o serviço e exibir a predição:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = aks_service.run(input_data = test_sample)
print(prediction)
```

## <a name="cleanup"></a>Limpeza

Para eliminar o serviço, imagem e o modelo, utilizam o seguinte fragmento de código:

```python
aks_service.delete()
image.delete()
model.delete()
```
