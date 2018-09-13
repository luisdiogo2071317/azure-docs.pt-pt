---
title: Instalar e utilizar a CLI para tarefas principais - Azure Machine Learning
description: Saiba como instalar e utilizar a CLI para os mais comuns de machine learning tarefas no Azure Machine Learning.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/10/2018
ms.openlocfilehash: 0e37e1839d2248507a30de08e2ac4c975bd3b859
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648283"
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Instalar e utilizar o machine learning CLI para tarefas principais do Azure Machine Learning

Serviços de Machine Learning do Azure são uma ciência de dados ponto a ponto integrada e solução de análise avançada. Os cientistas de dados profissionais podem utilizar os serviços do Azure Machine Learning para preparar dados, desenvolverem experimentações e implementarem modelos à escala da cloud. 

O Azure Machine Learning fornece uma interface de linha de comandos (CLI) que pode:
+ Gerir a sua área de trabalho e projetos
+ Configurar destinos de computação
+ Executar experiências de treinamento
+ Ver o histórico e as métricas para execuções anteriores
+ Implementar modelos para produção como serviços da web
+ Gerir modelos de produção e serviços

Este artigo apresenta alguns dos comandos da CLI mais úteis para sua comodidade. 

![Do Azure Machine Learning CLI](media/cli-for-azure-machine-learning/flow.png)

## <a name="what-you-need-to-get-started"></a>O que precisa para começar a utilizar

Precisa de acesso de contribuinte para uma subscrição do Azure ou um grupo de recursos onde pode implementar os seus modelos. Além disso, terá de instalar o Azure Machine Learning Workbench para executar a CLI. 

>[!IMPORTANT]
>A CLI, entregue com os serviços do Azure Machine Learning é diferente da [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), que é utilizado para gerir recursos do Azure.

## <a name="get-and-start-cli"></a>E comece a CLI

Para obter esta CLI, instale o Azure Machine Learning Workbench, que pode ser baixado a partir daqui:
    + Windows - https://aka.ms/azureml-wb-msi 
    + MacOS - https://aka.ms/azureml-wb-dmg 

Para iniciar a CLI:
+ No Azure Machine Learning Workbench, iniciar a CLI do menu **ficheiro -> Abrir linha de comandos.**

## <a name="get-command-help"></a>Obtenha a ajuda do comando 

Pode obter informações adicionais sobre os comandos da CLI usando `--debug` ou `--help` depois dos comandos como `az ml <xyz> --debug` onde `<xyz>` é o nome do comando. Por exemplo:
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Tarefas comuns da CLI do Azure Machine Learning 

Saiba mais sobre as tarefas mais comuns que pode realizar com a CLI nesta secção, incluindo:
+ [Configurar destinos de computação](#target)
+ [Submeter tarefas para execução remota](#jobs)
+ [Trabalhar com blocos de notas do Jupyter](#jupyter)
+ [Interagir com os históricos de execução](#history)
+ [Configurar o ambiente para operacionalizar](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Configurar um destino de computação

Pode computar o seu modelo de machine learning em diferentes destinos, incluindo:
+ no modo local
+ numa VM (DSVM) de ciência de dados
+ num cluster do HDInsight

Para anexar um destino de VM de ciência de dados:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

Para anexar um destino de HDInsight:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

Dentro de **aml_config** pasta, pode alterar as dependências conda. 

Além disso, pode operar com o Python numa GPU DSVM, Python ou PySpark. 

Para definir o modo de operação de Python:
+ Para o Python, adicionar `Framework:Python` no `<target name>.runconfig` 

+ Para PySpark, adicionar `Framework:PySpark` no `<target name>.runconfig` 

+ Para o Python numa GPU DSVM,
    1. Adicionar `Framework:Python` no `<target name>.runconfig` 

    1. Além disso, adicione `baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9 and nvidiaDocker:true` no `<target name>.compute`

Para preparar o destino de computação:
```azurecli
az ml experiment prepare -c <target name>
```

>[!TIP]
>Para mostrar a sua subscrição:<br/>
>`az account show`<br/>
>
>Para definir a sua subscrição:<br/>
>`az account set –s "my subscription name" `

<a name="jobs"></a>

### <a name="submit-remote-jobs"></a>Submeter tarefas remotas

Para submeter uma tarefa para um destino remoto:
```azurecli
az ml experiment submit -c <target name> myscript.py
```

<a name="jupyter"></a>

### <a name="work-with-jupyter-notebooks"></a>Trabalhar com blocos de notas do Jupyter

Para iniciar um bloco de notas do Jupyter:
```azurecli
az ml notebook start
```

Este comando inicia um bloco de notas do Jupyter no localhost. Pode trabalhar no local ao selecionar o kernel de Python 3 ou trabalhar na sua VM remota ao selecionar o kernel `<target name>`.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Interagir com e explore o histórico de execuções

Para listar o histórico de execuções:
```azurecli
az ml history list -o table
```

Para listar todas as concluído execuções:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

Para encontrar é executado com a maior precisão:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

Também pode transferir os ficheiros gerados por cada execução. 

Para promover um modelo que é guardado nas saídas de pasta:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

Para transferir este modelo:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Configurar o ambiente para operacionalizar

Para configurar o ambiente de operacionalização, tem de criar:

> [!div class="checklist"]
> * Um grupo de recursos 
> * Uma conta de armazenamento
> * Um Azure Container Registry (ACR)
> * Uma conta de informações da aplicação
> * Uma implementação de Kubernetes num cluster do Azure Container Service (ACS)


Para configurar uma implementação local para testar num contentor do Docker:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

Para configurar um cluster do ACS kubernetes:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name> --cluster
```

Para monitorizar o estado da implementação:
```azurecli
az ml env show -n <environment name> -g <resource group name>
```

Para definir o ambiente que deve ser utilizado:
```azurecli
az ml env set -n <environment name> -g <resource group name>
```

## <a name="next-steps"></a>Passos Seguintes

Comece com um dos seguintes artigos: 
+ [Instalar e começar a utilizar o Azure Machine Learning](../service/quickstart-installation.md)
+ [Classificar íris dados Tutorial: Parte 1](tutorial-classifying-iris-part-1.md)

Aprofunde-se com um dos seguintes artigos:
+ [Comandos da CLI para gerir modelos](model-management-cli-reference.md)
