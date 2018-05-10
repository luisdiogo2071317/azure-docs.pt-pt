---
title: Pacote FPGA para a aceleração de hardware para o Azure Machine Learning
description: Saiba mais sobre os pacotes de python disponíveis para utilizadores do Azure Machine Learning.
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: routlaw
author: rloutlaw
ms.date: 05/07/2018
ms.openlocfilehash: 905f6943470acfd3051a33db6f6f3269470406d7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Pacote de aceleração de Hardware do Machine Learning do Azure

O pacote de aceleração de Hardware do Azure Machine Learning é uma extensão de pip instalável do Python para o Azure Machine Learning que permite a cientistas de dados e aos programadores AI rapidamente:

+ Imagens de Featurize com uma versão quantized do ResNet 50

+ Preparar os classificadores com base nessas funcionalidades

+ Implementar modelos para [campo matrizes de porta programável (FPGA)](concept-accelerate-with-fpgas.md) no Azure para inferencing ultra-baixa latência

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. Tem de criar uma conta de gestão de modelo do Azure Machine Learning. Para obter mais informações sobre como criar a conta, consulte o [instalação de início rápido do Azure Machine Learning e Workbench](../service/quickstart-installation.md) documento. 

1. O pacote tem de estar instalado. 

 
## <a name="how-to-install-the-package"></a>Como instalar o pacote

1. Transfira e instale a versão mais recente do [Git](https://git-scm.com/downloads).

2. Instalar [Anaconda (Python 3.6)](https://conda.io/miniconda.html)

3. Para transferir um ambiente de Anaconda pré-configurado, utilize o seguinte comando da linha de comandos do Git:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
5. Para criar o ambiente, abra uma **Anaconda Prompt** e utilize o seguinte comando:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

6. Para ativar o ambiente, utilize o seguinte comando:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Código de exemplo

Este código de exemplo orienta utilizando o SDK para implementar um modelo para um FPGA.

1. Importe o pacote:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. Pré-processo de a imagem:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Featurize as imagens:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Crie um classificador:
   ```python
   model.import_graph_def(include_featurizer=False)
   print(model.classifier_input)
   print(model.classifier_output)
   ```

1. Crie a definição de serviço:
   ```python
   from amlrealtimeai.pipeline import ServiceDefinition, TensorflowStage, BrainWaveStage
   save_path = os.path.expanduser('~/models/save')
   service_def_path = os.path.join(save_path, 'service_def.zip')

   service_def = ServiceDefinition()
   service_def.pipeline.append(TensorflowStage(tf.Session(), in_images, image_tensors))
   service_def.pipeline.append(BrainWaveStage(model))
   service_def.pipeline.append(TensorflowStage(tf.Session(), model.classifier_input, model.classifier_output))
   service_def.save(service_def_path)
   print(service_def_path)
   ```
 
1. Prepare o modelo com um FPGA:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Implemente o modelo com um FPGA:
   ```python
   service = deployment_client.get_service_by_name(service_name)
   model_id = deployment_client.register_model(model_name, service_def_path)

   if(service is None):
      service = deployment_client.create_service(service_name, model_id)    
   else:
      service = deployment_client.update_service(service.id, model_id)
   ```

1. Crie o cliente:
    ```python
   from amlrealtimeai import PredictionClient
   client = PredictionClient(service.ipAddress, service.port)  
   ```

1. Chame a API:
   ```python
   image_file = R'C:\path_to_file\image.jpg'
   results = client.score_image(image_file)
   ```

## <a name="reporting-issues"></a>Relatórios de problemas

Utilize o [fórum](https://aka.ms/aml-forum) para reportar a quaisquer problemas que encontrar com o pacote.

## <a name="next-steps"></a>Passos Seguintes

[Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)