---
title: Configurar um ambiente de desenvolvimento do Python
titleSuffix: Azure Machine Learning service
description: Saiba como configurar um ambiente de desenvolvimento, quando trabalha com o serviço Azure Machine Learning. Neste artigo, saiba como utilizar ambientes de Conda, criar arquivos de configuração e configurar o Jupyter Notebooks, blocos de notas do Azure, IDEs, editores de código e a máquina de Virtual de ciência de dados.
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
ms.openlocfilehash: 46a1872d2ac5d1670620148edf7ee273580826d3
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811278"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar um ambiente de desenvolvimento do Azure Machine Learning

Neste artigo, irá aprender a configurar um ambiente de desenvolvimento para trabalhar com o serviço Azure Machine Learning. Serviço de Machine Learning é independente de plataforma. 

Os únicos requisitos para o seu ambiente de desenvolvimento são Python 3, Conda (para ambientes isolados) e um ficheiro de configuração que contém as informações da sua área de trabalho do Azure Machine Learning.

Este artigo enfoca os ambientes e ferramentas que se seguem:

* [Blocos de notas do Azure](#aznotebooks): Um serviço de blocos de notas do Jupyter que está alojado na cloud do Azure. É a maneira mais fácil para começar, porque já está instalado o SDK do Azure Machine Learning.

* [A máquina de Virtual de ciência de dados (DSVM)](#dsvm): Um ambiente de desenvolvimento ou experimentação pré-configurado na cloud do Azure foi concebido para o trabalho de ciência de dados e pode ser implementada para instâncias de VM apenas de CPU ou as instâncias baseadas em GPU. Python 3, Conda, blocos de notas do Jupyter e o SDK do Azure Machine Learning já estão instalados. A VM é fornecido com popular aprendizagem automática e aprendizagem profunda editores, ferramentas e estruturas para o desenvolvimento de soluções de aprendizagem automática. Provavelmente é o ambiente de desenvolvimento mais completo do Machine learning na plataforma do Azure.

* [O bloco de notas do Jupyter](#jupyter): Se já estiver a utilizar o bloco de notas do Jupyter, o SDK tem alguns sobrando que deve instalar.

* [Visual Studio Code](#vscode): Se usar o Visual Studio Code, ele tem algumas extensões úteis que podem instalar.

* [O Azure Databricks](#aml-databricks): Uma plataforma de análise de dados populares baseada no Apache Spark. Saiba como obter o SDK do Azure Machine Learning no seu cluster, de modo a que possa implementar modelos.

Se já tiver um ambiente de Python 3 ou deseja apenas os passos básicos para instalar o SDK, consulte a [computador Local](#local) secção.

## <a name="prerequisites"></a>Pré-requisitos

- Uma área de trabalho de serviço do Azure Machine Learning. Para criar a área de trabalho, consulte [introdução ao serviço Azure Machine Learning](quickstart-get-started.md).

- Ambos os [Continuum Anaconda](https://www.anaconda.com/download/) ou [Miniconda](https://conda.io/miniconda.html) Gestor de pacotes.

    > [!IMPORTANT]
    > Anaconda e Miniconda não são necessários quando estiver a utilizar blocos de notas do Azure.

- No Linux ou macOS, tem do shell de bash.

    > [!TIP]
    > Se estiver no Linux ou macOS e utiliza uma shell que não seja o bash (por exemplo, zsh) poderá receber erros ao executar alguns comandos. Para contornar este problema, utilize o `bash` comando para iniciar um novo shell do bash e execute os comandos lá.

- No Windows, terá da linha de comandos ou prompt Anaconda (por Anaconda e Miniconda instalado).

## <a id="anotebooks"></a>Blocos de notas do Azure

[Blocos de notas do Azure](https://notebooks.azure.com) (pré-visualização) é um ambiente de desenvolvimento interativo na cloud do Azure. É a maneira mais fácil para começar a utilizar com o desenvolvimento do Azure Machine Learning.

* Já está instalado o SDK do Azure Machine Learning.
* Depois de criar uma área de trabalho do serviço do Azure Machine Learning no portal do Azure, pode clicar num botão para configurar automaticamente o ambiente de bloco de notas do Azure para trabalhar com a área de trabalho.

Para começar a desenvolver com blocos de notas do Azure, veja [introdução ao serviço Azure Machine Learning](quickstart-get-started.md).

## <a id="dsvm"></a>Máquina de Virtual de ciência de dados

A DSVM é uma imagem personalizada de máquina virtual (VM). Foi concebido para o trabalho de ciência de dados que está pré-configurada com:

  - Como o TensorFlow, PyTorch, Scikit-saiba, XGBoost e o Azure Machine Learning SDK pacotes.
  - Ferramentas de ciência de dados populares, tais como Spark autónomo e exploração.
  - Ferramentas do Azure, como a CLI do Azure, o AzCopy e o Explorador de armazenamento.
  - Ambientes de desenvolvimento integrado (IDEs), como o Visual Studio Code, PyCharm e RStudio.
  - Servidor de bloco de notas do Jupyter. 

O SDK do Azure Machine Learning funciona na versão o Ubuntu ou o Windows da DSVM. Para utilizar a DSVM como um ambiente de desenvolvimento, faça o seguinte:

1. Crie uma DSVM em qualquer um dos seguintes ambientes:

    * O portal do Azure:

        * [Criar uma máquina de Virtual de ciência de dados do Ubuntu](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Criar uma máquina de Virtual de ciência de dados do Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * A CLI do Azure:

        > [!IMPORTANT]
        > * Ao utilizar a CLI do Azure, tem primeiro de iniciar sessão sua subscrição do Azure utilizando o `az login` comando.
        >
        > * Ao utilizar os comandos neste passo, tem de fornecer um nome de grupo de recursos, um nome para a VM, um nome de utilizador e uma palavra-passe.

        * Para criar uma máquina de Virtual de ciência de dados do Ubuntu, utilize o seguinte comando:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Para criar uma máquina de Virtual de ciência de dados do Windows, utilize o seguinte comando:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. O SDK do Azure Machine Learning já está instalado no DSVM. Para utilizar o ambiente de Conda que contém o SDK, utilize um dos seguintes comandos:

    * Para DSVM em Ubuntu:

        ```shell
        conda activate py36
        ```

    * Para Windows DSVM:

        ```shell
        conda activate AzureML
        ```

1. Para verificar se pode acessar o SDK e verificar a versão, utilize o seguinte código de Python:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Para configurar a DSVM para utilizar a sua área de trabalho do serviço do Azure Machine Learning, consulte a [criar um ficheiro de configuração da área de trabalho](#workspace) secção.

Para obter mais informações, consulte [máquinas de virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Computador local

Quando estiver a utilizar um computador local (que também pode ser uma máquina virtual remota), criar um ambiente de Conda e instalar o SDK, fazendo o seguinte:

1. Abra uma linha de comandos ou shell.

1. Crie um ambiente de Conda com os seguintes comandos:

    ```shell
    # create a new Conda environment with Python 3.6, NumPy, and Cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the Conda environment
    conda activate myenv

    # On macOS run
    source activate myenv
    ```

    Poderá demorar vários minutos para criar o ambiente, se o Python 3.6 e outros componentes têm de ser transferido.

1. Instale o SDK do Azure Machine Learning com alguns extras de bloco de notas e o SDK de preparação de dados com o seguinte comando:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   > [!NOTE]
   > Se obtiver uma mensagem que não pode ser desinstalado PyYAML, utilize o seguinte comando em vez disso:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Poderá demorar vários minutos para instalar o SDK.

1. Instale pacotes para o experimentação da aprendizagem automática. Utilize o seguinte comando e substitua  *\<novo pacote >* com o pacote que pretende instalar:

    ```shell
    conda install <new package>
    ```

1. Para verificar se o SDK é instalado, utilize o seguinte código de Python:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Blocos de notas do Jupyter

Blocos de notas do Jupyter fazem parte do [Jupyter projeto](https://jupyter.org/). Eles fornecem uma experiência interativa de codificação onde criar documentos que misturam código em direto com texto narrativo e gráficos. Blocos de notas do Jupyter também são uma ótima maneira de compartilhar seus resultados com outras pessoas, uma vez que pode salvar a saída de suas seções de código no documento. Pode instalar o Jupyter Notebooks numa variedade de plataformas.

O procedimento a [computador Local](#local) secção instala componentes opcionais para blocos de notas do Jupyter. Para ativar a esses componentes no seu ambiente de bloco de notas do Jupyter, faça o seguinte:

1. Abra uma linha de comandos ou shell.

1. Para instalar um servidor de bloco de notas do Jupyter com reconhecimento de Conda, utilize o seguinte comando:

    ```shell
    # install Jupyter
    conda install nb_conda
    ```

1. Abra o bloco de notas do Jupyter com o seguinte comando:

    ```shell
    jupyter notebook
    ```

1. Para verificar se o bloco de notas do Jupyter pode utilizar o SDK, abra um novo bloco de notas, selecione **myenv** como sua kernel e, em seguida, execute o seguinte comando numa célula de bloco de notas:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para configurar o bloco de notas do Jupyter para utilizar a sua área de trabalho do serviço do Azure Machine Learning, vá para o [criar um ficheiro de configuração da área de trabalho](#workspace) secção.

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code é um editor de código entre plataformas. Ele se baseia numa instalação de Python 3 e Conda local para o suporte de Python, mas ele fornece ferramentas adicionais para trabalhar com IA. Ele também fornece suporte para selecionar o ambiente de Conda a partir do editor de código.

Para utilizar o Visual Studio Code para desenvolvimento, faça o seguinte:

1. Para saber como utilizar o Visual Studio Code para o desenvolvimento de Python, veja [introdução ao Python no VSCode](https://code.visualstudio.com/docs/python/python-tutorial).

1. Para selecionar o ambiente de Conda, abra o VS Code e, em seguida, selecione Ctrl + Shift + P (Linux e Windows) ou comando + Shift + P (Mac).  
    O __palete de comando__ abre. 

1. Introduza __Python: Selecione o interpretador__e, em seguida, selecione o ambiente de Conda.

1. Para validar que pode utilizar o SDK, criar e, em seguida, execute um novo ficheiro de Python (. PY) que contém o seguinte código:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para instalar a extensão do Azure Machine Learning para Visual Studio Code, veja [Tools para IA](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Para obter mais informações, consulte [utilização do Azure Machine Learning para Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Pode utilizar uma versão personalizada do Azure Machine Learning SDK para o Azure Databricks para aprendizagem automática personalizada do ponto-a-ponto. Ou pode preparar o seu modelo dentro do Databricks e implantá-lo usando [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code).

Para preparar o seu cluster do Databricks e obter blocos de notas de exemplo:

1. Criar uma [cluster do Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) com uma versão de runtime do Databricks do 4.x (alta simultaneidade preferida) com o Python 3. 

1. Para instalar e anexar o Azure Machine Learning SDK para Python `azureml-sdk[databricks]` PyPi pacote ao seu cluster [criar uma biblioteca](https://docs.databricks.com/user-guide/libraries.html#create-a-library).  
    Quando tiver terminado, a biblioteca está ligada, conforme mostrado na imagem seguinte. Lembre-se de que esses [problemas comuns do Databricks](resource-known-issues.md#databricks).

   ![SDK instalado no Databricks ](./media/how-to-azure-machine-learning-on-databricks/sdk-installed-on-databricks.jpg)

   Se este passo falhar, reinicie o cluster ao fazer o seguinte:

   a. No painel esquerdo, selecione **Clusters**. 
   
   b. Na tabela, selecione o nome do cluster. 

   c. Sobre o **bibliotecas** separador, selecione **reiniciar**.

1. Transfira o [ficheiro de arquivo do bloco de notas do Azure Databricks/Azure Machine Learning SDK](https://github.com/Azure/MachineLearningNotebooks/blob/master/databricks/Databricks_AMLSDK_github.dbc).

   >[!Warning]
   > Muitos blocos de notas de exemplo estão disponíveis para utilização com o serviço Azure Machine Learning. Apenas [estes blocos de notas do exemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) profissional com o Azure Databricks.
   > 

1.  [Importar este ficheiro de arquivo](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) para o seu Databricks do cluster e comece a explorar, tal como descrito no [blocos de notas do Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) página.


## <a id="workspace"></a>Criar um ficheiro de configuração da área de trabalho

O ficheiro de configuração da área de trabalho é um ficheiro JSON que informa o SDK como se comunicar com a sua área de trabalho do serviço do Azure Machine Learning. O ficheiro é denominado *config*, e ele tem o seguinte formato:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Este ficheiro JSON tem de ser a estrutura do diretório que contém os scripts de Python ou blocos de notas do Jupyter. Pode ter o mesmo diretório, um subdiretório nomeado *aml_config*, ou num diretório principal.

Para utilizar este ficheiro a partir do código, utilize `ws=Workspace.from_config()`. Esse código carrega as informações do arquivo e liga-se a sua área de trabalho.

Pode criar o ficheiro de configuração de três formas:

* **Siga os [guia de início rápido do Azure Machine Learning](quickstart-get-started.md)**: R *config* ficheiro é criado na sua biblioteca de blocos de notas do Azure. O ficheiro contém as informações de configuração para a área de trabalho. Pode transferir ou copiar o *config* para outros ambientes de desenvolvimento.

* **Criar o ficheiro manualmente**: Com este método, utilize um editor de texto. Pode encontrar os valores que ir para o ficheiro de configuração ao visitar a sua área de trabalho do [portal do Azure](https://portal.azure.com). Copie o nome de área de trabalho, grupo de recursos e os valores de ID de subscrição e utilizá-los no ficheiro de configuração.

     ![Portal do Azure](./media/how-to-configure-environment/configure.png)

* **Criar o ficheiro através de programação**: O fragmento de código seguinte, vai ligar a uma área de trabalho, fornecendo o ID de subscrição, o grupo de recursos e o nome de área de trabalho. Em seguida, guardar a configuração de área de trabalho para o ficheiro:

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

    Esse código grava o ficheiro de configuração para o *aml_config/config.json* ficheiro.

## <a name="next-steps"></a>Passos Seguintes

- [Preparar um modelo no Azure Machine Learning com o conjunto de dados MNIST](tutorial-train-models-with-aml.md)
- [Do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk)
- [SDK de Preparação de Dados do Azure Machine Learning](https://aka.ms/data-prep-sdk)
