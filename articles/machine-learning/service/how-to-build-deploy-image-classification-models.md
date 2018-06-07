---
title: Criar e implementar um modelo de classificação de imagem utilizando o pacote do Azure Machine Learning para problemas de visão do computador.
description: Saiba como criar, dar formação, testar e implementar um modelo de classificação do imagem do computador visão utilizando o pacote do Azure Machine Learning para problemas de visão do computador.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: 72f5215bac9254c9e3295b2cade7b6d44d516af6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34637740"
---
# <a name="build-and-deploy-image-classification-models-with-azure-machine-learning"></a>Criar e implementar modelos de classificação de imagem com o Azure Machine Learning

Neste artigo, saiba como utilizar **Azure Machine Learning pacote para computador visão** (AMLPCV) para dar formação, testar e implementar um modelo de classificação de imagem. 

Um grande número de problemas no domínio de visão computador possam ser resolvido através da classificação de imagem. Estes problemas incluem a criação de modelos que responda às perguntas, tais como:
+ _Não existe um OBJETO de imagem? Por exemplo, "preguiçoso", "carro", "são enviados" e assim sucessivamente_
+ _Que tipo de gravidade de disease olho é evinced pela análise retinal este patient?_

Quando criar e implementar este modelo com AMLPCV, leia os seguintes passos:
1. Criação de conjunto de dados
2. Visualização de imagem e anotação
3. Imagem Augmentation
4. Definição de modelo de rede neuronal profundo (DNN)
5. Classificador formação
6. Avaliação e visualização
7. Implementação de serviço Web
8. Serviço Web do teste de carga

[CNTK](https://www.microsoft.com/cognitive-toolkit/) é utilizado como o framework de aprendizagem profunda, formação é executada localmente numa máquina GPU ligada, tal como a ([profunda de VM de ciência de dados de aprendizagem](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), e a implementação utilizar a CLI do Azure ML Operationalization.

Consulte o [documentação de referência do pacote](https://aka.ms/aml-packages/vision) para a referência de detalhado para cada módulo e uma classe.

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As seguintes contas e aplicação tem de ser configurados e instalados:
   - Uma conta de Experimentação do Azure Machine Learning 
   - Uma conta de gestão de modelo do Azure Machine Learning
   - O Azure Machine Learning Workbench instalado

   Se estes três são ainda não foi criadas ou instaladas, siga o [instalação de início rápido do Azure Machine Learning e Workbench](../service/quickstart-installation.md) artigo. 

1. O pacote de aprendizagem máquina do Azure para problemas de visão do computador tem de estar instalado. Saiba como [instalar este pacote aqui](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Bloco de notas e dados de exemplo

### <a name="get-the-jupyter-notebook"></a>Obter o bloco de notas do Jupyter

Transferir o bloco de notas para executar o exemplo descrito aqui por si.

> [!div class="nextstepaction"]
> [Obter o bloco de notas do Jupyter](https://aka.ms/aml-packages/vision/notebooks/image_classification)

### <a name="load-the-sample-data"></a>Carregar os dados de exemplo

O exemplo seguinte utiliza um conjunto de dados constituída por 63 imagens de tableware. Cada imagem é identificada como pertencentes a um dos quatro diferentes classes (bowl cup, cutlery, plate). O número de imagens neste exemplo é pequeno para que este exemplo pode ser executado rapidamente. Na prática, devem ser fornecidas, pelo menos, 100 imagens por classe. Todas as imagens estão localizadas em *".. /sample_data/imgs_recycling / "* no subdiretórios chamados"bowl","cup","cutlery"e"plate".

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="storage-context"></a>Contexto de armazenamento

O contexto de armazenamento é utilizado para determinar onde serão armazenados vários ficheiros de saída, tais como imagens aumentados ou ficheiros de modelo DNN. Para obter mais informações sobre contextos de armazenamento, consulte o [StorageContext documentação](https://review.docs.microsoft.com/en-us/python/api/cvtk.core.context.storagecontext?view=azure-python&branch=smoke-test). 

Normalmente, os conteúdos de armazenamento não necessitam de ser definida explicitamente. No entanto, para evitar o limite de 25 MB no tamanho projeto imposto o Workbench do Azure Machine Learning, defina o diretório de saídas para o pacote do Azure Machine Learning visão de computador para uma localização fora do projecto de Azure Machine Learning (".. /.. /.. /.. / cvtk_output "). Não se esqueça de remover o diretório de "cvtk_output" uma vez que já não é necessária.


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
from cvtk.evaluation import ClassificationEvaluation, graph_roc_curve, graph_pr_curve, graph_confusion_matrix, basic_plot
import matplotlib.pyplot as plt
%matplotlib inline

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

# Set storage context.
out_root_path = "../../../cvtk_output"
Context.create(outputs_path=out_root_path, persistent_path=out_root_path, temp_path=out_root_path)
```




    {
        "storage": {
            "outputs_path": "../../../cvtk_output",
            "persistent_path": "../../../cvtk_output",
            "temp_path": "../../../cvtk_output"
        }
    }



## <a name="create-a-dataset"></a>Criar um conjunto de dados

Depois de ter importado as dependências e definir o contexto de armazenamento, pode criar o objeto do conjunto de dados.

Para criar esse objeto com o pacote do Azure Machine Learning para problemas de visão do computador, forneça o diretório de raiz das imagens no disco local. Este diretório tem de seguir a mesma estrutura geral de como o conjunto de dados tableware, ou seja, conter subdiretórios com as imagens reais:
- raiz
    - etiqueta 1
    - etiqueta de 2
    - ...
    - etiqueta n
  
Um modelo de classificação de imagem de preparação para um conjunto de dados diferentes é tão fácil como alterar o caminho raiz `dataset_location` no seguinte código para apontarem imagens diferentes.


```python
# Root image directory 
dataset_location = os.path.abspath(os.path.join(os.getcwd(), "../sample_data/imgs_recycling"))

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

O objeto do conjunto de dados fornece funcionalidades para transferir imagens a utilizar o [API de pesquisa do Bing imagem](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/). 

São suportados dois tipos de consultas de pesquisa: 
+ Consultas de texto regular
+ Consultas de URL da imagem

Estas consultas, juntamente com a etiqueta de classe tem de ser fornecidas no interior de um ficheiro de texto codificada em JSON. Por exemplo:

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

Além disso, tem de criar explicitamente um objeto de contexto para conter a chave de API de pesquisa do Bing imagem. Isto requer uma subscrição de API de pesquisa do Bing imagem.

## <a name="visualize-and-annotate-images"></a>Visualizar e anotar imagens

Pode visualizar as imagens e etiquetas corretas no objeto do conjunto de dados com a miniaplicação seguinte. 

Se ocorrer o erro "O Javascript Widget não detetado", execute este comando para resolver este:
<br>`jupyter nbextension enable --py --sys-prefix widgetsnbextension`


```python
from ui_utils.ui_annotation import AnnotationUI
annotation_ui = AnnotationUI(dataset, Context.get_global_context())
display(annotation_ui.ui)
```

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/image_annotation.png)

## <a name="augment-images"></a>Aumentar imagens

O [ `augmentation` módulo](https://docs.microsoft.com/en-us/python/api/cvtk.augmentation) fornece funcionalidades para aumentar um objeto de conjunto de dados com todas as transformações descritas a [imgaug](https://github.com/aleju/imgaug) biblioteca. Transformações de imagem podem ser agrupadas num único pipeline, caso em que todas as transformações no pipeline são aplicadas em simultâneo cada imagem. 

Se gostaria de aplicar os passos de augmentation diferentes em separado, ou qualquer outra forma, pode definir vários pipelines e transmita-los para o *augment_dataset* função. Para obter mais informações e exemplos de augmentation de imagem, consulte o [imgaug documentação](https://github.com/aleju/imgaug).

A adição de imagens aumentadas ao conjunto de preparação é especialmente vantajoso para conjuntos de dados pequenos. Uma vez que o processo de formação DNN mais lento devido ao aumento do número de imagens de formação, recomendamos que comece experimentação sem augmentation.


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

São suportados os seguintes modelos de rede neuronal profunda pretrained com este pacote: 
+ Resnet 18
+ Resnet 34
+ Resnet 50
+ Resnet 101
+ Resnet 152

Estes DNNs podem ser utilizadas como classificador ou como featurizer. 

Podem encontrar mais informações sobre as redes [aqui](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md), não sendo uma introdução básica a transferência de aprendizagem [aqui](https://blog.slavv.com/a-gentle-intro-to-transfer-learning-2c0b674375a0).

Os parâmetros de classificação de imagem predefinido para este pacote são resolução de pixel 224 x 224 e um DNN Resnet-18. Estes parâmetros foram selecionados para funcionar melhor numa ampla variedade de tarefas. Precisão pode, muitas vezes, ser melhorada, por exemplo, aumentando a resolução da imagem a 500 x 500 pixéis, e/ou selecionar um modelo mais profundo (Resnet 50). No entanto, a alteração dos parâmetros pode ter num aumento significativo no tempo de preparação. Consulte o artigo sobre [como melhorar a precisão](https://docs.microsoft.com/azure/machine-learning/service/how-to-improve-accuracy-for-computer-vision-models).


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
    

## <a name="train-the-classifier"></a>Preparar o classificador

Pode escolher um dos seguintes métodos para a pré-treinado DNN.

  - **DNN refinement**, que trains DNN para efetuar a classificação diretamente. Enquanto formação DNN estiver lenta,-normalmente leva a obter os melhores resultados, uma vez que todas as ponderações de rede podem ser melhoradas durante a preparação para dar melhor exatidão.

  - **DNN featurization**, que é executada DNN como-é para obter uma representação de uma imagem inferior dimensional (512, 2048 ou 4096 floats). Esse representação, em seguida, é utilizada como entrada para preparar um classificador separado. Uma vez que o DNN é mantida inalterado, esta abordagem é muito mais rapidamente em comparação com DNN refinement, no entanto, a precisão não é bom como. Contudo, um classificador externo, como um SVM linear de preparação (conforme ilustrado no seguinte código) pode fornecer uma linha de base segura e ajudar a compreender a viabilidade de um problema.
  
TensorBoard pode ser utilizado para visualizar o progresso de formação. Para ativar TensorBoard:
1. Adicione o parâmetro `tensorboard_logdir=PATH` conforme mostrado no seguinte código
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


## <a name="evaluate-and-visualize-model-performance"></a>Avaliar e visualizar o desempenho do modelo

Pode avaliar o desempenho do modelo treinado um conjunto de dados de teste independentes utilizando o módulo de avaliação. Algumas das métricas de avaliação calcula incluem:
 
+ Precisão (por predefinição uma média de classe)
+ Curva ELI
+ Curva ROC
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

from ui_utils.ui_results_viewer import ResultsUI
results_ui = ResultsUI(test_set, Context.get_global_context(), pred_scores, pred_labels)
display(results_ui.ui)
```

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/Image_Classification_Results.png)


```python
# Precision / recall curve UI
precisions, recalls, thresholds = ce.compute_precision_recall_curve() 
thresholds = list(thresholds)
thresholds.append(thresholds[-1])
from ui_utils.ui_precision_recall import PrecisionRecallUI
pr_ui = PrecisionRecallUI(100*precisions[::-1], 100*recalls[::-1], thresholds[::-1])
display(pr_ui.ui) 
```

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/image_precision_curve.png)

## <a name="operationalization-deploy-and-consume"></a>Operationalization: implementar e consumir

Operationalization é o processo de publicação modelos e código como serviços web e o consumo destes serviços para produzir resultados de negócios. 

Depois do seu modelo está preparado, pode implementar este modelo como um serviço web para utilizar o consumo [CLI do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Os modelos podem ser implementados para o computador local ou o cluster do serviço de contentor do Azure (ACS). Utilizar ACS, pode dimensionar o seu serviço web manualmente ou utilizar a funcionalidade de dimensionamento automático.

**Iniciar sessão com a CLI do Azure**

Utilizar um [Azure](https://azure.microsoft.com/) conta com uma subscrição válida, inicie sessão com o seguinte comando da CLI:
<br>`az login`

+ Para mudar para outra subscrição do Azure, utilize o comando:
<br>`az account set --subscription [your subscription name]`

+ Para ver a conta de gestão do modelo atual, utilize o comando:
  <br>`az ml account modelmanagement show`

**Criar e definir o seu ambiente de implementação de cluster**

Só tem de definir o seu ambiente de implementação de uma vez. Se ainda não tem um, configure o ambiente de implementação utiliza agora [estas instruções](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Para ver o seu ambiente de implementação do Active Directory, utilize o seguinte comando da CLI:
<br>`az ml env show`
   
Comando da CLI do Azure para criar e definir o ambiente de implementação de exemplo

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Gerir serviços web e implementações

As APIs seguintes pode ser utilizadas para implementar modelos como serviços web, gerir esses serviços web e gerir implementações.

|Tarefa|API|
|----|----|
|Criar o objeto de implementação|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Implementar o serviço web|`deploy_obj.deploy()`|
|Imagem de modelo de pontuação|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Eliminar o serviço web|`deploy_obj.delete()`|
|Compilar a imagem do docker sem o serviço web|`deploy_obj.build_docker_image()`|
|Lista de implementação existente|`AMLDeployment.list_deployment()`|
|Eliminar se o serviço existe com o nome da implementação|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**Documentação da API:** consulte o [documentação de referência do pacote](https://aka.ms/aml-packages/vision) para a referência de detalhado para cada módulo e uma classe.

**Referência da CLI:** para mais avançadas operações relacionadas com a implementação, consulte o [modelo gestão referência CLI](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Gestão de implementação no portal do Azure**: pode controlar e gerir as implementações no [portal do Azure](https://ms.portal.azure.com/). No portal do Azure, localize a sua página de conta de gestão de modelo do Machine Learning utilizando o respetivo nome. Em seguida, avance para a página de conta de gestão do modelo > Gestão de modelo > serviços.


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

Depois de implementar o modelo como um serviço web, pode Pontuar imagens com o serviço web utilizando um dos seguintes métodos:

- O serviço web diretamente com a utilização de objeto de implementação de pontuação `deploy_obj.score_image(image_path_or_url)`

- Utilize a chave de URL e o serviço de ponto final de serviço (nenhum para a implementação local) com: `AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

- Forma os pedidos de HTTP diretamente para o ponto final do serviço web de pontuação. Esta opção é para utilizadores avançados.

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

### <a name="score-with-service-endpoint-url-and-service-key"></a>Pontuar com o url de ponto final de serviço e a chave de serviço

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

### <a name="score-endpoint-with-http-request-directly"></a>Pontuar diretamente o ponto final com o pedido de http

O código de exemplo seguintes compõe o pedido HTTP diretamente no Python. No entanto, pode fazê-lo noutras linguagens de programação.


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

### <a name="parse-serialized-result-from-web-service"></a>Analisar o resultado serializado do serviço web

A saída do serviço web é uma cadeia JSON. Pode analisar esta cadeia JSON com diferentes classes de modelo DNN.


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

Saiba mais sobre o pacote do Azure Machine Learning para visão de computador nestes artigos:

+ Saiba como [melhorar a precisão deste modelo](how-to-improve-accuracy-for-computer-vision-models.md).

+ Leia o [descrição geral do pacote e aprender a instalá-lo](https://aka.ms/aml-packages/vision).

+ Explorar o [documentação de referência](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) para este pacote.

+ Saiba mais sobre [outros pacotes do Python para o Azure Machine Learning](reference-python-package-overview.md).
