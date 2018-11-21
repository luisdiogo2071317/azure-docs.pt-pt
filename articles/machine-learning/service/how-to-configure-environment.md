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
ms.date: 11/6/2018
ms.openlocfilehash: fa70e0dfa1f131e38e43faa3d80497d50a52e135
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275219"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar um ambiente de desenvolvimento do Azure Machine Learning

Neste documento, saiba como configurar um ambiente de desenvolvimento para trabalhar com o serviço Azure Machine Learning. O serviço Azure Machine Learning é independente de plataforma. São os únicos requisitos para o seu ambiente de desenvolvimento __Python 3__, __Conda__ (para ambientes isolados) e um ficheiro de configuração que contém as informações da sua área de trabalho do Azure Machine Learning.

Este documento centra-se nos seguintes ambientes específicos e ferramentas:

* [Blocos de notas do Azure](#aznotebooks): serviço de um Jupyter Notebooks alojado na cloud do Azure. É __a mais fácil__ forma de começar a utilizar, como já está instalado o SDK do Azure Machine Learning.
* [A máquina de Virtual de ciência de dados](#dsvm): uma máquina virtual na cloud do Azure, que é __concebido para o trabalho de ciência de dados__. Python 3, Conda, blocos de notas do Jupyter e o SDK do Azure Machine Learning já estão instalados. A VM é fornecido com estruturas, ferramentas e editores para desenvolver soluções de ML ML populares. É provavelmente __os mais completos__ ambiente de desenvolvimento para ML na plataforma do Azure.
* [Blocos de notas do Jupyter](#jupyter): Se já estiver a utilizar blocos de notas do Jupyter, o SDK tem alguns sobrando que deve instalar.
* [Visual Studio Code](#vscode): Se utilizar o Visual Studio Code, existem algumas extensões úteis que podem instalar.

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

A Máquina Virtual de ciência de dados (DSVM) é uma imagem de máquina de virtual (VM) personalizada **concebido para o trabalho de ciência de dados**. Ele inclui:

  - Ferramentas de ciência de dados populares
  - Ambientes de desenvolvimento integrado (IDEs), por exemplo, PyCharm e o RStudio
  - Pacotes como blocos de notas do Jupyter e Tensorflow

O SDK do Azure Machine Learning funciona na versão o Ubuntu ou o Windows da DSVM. Para utilizar DSVM como um ambiente de desenvolvimento, siga estes passos:

1. Para criar uma máquina de Virtual de ciência de dados, siga os passos dos seguintes documentos:

    * [Criar uma Máquina Virtual de ciência de dados do Ubuntu](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)
    * [Criar uma máquina de Virtual de ciência de dados do Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

1. O SDK do Azure Machine Learning é **já instalado** na DSVM. Para utilizar o ambiente de Conda que contém o SDK, utilize um dos seguintes comandos:

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

1. Para instalar um servidor com reconhecimento de conda do bloco de notas do Jupyter e ativar widgets de experimentação, utilize os seguintes comandos:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.widgets
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

1. Para instalar as ferramentas de código do Visual Studio para a extensão de IA, consulte a [Tools para IA](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) página.

    Para obter mais informações, consulte a [utilização VS Code Tools para IA com o Azure Machine Learning](how-to-vscode-tools.md) documento.

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