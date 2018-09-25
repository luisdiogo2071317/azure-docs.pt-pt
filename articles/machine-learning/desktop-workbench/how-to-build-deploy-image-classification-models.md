---
title: Criar e implementar um modelo de classificação de imagem usando o pacote do Azure Machine Learning para imagem digitalizada.
description: Saiba como criar, formar, testar e implementar um imagem classificação modelo de imagem digitalizada com o pacote do Azure Machine Learning para imagem digitalizada.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 1dfcda5004dcf30c6dc112fb8150180849383016
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971574"
---
# <a name="build-and-deploy-image-classification-models-with-azure-machine-learning"></a>Criar e implementar modelos de classificação de imagens com o Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Neste artigo, saiba como utilizar o pacote do Azure Machine Learning para imagem digitalizada de computador (AMLPCV) para dar formação, testar e implementar um modelo de classificação de imagem. Para este pacote e a sua documentação de referência detalhada [ver aqui](https://aka.ms/aml-packages/vision).

Um grande número de problemas do domínio de visão do computador pode ser resolvido com a classificação de imagens. Esses problemas incluem a criação de modelos que responder a perguntas como:
+ _É um OBJETO presente na imagem? Por exemplo, "cachorro", "car", "Enviar" e assim por diante_
+ _O que a classe de severidade de doenças de olho é evinced por verificação de retinal este paciente?_

Ao criar e implementar este modelo com AMLPCV, que execute os seguintes passos:
1. Criação de conjunto de dados
2. Visualização da imagem e a anotação
3. Aumento de imagem
4. Definição de modelo de rede Neurais profundas (DNN)
5. Treinamento de classificador
6. Avaliação e visualização
7. Implementação do serviço Web
8. Serviço Web de teste de carga

[CNTK](https://www.microsoft.com/cognitive-toolkit/) é utilizado como a estrutura de aprendizagem profunda, treinamento é executado localmente num computador com a tecnologia de GPU, como o ([VM de ciência de dados de aprendizagem profunda](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), e a implementação utiliza a CLI do Azure ML Operacionalização.

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As seguintes contas e aplicação tem de ser configurados e instalados:
   - Uma conta de Experimentação do Azure Machine Learning 
   - Uma conta de gestão de modelos do Azure Machine Learning
   - O Azure Machine Learning Workbench instalado

   Se esses três são ainda não foi criados ou instalados, siga os [instalação manual de início rápido do Azure Machine Learning e Bancada de trabalho](../desktop-workbench/quickstart-installation.md) artigo. 

1. Tem de estar instalado o pacote de Aprendizado de máquina do Azure para imagem digitalizada. Saiba como [instalar este pacote aqui](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Dados de exemplo e o bloco de notas

### <a name="get-the-jupyter-notebook"></a>Obter o bloco de notas do Jupyter

Transferir o bloco de notas para executar o exemplo descrito aqui por conta própria.

> [!div class="nextstepaction"]
> [Obter o bloco de notas do Jupyter](https://aka.ms/aml-packages/vision/notebooks/image_classification)

### <a name="load-the-sample-data"></a>Carregar os dados de exemplo

O exemplo seguinte utiliza um conjunto de dados consiste em imagens de tableware 63. Cada imagem é identificada como pertencente a um dos quatro diferentes classes (bowl, cup, cutlery, prato). O número de imagens neste exemplo é pequeno para que este exemplo pode ser executado rapidamente. Na prática, devem ser fornecidas, pelo menos, 100 imagens por classe. Todas as imagens estão localizadas em *"... /sample_data/imgs_recycling / "* em subdiretórios chamados"bowl","cup","cutlery"e"prato".

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)


```python
import warnings
warnings.filterwarnings("ignore")
import json, numpy as np, os, timeit 
from azureml.logging import get_azureml_logger
from imgaug import augmenters
from IPython.display import display
from sklearn import svm
from cvtk import ClassificationDataset, CNTKTLModel, Context, Splitter, StorageContext
from cvtk.augmentation import augment_dataset
from cvtk.core.classifier import ScikitClassifier
from cvtk.evaluation import ClassificationEvaluation, graph_roc_curve, graph_pr_curve, graph_confusion_matrix
import matplotlib.pyplot as plt

from classification.notebook.ui_utils.ui_annotation import AnnotationUI
from classification.notebook.ui_utils.ui_results_viewer import ResultsUI
from classification.notebook.ui_utils.ui_precision_recall import PrecisionRecallUI

%matplotlib inline

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)
```



## <a name="create-a-dataset"></a>Criar um conjunto de dados

Assim que tiver importado as dependências e defina o contexto de armazenamento, pode criar o objeto de conjunto de dados.

Para criar esse objeto com o pacote do Azure Machine Learning para imagem digitalizada, forneça o diretório de raiz das imagens no disco local. Este diretório tem de seguir a mesma estrutura geral de como o conjunto de dados tableware, ou seja, contém subdiretórios com as imagens reais:
- raiz
    - etiqueta 1
    - etiqueta de 2
    - ...
    - etiqueta n
  
Um modelo de classificação de imagem de preparação para um conjunto de dados diferente é tão fácil como alterar o caminho de raiz `dataset_location` no código a seguir para apontar para diferentes imagens.


```python
# Root image directory
dataset_location = os.path.abspath("classification/sample_data/imgs_recycling")

dataset_name = 'recycling'
dataset = ClassificationDataset.create_from_dir(dataset_name, dataset_location)
print("Dataset consists of {} images with {} labels.".format(len(dataset.images), len(dataset.labels)))
print("Select information for image 2: name={}, label={}, unique id={}.".format(
    dataset.images[2].name, dataset.images[2]._labels[0].name, dataset.images[2]._storage_id))
```

    F1 2018-04-23 17:12:57,593 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-04-23 17:12:57,599 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Dataset consists of 63 images with 4 labels.
    Select information for image 2: name=msft-plastic-bowl20170725152154282.jpg, label=bowl, unique id=3.

O objeto de conjunto de dados fornece uma funcionalidade para transferir imagens a utilizar o [API de pesquisa de imagens do Bing](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/). 

Dois tipos de consultas de pesquisa são suportados: 
+ Consultas de texto normal
+ Consultas de URL de imagem

Estas consultas, juntamente com a etiqueta de classe devem ser fornecidas dentro de um arquivo de texto codificado em JSON. Por exemplo:

```json
{
    "bowl": [
                    "plastic bowl",
                    "../imgs_recycling/bowl"
    ],
    "cup": [
                    "plastic cup",
                    "../imgs_recycling/cup",
                    "http://cdnimg2.webstaurantstore.com/images/products/main/123662/268841/dart-solo-ultra-clear-conex-tp12-12-oz-pet-plastic-cold-cup-1000-case.jpg"
    ],
    "cutlery": [
                    "plastic cutlery",
                    "../imgs_recycling/cutlery",
                    "http://img4.foodservicewarehouse.com/Prd/1900SQ/Fineline_2514-BO.jpg"
    ],
    "plate": [
                    "plastic plate",
                    "../imgs_recycling/plate"
    ]
}
```

Além disso, tem de criar explicitamente um objeto de contexto para conter a chave de API de pesquisa de imagens do Bing. Isto requer uma subscrição de API de pesquisa de imagens do Bing.

## <a name="visualize-and-annotate-images"></a>Visualize e anotar imagens

É possível visualizar as imagens e etiquetas corretas no objeto de conjunto de dados utilizando a miniaplicação seguinte. 

Se ocorrer o erro "Não detectada o Javascript de Widget", execute este comando para resolvê-lo:
<br>`jupyter nbextension enable --py --sys-prefix widgetsnbextension`


```python
annotation_ui = AnnotationUI(dataset, Context.get_global_context())
display(annotation_ui.ui)
```

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/image_annotation.png)

## <a name="augment-images"></a>Incrementar imagens

O [ `augmentation` módulo](https://docs.microsoft.com/python/api/cvtk.augmentation) fornece funcionalidades para incrementar um objeto de conjunto de dados usando todas as transformações descritas o [imgaug](https://github.com/aleju/imgaug) biblioteca. Transformações de imagem podem ser agrupadas num único pipeline, caso em que todas as transformações no pipeline são aplicadas em simultâneo cada imagem. 

Se gostaria de aplicar os passos do aumento diferentes em separado, ou de forma diferente, pode definir vários pipelines e passá-los para o *augment_dataset* função. Para obter mais informações e exemplos de aumento de imagem, consulte a [imgaug documentação](https://github.com/aleju/imgaug).

A adição de imagens aumentadas ao conjunto de preparação é benéfico sobretudo para conjuntos de dados pequeno. Uma vez que o processo de treinamento de DNN é mais lento devido ao aumento do número de imagens de formação, recomendamos que comece experimentação sem aumento.


```python
# Split the dataset into train and test  
train_set_orig, test_set = dataset.split(train_size = 0.66, stratify = "label")
print("Number of training images = {}, test images = {}.".format(train_set_orig.size(), test_set.size()))
```

    F1 2018-04-23 17:13:01,780 INFO azureml.vision:splitter splitting a dataset 
    F1 2018-04-23 17:13:01,805 INFO azureml.vision:dataset creating dataset for scenario=classification 
    F1 2018-04-23 17:13:01,809 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Number of training images = 41, test images = 22.
    


```python
augment_train_set = False

if augment_train_set:
    aug_sequence = augmenters.Sequential([
            augmenters.Fliplr(0.5),             # horizontally flip 50% of all images
            augmenters.Crop(percent=(0, 0.1)),  # crop images by 0-10% of their height/width
        ])
    train_set = augment_dataset(train_set_orig, [aug_sequence])
    print("Number of original training images = {}, with augmented images included = {}.".format(train_set_orig.size(), train_set.size()))
else:
    train_set = train_set_orig  
```

## <a name="define-dnn-models"></a>Definir modelos de DNN

Os seguintes modelos de rede Neural profunda pretrained são suportados com este pacote: 
+ Utilizar o Resnet-18
+ Utilizar o Resnet-34
+ Utilizar o Resnet-50
+ Utilizar o Resnet-101
+ Utilizar o Resnet-152

Estes DNNs podem ser utilizados como classificador, ou como featurizer. 

Podem encontrar mais informações sobre as redes [aqui](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md), e é uma introdução básica a aprendizagem de transferência [aqui](https://blog.slavv.com/a-gentle-intro-to-transfer-learning-2c0b674375a0).

Os parâmetros de classificação de imagem padrão para este pacote são a resolução de pixel de 224 x 224 e um DNN de utilizar o Resnet-18. Estes parâmetros foram selecionados para funcionar melhor numa grande variedade de tarefas. Precisão pode, muitas vezes, ser aperfeiçoada, por exemplo, ao aumentar a resolução da imagem para 500 x 500 pixels, e/ou selecionar um modelo mais profundo (utilizar o Resnet-50). No entanto, alteração dos parâmetros têm um aumento significativo no tempo de treinamento. Consulte o artigo sobre [como melhorar a precisão](https://docs.microsoft.com/azure/machine-learning/service/how-to-improve-accuracy-for-computer-vision-models).


```python
# Default parameters (224 x 224 pixels resolution, Resnet-18)
lr_per_mb = [0.05]*7 + [0.005]*7 +  [0.0005]
mb_size = 32
input_resoluton = 224
base_model_name = "ResNet18_ImageNet_CNTK"

# Suggested parameters for 500 x 500 pixels resolution, Resnet-50
# (see in the Appendix "How to improve accuracy", last row in table)
# lr_per_mb   = [0.01] * 7 + [0.001] * 7 + [0.0001]
# mb_size    = 8
# input_resoluton = 500
# base_model_name = "ResNet50_ImageNet_CNTK"

# Initialize model
dnn_model = CNTKTLModel(train_set.labels,
                       base_model_name=base_model_name,
                       image_dims = (3, input_resoluton, input_resoluton))
```

    Successfully downloaded ResNet18_ImageNet_CNTK
    

## <a name="train-the-classifier"></a>Treinar o classificador

Pode escolher um dos seguintes métodos para o DNN com formação prévia.

  - **Refinamento de DNN**, que prepara o DNN para executar a classificação diretamente. Enquanto o treinamento de DNN estiver lento, isso geralmente leva aos melhores resultados, uma vez que todos os pesos de rede podem ser melhorados durante o treinamento para dar maior precisão.

  - **DNN featurization**, que é executada DNN como-é obter uma representação inferior-dimensional de uma imagem (512, 2048 ou 4096 flutua). Essa representação, em seguida, é utilizada como entrada para treinar um classificador separado. Uma vez que o DNN é mantido inalterado, essa abordagem é muito mais rápido em comparação com Refinamento de DNN, no entanto, não é tão bom precisão. No entanto, treinar um classificador externo, como um SVM linear (como mostrado no código a seguir) pode fornecer uma linha de base segura e ajudam a compreender a viabilidade de um problema.
  
TensorBoard pode ser utilizado para visualizar o curso de treinamento. Para ativar TensorBoard:
1. Adicione o parâmetro `tensorboard_logdir=PATH` conforme mostrado no código a seguir
1. Iniciar o cliente de TensorBoard utilizando o comando `tensorboard --logdir=PATH` numa nova consola.
1. Abra um browser, conforme indicado pelo TensorBoard, que, por predefinição é localhost:6006. 


```python
# Train either the DNN or a SVM as classifier 
classifier_name = "dnn"

if classifier_name.lower() == "dnn":  
    dnn_model.train(train_set, lr_per_mb = lr_per_mb, mb_size = mb_size, eval_dataset=test_set) #, tensorboard_logdir=r"tensorboard"
    classifier = dnn_model
elif classifier_name.lower() == "svm":
    learner = svm.LinearSVC(C=1.0, class_weight='balanced', verbose=0)
    classifier = ScikitClassifier(dnn_model, learner = learner)
    classifier.train(train_set)
else:
    raise Exception("Classifier unknown: " + classifier)   
```

    F1 2018-04-23 17:13:28,238 INFO azureml.vision:Fit starting in experiment  1541466320 
    F1 2018-04-23 17:13:28,239 INFO azureml.vision:model starting training for scenario=classification 
    <class 'int'>
    1 worker
    Training transfer learning model for 15 epochs (epoch_size = 41).
    non-distributed mode
    Training 15741700 parameters in 53 parameter tensors.
    Training 15741700 parameters in 53 parameter tensors.
    Learning rate per minibatch: 0.05
    Momentum per minibatch: 0.9
    PROGRESS: 0.00%
    Finished Epoch[1 of 15]: [Training] loss = 2.820586 * 41, metric = 68.29% * 41 5.738s (  7.1 samples/s);
    Evaluation Set Error :: 29.27%
    Finished Epoch[2 of 15]: [Training] loss = 0.286728 * 41, metric = 9.76% * 41 0.752s ( 54.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[3 of 15]: [Training] loss = 0.206938 * 41, metric = 4.88% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 41.46%
    Finished Epoch[4 of 15]: [Training] loss = 0.098931 * 41, metric = 2.44% * 41 0.785s ( 52.2 samples/s);
    Evaluation Set Error :: 48.78%
    Finished Epoch[5 of 15]: [Training] loss = 0.046547 * 41, metric = 0.00% * 41 0.724s ( 56.6 samples/s);
    Evaluation Set Error :: 43.90%
    Finished Epoch[6 of 15]: [Training] loss = 0.059709 * 41, metric = 4.88% * 41 0.636s ( 64.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[7 of 15]: [Training] loss = 0.005817 * 41, metric = 0.00% * 41 0.710s ( 57.7 samples/s);
    Evaluation Set Error :: 14.63%
    Learning rate per minibatch: 0.005
    Finished Epoch[8 of 15]: [Training] loss = 0.014917 * 41, metric = 0.00% * 41 0.649s ( 63.2 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[9 of 15]: [Training] loss = 0.040539 * 41, metric = 2.44% * 41 0.777s ( 52.8 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[10 of 15]: [Training] loss = 0.024606 * 41, metric = 0.00% * 41 0.626s ( 65.5 samples/s);
    Evaluation Set Error :: 7.32%
    PROGRESS: 0.00%
    Finished Epoch[11 of 15]: [Training] loss = 0.004225 * 41, metric = 0.00% * 41 0.656s ( 62.5 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[12 of 15]: [Training] loss = 0.004364 * 41, metric = 0.00% * 41 0.702s ( 58.4 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[13 of 15]: [Training] loss = 0.007974 * 41, metric = 0.00% * 41 0.721s ( 56.9 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[14 of 15]: [Training] loss = 0.000655 * 41, metric = 0.00% * 41 0.711s ( 57.7 samples/s);
    Evaluation Set Error :: 4.88%
    Learning rate per minibatch: 0.0005
    Finished Epoch[15 of 15]: [Training] loss = 0.024865 * 41, metric = 0.00% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 4.88%
    Stored trained model at ../../../cvtk_output\model_trained\ImageClassification.model
    F1 2018-04-23 17:13:45,097 INFO azureml.vision:Fit finished in experiment  1541466320 
    


```python
# Plot how the training and test accuracy increases during gradient descent. 
if classifier_name == "dnn":
    [train_accs, test_accs, epoch_numbers] = classifier.train_eval_accs
    plt.xlabel("Number of training epochs") 
    plt.ylabel("Classification accuracy") 
    train_plot = plt.plot(epoch_numbers, train_accs, 'r-', label = "Training accuracy")
    test_plot = plt.plot(epoch_numbers, test_accs, 'b-.', label = "Test accuracy")
    plt.legend()
```

![PNG](media/how-to-build-deploy-image-classification-models/output_17_0.png)


## <a name="evaluate-and-visualize-model-performance"></a>Avaliar e visualize o desempenho do modelo

Pode avaliar o desempenho do modelo preparado num conjunto de dados independente de teste utilizando o módulo de avaliação. Algumas das métricas de avaliação computa incluem:
 
+ Precisão (por predefinição a média de classe)
+ Curva de PR
+ Curva cor MULTICLASSE
+ Área em curva
+ Matriz de confusão


```python
# Run the classifier on all test set images
ce = ClassificationEvaluation(classifier, test_set, minibatch_size = mb_size)

# Compute Accuracy and the confusion matrix
acc = ce.compute_accuracy()
print("Accuracy = {:2.2f}%".format(100*acc))
cm  = ce.compute_confusion_matrix()
print("Confusion matrix = \n{}".format(cm))

# Show PR curve, ROC curve, and confusion matrix
fig, ((ax1, ax2, ax3)) = plt.subplots(1,3)
fig.set_size_inches(18, 4)
graph_roc_curve(ce, ax=ax1)
graph_pr_curve(ce, ax=ax2)
graph_confusion_matrix(ce, ax=ax3)
plt.show()
```

    F1 2018-04-23 17:14:37,449 INFO azureml.vision:evaluation doing evaluation for scenario=classification 
    F1 2018-04-23 17:14:37,450 INFO azureml.vision:model scoring dataset for scenario=classification 
    Accuracy = 95.45%
    Confusion matrix = 
    [[ 0  1  0  1]
     [ 0  7  0  0]
     [ 0  0  2  0]
     [ 0  0  0 11]]
    


![PNG](media/how-to-build-deploy-image-classification-models/output_20_1.png)



```python
# Results viewer UI
labels = [l.name for l in dataset.labels] 
pred_scores = ce.scores #classification scores for all images and all classes
pred_labels = [labels[i] for i in np.argmax(pred_scores, axis=1)]

results_ui = ResultsUI(test_set, Context.get_global_context(), pred_scores, pred_labels)
display(results_ui.ui)
```

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/Image_Classification_Results.png)


```python
# Precision / recall curve UI
precisions, recalls, thresholds = ce.compute_precision_recall_curve() 
thresholds = list(thresholds)
thresholds.append(thresholds[-1])
pr_ui = PrecisionRecallUI(100*precisions[::-1], 100*recalls[::-1], thresholds[::-1])
display(pr_ui.ui) 
```

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/image_precision_curve.png)

## <a name="operationalization-deploy-and-consume"></a>Operacionalização: implementar e consumir

Operacionalização é o processo de publicação de modelos e código como serviços da web e o consumo de um destes serviços para produzir resultados de negócios. 

Assim que é preparado o modelo, pode implementar esse modelo como um serviço web para a utilização de consumo [da CLI do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Os modelos podem ser implementados em seu computador local ou o cluster do Azure Container Service (ACS). Usando o ACS, pode dimensionar manualmente o seu serviço web ou utilizar a funcionalidade de dimensionamento automático.

**Inicie sessão com a CLI do Azure**

Utilizar um [Azure](https://azure.microsoft.com/) conta com uma subscrição válida, inicie sessão com o seguinte comando da CLI:
<br>`az login`

+ Para mudar para outra subscrição do Azure, utilize o comando:
<br>`az account set --subscription [your subscription name]`

+ Para ver a conta de gestão de modelo atual, utilize o comando:
  <br>`az ml account modelmanagement show`

**Criar e definir o seu ambiente de implementação de cluster**

Apenas terá de definir o seu ambiente de implantação uma vez. Se ainda não tiver uma, configurar o ambiente de implantação agora usando [estas instruções](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Para ver o seu ambiente de implantação do Active Directory, utilize o seguinte comando da CLI:
<br>`az ml env show`
   
Comando da CLI do Azure de exemplo para criar e definir o ambiente de implantação

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Gerir implementações e serviços da web

As seguintes APIs pode ser utilizadas para implementar modelos como serviços da web, gerir os serviços da web e gerir implementações.

|Tarefa|API|
|----|----|
|Criar o objeto de implementação|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Implementar o serviço web|`deploy_obj.deploy()`|
|Imagem de pontuação|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Eliminar serviço web|`deploy_obj.delete()`|
|Criar a imagem do docker sem o serviço web|`deploy_obj.build_docker_image()`|
|Lista de implementação existente|`AMLDeployment.list_deployment()`|
|Eliminar se o serviço existe com o nome da implementação|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**Documentação da API:** consulte a [documentação de referência do pacote](https://aka.ms/aml-packages/vision) para obter a referência detalhada para cada módulo e uma classe.

**Referência da CLI:** para mais avançada operações relacionadas com a implantação, consulte a [referência da CLI de gestão de modelos](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Gestão de implementação no portal do Azure**: pode controlar e gerir as implementações no [portal do Azure](https://ms.portal.azure.com/). No portal do Azure, encontre a sua página de conta de gestão de modelos do Machine Learning usa seu nome. Em seguida, vá para a página de conta de gestão de modelos > Gestão de modelos > serviços.


```python
# ##### OPTIONAL###### 
# Interactive CLI setup helper, including model management account and deployment environment.
# If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
#
# UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT
#
# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
# # Optional. Persist your model on disk and reuse it later for deployment. 
# from cvtk import TFFasterRCNN, Context
# import os
# save_model_path = os.path.join(Context.get_global_context().storage.persistent_path, "saved_classifier.model")
# # Save model to disk
# dnn_model.serialize(save_model_path)
# # Load model from disk
# dnn_model = CNTKTLModel.deserialize(save_model_path)
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Optional Azure Machine Learning deployment cluster name (environment name) and resource group name
# If you don't provide here. It will use the current deployment environment (you can check with CLI command "az ml env show").
azureml_rscgroup = "<resource group>"
cluster_name = "<cluster name>"

# If you provide the cluster information, it will use the provided cluster to deploy. 
# Example: deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Create deployment object
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=dnn_model, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# Create the web service
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Consumir o serviço web 

Depois de implementar o modelo como um serviço web, pode classificar imagens com o serviço web usando um dos seguintes métodos:

- Pontuação do serviço web diretamente com o objeto de implementação que utilizar `deploy_obj.score_image(image_path_or_url)`

- Utilize a chave de serviço e o URL de ponto final de serviço (nenhum para a implementação local): `AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

- Forma os pedidos de HTTP diretamente para o ponto de final de serviço web de pontuação. Esta opção é para os utilizadores avançados.

### <a name="score-with-existing-deployment-object"></a>Com o objeto de implementação existente de pontuação

```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url with added paramters. Add softmax to score.
print("Score image url with added paramters. Add softmax to score")
from cvtk.utils.constants import ClassificationRESTApiParamters
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224], parameters={ClassificationRESTApiParamters.ADD_SOFTMAX:True})
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Time image scoring
import timeit

for img_index, img_obj in enumerate(test_set.images[:10]):
    print("Calling API for image {} of {}: {}...".format(img_index, len(test_set.images), img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
    print(return_json)
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Classificar pelo url do ponto final de serviço e a chave de serviço

`AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "" # please replace with your own service url
service_key = "" # please replace with your own service key
# score local image with file path
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key)
print("serialized_result_in_json:", serialized_result_in_json)

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```

### <a name="score-endpoint-with-http-request-directly"></a>Ponto final de pontuação com diretamente a solicitação de http

O seguinte código de exemplo faz o pedido HTTP diretamente em Python. No entanto, pode fazê-lo em outras linguagens de programação.


```python
def score_image_list_with_http(images, service_endpoint_url, service_key=None, parameters={}):
    """Score image list with http request

    Args:
        images(list): list of (input image file path, base64 image string, url or buffer)
        service_endpoint_url(str): endpoint url
        service_key(str): service key, None for local deployment.
        parameters(dict): service additional paramters in dictionary


    Returns:
        result (list): list of serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    routing_id = ""

    if service_key is None:
        headers = {'Content-Type': 'application/json',
                   'X-Marathon-App-Id': routing_id}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key), 'X-Marathon-App-Id': routing_id}
    payload = []
    for image in images:
        encoded = None
        # read image
        with open(image,'rb') as f:
            image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        # convert your image to base64 string
        encoded = base64.b64encode(image_buffer.getvalue())
        image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
        payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result

# Test with images
images = [test_set.images[0].storage_path, test_set.images[1].storage_path] # A list of local image files
score_image_list_with_http(images, service_endpoint_url, service_key)
```

### <a name="parse-serialized-result-from-web-service"></a>Analisar o resultado serializado de serviço da web

A saída do serviço web é uma cadeia de caracteres do JSON. Pode analisar essa cadeia de caracteres do JSON com diferentes classes de modelo DNN.


```python
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Parse result from json string
import numpy as np
parsed_result = CNTKTLModel.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
# Map result to image class
class_index = np.argmax(np.array(parsed_result))
print("Class index:", class_index)
dnn_model.class_map
print("Class label:", dnn_model.class_map[class_index])
```


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o pacote do Azure Machine Learning para imagem digitalizada nestes artigos:

+ Saiba como [melhorar a precisão desse modelo](how-to-improve-accuracy-for-computer-vision-models.md).

+ Leitura a [descrição geral do pacote](https://aka.ms/aml-packages/vision).

+ Explore os [documentação de referência](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) para este pacote.

+ Saiba mais sobre [outros pacotes de Python para o Azure Machine Learning](reference-python-package-overview.md).
