---
title: Sobre a extensão da CLI do Azure Machine Learning
description: Saiba mais sobre a extensão da CLI de aprendizagem para o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 53e737f35904a90bb56ec15c8a8282f8775e3c3a
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393496"
---
# <a name="what-is-the-azure-machine-learning-cli"></a>O que é da CLI do Azure Machine Learning?

A extensão do Azure Machine Learning Interface de linha de comandos (CLI) destina-se a cientistas de dados e programadores que trabalham com o serviço Azure Machine Learning. Permite-lhe rapidamente a automatizar fluxos de trabalho do machine learning e colocá-los em produção, tais como:
+ Executar experimentações para criar modelos de machine learning

+ Registe-se os modelos de aprendizagem automática para utilização do cliente

+ Empacotar, implementar e controlar o ciclo de vida de seus modelos de machine learning

Do machine learning CLI é uma extensão da [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) e foi criado a partir do Python com base na <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> para o serviço Azure Machine Learning.

> [!NOTE]
> A CLI está atualmente em pré-visualização e será atualizada.

## <a name="installing-and-uninstalling"></a>Instalação e desinstalação

Pode instalar a CLI com o comando da nossa pré-visualização PyPi índice:
```AzureCLI
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Pode remover a CLI com o comando:
```AzureCLI
az extension remove -n azure-cli-ml
```

Pode atualizar a CLI com o **Remova** e **adicionar** passos acima.

## <a name="using-the-cli-vs-the-sdk"></a>Com a CLI vs. o SDK
A CLI é mais adequada, a automatização, por uma pessoa de operações de programação, ou como parte de um pipeline de integração e entrega contínuo. Está otimizado para lidar com tarefas pouco frequentes e altamente parametrizadas. 

Os exemplos incluem:
- computação de aprovisionamento
- submissão de experimentação parametrizado
- registo do modelo, criação de imagens
- Implementação do serviço

Os cientistas de dados são recomendados para utilizar o SDK do Azure ML.

## <a name="common-machine-learning-cli-commands"></a>Aprendizagem comuns comandos da CLI
> [!NOTE]
> Pode utilizar para executar com êxito de ficheiros de exemplo o abaixo comandos podem ser encontradas [aqui.](https://github.com/Azure/MachineLearningNotebooks/tree/cli/cli)

Utilize o conjunto avançado de `az ml` comandos para interagir o serviço em qualquer ambiente de linha de comando, incluindo o shell de cloud de portal do Azure.

Eis um exemplo dos comandos comuns:

### <a name="workspace-creation--compute-setup"></a>Configuração de criação e a computação da área de trabalho

+ Crie uma área de serviço Azure Machine Learning, o recurso de nível superior para o machine learning.
   ```AzureCLI
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Defina o CLI para utilizar esta área de trabalho por predefinição.
   ```AzureCLI
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Crie uma DSVM (ciência de dados VM). Também pode criar clusters de BatchAI para a formação distribuída ou clusters do AKS para a implementação.
  ```AzureCLI
  az ml computetarget setup dsvm -n mydsvm
  ```

### <a name="experiment-submission"></a>Submissão de experimentação
+ Anexe a um projeto (configuração de execução) para submeter uma experimentação. Isto é utilizado para controlar as execuções da experimentação.
  ```AzureCLI
  az ml project attach --experiment-name myhistory
  ```

+ Submeta uma experimentação com o serviço do Azure Machine Learning no destino de computação à sua escolha. Neste exemplo serão executados em seu ambiente de computação local. Certifique-se de que o ficheiro de ambiente de conda captura as suas dependências de python.

  ```AzureCLI
  az ml run submit -c local train.py
  ```

+ Ver uma lista de experimentações submetidas.
```AzureCLI
az ml history list
```

### <a name="model-registration-image-ceation--deployment"></a>Registo de modelo, ceation de imagem e implantação

+ Registe um modelo com o Azure Machine Learning.
  ```AzureCLI
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Crie uma imagem para conter o modelo e as dependências da aprendizagem automática. 
  ```AzureCLI
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Implemente o seu modelo em pacote destinos, incluindo o ACI e o AKS.
  ```AzureCLI
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
    
## <a name="full-command-list"></a>Lista de comando completo
Pode encontrar a lista completa de comandos para a extensão da CLI (e os respetivos parâmetros suportados) executando ```az ml COMMANDNAME -h```. 
