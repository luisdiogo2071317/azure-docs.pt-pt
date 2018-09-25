---
title: ONNX e do Azure Machine Learning | Criar e implementar modelos
description: Saiba mais sobre ONNX e como utilizar o Azure Machine Learning para criar e implementar modelos ONNX
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 09/24/2018
ms.openlocfilehash: cc259e89bf6e4bcde5635d13951eaedb6f7b8849
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035016"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX e o Azure Machine Learning: crie e implemente modelos de IA interoperáveis

O [abra Exchange de rede Neural](http://onnx.ai) formato (ONNX) é um padrão aberto para que representa os modelos de aprendizagem automática. ONNX é suportado por um [Comunidade de parceiros](http://onnx.ai/supported-tools), incluindo a Microsoft, que criam compatíveis estruturas e ferramentas. Microsoft está empenhada em IA aberta e interoperável, para que os cientistas de dados e os programadores podem:

+ Usar a estrutura de sua preferência para criar e formar modelos
+ Implementar modelos para várias plataformas com o trabalho de integração mínima

A Microsoft suporta ONNX em seus produtos, incluindo o Azure e do Windows para o ajudar a alcançar essas metas.  

## <a name="why-choose-onnx"></a>Por que escolher ONNX?
A interoperabilidade que obtém com ONNX torna possível a boas ideias para a produção mais rapidamente. Com ONNX, cientistas de dados podem escolher a sua estrutura preferencial para a tarefa. Da mesma forma, os desenvolvedores podem gastar menos tempo a preparar modelos para produção e implemente na cloud e no edge.  

Pode exportar modelos ONNX a partir de muitas estruturas, incluindo PyTorch, Chainer, Microsoft Cognitive Toolkit (CNTK), MXNet e ML.Net. Conversores existem para outras estruturas, como o TensorFlow, Keras, SciKit-saiba e muito mais.

Também existe um ecossistema de ferramentas para a visualização e acelerando modelos ONNX. Um número de modelos ONNX a pré-preparadas também está disponível para cenários comuns.

[Podem ser implementados modelos ONNX](#deploy) na cloud com o Azure Machine Learning e o tempo de execução ONNX. Eles também podem ser implementados para dispositivos Windows 10 com [Windows ML](https://docs.microsoft.com/windows/ai/). Pode até mesmo ser implementados para outras plataformas usando conversores que estão disponíveis da Comunidade ONNX. 

[ ![Diagrama de fluxo ONNX mostrando treinamento, conversores e implementação](media/concept-onnx/onnx.png) ] (. / media/concept-onnx/onnx.png#lightbox)

## <a name="create-onnx-models-in-azure"></a>Criar modelos ONNX no Azure

Pode criar modelos ONNX de várias formas:
+ Preparar um modelo no serviço Azure Machine Learning e converter ou exportá-la para ONNX (veja o exemplo na parte inferior deste artigo)

+ Obter um modelo de ONNX com formação prévia do [ONNX Zoo de modelo](https://github.com/onnx/models)

+ Gerar um modelo de ONNX personalizado do [serviço de visão de personalizado do Azure](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/)

## <a name="exportconvert-your-models-to-onnx"></a>Exportação/converter seus modelos para ONNX

Também pode converter seus modelos existentes para ONNX.
+ Para **PyTorch** modelos, experimente [este bloco de notas do Jupyter](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)

+ Para **Microsoft Cognitive Toolkit (CNTK)** modelos, experimente [este bloco de notas do Jupyter](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)

+ Para **Chainer** modelos, experimente [este bloco de notas do Jupyter](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)

+ Para **MXNet** modelos, experimente [este bloco de notas do Jupyter](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)

+ Para **TensorFlow** utilizar modelos, o [tensorflow onnx conversor](https://github.com/onnx/tensorflow-onnx).

+ Para **Keras**, **ScitKit de aprender**, **CoreML**, **XGBoost**, e **libSVM** modelos, converter ONNX utilizar o [WinMLTools](https://docs.microsoft.com/windows/ai/convert-model-winmltools) pacote.

Pode encontrar a lista mais recente de arquiteturas suportadas e conversores no [site de tutoriais ONNX](https://github.com/onnx/tutorials).

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>Implementar modelos ONNX no Azure

Com o serviço do Azure Machine Learning, pode implementar, gerir e monitorizar os seus modelos ONNX. Através da norma [fluxo de trabalho de implantação](concept-model-management-and-deployment.md) e o tempo de execução ONNX, pode criar um ponto final REST alojado na cloud. Ver um bloco de notas do Jupyter do exemplo completo no final deste artigo para experimentar por conta própria. 

### <a name="install-and-configure-the-onnx-runtime"></a>Instalar e configurar o tempo de execução ONNX

O tempo de execução ONNX são um motor de inferência de tipos de alto desempenho para modelos ONNX. Ele vem com uma API de Python e fornece a aceleração de hardware na CPU e GPU. Atualmente ele oferece suporte a modelos ONNX 1.2 e é executado no Ubuntu 16.04 Linux.

Para instalar o tempo de execução ONNX, utilize:
```python
pip install onnxruntime
```

Para chamar o tempo de execução ONNX no seu script de Python, utilize:
```python
import onnxruntime

session = onnxruntime.InferenceSession("path to model")
```

A documentação que acompanha o modelo geralmente indica as entradas e saídas para utilizar o modelo. Também pode utilizar como uma ferramenta de visualização [Netron](https://github.com/lutzroeder/Netron) para ver o modelo. Tempo de execução ONNX também permite-lhe os metadados do modelo de consulta, entradas e saídas:
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

A inferência de seu modelo, utilize `run` e passar na lista de saídas quer devolvidas (deixe vazio se pretender que todos eles) e um mapa dos valores introduzidos. O resultado é uma lista das saídas.
```python
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Para obter a referência de API completa, consulte a [documentos de referência de tempo de execução ONNX](https://aka.ms/onnxruntime-python).

### <a name="example-deployment-steps"></a>Passos de implementação de exemplo

Eis um exemplo para implementar um modelo ONNX:

1. Inicialize de área de trabalho de aprendizagem do Azure. Se ainda não tiver uma, saiba como criar uma área de trabalho [este guia de introdução](quickstart-get-started.md).

   ```python
   from azureml.core import Workspace

   ws = Workspace.from_config()
   print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
   ```

2. Registe o modelo com o Azure Machine Learning.

   ```python
   from azureml.core.model import Model

   model = Model.register(model_path = "model.onnx",
                          model_name = "MyONNXmodel",
                          tags = ["onnx"],
                          description = "test",
                          workspace = ws)
   ```

3. Crie uma imagem com o modelo e todas as dependências.

   ```python
   from azureml.core.image import ContainerImage
   
   image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                     runtime = "python",
                                                     conda_file = "myenv.yml",
                                                     description = "test",
                                                     tags = ["onnx"]
                                                    )

   image = ContainerImage.create(name = "myonnxmodelimage",
                                 # this is the model object
                                 models = [model],
                                 image_config = image_config,
                                 workspace = ws)

   image.wait_for_creation(show_output = True)
   ```

   O ficheiro `score.py` contém a lógica de classificação e precisa ser incluído na imagem. Este ficheiro é utilizado para executar o modelo na imagem. Ver isso [tutorial](tutorial-deploy-models-with-aml.md#create-scoring-script) para obter instruções sobre como criar uma classificação de script. Um exemplo de ficheiro para um modelo ONNX é mostrado abaixo:

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global model_path
       model_path = Model.get_model_path(model_name = 'MyONNXmodel')

   def run(raw_data):
       try:
           data = json.loads(raw_data)['data']
           data = np.array(data)
        
           sess = onnxruntime.InferenceSession(model_path)
           result = sess.run(["outY"], {"inX": data})
        
           return json.dumps({"result": result.tolist()})
       except Exception as e:
           result = str(e)
           return json.dumps({"error": result})
   ```

   O ficheiro `myenv.yml` descreve as dependências necessárias para a imagem. Ver isso [tutorial](tutorial-deploy-models-with-aml.md#create-environment-file) para obter instruções sobre como criar um arquivo de ambiente, como este ficheiro de exemplo:

   ```
   name: myenv
   channels:
     - defaults
   dependencies:
     - pip:
       - onnxruntime
       - azureml-core
   ```

4. Implemente o seu modelo ONNX com o Azure Machine Learning para:
   + Azure Container Instances (ACI): [saber como...](how-to-deploy-to-aci.md)

   + Serviço Kubernetes do Azure (AKS): [saber como...](how-to-deploy-to-aks.md)


## <a name="examples"></a>Exemplos
 
Os blocos de notas seguintes demonstram como implementar modelos ONNX com o Azure Machine Learning: 
+ `/onnx/onnx-inference-mnist.ipynb`
 
+ `/onnx/onnx-inference-emotion-recognition.ipynb`
 
Obter este bloco de notas:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Mais informações

Saiba mais sobre ONNX ou contribuir para o projeto:
+ [Site de projeto ONNX](http://onnx.ai)

+ [Código ONNX no GitHub](https://github.com/onnx/onnx)
