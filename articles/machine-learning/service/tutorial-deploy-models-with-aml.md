---
title: 'Tutorial: Implementar um modelo de classificação de imagens no Azure Container Instance (ACI) com o serviço do Azure Machine Learning'
description: Este tutorial mostra como utilizar o serviço do Azure Machine Learning para implementar um modelo de classificação de imagens com scikit-learn num bloco de notas Jupyter em Python.  Este tutorial é a segunda parte de uma série composta por duas partes.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: ad6b296543cffedb215c87b1fc893a22b29a3052
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427353"
---
# <a name="tutorial-2--deploy-an-image-classification-model-in-azure-container-instance-aci"></a>Tutorial n.º2:  Implementar um modelo de classificação de imagens no Azure Container Instance (ACI)

Este tutorial é a **segunda parte de uma série composta por duas partes**. No [tutorial anterior](tutorial-train-models-with-aml.md), preparou os modelos de machine learning e registou um modelo na sua área de trabalho na cloud.  

Agora, está pronto para implementar o modelo como um serviço Web no [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI). Um serviço Web é uma imagem, neste caso, uma imagem do Docker, que encapsula a lógica de classificação e o próprio modelo. 

Nesta parte do tutorial, vai utilizar o serviço do Azure Machine Learning (Pré-visualização) para:

> [!div class="checklist"]
> * Configurar o ambiente de teste
> * Obter o modelo na área de trabalho
> * Testar o modelo localmente
> * Implementar o modelo no ACI
> * Testar o modelo implementado

O ACI não é ideal para implementações de produção, mas é muito bom para testar e compreender o fluxo de trabalho. Relativamente a implementação de produção dimensionáveis, considere utilizar o [Azure Kubernetes Service](how-to-deploy-to-aks.md).

## <a name="get-the-notebook"></a>Obter o bloco de notas

Para sua comodidade, este tutorial está disponível como [bloco de notas do Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/02.deploy-models.ipynb). Execute o bloco de notas `02.deploy-models.ipynb` no Azure Notebooks ou no seu próprio servidor Jupyter Notebook.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="prerequisites"></a>Pré-requisitos

Conclua a preparação do modelo no bloco de notas [Tutorial 1: Preparar um modelo de classificação de imagens com o serviço do Azure Machine Learning](tutorial-train-models-with-aml.md).  


## <a name="set-up-the-environment"></a>Configurar o ambiente

Comece por configurar um ambiente de teste.

### <a name="import-packages"></a>Importar pacotes

Importe os pacotes de Python necessários para este tutorial.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
 
import azureml
from azureml.core import Workspace, Run

# display the core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="retrieve-the-model"></a>Obter o modelo

No tutorial anterior, registou um modelo na sua área de trabalho. Agora, carregue essa área de trabalho e transfira o modelo para o seu diretório local.


```python
from azureml.core import Workspace
from azureml.core.model import Model

ws = Workspace.from_config()
model=Model(ws, 'sklearn_mnist')
model.download(target_dir = '.')
import os 
# verify the downloaded model file
os.stat('./sklearn_mnist_model.pkl')
```

## <a name="test-model-locally"></a>Testar o modelo localmente

Antes de implementar, confirme que o seu modelo está a funcionar localmente ao:
* Carregar os dados de teste
* Prever os dados de teste
* Examinar a matriz de confusão

### <a name="load-test-data"></a>Carregar os dados de teste

Carregue os dados de teste a partir do diretório **./data/** que foi criado durante o tutorial de preparação.

```python
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

```

### <a name="predict-test-data"></a>Prever os dados de teste

Insira o conjunto de dados de teste no modelo, para obter previsões.

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load('./sklearn_mnist_model.pkl')
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>Examinar a matriz de confusão

Gere uma matriz de confusão para ver quantas amostras do conjunto de teste foram classificadas corretamente. Observe o valor “mal classificada” nas previsões incorretas. 

```python
from sklearn.metrics import confusion_matrix

conf_mx = confusion_matrix(y_test, y_hat)
print(conf_mx)
print('Overall accuracy:', np.average(y_hat == y_test))
```

A saída mostra a matriz de confusão:

    [[ 960    0    1    2    1    5    6    3    1    1]
     [   0 1112    3    1    0    1    5    1   12    0]
     [   9    8  920   20   10    4   10   11   37    3]
     [   4    0   17  921    2   21    4   12   20    9]
     [   1    2    5    3  915    0   10    2    6   38]
     [  10    2    0   41   10  770   17    7   28    7]
     [   9    3    7    2    6   20  907    1    3    0]
     [   2    7   22    5    8    1    1  950    5   27]
     [  10   15    5   21   15   27    7   11  851   12]
     [   7    8    2   13   32   13    0   24   12  898]]
    Overall accuracy: 0.9204
   

Utilize `matplotlib` para apresentar a matriz de confusão como um gráfico. Neste gráfico, o eixo X representa os valores reais e o Y os valores previstos. A cor em cada grelha representa a taxa de erros. Quanto mais clara for a cor, mais alta é a taxa de erros. Por exemplo, muitos 5 foram mal classificados como 3. Por isso, a grelha é clara em (5,3).

```python
# normalize the diagnal cells so that they don't overpower the rest of the cells when visualized
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
np.fill_diagonal(norm_conf_mx, 0)

fig = plt.figure(figsize=(8,5))
ax = fig.add_subplot(111)
cax = ax.matshow(norm_conf_mx, cmap=plt.cm.bone)
ticks = np.arange(0, 10, 1)
ax.set_xticks(ticks)
ax.set_yticks(ticks)
ax.set_xticklabels(ticks)
ax.set_yticklabels(ticks)
fig.colorbar(cax)
plt.ylabel('true labels', fontsize=14)
plt.xlabel('predicted values', fontsize=14)
plt.savefig('conf.png')
plt.show()
```

![matriz de confusão](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-web-service"></a>Implementar como serviço Web

Depois de testar o modelo e se estiver satisfeito com os resultados, implemente-o como um serviço Web alojado no ACI. 

Para criar o ambiente certo para o ACI, forneça o seguinte:
* Um script de classificação para mostrar como utilizar o modelo
* Um ficheiro de ambiente para mostrar os ficheiros que têm de ser instalados
* Um ficheiro de configuração para criar o ACI
* O modelo que preparou anteriormente

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Criar o script de classificação

Crie o script de classificação, denominado score.py, que a chamada do serviço Web utiliza para mostrar como o modelo deve ser utilizado.

Tem de incluir duas funções obrigatórias no script de classificação:
* A função `init()`, que, geralmente, carrega o modelo para um objeto global. Esta função só é executada uma vez, quando o contentor do Docker é iniciado. 

* A função `run(input_data)` utiliza o modelo para prever um valor com base nos dados de entrada. Regra geral, as entradas e as saídas da execução utilizam JSON para a serialização e a desserialização, mas são suportados outros formatos.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

def init():
    global model
    # retreive the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Criar o ficheiro de ambiente

Em seguida, crie um ficheiro de ambiente, denominado myenv.yml, que especifica todas as dependências do pacote do script. Este ficheiro é utilizado para garantir que todas essas dependências são instaladas na imagem do Docker. Este modelo precisa de `scikit-learn` e `azureml-sdk`.

```python
from azureml.core.conda_dependencies import CondaDependencies 

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
```
Reveja o conteúdo do ficheiro `myenv.yml`.

```python
with open("myenv.yml","r") as f:
    print(f.read())
```

### <a name="create-configuration-file"></a>Criar um ficheiro de configuração

Crie um ficheiro de configuração de implementação e indique o número de CPUs e de gigabytes de RAM necessários para o contentor do ACI. Embora dependa do seu modelo, a predefinição de 1 núcleo e 1 gigabyte de RAM costuma ser suficiente para a maioria dos modelos. Se precisar de adicionar mais no futuro, terá de recriar a imagem e reimplementar o serviço.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  "method" : "sklearn"}, 
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-aci"></a>Implementar no ACI
Tempo estimado de conclusão: **cerca de 7 a 8 minutos**

Configure a imagem e implemente. O código abaixo realiza estes passos:

1. Criar uma imagem com:
   * O ficheiro de classificação (`score.py`)
   * O ficheiro de ambiente (`myenv.yml`)
   * O ficheiro de modelo
1. Registe a imagem na área de trabalho. 
1. Envie a imagem para o contentor do ACI.
1. Arranque um contentor no ACI com a imagem.
1. Obtenha o ponto final HTTP do serviço Web.


```python
%%time
from azureml.core.webservice import Webservice
from azureml.core.image import ContainerImage

# configure the image
image_config = ContainerImage.image_configuration(execution_script="score.py", 
                                                  runtime="python", 
                                                  conda_file="myenv.yml")

service = Webservice.deploy_from_model(workspace=ws,
                                       name='sklearn-mnist-svc',
                                       deployment_config=aciconfig,
                                       models=[model],
                                       image_config=image_config)

service.wait_for_deployment(show_output=True)
```

Obtenha o ponto final HTTP do serviço Web de classificação, que aceita chamadas do cliente REST. Esse ponto final pode ser partilhado com qualquer pessoa que queira testar o serviço Web ou integrá-lo numa aplicação. 

```python
print(service.scoring_uri)
```


## <a name="test-deployed-service"></a>Testar o serviço implementado

Anteriormente, classificou todos os dados de teste com a versão local do modelo. Agora, pode testar o modelo implementado com uma amostra aleatória de 30 imagens dos dados de teste.  

O código abaixo realiza estes passos:
1. Envie os dados como uma matriz JSON ao serviço Web alojado no ACI. 

1. Utilize a API `run` do SDK para invocar o serviço. Também pode fazer chamadas diretas com qualquer ferramenta HTTP, como curl.

1. Imprima as predições devolvidas e represente-as juntamente com as imagens de entrada. Para realçar as amostras mal classificadas, é utilizado um tipo de letra vermelho e uma imagem inversa (branca ou preta). 

 Uma vez que a precisão do modelo é elevada, poderá ter de executar o seguinte código algumas vezes até conseguir ver uma amostra mal classificada.

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
result = json.loads(service.run(input_data=test_samples))

# compare actual value vs. the predicted values:
i = 0
plt.figure(figsize = (20, 1))

for s in sample_indices:
    plt.subplot(1, n, i + 1)
    plt.axhline('')
    plt.axvline('')
    
    # use different color for misclassified sample
    font_color = 'red' if y_test[s] != result[i] else 'black'
    clr_map = plt.cm.gray if y_test[s] != result[i] else plt.cm.Greys
    
    plt.text(x=10, y =-10, s=result[i], fontsize=18, color=font_color)
    plt.imshow(X_test[s].reshape(28, 28), cmap=clr_map)
    
    i = i + 1
plt.show()
```

Eis o resultado de uma amostra aleatória das imagens de teste: ![resultados](./media/tutorial-deploy-models-with-aml/results.png)

Também pode enviar um pedido HTTP direto para testar o serviço Web.

```python
import requests
import json

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

headers = {'Content-Type':'application/json'}

# for AKS deployment you'd need to the service key in the header as well
# api_key = service.get_key()
# headers = {'Content-Type':'application/json',  'Authorization':('Bearer '+ api_key)} 

resp = requests.post(service.scoring_uri, input_data, headers=headers)

print("POST to url", service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

## <a name="clean-up-resources"></a>Limpar recursos

Para manter o grupo de recursos e a área de trabalho para outros tutoriais e outras explorações, pode eliminar apenas a implementação do ACI com a chamada à API:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Passos seguintes

Neste tutorial do serviço do Azure Machine Learning, utilizou o Python para:

> [!div class="checklist"]
> * Configurar o ambiente de teste
> * Obter o modelo na área de trabalho
> * Testar o modelo localmente
> * Implementar o modelo no ACI
> * Testar o modelo implementado
 
Também pode experimentar o tutorial [Seleção de algoritmos automática](tutorial-auto-train-models.md) para ver como é que o serviço do Azure Machine Learning consegue selecionar automaticamente o melhor algoritmo para o seu modelo e criar o modelo por si.
