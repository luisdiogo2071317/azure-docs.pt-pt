---
title: 'Início rápido: Introdução em Python'
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
ms.openlocfilehash: 2ca97275848d87ccc03c7839265f867f9c3c3948
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53073363"
---
# <a name="quickstart-use-python-sdk-to-get-started-with-azure-machine-learning"></a>Início rápido: Utilizar o Python SDK para começar a utilizar com o Azure Machine Learning

Neste início rápido, irá utilizar o SDK para Python do Azure Machine Learning para criar e, em seguida, utilizar uma [área de trabalho](concept-azure-machine-learning-architecture.md) de serviço do Machine Learning. Esta área de trabalho é o pilar na cloud que utiliza para experimentar, preparar e implementar modelos de machine learning com Machine Learning. Neste início rápido, comece por configurar o seu próprio ambiente de Python e o servidor de blocos de notas do Jupyter. Para executar sem instalação, veja [Início Rápido: Utilizar o portal do Azure para começar a trabalhar com o Azure Machine Learning](quickstart-get-started.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Neste tutorial, irá instalar o SDK Python e:

* Criar uma área de trabalho na sua subscrição do Azure.
* Criar um ficheiro de configuração para essa área de trabalho, para utilização posterior noutros blocos de notas e scripts.
* Escrever código que regista os valores dentro da área de trabalho.
* Ver os valores registados na área de trabalho.

Neste início rápido, vai criar uma área de trabalho e um ficheiro de configuração. Pode utilizá-los como pré-requisitos para outros tutoriais de Machine Learning e artigos de instruções. Tal como acontece com outros serviços do Azure, existem limites e quotas associadas ao Machine Learning. [Saiba mais sobre quotas e como pedir mais.](how-to-manage-quotas.md)

Os seguintes recursos do Azure são adicionados automaticamente à sua área de trabalho quando estiverem disponíveis regionalmente:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Armazenamento do Azure](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Cofre de Chaves do Azure](https://azure.microsoft.com/services/key-vault/)

>[!NOTE]
> Código neste artigo requer o Azure Machine Learning SDK versão 1.0.2 ou posterior. 


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.


## <a name="install-the-sdk"></a>Instalar o SDK

*Ignore esta secção se utilizar uma máquina virtual de ciência de dados criada depois de 27 de setembro de 2018.* Essas máquinas virtuais de ciência de dados são disponibilizadas com o SDK Python pré-instalado.

Antes de instalar o SDK, recomendamos que crie um ambiente Python isolado. Embora este início rápido utilize [Miniconda](https://conda.io/docs/user-guide/install/index.html), também pode utilizar o [Anaconda](https://www.anaconda.com/) completo instalado ou [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Instalar o Miniconda


[Transfira](https://conda.io/miniconda.html) e instale o Miniconda. Selecione a versão Python 3.7 ou posterior. Não selecione a versão Python 2.x.

### <a name="create-an-isolated-python-environment"></a>Criar um ambiente Python isolado 

Abra uma janela da linha de comandos. Em seguida, crie um novo ambiente conda com o nome `myenv`, com o Python 3.6.

```shell
conda create -n myenv -y Python=3.6
```

Ative o ambiente.

  ```shell
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>Instalar o SDK

No ambiente conda ativado, instale o SDK. Este código instala os componentes principais do SDK de Machine Learning. Também instala um servidor de bloco de notas do Jupyter no ambiente de conda. A instalação demorasse alguns minutos a concluir, dependendo da configuração do seu computador.

```sh
# install Jupyter
conda install nb_conda

# install the base SDK and Jupyter Notebook
pip install azureml-sdk[notebooks]

```

Também pode utilizar as palavras-chave "extras" diferentes para instalar componentes adicionais do SDK.

```sh
# install the base SDK and auto ml components
pip install azureml-sdk[automl]

# install the base SDK and model explainability component
pip install azureml-sdk[explain]

# install the base SDK and experimental components
pip install azureml-sdk[contrib]
```

Use esta instalação num ambiente do Databricks.

```
# install the base SDK and automl components in Azure Databricks environment
# read more at: https://github.com/Azure/MachineLearningNotebooks/tree/master/databricks
pip install azureml-sdk[databricks]
```


## <a name="create-a-workspace"></a>Criar uma área de trabalho

Para iniciar o Jupyter Notebook, introduza este comando.
```shell
jupyter notebook
```

Na janela do browser, crie um novo bloco de notas com o kernel `Python 3` predefinido. 

Para apresentar a versão do SDK, introduza o seguinte código do Python numa célula do bloco de notas e execute-o.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

Crie um novo grupo de recursos do Azure e uma nova área de trabalho.

Encontre um valor para `<azure-subscription-id>` na [lista de subscrições no portal do Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Utilize qualquer subscrição em que a sua função seja proprietário ou contribuidor.

```python
from azureml.core import Workspace
ws = Workspace.create(name='myworkspace',
                      subscription_id='<azure-subscription-id>',    
                      resource_group='myresourcegroup',
                      create_resource_group=True,
                      location='eastus2' # or other supported Azure region  
                     )
```

A execução do código anterior pode acionar uma nova janela do browser para iniciar sessão na sua conta do Azure. Depois de iniciar sessão, o token de autenticação ficará em cache localmente.

Para ver os detalhes da área de trabalho, como o armazenamento associado, o registo de contentor e o cofre de chaves, introduza o código seguinte.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


## <a name="write-a-configuration-file"></a>Escrever um ficheiro de configuração

Guarde os detalhes da sua área de trabalho num ficheiro de configuração no diretório atual. Este ficheiro tem o nome “aml_config\config.json”.  

Este ficheiro de configuração da área de trabalho torna mais fácil de carregar a mesma área de trabalho mais tarde. Pode carregá-la com outros blocos de notas e scripts no mesmo diretório ou num subdiretório. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]


A chamada à API `write_config()` cria o ficheiro de configuração no diretório atual. O ficheiro `config.json` contém o seguinte script.

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
Quando a execução for concluída, pode ver a execução da experimentação no portal do Azure. Utilize o seguinte código para imprimir um URL para os resultados da última execução.

```python
print(run.get_portal_url())
```

Utilize a ligação para ver os valores registados no portal do Azure no seu browser.

![Valores registados no portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Limpar recursos 
>[!IMPORTANT]
>Os recursos que criou podem ser utilizados como pré-requisitos para outros tutoriais e artigos de procedimentos do Machine Learning.

Se não pretende utilizar os recursos que foram criados, elimine-os para não incorrer em custos.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]


## <a name="next-steps"></a>Passos Seguintes

Criou os recursos que precisa para experimentar e implementar modelos. Também executou código num bloco de notas. E explorou o histórico de execuções desse código na sua área de trabalho na cloud.

Precisa de mais alguns pacotes no seu ambiente para utilizá-lo com os tutoriais do Machine Learning.

1. No browser, feche o bloco de notas.
1. Na janela da linha de comandos, utilize `Ctrl`+`C` para parar o servidor de blocos de notas.
1. Instale pacotes adicionais.

    ```shell
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```


Depois de instalar estes pacotes, siga os tutoriais para preparar e implementar um modelo. 

> [!div class="nextstepaction"]
> [Tutorial: Preparar um modelo de classificação de imagens](tutorial-train-models-with-aml.md)

Também pode explorar [exemplos mais avançados no GitHub](https://aka.ms/aml-notebooks).
