---
title: Do Azure Machine Learning a implementação do serviço de Web de gestão de modelo | Documentos da Microsoft
description: Este documento descreve os passos envolvidos na implantação de um modelo de aprendizagem automática com o Azure Machine Learning modelo de gestão.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/03/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2a486da73d00ddaebee5c1fe32fc6112c177f01e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992343"
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Implementar um modelo de aprendizagem automática como um serviço web

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Gestão de modelo do Azure Machine Learning fornece interfaces para implementar modelos como em contentores de serviços web baseados no Docker. Pode implementar modelos que criar com estruturas, como o Spark, o Microsoft Cognitive Toolkit (CNTK), Keras, Tensorflow e Python. 

Este documento aborda os passos para implementar os seus modelos como serviços da web usando a interface de linha de comandos (CLI) de gestão de modelos do Azure Machine Learning.

## <a name="what-you-need-to-get-started"></a>O que precisa para começar a utilizar

Para tirar o máximo proveito deste guia, deve ter acesso de contribuinte a uma subscrição do Azure ou um grupo de recursos que pode implementar os seus modelos para.
A CLI vem pré-instalada no Azure Machine Learning Workbench e, no [Azure DSVMs](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).  Também pode ser instalado como um pacote autônomo.

Além disso, um ambiente de conta e a implementação da gestão de modelo tem já de ser configurado.  Para obter mais informações sobre como configurar a sua conta de gestão de modelos e o ambiente para o local e a implementação de cluster, consulte [configuração da gestão de modelos](deployment-setup-configuration.md).

## <a name="deploying-web-services"></a>Implementar serviços web
Utilizar a CLI, pode implementar serviços web para ser executado no computador local ou num cluster.

Recomendamos que comece com uma implantação local. Em primeiro lugar a validar que o modelo e o código funcionam, em seguida, implementar o serviço web para um cluster para utilização de escala de produção.

Seguem-se os passos de implementação:
1. Utilizar o seu modelo de Machine Learning guardado, preparado,
2. Crie um esquema para a entrada do seu serviço da web e dados de saída
3. Criar uma imagem de contentor Docker com base em
4. Criar e implementar o serviço web

### <a name="1-save-your-model"></a>1. Guardar o seu modelo
Comece com o seu ficheiro guardado modelo preparado, por exemplo, mymodel.pkl. A extensão de ficheiro varia consoante a plataforma que utiliza para treinar e guardar o modelo. 

Por exemplo, pode utilizar a biblioteca de Pickle do Python para guardar um modelo preparado para um ficheiro. Aqui está uma [exemplo](http://scikit-learn.org/stable/modules/model_persistence.html) usando o conjunto de dados de Iris:

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf = linear_model.LogisticRegression()
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. Crie um ficheiro de schema.json

Enquanto a geração do esquema é opcional, é altamente recomendado para definir o formato de variável pedido e de entrada para a manipulação de melhor.

Crie um esquema para automaticamente validar a entrada e saída do seu serviço web. As CLIs também utilizam o esquema para gerar um documento Swagger para o seu serviço web.

Para criar o esquema, importe as seguintes bibliotecas:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Em seguida, defina as variáveis de entrada como um dataframe do Spark, Pandas dataframe ou NumPy matriz. O exemplo seguinte utiliza uma matriz de Numpy:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```
O exemplo seguinte utiliza um dataframe do Spark:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

O exemplo seguinte utiliza um PANDAS dataframe:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

O exemplo seguinte utiliza o formato JSON genérico:

```python
inputs = {"input_json": SampleDefinition(DataTypes.STANDARD, yourinputjson)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Crie um ficheiro de score.py
Forneça um arquivo de score.py, que carrega o modelo e retorna a predição de resultado ((de s), utilizando o modelo.

O ficheiro deve incluir duas funções: init e executar.

Adicionar o seguinte código na parte superior do ficheiro score.py para ativar a funcionalidade de recolha de dados que o ajuda a recolher dados de entrada e de predição do modelo

```python
from azureml.datacollector import ModelDataCollector
```

Verifique [recolha de dados de modelo](how-to-use-model-data-collection.md) secção para obter mais detalhes sobre como utilizar esta funcionalidade.

#### <a name="init-function"></a>Função init
Utilize a função de inicialização para carregar o modelo guardado.

Exemplo de uma função de init simples carregar o modelo:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>Executar a função
A função de execução usa o modelo e os dados de entrada para retornar uma predição.

Exemplo de um simples de executar a função de processamento de entrada e retornar o resultado de predição:

```python
def run(input_df):
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Registar um modelo
O comando a seguir é utilizada para registar um modelo que criou no passo 1 acima,

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Crie o manifesto.
O comando a seguir ajuda a criar um manifesto para o modelo,

```
az ml manifest create --manifest-name [your new manifest name] -f [path to score file] -r [runtime for the image, e.g. spark-py]
```
Pode adicionar um modelo registrado anteriormente para o manifesto com o argumento `--model-id` ou `-i` no comando mostrado acima. Vários modelos podem ser especificados com adicionais -i argumentos.

### <a name="6-create-an-image"></a>6. Criar uma imagem 
Pode criar uma imagem com a opção de ter criado seu manifesto antes. 

```
az ml image create -n [image name] --manifest-id [the manifest ID]
```

>[!NOTE] 
>Também pode utilizar um único comando para executar a criação de registo, o manifesto e o modelo de modelo. Utilize -h com o serviço criar comando para obter mais detalhes.

Como alternativa, há um único comando para registar um modelo, criar um manifesto e criar uma imagem (mas não criar e implementar o serviço web, ainda) como um passo da seguinte forma.

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime e.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Para obter mais detalhes sobre os parâmetros de comando, crie -h tipo -h no final do comando, por exemplo, imagem do az ml.


### <a name="7-create-and-deploy-the-web-service"></a>7. Criar e implementar o serviço web
Implemente o serviço com o seguinte comando:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>Também pode utilizar um único comando para executar todas as 4 passos anteriores. Utilize -h com o serviço criar comando para obter mais detalhes.

Como alternativa, há um único comando para registar um modelo, criar um manifesto, crie uma imagem, bem como, criar e implementar o serviço Web, como um passo da seguinte forma.

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```


### <a name="8-test-the-service"></a>8. Testar o serviço
Utilize o seguinte comando para obter informações sobre como chamar o serviço:

```
az ml service usage realtime -i <service id>
```

Chame o serviço usando a função de execução da linha de comandos:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

O exemplo a seguir chama um serviço da web de íris de exemplo:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Passos Seguintes
Agora que testou seu serviço web para ser executado localmente, pode implementá-la a um cluster para utilização em grande escala. Para obter detalhes sobre como configurar um cluster para a implementação de serviço da web, consulte [configuração de gestão de modelo](deployment-setup-configuration.md). 
