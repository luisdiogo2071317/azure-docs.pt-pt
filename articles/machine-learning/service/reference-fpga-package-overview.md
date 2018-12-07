---
title: Pacote FPGA para a aceleração de hardware para o Azure Machine Learning
description: Este artigo foi preterido.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 002f521343d96ee50944428b1df9af2ce96bf88b
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53013497"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Pacote de aceleração de Hardware do Machine Learning do Azure

>[!Note]
>**Este artigo foi preterido.** Este pacote FPGA foi preterido. Foi adicionado suporte para esta funcionalidade para o SDK do Azure ML. Suporte para este pacote irá terminar a forma incremental. [Ver a linha cronológica de suporte](overview-what-happened-to-workbench.md#timeline). Saiba mais sobre atualizado [suporte FPGA](concept-accelerate-with-fpgas.md).

O pacote de aceleração de Hardware do Azure Machine Learning é uma extensão de instaláveis no pip do Python para o Azure Machine Learning, que permite aos cientistas de dados e programadores de inteligência artificial para rapidamente:

+ Caracterização de imagens com uma versão quantificada de utilizar o ResNet 50

+ Treinar classificadores com base nesses recursos

+ Implemente modelos para [campo (FPGA) de matrizes de porta programável por](concept-accelerate-with-fpgas.md) no Azure para inferência de latência ultrabaixa

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

1. Uma conta de gestão de modelos do Azure Machine Learning. Para obter mais informações sobre como criar a conta, consulte a [instalação manual de início rápido do Azure Machine Learning e Bancada de trabalho](../desktop-workbench/quickstart-installation.md) documento. 

1. O pacote tem de ser instalado. 

 
## <a name="how-to-install-the-package"></a>Como instalar o pacote

1. Transfira e instale a versão mais recente do [Git](https://git-scm.com/downloads).

2. Instalar [Anaconda (Python 3.6)](https://conda.io/miniconda.html)

   Para transferir um ambiente de Anaconda previamente configurado, utilize o comando seguinte a partir da linha do Git:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
1. Para criar o ambiente, abra um **Anaconda Prompt** e utilize o seguinte comando:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

1. Para ativar o ambiente, utilize o seguinte comando:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Código de exemplo

Este código de exemplo explica como utilizar o SDK para implementar um modelo para um FPGA.

1. Importe o pacote:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. Pré-processe a imagem:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Caracterização de imagens:
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
 
1. Prepare o modelo para serem executadas num FPGA:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Implemente o modelo para serem executadas num FPGA:
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

## <a name="reporting-issues"></a>Problemas de relatórios

Utilize o [fórum](https://aka.ms/aml-forum-service) para reportar quaisquer problemas que encontrar com o pacote.

## <a name="next-steps"></a>Passos Seguintes

[Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)
