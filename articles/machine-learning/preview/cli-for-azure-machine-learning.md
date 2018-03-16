---
title: Instalar e utilizar a CLI de aprendizagem para as principais tarefas do Azure Machine Learning
description: Saiba como instalar e utilizar a CLI para o mais comuns de machine learning tarefas no Azure Machine Learning.
services: machine-learning
author: haining
ms.author: haining
manager: mwinkler
ms.reviewer: mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 03/10/2018
ms.openlocfilehash: f34c247728c854c47f486925d440eee0dc5b1945
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Instalar e utilizar a CLI de aprendizagem para as principais tarefas no Azure Machine Learning

Serviços de Machine Learning do Azure são uma ciência de dados ponto-a-ponto integrada e avançadas a solução de análise. Cientistas de dados profissional podem utilizar os serviços do Azure Machine Learning para preparar dados, experimentações de desenvolver e implementar modelos na escala da nuvem. 

O Azure Machine Learning fornece uma interface de linha de comandos (CLI) que pode:
+ Gerir a sua área de trabalho e projetos
+ Configurar destinos de computação
+ Executar experimentações de formação
+ Ver o histórico e métricas para execuções passadas
+ Implementar modelos em produção como serviços web
+ Gerir serviços e modelos de produção

Este artigo apresenta alguns dos comandos CLI mais úteis para sua comodidade. 

![Do Azure Machine Learning CLI](media/cli-for-azure-machine-learning/flow.png)

>[!NOTE]
>A CLI entregue com os serviços do Azure Machine Learning é diferente do [CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest), que é utilizado para gerir recursos do Azure.

## <a name="get-and-start-cli"></a>Obter e comece a CLI

Para obter esta CLI, instale o Azure Machine Learning Workbench, que pode ser transferido a partir daqui:
    + Windows - https://aka.ms/azureml-wb-msi 
    + MacOS - https://aka.ms/azureml-wb-dmg 

Para iniciar o CLI:
+ No Azure Machine Learning Workbench, inicie o CLI no menu **ficheiro -> Abra a linha de comandos.**

## <a name="get-command-help"></a>Obter ajuda do comando 

Pode obter informações adicionais sobre a utilização de comandos da CLI `--debug` ou `--help` depois dos comandos como `az ml <xyz> --debug` onde `<xyz>` é o nome do comando. Por exemplo:
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Tarefas comuns da CLI do Azure Machine Learning 

Saiba mais sobre as tarefas mais comuns que pode realizar com o CLI nesta secção, incluindo:
+ [Configurar destinos de computação](#target)
+ [Submeter tarefas de execução remota](#jobs)
+ [Trabalhar com blocos de notas do Jupyter](#jupyter)
+ [Interagir com histories execução](#history)
+ [Configurar o ambiente para operacionalizar](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Configurar um destino de computação

Pode calcular o modelo de machine learning no outros destinos, incluindo:
+ no modo local
+ numa VM (DSVM) de ciência de dados
+ num cluster do HDInsight

Para ligar a um destino de VM de ciência de dados:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

Para ligar a um destino de HDInsight:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

Dentro do **aml_config** pasta, pode alterar as dependências de conda. 

Além disso, pode operar com PySpark, Python ou Python no DSVM GPU. 

Para definir o modo de funcionamento do Python:
+ Para o Python, adicionar `Framework:Python` no `<target name>.runconfig` 

+ Para PySpark, adicione `Framework:PySpark` no `<target name>.runconfig` 

+ Para Python num DSVM de GPU
    1. Adicionar `Framework:Python` no `<target name>.runconfig` 

    1. Além disso, adicionar `baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9 and nvidiaDocker:true` no `<target name>.compute`

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

Este comando inicia um bloco de notas do Jupyter no localhost. Pode trabalhar no local, selecionando o kernel do Python 3 ou funcionar na sua VM remoto, selecionando o kernel `<target name>`.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Interagir e explorar o histórico de execução

Para listar o histórico de execução:
```azurecli
az ml history list -o table
```

Para listar todas as concluída executa:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

Para localizar é executado com o melhor exatidão:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

Também pode transferir os ficheiros gerados por cada execução. 

Para promover um modelo que é guardado nas saídas da pasta:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

Para transferir esse modelo:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Configurar o ambiente para operacionalizar

Para configurar o ambiente de operationalization, tem de criar:

> [!div class="checklist"]
> * Um grupo de recursos 
> * uma conta de armazenamento
> * Um registo de contentor do Azure (ACR)
> * Uma conta de conhecimentos aprofundados da aplicação
> * Uma implementação de Kubernetes num cluster do serviço de contentor do Azure (ACS)


Para configurar uma implementação local para um contentor de Docker de teste:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

Para configurar um cluster de ACS com Kubernetes:
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

## <a name="next-steps"></a>Passos seguintes

Começar com um dos seguintes artigos: 
+ [Instalar e começar a utilizar o Azure Machine Learning](quickstart-installation.md)
+ [Classificar dados Iris Tutorial: Parte 1](tutorial-classifying-iris-part-1.md)

Aprofundar-se com um dos seguintes artigos:
+ [Comandos da CLI para gerir modelos](model-management-cli-reference.md)