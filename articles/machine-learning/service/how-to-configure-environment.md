---
title: Configurar um ambiente de desenvolvimento do Azure Machine Learning | Documentos da Microsoft
description: Saiba como configurar um ambiente de desenvolvimento ao trabalhar com o serviço Azure Machine Learning. Neste documento, saiba como utilizar ambientes de Conda, criar arquivos de configuração e configurar o Jupyter Notebooks, blocos de notas do Azure, IDEs, editores de código e a máquina de Virtual de ciência de dados.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 10/24/2018
ms.openlocfilehash: 6c2d5a776f603161ef730028168b91844c120aec
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158997"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar um ambiente de desenvolvimento do Azure Machine Learning

Este artigo mostra como configurar um ambiente de desenvolvimento para trabalhar com o serviço Azure Machine Learning, incluindo:

- Como criar um ficheiro de configuração que associa o seu ambiente de uma área de trabalho do serviço do Azure Machine Learning.
- Como configurar os seguintes ambientes de desenvolvimento:
  - Blocos de notas do Jupyter no seu computador
  - Visual Studio Code
  - Editor de código personalizado
- Como configurar uma [ambiente virtual de conda](https://conda.io/docs/user-guide/tasks/manage-environments.html) e utilizá-lo para o Azure Machine Learning. Recomendamos que utilize Continuum Anaconda para isolar o seu ambiente de trabalho para evitar conflitos de dependência entre pacotes.

## <a name="prerequisites"></a>Pré-requisitos

- Configure uma área de trabalho do serviço do Azure Machine Learning. Siga os passos em [introdução ao serviço Azure Machine Learning](quickstart-get-started.md).
- Instalar o [Continuum Anaconda](https://www.anaconda.com/download/) ou [Miniconda](https://conda.io/miniconda.html) Gestor de pacotes.
- Se estiver a utilizar o Visual Studio Code, obter o [extensão Python](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> Pode testar os comandos da shell mostrados neste artigo, ao utilizar o bash (em Linux e Mac OS) ou linha de comandos (Windows).

## <a name="create-a-workspace-configuration-file"></a>Criar um ficheiro de configuração da área de trabalho

O SDK do Azure Machine Learning utiliza o ficheiro de configuração da área de trabalho para comunicar com a área de trabalho do serviço do Azure Machine Learning.

- Para criar o ficheiro de configuração, conclua os [guia de início rápido do Azure Machine Learning](quickstart-get-started.md).
  - O processo de início rápido cria uma `config.json` arquivo em blocos de notas do Azure. Este ficheiro contém as informações de configuração para a área de trabalho.
  - Transfira ou copie o `config.json` no mesmo diretório que os scripts ou blocos de notas que fazem referência a ele.

- Em alternativa, pode criar manualmente o ficheiro ao seguir estes passos:

    1. Abra a área de trabalho do [portal do Azure](https://portal.azure.com). Copiar o __nome da área de trabalho__, __grupo de recursos__, e __ID de subscrição__. Estes valores são utilizados para criar o ficheiro de configuração.
        ![Portal do Azure](./media/how-to-configure-environment/configure.png)

    1. Criar o ficheiro com o seguinte código de Python e certifique-se executar o código no mesmo diretório que os scripts ou blocos de notas que fazem referência a área de trabalho:

        ```python
        from azureml.core import Workspace

        subscription_id ='<subscription-id>'
        resource_group ='<resource-group>'
        workspace_name = '<workspace-name>'

        try:
           ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
           ws.write_config()
           print('Library configuration succeeded')
        except:
           print('Workspace not found')
        ```
        O código é gravado o seguinte `aml_config/config.json` ficheiro:

        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Pode copiar o `aml_config` diretório ou apenas o `config.json` ficheiro para outro diretório que faça referência a área de trabalho.

       > [!NOTE]
       > Outros scripts ou blocos de notas no mesmo diretório ou abaixo de carga a área de trabalho com `ws=Workspace.from_config()`.

## <a name="azure-notebooks-and-data-science-virtual-machines"></a>Blocos de notas do Azure e máquinas de virtuais de ciência de dados

Blocos de notas e máquinas de virtuais de ciência de dados de Azure (DSVMs) do Azure são configurados para trabalhar com o serviço Azure Machine Learning. Estes ambientes incluem os componentes necessários, como o SDK do Azure Machine Learning.

- Blocos de notas do Azure é um serviço de bloco de notas do Jupyter na cloud do Azure.
- A máquina de Virtual de ciência de dados é uma imagem personalizada de máquina virtual (VM), concebida para o trabalho de ciência de dados. Ele inclui:
  - Ferramentas populares
  - Ambientes de desenvolvimento integrado (IDEs)
  - Como o Jupyter Notebooks, PyCharm e Tensorflow pacotes
- Ainda precisará um ficheiro de configuração de área de trabalho para utilizar estes ambientes.

Para obter um exemplo do uso de blocos de notas do Azure com o serviço Azure Machine Learning, consulte [introdução ao serviço Azure Machine Learning](quickstart-get-started.md).

Para obter mais informações sobre as máquinas de virtuais de ciência de dados, consulte [máquinas de virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="configure-jupyter-notebooks-on-your-computer"></a>Configurar blocos de notas do Jupyter no seu computador

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

   Pode ver os documentos de referência de Python para classes e métodos nos SDKs seguintes:
   + [Do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk)
   + [SDK de Preparação de Dados do Azure Machine Learning](https://aka.ms/data-prep-sdk)

   > [!NOTE]
   > Se receber uma mensagem que `PyYAML` não pode ser desinstalado, utilize o seguinte comando em vez disso:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Poderá demorar vários minutos para instalar o SDK.

1. Instale pacotes para o experimentação da aprendizagem automática. Utilize o seguinte comando e substitua `<new package>` com o pacote que pretende instalar:

    ```shell
    conda install <new package>
    ```

1. Instalar um servidor com reconhecimento de conda do bloco de notas do Jupyter e ativar widgets de experimentação (ver as informações de execução). Utilize os seguintes comandos:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

1. Abra o bloco de notas do Jupyter com o seguinte comando:

    ```shell
    jupyter notebook
    ```

1. Abra um novo bloco de notas, selecione "myenv" como o kernel e, em seguida, valide que tem instalado de SDK do Azure Machine Learning. Execute o seguinte comando numa célula de bloco de notas:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Configurar o Visual Studio Code

1. Abra uma linha de comandos ou shell.

1. Crie um ambiente de conda com os seguintes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

1. Instale o SDK do Azure Machine Learning e o SDK de preparação de dados com o seguinte comando:

    ```shell
    pip install --upgrade azureml-sdk[automl] azureml-dataprep
    ```

1. Instale as ferramentas de código do Visual Studio para a extensão de IA. Ver [Tools para IA](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

1. Instale pacotes para o experimentação da aprendizagem automática. Utilize o seguinte comando, substituindo `<new package>` com o pacote que pretende instalar:

    ```shell
    conda install <new package>
    ```

1. Abra o Visual Studio Code e, em seguida, utilize **CTRL-SHIFT-P** (no Windows) ou **comando-SHIFT-P** (em Mac OS) para obter o **paleta de comandos**. Introduza _Python: selecione o interpretador_ e selecione o ambiente de conda que criou.

   > [!NOTE]
   > Visual Studio Code é automaticamente com suporte para ambientes de conda no seu computador. Para obter mais informações, consulte [documentação de código do Visual Studio](https://code.visualstudio.com/docs/python/environments#_conda-environments).

1. Valide a configuração usando o Visual Studio Code para criar um novo ficheiro de script de Python com o código a seguir e, em seguida, executá-lo:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-a-custom-code-editor"></a>Configurar um editor de código personalizado

Pode usar um editor de código à sua escolha com o SDK do Azure Machine Learning.

1. Crie o seu ambiente de conda conforme descrito no passo 2 dos [configurar o Visual Studio Code](#configure-visual-studio-code) acima.
1. Siga as instruções para cada editor para utilizar o ambiente de conda. Por exemplo, pode seguir a [PyCharm instruções](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).

## <a name="next-steps"></a>Passos Seguintes

- [Preparar um modelo no Azure Machine Learning com o conjunto de dados MNIST](tutorial-train-models-with-aml.md)
