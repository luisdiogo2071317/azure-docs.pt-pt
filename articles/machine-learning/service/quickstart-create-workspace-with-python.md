---
title: 'Início rápido: Comece a utilizar em Python'
titleSuffix: Azure Machine Learning service
description: Introdução ao serviço Azure Machine Learning em Python. Utilize o Python SDK para criar uma área de trabalho, o que é o bloco fundamental na cloud que utilizar para testar, preparar e implementar modelos de aprendizagem automática.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: fd86e3a65b542bad0f3114a32362041c34fb74d8
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53791239"
---
# <a name="quickstart-use-the-python-sdk-to-get-started-with-azure-machine-learning"></a>Início rápido: Utilizar o Python SDK para começar a utilizar com o Azure Machine Learning

Neste início rápido, vai utilizar o Azure Machine Learning SDK para Python para criar e, em seguida, utilizar o serviço Azure Machine Learning [área de trabalho](concept-azure-machine-learning-architecture.md). Esta área de trabalho é o pilar na cloud que utiliza para experimentar, preparar e implementar modelos de machine learning com Machine Learning. Comece por configurar o seu próprio ambiente de Python e o servidor de bloco de notas do Jupyter. Para executar sem instalação, consulte [início rápido: Utilizar o portal do Azure para começar a utilizar com o Azure Machine Learning](quickstart-get-started.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Neste tutorial, instale o SDK de Python e concluir estas tarefas:

* Criar uma área de trabalho na sua subscrição do Azure.
* Criar um ficheiro de configuração para essa área de trabalho, para utilização posterior noutros blocos de notas e scripts.
* Escrever código que regista os valores dentro da área de trabalho.
* Ver os valores registados na área de trabalho.

Neste início rápido, vai criar uma área de trabalho e um ficheiro de configuração. Pode usar esses recursos como pré-requisitos para outros tutoriais de Machine Learning e artigos de instruções. Tal como acontece com outros serviços do Azure, existem limites e quotas que estão associadas com o Machine Learning. [Saiba mais sobre quotas e como pedir mais](how-to-manage-quotas.md).

Os seguintes recursos do Azure são adicionados automaticamente à sua área de trabalho quando estiverem disponíveis regionalmente:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Armazenamento do Azure](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Cofre de Chaves do Azure](https://azure.microsoft.com/services/key-vault/)

Se não tiver uma subscrição do Azure, crie uma conta gratuita antes de começar. Experimente o [uma versão gratuita ou paga do serviço Azure Machine Learning](http://aka.ms/AMLFree) hoje mesmo.

## <a name="install-the-sdk"></a>Instalar o SDK

> [!IMPORTANT]
> Ignore esta secção se utilizar uma máquina de virtual de ciência de dados criada após a 27 de Setembro de 2018.
> Máquinas de virtuais do ciência de dados criadas após esta data vêm com o SDK de Python pré-instalado.

O código neste artigo requer o Azure Machine Learning SDK versão 1.0.2 ou posterior.

Antes de instalar o SDK, recomendamos que crie um ambiente Python isolado. Embora este início rápido utilize [Miniconda](https://conda.io/docs/user-guide/install/index.html), também pode utilizar o [Anaconda](https://www.anaconda.com/) completo instalado ou [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Instalar o Miniconda


[Transfira](https://conda.io/miniconda.html) e instale o Miniconda. Selecione a versão Python 3.7 ou posterior. Não selecione a versão Python 2.x.

### <a name="create-an-isolated-python-environment"></a>Criar um ambiente Python isolado 

Abra uma janela da linha de comandos. Em seguida, crie um novo com o nome de ambiente de conda **myenv** com o Python 3.6.

```shell
conda create -n myenv -y Python=3.6
```

Ative o ambiente:

```shell
conda activate myenv
```

### <a name="install-the-sdk"></a>Instalar o SDK

No ambiente conda ativado, instale o SDK. Este código instala os componentes principais do SDK de Machine Learning. Também instala um servidor de bloco de notas do Jupyter no ambiente de conda. A instalação demora alguns minutos para concluir com base na configuração do seu computador.

```sh
# Install Jupyter
conda install nb_conda

# Install the base SDK and Jupyter Notebook
pip install azureml-sdk[notebooks]
```

Também pode utilizar palavras-chave adicional para instalar outros componentes do SDK.

```sh
# Install the base SDK and auto ml components
pip install azureml-sdk[automl]

# Install the base SDK and the model explainability component
pip install azureml-sdk[explain]

# Install the base SDK and experimental components
pip install azureml-sdk[contrib]
```

No ambiente do Azure Databricks, utilize o seguinte comando de instalação em vez disso:

```
# Install the base SDK and automl components in the Azure Databricks environment.
# For more information, see https://github.com/Azure/MachineLearningNotebooks/tree/master/databricks.
pip install azureml-sdk[databricks]
```


## <a name="create-a-workspace"></a>Criar uma área de trabalho

Para iniciar o bloco de notas do Jupyter, introduza este comando:

```shell
jupyter notebook
```

Na janela do browser, criar um novo notebook com a predefinição **Python 3** kernel. 

Para apresentar a versão do SDK, introduza o seguinte código do Python numa célula do bloco de notas e execute-o.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

Crie um novo grupo de recursos do Azure e uma nova área de trabalho.

Encontrar um valor para o `<azure-subscription-id>` parâmetro na [lista de subscrições no portal do Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Utilize qualquer subscrição em que a sua função seja proprietário ou contribuidor.

```python
from azureml.core import Workspace
ws = Workspace.create(name='myworkspace',
                      subscription_id='<azure-subscription-id>',    
                      resource_group='myresourcegroup',
                      create_resource_group=True,
                      location='eastus2' # Or other supported Azure region  
                     )
```

Quando executar o código, poderá ser-lhe pedido para iniciar sessão na sua conta do Azure. Depois de iniciar sessão, o token de autenticação ficará em cache localmente.

Para ver a área de trabalho a obter detalhes, como o armazenamento associado, o registo de contentor e o Cofre de chaves, introduza o código a seguir:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


## <a name="write-a-configuration-file"></a>Escrever um ficheiro de configuração

Guarde os detalhes da sua área de trabalho num arquivo de configuração no diretório atual. O ficheiro é denominado **aml_config\config.json**.  

O ficheiro de configuração da área de trabalho torna mais fácil de carregar esta mesma área de trabalho mais tarde. É possível carregar a área de trabalho com outros blocos de notas e scripts no mesmo diretório ou subdiretório. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]


A chamada à API `write_config()` cria o ficheiro de configuração no diretório atual. O ficheiro Config JSON contém o seguinte script:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>Utilizar a área de trabalho

Escreva algum código que utiliza as APIs básicas do SDK para monitorizar as execuções da experimentação.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=useWs)]


## <a name="view-logged-results"></a>Ver resultados registados
Quando a execução for concluída, pode ver a execução da experimentação no portal do Azure. Utilize o seguinte código para imprimir um URL que navega para os resultados para a última execução:

```python
print(run.get_portal_url())
```

Utilize a ligação para ver os valores registados no portal do Azure no seu browser.

![Valores com sessão iniciada no portal do Azure](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Limpar recursos 

> [!IMPORTANT]
> Podem ser utilizados os recursos que criou como pré-requisitos para outros tutoriais de Machine Learning e artigos de instruções.

Se não planeja usar os recursos que criou neste início rápido, eliminá-los para que não existem custos.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]


## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou os recursos para experimentar e implementar modelos. Executou o código num bloco de notas e explorou o histórico de execução para o código na sua área de trabalho na cloud.

Para utilizar o código com os tutoriais de Machine Learning, terá de alguns pacotes mais no seu ambiente.

1. No browser, feche o bloco de notas.
1. Na janela de linha de comandos, introduza Ctrl + C para parar o servidor de bloco de notas do Jupyter.
1. Instale os pacotes adicionais:

    ```shell
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```


Depois de instalar os pacotes, continue com os tutoriais para preparar e implementar um modelo. 

> [!div class="nextstepaction"]
> [Tutorial: Preparar um modelo de classificação de imagem](tutorial-train-models-with-aml.md)

Também pode explorar [exemplos mais avançados no GitHub](https://aka.ms/aml-notebooks).
