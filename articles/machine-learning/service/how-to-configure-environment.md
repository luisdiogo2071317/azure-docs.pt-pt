---
title: Configurar um ambiente de desenvolvimento
titleSuffix: Azure Machine Learning service
description: Saiba como configurar um ambiente de desenvolvimento ao trabalhar com o serviço Azure Machine Learning. Neste documento, saiba como utilizar ambientes de Conda, criar arquivos de configuração e configurar o Jupyter Notebooks, blocos de notas do Azure, IDEs, editores de código e a máquina de Virtual de ciência de dados.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6e2222d56ea37983b1efafedaac8e01058cb44fa
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53098054"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar um ambiente de desenvolvimento do Azure Machine Learning

Neste documento, saiba como configurar um ambiente de desenvolvimento para trabalhar com o serviço Azure Machine Learning. O serviço Azure Machine Learning é independente de plataforma. São os únicos requisitos para o seu ambiente de desenvolvimento __Python 3__, __Conda__ (para ambientes isolados) e um ficheiro de configuração que contém as informações da sua área de trabalho do Azure Machine Learning.

Este documento centra-se nos seguintes ambientes específicos e ferramentas:

* [Blocos de notas do Azure](#aznotebooks): serviço de um Jupyter Notebooks alojado na cloud do Azure. É __a mais fácil__ forma de começar a utilizar, como já está instalado o SDK do Azure Machine Learning.
* [A máquina de Virtual de ciência de dados](#dsvm): A __ambiente de desenvolvimento/experimentação pré-configurados__ na cloud do Azure ou seja __concebido para o trabalho de ciência de dados__ e podem ser implementados em qualquer uma Instâncias de VM apenas de CPU ou de GPU baseado em instâncias. Python 3, Conda, blocos de notas do Jupyter e o SDK do Azure Machine Learning já estão instalados. A VM é fornecido com popular ML / aprendizagem profunda estruturas, ferramentas e editores para desenvolver soluções de ML. É provavelmente __os mais completos__ ambiente de desenvolvimento para ML na plataforma do Azure.
* [Blocos de notas do Jupyter](#jupyter): Se já estiver a utilizar blocos de notas do Jupyter, o SDK tem alguns sobrando que deve instalar.
* [Visual Studio Code](#vscode): Se utilizar o Visual Studio Code, existem algumas extensões úteis que podem instalar.
* [O Azure Databricks](#aml-databricks): uma plataforma de análise de dados populares baseada no Apache Spark. Saiba como obter o SDK do Azure Machine Learning no seu cluster, para que possa implementar modelos.

Se já tiver um ambiente de Python 3 ou deseja apenas os passos básicos para instalar o SDK, consulte a [computador Local](#local) secção.

## <a name="prerequisites"></a>Pré-requisitos

- Uma área de trabalho de serviço do Azure Machine Learning. Siga os passos em [introdução ao serviço Azure Machine Learning](quickstart-get-started.md) para criar um.

- Ambos os [Continuum Anaconda](https://www.anaconda.com/download/) ou [Miniconda](https://conda.io/miniconda.html) Gestor de pacotes.

    > [!IMPORTANT]
    > Anaconda e Miniconda não são necessários quando utilizar blocos de notas do Azure.

- No Linux ou Mac OS, terá do shell de bash.

    > [!TIP]
    > Se estiver no Linux ou Mac OS e utilizar uma shell que não seja o bash (por exemplo, zsh) poderá receber erros ao executar alguns comandos. Para contornar este problema, utilize o `bash` comando para iniciar um novo shell do bash e execute os comandos lá.

- No Windows, terá da linha de comandos ou prompt Anaconda (por Anaconda e Miniconda instalado).

## <a id="anotebooks"></a>Blocos de notas do Azure

[Blocos de notas do Azure](https://notebooks.azure.com) (pré-visualização) é um ambiente de desenvolvimento interativo na cloud do Azure. É __a mais fácil__ forma de começar com o desenvolvimento do Azure Machine Learning.

* O SDK do Azure Machine Learning é __já instalado__.
* Depois de criar uma área de trabalho do serviço do Azure Machine Learning no portal do Azure, pode clicar num botão para configurar automaticamente o ambiente de bloco de notas do Azure para trabalhar com a área de trabalho.

Para começar a desenvolver com blocos de notas do Azure, siga os [introdução ao serviço Azure Machine Learning](quickstart-get-started.md) documento.

## <a id="dsvm"></a>Máquina de Virtual de ciência de dados

A Máquina Virtual de ciência de dados (DSVM) é uma imagem de máquina de virtual (VM) personalizada **concebido para o trabalho de ciência de dados** que é pré-configurado com:

  - Pacotes, como o Tensorflow, Pytorch, scikit-saiba, Xgboost e SDK do Azure ML
  - Ferramentas de ciência de dados populares, como versão autónoma do Spark, desagregar
  - Ferramentas do Azure, como o Explorador de armazenamento, o Azcopy e da CLI
  - Ambientes de desenvolvimento integrado (IDEs), como o Visual Studio Code, PyCharm e o RStudio
  - Servidor de bloco de notas do Jupyter 

O SDK do Azure Machine Learning funciona na versão o Ubuntu ou o Windows da DSVM. Para utilizar a máquina de Virtual de ciência de dados como um ambiente de desenvolvimento, utilize os seguintes passos:

1. Para criar uma máquina de Virtual de ciência de dados, utilize um dos seguintes métodos:

    * No Portal do Azure:

        * [Criar uma __Ubuntu__ máquina de Virtual de ciência de dados](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Criar uma __Windows__ máquina de Virtual de ciência de dados](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Utilizar a CLI do Azure:

        > [!IMPORTANT]
        > Ao utilizar a CLI do Azure, tem primeiro de iniciar sessão sua subscrição do Azure utilizando o `az login` comando.
        >
        > Ao utilizar os comandos neste passo, tem de fornecer um nome de grupo de recursos, um nome para a VM, um nome de utilizador e uma palavra-passe.

        * Para criar uma __Ubuntu__ máquina de Virtual de ciência de dados, utilize o seguinte comando:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Para criar uma __Windows__ máquina de Virtual de ciência de dados, utilize o seguinte comando:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. O SDK do Azure Machine Learning é **já instalado** na DSVM. Para utilizar o ambiente de Conda que contém o SDK, utilize um dos seguintes comandos:

    * No __Ubuntu__ DSVM, utilize este comando:

        ```shell
        conda activate py36
        ```

    * No __Windows__ DSVM, utilize este comando:

        ```shell
        conda activate AzureML
        ```

1. Para verificar se pode acessar o SDK e verificar a versão, utilize o seguinte código de Python:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Para configurar a DSVM para utilizar a sua área de trabalho do serviço do Azure Machine Learning, consulte a [configurar a área de trabalho](#workspace) secção.

Para obter mais informações sobre as máquinas de virtuais de ciência de dados, consulte [máquinas de virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Computador local

Quando utilizar um computador local (que também pode ser uma máquina virtual remota), utilize os seguintes passos para criar um ambiente de conda e instalar o SDK:

1. Abra uma linha de comandos ou shell.

1. Crie um ambiente de conda com os seguintes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy, and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # On Mac OS run
    source activate myenv
    ```

    Poderá demorar vários minutos para criar o ambiente, se o Python 3.6 e outros componentes têm de ser transferido.

1. Instale o SDK do Azure Machine Learning com alguns extras de bloco de notas e o SDK de preparação de dados com o seguinte comando:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   > [!NOTE]
   > Se receber uma mensagem que `PyYAML` não pode ser desinstalado, utilize o seguinte comando em vez disso:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Poderá demorar vários minutos para instalar o SDK.

1. Instale pacotes para o experimentação da aprendizagem automática. Utilize o seguinte comando e substitua `<new package>` com o pacote que pretende instalar:

    ```shell
    conda install <new package>
    ```

1. Para verificar se o SDK está instalado, o código de Python seguinte:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Blocos de notas do Jupyter

Blocos de notas do Jupyter fazem parte do [Jupyter projeto](https://jupyter.org/). Eles fornecem uma experiência interativa de codificação onde criar documentos que misturam código em direto com texto narrativo e gráficos. Blocos de notas do Jupyter também são uma ótima maneira de compartilhar seus resultados com outras pessoas, como pode salvar a saída de suas seções de código no documento. Pode instalar o Jupyter Notebooks numa variedade de plataformas.

Os passos a [computador Local](#local) secção instalar componentes opcionais para blocos de notas do Jupyter. Para ativar a esses componentes no seu ambiente de bloco de notas do Jupyter, utilize os seguintes passos:

1. Abra uma linha de comandos ou shell.

1. Para instalar um servidor de bloco de notas do Jupyter com reconhecimento de conda utilizando o seguinte comando:

    ```shell
    # install Jupyter
    conda install nb_conda
    ```

1. Abra o bloco de notas do Jupyter com o seguinte comando:

    ```shell
    jupyter notebook
    ```

1. Para verificar que o bloco de notas do Jupyter pode utilizar o SDK, abra um novo bloco de notas e selecione "myenv" como o kernel. Em seguida, execute o seguinte comando numa célula de bloco de notas:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para configurar o bloco de notas do Jupyter para utilizar a sua área de trabalho do serviço do Azure Machine Learning, consulte a [configurar a área de trabalho](#workspace) secção.

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code é um editor de código entre plataformas. Ele se baseia numa instalação de Python 3 e Conda local para o suporte de Python, mas ele fornece ferramentas adicionais para trabalhar com IA. Ele também fornece suporte para selecionar o ambiente de Conda a partir do editor de código.

Para utilizar o Visual Studio Code para o desenvolvimento, utilize os seguintes passos:

1. Para saber como utilizar o Visual Studio Code para o desenvolvimento de Python, veja a [introdução ao Python no VSCode](https://code.visualstudio.com/docs/python/python-tutorial) documento.

1. Para selecionar o ambiente de Conda, abra o VS Code e, em seguida, utilize __Ctrl-Shift-P__ (Linux e Windows) ou __comando-Shift-P__ (Mac) para obter o __palete de comando__. Introduza __Python: selecione o interpretador__ e, em seguida, selecione o ambiente de conda.

1. Para validar que pode utilizar o SDK, crie um novo ficheiro de Python (. PY) que contém o código a seguir. Em seguida, execute o ficheiro:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para instalar a extensão do Azure Machine Learning para Visual Studio Code, consulte a [Tools para IA](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) página.

    Para obter mais informações, consulte [utilizando o Azure Machine Learning para Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Pode utilizar uma versão personalizada do Azure Machine Learning SDK para o Azure Databricks para aprendizagem automática personalizada do ponto-a-ponto. Ou, preparar o seu modelo dentro do Databricks e o uso [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code) para implementar o modelo

Para preparar o seu cluster do Databricks e obter blocos de notas de exemplo:

1. Criar uma [cluster do Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) com uma versão de runtime do Databricks do 4.x (alta simultaneidade preferida) com **Python 3**. 

1. Criar uma biblioteca para [instalar e anexar](https://docs.databricks.com/user-guide/libraries.html#create-a-library) para o Azure Machine Learning SDK para Python `azureml-sdk[databricks]` PyPi pacote ao seu cluster. Quando tiver terminado, verá a biblioteca anexada tal como apresentado nesta imagem. Lembre-se de que esses [problemas comuns do Databricks](resource-known-issues.md#databricks).

   ![SDK instalado no Databricks ](./media/how-to-azure-machine-learning-on-databricks/sdk-installed-on-databricks.jpg)

   Se este passo falhar, reinicie o cluster:
   1. Selecione `Clusters`no painel esquerdo. Selecione o nome do cluster na tabela. 
   1. Sobre o `Libraries` separador, selecione `Restart`.

1. Transfira o [Azure Databricks / ficheiro de arquivo do bloco de notas do SDK do Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/databricks/Databricks_AMLSDK_github.dbc).

   >[!Warning]
   > Muitos blocos de notas de exemplo estão disponíveis para utilização com o serviço Azure Machine Learning. Apenas estes blocos de notas do exemplo de trabalho com o Azure Databricks: https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks

1.  [Importar este ficheiro de arquivo](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) para o seu Databricks do cluster e comece a explorar como [descrito aqui](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks).


## <a id="workspace"></a>Criar um ficheiro de configuração da área de trabalho

O ficheiro de configuração da área de trabalho é um documento JSON que informa o SDK como se comunicar com a sua área de trabalho do serviço do Azure Machine Learning. O ficheiro é denominado `config.json` e tem o seguinte formato:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Este ficheiro tem de ser a estrutura do diretório que contém os scripts de Python ou blocos de notas do Jupyter. Ele pode ser no mesmo diretório, um subdiretório nomeado `aml_config`, ou num diretório principal.

Para utilizar este ficheiro a partir do código, utilize `ws=Workspace.from_config()`. Esse código carrega as informações do arquivo e liga-se a sua área de trabalho.

Existem três formas de criar o ficheiro de configuração:

* Se seguir a [guia de início rápido do Azure Machine Learning](quickstart-get-started.md), um `config.json` ficheiro é criado na sua biblioteca de blocos de notas do Azure. Este ficheiro contém as informações de configuração para a área de trabalho. Pode transferir ou copie esta `config.json` para outros ambientes de desenvolvimento.

* Pode **criar manualmente o ficheiro** com um editor de texto. Pode encontrar os valores para que entrar no ficheiro de configuração ao visitar a sua área de trabalho do [portal do Azure](https://portal.azure.com). Copiar o __nome da área de trabalho__, __grupo de recursos__, e __ID de subscrição__ valores e utilizá-los no ficheiro de configuração.
        ![Portal do Azure](./media/how-to-configure-environment/configure.png)

* Pode **criar o ficheiro através de programação**. O fragmento de código seguinte demonstra como ligar a uma área de trabalho, fornecendo o ID de subscrição, o grupo de recursos e o nome de área de trabalho. Em seguida, guarda a configuração de área de trabalho para o ficheiro:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Esse código grava o ficheiro de configuração para o `aml_config/config.json` ficheiro.

## <a name="next-steps"></a>Passos Seguintes

- [Preparar um modelo no Azure Machine Learning com o conjunto de dados MNIST](tutorial-train-models-with-aml.md)
- [Do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk)
- [SDK de Preparação de Dados do Azure Machine Learning](https://aka.ms/data-prep-sdk)
