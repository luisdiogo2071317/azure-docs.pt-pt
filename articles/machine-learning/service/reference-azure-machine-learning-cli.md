---
title: Como utilizar a extensão da CLI do Azure Machine Learning
description: Saiba mais sobre a extensão da CLI do Azure Machine Learning para a CLI do Azure. A CLI do Azure é um utilitário de linha de comandos para várias plataformas que lhe permite trabalhar com os recursos na cloud do Azure. A extensão de Machine Learning permite-lhe trabalhar com o serviço do Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 13d09471191deed670db97a9f18e15bc9577dd1a
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713423"
---
# <a name="use-the-azure-machine-learning-cli-extension"></a>Utilizar a extensão da CLI do Azure Machine Learning

A CLI do Azure Machine Learning é uma extensão para o [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), uma interface de linha de comandos para várias plataformas para a plataforma Azure. Esta extensão fornece comandos para trabalhar com o serviço Azure Machine Learning na linha de comando. Permite-lhe criar scripts que automatizam o fluxos de trabalho da aprendizagem automática. Por exemplo, pode criar scripts que executem as seguintes ações:

+ Executar experimentações para criar modelos de machine learning

+ Registe-se os modelos de aprendizagem automática para utilização do cliente

+ Empacotar, implementar e controlar o ciclo de vida de seus modelos de machine learning

A CLI não é uma substituição para o SDK do Azure Machine Learning. É uma ferramenta complementar otimizado para lidar com tarefas altamente parametrizadas, tais como:

* Criação de recursos de computação

* submissão de experimentação parametrizado

* Registo do modelo

* Criação de imagens

* Implementação do serviço

## <a name="prerequisites"></a>Pré-requisitos

* O [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!NOTE]
> Para utilizar a CLI, tem de ter uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

## <a name="install-the-extension"></a>Instalar a extensão

Para instalar a extensão da CLI do Machine Learning, utilize o seguinte comando:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Quando lhe for pedido, selecione `y` para instalar a extensão.

Para verificar se a extensão foi instalada, utilize o seguinte comando para apresentar uma lista de subcomandos do ML específicas:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Para atualizar a extensão tem __Remova__ e, em seguida __instalar__ -lo. Esta ação instala a versão mais recente.

## <a name="remove-the-extension"></a>Remover a extensão

Para remover a extensão da CLI, utilize o seguinte comando:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Gestão de recursos

Os comandos seguintes demonstram como utilizar a CLI para gerir os recursos utilizados pelo Azure Machine Learning.


+ Crie uma área de trabalho do serviço do Azure Machine Learning:

   ```azurecli-interactive
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Defina uma área de trabalho predefinido:

   ```azurecli-interactive
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Crie uma DSVM (ciência de dados VM). Também pode criar clusters de BatchAI para a formação distribuída ou clusters do AKS para a implementação.


  ```azurecli-interactive
  az ml computetarget setup dsvm -n mydsvm
  ```

## <a name="experiments"></a>Experimentações

Os comandos seguintes demonstram como utilizar a CLI para trabalhar com experiências:

* Anexe um projeto (configuração de execução) antes de submeter uma experimentação:

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Inicie uma execução da sua experimentação. Quando utilizar este comando, especifique um destino de computação. Neste exemplo, `local` utiliza o computador local para treinar o modelo ao utilizar o `train.py` script:

    ```azurecli-interactive
    az ml run submit -c local train.py
    ```

* Ver uma lista de experimentações submetidas:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Registo de modelo, criação de imagens e implantação

Os comandos seguintes demonstram como registar um modelo preparado e, em seguida, implementá-la como um serviço de produção:

+ Registe um modelo com o Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Crie uma imagem que contém o modelo e as dependências da aprendizagem automática: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Implemente uma imagem para um destino de computação:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
