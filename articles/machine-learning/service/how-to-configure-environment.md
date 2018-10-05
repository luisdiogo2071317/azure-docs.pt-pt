---
title: Configurar um ambiente de desenvolvimento do Azure Machine Learning | Documentos da Microsoft
description: Saiba como configurar um ambiente de desenvolvimento ao trabalhar com o serviço Azure Machine Learning. Neste documento, saiba como utilizar ambientes de Conda, criar arquivos de configuração e configurar o Jupyter Notebooks, blocos de notas do Azure, IDEs, editores de código e a máquina de Virtual de ciência de dados.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 8/6/2018
ms.openlocfilehash: 40a74af4f71fe649e5e4c6e67d4fdcb9751cc06e
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48800762"
---
# <a name="configure-a-development-environment-for-the-azure-machine-learning-service"></a>Configurar um ambiente de desenvolvimento para o serviço Azure Machine Learning

Saiba como configurar o ambiente de desenvolvimento para trabalhar com o serviço Azure Machine Learning. Aprenderá como criar um ficheiro de configuração que associa o seu ambiente de uma área de trabalho do serviço do Azure Machine Learning. Também aprenderá como configurar os seguintes ambientes de desenvolvimento:

* Blocos de notas do Jupyter no seu computador
* Visual Studio Code
* Editor de código à sua escolha

A abordagem recomendada é usar o Continuum Anaconda [ambientes virtuais de conda](https://conda.io/docs/user-guide/tasks/manage-environments.html) para isolar o seu ambiente de trabalho de modo a evitar conflitos de dependência entre pacotes. Este artigo mostra-lhe os passos para configurar um ambiente de conda e usá-lo para o Azure Machine Learning.


## <a name="prerequisites"></a>Pré-requisitos

* Uma área de trabalho de serviço do Azure Machine Learning. Para criar um, utilize os passos no [introdução ao serviço Azure Machine Learning](quickstart-get-started.md) documento.

* [Continuum Anaconda](https://www.anaconda.com/download/) ou [Miniconda](https://conda.io/miniconda.html) Gestor de pacotes.

 * Para o ambiente do Visual Studio Code, o [extensão Python](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> Comandos de shell utilizados neste documento são testados com o bash no Linux e macOS. Os comandos também são testados com cmd.exe no Windows.

## <a name="create-workspace-configuration-file"></a>Criar ficheiro de configuração da área de trabalho

O ficheiro de configuração da área de trabalho é utilizado pelo SDK para comunicar com a sua área de trabalho do serviço do Azure Machine Learning.  Existem duas formas de obter este ficheiro:

* Concluir o [guia de introdução](quickstart-get-started.md) para criar um ficheiro de configuração e de área de trabalho. O ficheiro `config.json` é criada para em blocos de notas do Azure.  Este ficheiro contém as informações de configuração para a área de trabalho.  Baixe ou copie-as para o mesmo diretório que os scripts ou blocos de notas que fazem referência a ele.


* Crie o ficheiro de configuração por conta própria com seguindo estes passos:

    1. Abra a área de trabalho do [portal do Azure](https://portal.azure.com). Copiar o __nome da área de trabalho__, __grupo de recursos__, e __ID de subscrição__. Estes valores são utilizados para criar o ficheiro de configuração.

        ![Portal do Azure](./media/how-to-configure-environment/configure.png) 
    
    1. Crie o ficheiro com este código de Python. Execute o código no mesmo diretório que os scripts ou blocos de notas que fazem referência a área de trabalho:

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
        Isto grava o seguinte `aml_config/config.json` ficheiro: 
    
        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Pode copiar o `aml_config` diretório ou apenas o `config.json` ficheiro para outro diretório que faça referência a área de trabalho.

>[!NOTE] 
>Outros scripts ou blocos de notas no mesmo diretório ou abaixo irão carregar a área de trabalho com `ws=Workspace.from_config()`

## <a name="azure-notebooks-and-data-science-virtual-machine"></a>Blocos de notas do Azure e de máquina de Virtual de ciência de dados

Blocos de notas e máquinas virtuais de ciência de dados do Azure (DSVM) do Azure são pré-configurada para funcionar com o serviço Azure Machine Learning. Componentes necessários, por exemplo, o SDK do Azure Machine Learning, estão pré-instalados nesses ambientes.

Blocos de notas do Azure é um serviço de bloco de notas do Jupyter na cloud do Azure. A máquina de Virtual de ciência de dados é uma imagem VM que está pré-configurada para o trabalho de ciência de dados. A VM inclui ferramentas populares, IDEs e pacotes, como o Jupyter Notebooks, PyCharm e Tensorflow.

Ainda precisará um ficheiro de configuração de área de trabalho para utilizar estes ambientes.

Para obter mais informações sobre as máquinas de virtuais de ciência de dados, consulte [máquinas de virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) documentação.

Para obter um exemplo do uso de blocos de notas do Azure com o serviço Azure Machine Learning, consulte a [introdução ao serviço Azure Machine Learning](quickstart-get-started.md) documento.

## <a name="configure-jupyter-notebooks-on-your-own-computer"></a>Configurar blocos de notas do Jupyter no seu computador

1. Abra uma linha de comandos ou shell.

2. Para criar um ambiente de conda, utilize os seguintes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

    Pode demorar vários minutos para criar o ambiente, como o Python 3.6 e outros componentes talvez precisem ser transferido.

3. Para instalar o SDK do Azure Machine Learning com extras de bloco de notas, utilize o seguinte comando:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl]
    ```

    > [!NOTE]
    > Se receber uma mensagem que `PyYAML` não pode ser desinstalado, utilize o seguinte comando em vez disso:
    > 
    > `pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML`

    Pode demorar vários minutos para instalar o SDK.

4. Para instalar pacotes para o experimentação da aprendizagem automática, utilize o seguinte comando e substitua `<new package>` com o pacote que pretende instalar:

    ```shell
    conda install <new package>
    ```

5. Para instalar um servidor com reconhecimento de conda do bloco de notas do Jupyter e ativar widgets de experimentação (ver as informações de execução), utilize os seguintes comandos:

    ```shell
    # install Jupyter 
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

6. Para iniciar o bloco de notas do Jupyter, utilize o seguinte comando:

    ```shell
    jupyter notebook
    ```

7. Abra um novo bloco de notas e selecione "myenv" como o kernel. Em seguida, valide que tem instalado ao executar o seguinte comando numa célula de bloco de notas de SDK do Azure Machine Learning:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Configurar o Visual Studio Code

1. Abra uma linha de comandos ou shell.

2. Para criar um ambiente de conda, utilize os seguintes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

2. Para instalar o SDK do Azure Machine Learning, utilize o seguinte comando:
 
    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

4. Para instalar o Visual Studio code Tools para IA, consulte a entrada de marketplace do Visual Studio para [Tools para IA](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai). 

5. Para instalar pacotes para o experimentação da aprendizagem automática, utilize o seguinte comando e substitua `<new package>` com o pacote que pretende instalar:

    ```shell
    conda install <new package>
    ```

6. Inicie o Visual Studio Code e, em seguida, utilize __CTRL-SHIFT-P__ para Windows ou __comando-SHIFT-P__ para Mac obter o __paleta de comandos__. Introduza *Python: selecione o interpretador*e selecione o ambiente de conda que criou.

    > [!NOTE]
    > Visual Studio Code é automaticamente com suporte para ambientes de conda no seu computador. Para obter mais informações, consulte [documentação de código do Visual Studio](https://code.visualstudio.com/docs/python/environments#_conda-environments).

7. Para validar a configuração, utilize o Visual Studio Code para criar um novo ficheiro de script de Python com o código seguinte e, em seguida, executá-lo:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-code-editor-of-your-choice"></a>Configurar o editor de código à sua escolha

Para utilizar um editor de código personalizado com o SDK do Azure Machine Learning, crie primeiro o ambiente de conda conforme descrito acima. Em seguida, siga as instruções para cada editor para utilizar o ambiente de conda. Por exemplo, as instruções para PyCharm estão localizadas em [ https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html ](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).
 
## <a name="next-steps"></a>Passos Seguintes

* [Preparar um modelo no Azure Machine Learning com o conjunto de dados MNIST](tutorial-train-models-with-aml.md)
