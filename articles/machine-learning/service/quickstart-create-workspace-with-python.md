---
title: 'Início Rápido: utilizar o SDK Python para criar uma área de trabalho de serviço do machine learning - Azure Machine Learning'
description: Comece a utilizar o Azure Machine Learning. Instale o SDK Python e utilize-o para criar uma área de trabalho. Esta área de trabalho é o pilar na cloud que utiliza para experimentar, preparar e implementar modelos de machine learning com o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: cc348ca50b942b6b8b1474ed4dac4067d107a4af
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378004"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Início Rápido: utilizar o Python para começar a utilizar o Azure Machine Learning

Neste início rápido, irá utilizar o SDK para Python do Azure Machine Learning para criar e, em seguida, utilizar uma [área de trabalho](concept-azure-machine-learning-architecture.md) de serviço do Machine Learning. Esta área de trabalho é o pilar na cloud que utiliza para experimentar, preparar e implementar modelos de machine learning com Machine Learning.

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

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="install-the-sdk"></a>Instalar o SDK

*Ignore esta secção se utilizar uma máquina virtual de ciência de dados criada depois de 27 de setembro de 2018.* Essas máquinas virtuais de ciência de dados são disponibilizadas com o SDK Python pré-instalado.

Antes de instalar o SDK, recomendamos que crie um ambiente Python isolado. Embora este início rápido utilize [Miniconda](https://conda.io/docs/user-guide/install/index.html), também pode utilizar o [Anaconda](https://www.anaconda.com/) completo instalado ou [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Instalar o Miniconda


[Transfira](https://conda.io/miniconda.html) e instale o Miniconda. Selecione a versão Python 3.7 ou posterior. Não selecione a versão Python 2.x.

### <a name="create-an-isolated-python-environment"></a>Criar um ambiente Python isolado 

Abra uma janela da linha de comandos. Em seguida, crie um novo ambiente conda com o nome `myenv`, com o Python 3.6.

```sh
conda create -n myenv -y Python=3.6
```

Ative o ambiente.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>Instalar o SDK

No ambiente conda ativado, instale o SDK. Este código instala os componentes principais do SDK de Machine Learning. Também instala um servidor do Jupyter Notebook no ambiente conda `myenv`. A instalação demora **cerca de quatro minutos** a concluir.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Criar uma área de trabalho

Para iniciar o Jupyter Notebook, introduza este comando.
```sh
jupyter notebook
```

Na janela do browser, crie um novo bloco de notas com o kernel `Python 3` predefinido. 

Para apresentar a versão do SDK, introduza o seguinte código do Python numa célula do bloco de notas e execute-o.

```python
import azureml.core
print(azureml.core.VERSION)
```

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

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Escrever um ficheiro de configuração

Guarde os detalhes da sua área de trabalho num ficheiro de configuração no diretório atual. Este ficheiro tem o nome “aml_config\config.json”.  

Este ficheiro de configuração da área de trabalho torna mais fácil de carregar a mesma área de trabalho mais tarde. Pode carregá-la com outros blocos de notas e scripts no mesmo diretório ou num subdiretório. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

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

```python
from azureml.core import Experiment

# create a new experiment
exp = Experiment(workspace=ws, name='myexp')

# start a run
run = exp.start_logging()

# log a number
run.log('my magic number', 42)

# log a list (Fibonacci numbers)
run.log_list('my list', [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]) 

# finish the run
run.complete()
```

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

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Passos seguintes

Criou os recursos que precisa para experimentar e implementar modelos. Também executou código num bloco de notas. E explorou o histórico de execuções desse código na sua área de trabalho na cloud.

Precisa de mais alguns pacotes no seu ambiente para utilizá-lo com os tutoriais do Machine Learning.

1. No browser, feche o bloco de notas.
1. Na janela da linha de comandos, utilize `Ctrl`+`C` para parar o servidor de blocos de notas.
1. Instale pacotes adicionais.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Depois de instalar estes pacotes, siga os tutoriais para preparar e implementar um modelo. 

> [!div class="nextstepaction"]
> [Tutorial: Preparar um modelo de classificação de imagens](tutorial-train-models-with-aml.md)

Também pode explorar [exemplos mais avançados no GitHub](https://aka.ms/aml-notebooks).
