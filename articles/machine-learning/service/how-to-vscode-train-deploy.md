---
title: Utilizar o Visual Studio Code para preparar e implementar modelos com o serviço Azure Machine Learning
description: Saiba mais sobre o Azure Machine Learning para Visual Studio Code e como começar a machine learning e modelos no serviço Azure Machine Learning com o Visual Studio Code de aprendizagem profunda de treinamento e de implantação.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec12
ms.openlocfilehash: e1ea0b867738f72d9ff4baff25b630887b9232c9
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016441"
---
# <a name="use-visual-studio-code-to-train-and-deploy-machine-learning-models"></a>Utilizar o Visual Studio Code para preparar e implementar modelos de machine learning

Neste artigo, ficará a saber como utilizar o **Azure Machine Learning para Visual Studio Code** extensão para preparar e implementar modelos com o serviço Azure Machine Learning no Visual Studio Code (VS Code) de aprendizagem profunda e de aprendizagem automática.

O Azure Machine Learning fornece suporte para a execução de experimentações localmente e destinos de computação remota. Para cada fase experimental, pode manter um registo de várias execuções, muitas vezes, o que precisar tentar iterativamente técnicas diferentes, hiperparâmetros e muito mais. Pode utilizar o Azure Machine Learning para controlar métricas personalizadas e experimente execuções, ativar a capacidade de reprodução de ciência de dados e auditability.

E pode implantar esses modelos para suas necessidades de teste e produção.

## <a name="prerequisites"></a>Pré-requisitos

+ Tem a [do Azure Machine Learning para o VS Code](how-to-vscode-tools.md) configurar a extensão.

+ Tem a [do Azure Machine Learning SDK para Python instalada](how-to-vscode-tools.md) com o VS Code.

+ Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

## <a name="create-and-manage-compute-targets"></a>Criar e gerir os destinos de computação

Com o Azure Machine Learning para o VS Code, pode preparar os dados, formar modelos e implementá-las tanto localmente como destinos de computação remota.

Esta extensão oferece suporte a vários destinos de computação remota diferentes para o Azure Machine Learning. Consulte a [uma lista completa de destinos de computação suportados](how-to-set-up-training-targets.md) para o Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Criar destinos de computação do Azure Machine Learning no VS Code

**Para criar um destino de computação:**

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

2. Na vista de árvore, expanda a sua subscrição do Azure e a área de trabalho do serviço Azure Machine Learning. Na imagem animada, o nome da subscrição é "Versão de avaliação" e a área de trabalho é 'TeamWorkspace'. 

3. Sob o nó de área de trabalho, clique com botão direito a **computação** nó e escolha **criar computação**.

4. Escolha o tipo de destino de computação da lista. 

5. Especifique quaisquer propriedades avançadas no ficheiro de configuração JSON que se abre num novo separador. Pode especificar um nome exclusivo para o destino de computação neste ficheiro.

6. Quando tiver concluído a configurar o destino de computação, clique em **submeter** na parte inferior direita.

Eis um exemplo de computação do Azure Machine Learning (AMLCompute): [ ![criar AML de computação no VS Code](./media/vscode-tools-for-ai/CreateAMLCompute.gif)](./media/vscode-tools-for-ai/CreateAMLCompute.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Utilizar computações remotas para experimentações no VS Code

Para utilizar um destino de computação remota ao treinar, terá de criar um ficheiro de configuração de execução. Este ficheiro diz ao Azure Machine Learning não só onde executar a sua experimentação, mas também como preparar o ambiente.

#### <a name="the-run-configuration-file"></a>O ficheiro de configuração de execução

A extensão do VS Code, irá criar automaticamente uma configuração de execução para a sua **local** e **docker** ambientes no seu computador local.

Este é um trecho do padrão, execute o ficheiro de configuração.

Se pretende instalar todas as suas bibliotecas/dependências por conta própria, defina `userManagedDependencies: True` e, em seguida, execuções da experimentação local irão utilizar o seu ambiente de Python padrão como especificado pela extensão do VS código Python.

```yaml
# user_managed_dependencies=True indicates that the environment will be user managed. False indicates that AzureML will manage the user environment.
    userManagedDependencies: False
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
    docker:
# Set True to perform this run inside a Docker container.
    enabled: false
```

#### <a name="the-conda-dependencies-file"></a>O ficheiro de dependências de conda

Por predefinição, é criado um novo ambiente de conda para e suas dependências de instalação são geridas. No entanto, tem de especificar as suas dependências no `aml_config/conda_dependencies.yml` ficheiro.

Este é um trecho da predefinição "aml_config/conda_dependencies.yml'.
Pode adicionar dependências adicionais no ficheiro de configuração.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

## <a name="train-and-tune-models"></a>Dar formação e otimizar modelos

Utilize o Azure Machine Learning para o VS Code (pré-visualização) para rapidamente reanalisa o seu código, siga os passos e depurar e utilizar a solução de controlo de código de origem à escolha. 

**Para executar a experimentação com o Azure Machine Learning:**

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

1. Na vista de árvore, expanda a sua subscrição do Azure e a área de trabalho do serviço Azure Machine Learning. 

1. Sob o nó de área de trabalho, expanda o **computação** nó e o botão direito do mouse a **executar configuração** de computação que pretende utilizar. 

1. Selecione **execute experimentação**.

1. Clique em **veja a execução da experimentação** para ver o portal do Azure Machine Learning integrado para monitorizar as execuções e ver os modelos de formação.

## <a name="deploy-and-manage-models"></a>Implementar e gerir modelos
O Azure Machine Learning permite implementar e gerir os seus modelos de machine learning na cloud e no edge. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Registar o seu modelo para o Azure Machine Learning a partir do código de VS

Agora que tem a formação seu modelo, pode registá-lo na sua área de trabalho.
Modelos de registado podem ser monitorizados e implementados.

**Para registar o seu modelo:**

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

1. Na vista de árvore, expanda a sua subscrição do Azure e a área de trabalho do serviço Azure Machine Learning.

1. Sob o nó de área de trabalho, clique com botão direito **modelos** e escolha **modelo registar**.

1. Na lista, escolha se pretende carregar um **ficheiro de modelo** (para modelos de únicos) um **pasta modelo** (para modelos com vários ficheiros, como o Tensorflow). 

1. Selecione o ficheiro ou pasta.

1. Quando tiver concluído a configurar as propriedades de seu modelo, clique em **submeter** na parte inferior direita. 



### <a name="deploy-your-service-from-vs-code"></a>Implementar o seu serviço a partir do código VS

Utilizar o VS Code, pode implementar o serviço web para:
+ Instância de contentor do Azure (ACI): para fins de teste
+ O Azure Kubernetes Service (AKS): para produção 

Não é necessário criar um contentor do ACI para testar com antecedência, uma vez que são criados dinamicamente. No entanto, os clusters do AKS é necessário ser configurado com antecedência. 

Saiba mais sobre [implementação com o Azure Machine Learning](how-to-deploy-and-where.md) em geral.

**Para implementar um serviço web:**

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

1. Na vista de árvore, expanda a sua subscrição do Azure e a sua área de trabalho do serviço do Azure Machine Learning.

1. Sob o nó de área de trabalho, expanda o **modelos** nó.

1. Com o botão direito do modelo que pretende implementar e escolha **implementar o serviço do modelo registado** comando no menu de contexto.

1. Na paleta de comandos, escolha o destino de computação para a implementação da lista. 

1. No campo, introduza um nome para este serviço. 

1. Na paleta de comandos, prima a tecla Enter no teclado para procurar e selecione o ficheiro de script.

1. Na paleta de comandos, prima a tecla Enter no teclado para procurar e selecione o ficheiro de dependências de conda.

1. Quando tiver concluído a configurar as propriedades de seu serviço, clique em **submeter** na parte inferior direita. Nesse arquivo de propriedades do serviço, pode especificar um ficheiro de Docker local ou um ficheiro de schema.json que pretende utilizar.

O serviço web agora é implementado.

## <a name="next-steps"></a>Passos Seguintes

Para um passo a passo de treinamento com o Machine Learning fora do VS Code, leia [Tutorial: criar modelos com o Azure Machine Learning](tutorial-train-models-with-aml.md).

Para um passo a passo de editar, executar e depurar o código localmente, consulte o [Python Hello World Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
