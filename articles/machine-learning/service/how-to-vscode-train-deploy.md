---
title: Usando o Visual Studio Code Tools para a extensão de IA com o Azure Machine Learning
description: Saiba mais sobre o Visual Studio Code Tools para IA e como começar a formação e implementar modelos com o serviço Azure Machine Learning no VS Code de aprendizagem profunda e de aprendizagem automática.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: 3a5ee8795dabcbf9e35e16a8ba0f9c0eb5490ead
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242975"
---
# <a name="vs-code-tools-for-ai-train-and-deploy-ml-models-from-vs-code"></a>VS Code Tools para IA: preparar e implementar modelos de ML a partir do VS Code
Neste artigo, ficará a saber como utilizar o **VS Code Tools para IA** extensão para preparar e implementar modelos com o serviço Azure Machine Learning no VS Code de aprendizagem profunda e de aprendizagem automática.

O Azure Machine Learning fornece suporte para a execução de experimentações localmente e destinos de computação remota. Para cada fase experimental, pode manter um registo de várias execuções, muitas vezes, o que precisar tentar iterativamente técnicas diferentes, hiperparâmetros e muito mais. Pode utilizar o Azure Machine Learning para controlar métricas personalizadas e experimente execuções, ativar a capacidade de reprodução de ciência de dados e auditability.

E pode implantar esses modelos para suas necessidades de teste e produção.

## <a name="prerequisites"></a>Pré-requisitos

+ [Tem o VS Code Tools para IA](how-to-vscode-tools.md) configurados para o Azure Machine Learning.

+ Tem a [do Azure Machine Learning SDK para Python instalada](how-to-vscode-tools.md) com o VS Code.

+ Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-and-manage-compute-targets"></a>Criar e gerir os destinos de computação

Com o Visual Studio Code Tools para IA, pode preparar os dados, formar modelos e implementá-las tanto localmente como destinos de computação remota.

Esta extensão oferece suporte a vários destinos de computação remota diferentes para o Azure Machine Learning. Consulte a [uma lista completa de destinos de computação suportados](how-to-set-up-training-targets.md) para o Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Criar destinos de computação do Azure Machine Learning no VS Code

**Para criar um destino de computação:**

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

2. Na vista de árvore, expanda a sua subscrição do Azure e a área de trabalho do serviço Azure Machine Learning. Na imagem animada, o nome da subscrição é 'OpenMind Studio' e a área de trabalho é 'MyWorkspace'. 

3. Sob o nó de área de trabalho, clique com botão direito a **computação** nó e escolha **criar computação**.

4. Escolha o tipo de destino de computação da lista. 

5. No campo, introduza um nome exclusivo para este destino de computação e especificar o tamanho da máquina virtual.

6. Especifique quaisquer propriedades avançadas no ficheiro de configuração JSON que se abre num novo separador. 

7. Quando tiver concluído a configurar o destino de computação, clique em **concluir** na parte inferior direita.

Eis um exemplo do Azure Batch AI: [ ![computação de criar o Azure Batch AI no VS Code](./media/vscode-tools-for-ai/createcompute.gif)](./media/vscode-tools-for-ai/createcompute.gif#lightbox)

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

Utilize o Azure Machine Learning a partir do código de VS rapidamente reanalisa o seu código, siga os passos e depurar e utilizar a solução de controlo de código de origem à escolha. 

**Para executar a experimentação com o Azure Machine Learning:**

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

1. Na vista de árvore, expanda a sua subscrição do Azure e a área de trabalho do serviço Azure Machine Learning. Na imagem animada, o nome da subscrição é 'OpenMind Studio' e a área de trabalho é 'MyWorkspace'. 

1. Sob o nó de área de trabalho, expanda o **computação** nó e o botão direito do mouse a **executar configuração** de computação que pretende utilizar. 

1. Selecione **execute experimentação**.

1. Clique em **veja a execução da experimentação** para ver o portal do Azure Machine Learning integrado para monitorizar as execuções e ver os modelos de formação.

   [![Execute a experimentação de aprendizagem do VS Code](./media/vscode-tools-for-ai/runexperiment.gif)](./media/vscode-tools-for-ai/runexperiment.gif#lightbox)

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

1. Utilize a caixa de diálogo do selecionador de ficheiros para selecionar o ficheiro ou pasta.

   [![Computação](./media/vscode-tools-for-ai/registermodel.gif)](./media/vscode-tools-for-ai/registermodel.gif#lightbox)

> [!Warning]
> Por enquanto, remova as etiquetas do ficheiro json gerado.

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

1. Na paleta de comandos de código VS, escolha o destino de computação para a implementação da lista. 

1. No campo, introduza um nome para este serviço. 

1. Na caixa de diálogo no canto inferior direito, clique em **procurar** e selecione o seu script de classificação. Fecha a caixa de diálogo.

1. Se tiver um ficheiro de Docker local, clique em **procurar** na segunda caixa de diálogo que aparece. 

   Se cancelar a caixa de diálogo e não especificar um ficheiro local do Docker, um "Azure Machine Learning" um é utilizado por predefinição.

1. Na terceira caixa de diálogo que aparece, clique em **procurar** e selecionar o caminho do ficheiro local conda ou introduzir o caminho do ficheiro no editor de json mais tarde.

1. Se possui um arquivo de schema.json que pretende utilizar, clique em **procurar** na quarta caixa de diálogo que aparece e selecione o ficheiro.

O serviço web agora é implementado.

Eis um exemplo para a instância de contentor do Azure: [ ![instância de contentor do Azure do VS Code](./media/vscode-tools-for-ai/deploy.gif)](./media/vscode-tools-for-ai/deploy.gif#lightbox)

## <a name="next-steps"></a>Passos Seguintes

Para um passo a passo de treinamento com o Machine Learning fora do VS Code, leia [Tutorial: criar modelos com o Azure Machine Learning](tutorial-train-models-with-aml.md).

Para um passo a passo de editar, executar e depurar o código localmente, consulte o [Python Hello World Tutorial](https://code.visualstudio.com/docs/languages/python/docs/python/python-tutorial)
