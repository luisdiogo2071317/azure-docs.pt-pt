---
title: Migrar da bancada de trabalho
titleSuffix: Azure Machine Learning service
description: Saiba como atualizar ou migrar para a versão final do serviço Azure Machine Learning a partir de uma versão anterior.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: haining
author: haining
ms.date: 09/24/2018
ms.openlocfilehash: cc60fd6a9d5f154d26fc9c495f190296453a0db0
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106656"
---
# <a name="migrate-from-workbench-to-the-latest-version-of-azure-machine-learning-service"></a>Migrar da bancada de trabalho para a versão mais recente do serviço Azure Machine Learning 

**Se tiver instalado a aplicação Workbench e/ou ter experimentação e contas de pré-visualização de gestão de modelo, utilize este artigo para migrar para a versão mais recente.**  Se não tiver pré-visualização Workbench instalado, ou uma experimentação e/ou conta de gestão de modelos, não precisa de migrar nada.

## <a name="what-can-i-migrate"></a>O que posso migrar?
A maioria dos artefactos criados na primeira pré-visualização do serviço Azure Machine Learning são armazenados no seu próprio local ou de armazenamento na nuvem. Estes artefactos não desaparecerem. Para migrar, volte a registar os artefactos com o serviço Azure Machine Learning atualizado. 

A seguinte tabela e artigo explicam o que pode fazer com os seus ativos existentes e os recursos antes ou depois de mover-se ao longo para a versão mais recente do serviço Azure Machine Learning. Também pode continuar a utilizar a versão anterior e seus ativos durante algum tempo ([ver linha cronológica de suporte de transição](overview-what-happened-to-workbench.md#timeline)).

|Ativo ou no recurso da versão antiga|Posso migrar?|Ações|
|-----------------|:-------------:|-------------|
|Modelos (como arquivos locais) de Machine learning|Sim|Nenhum. Funciona como antes.|
|Dependências de modelos e esquemas (como arquivos locais)|Sim|Nenhum. Funciona como antes.|
|Projetos|Sim|[Anexar a pasta local para a nova área de trabalho](#projects).|
|Históricos de execução|Não|[Que pode ser baixado](#history) durante algum tempo.|
|Ficheiros de preparação de dados|Não|[Preparar a qualquer conjunto de dados de tamanho](#dataprep) para modelagem usando o novo SDK do Azure Machine Learning Data Prep ou utilizar o Azure Databricks.|
|Destinos de computação|Não|Registá-los na nova área de trabalho.|
|Modelos de registados|Não|Volte a registar o modelo numa nova área de trabalho.|
|Manifestos registados|Não|Nenhum. Os manifestos já não existe como um conceito na área de trabalho nova.|
|Imagens registadas|Não|Voltar a criar a imagem do Docker de implantação numa nova área de trabalho.|
|Serviços web implementados|Não|Nenhum. Eles ainda funcionarão como-é <br/>ou [implementá-las novamente com a versão mais recente](#services).|
|Experimentação e <br/>Contas de gestão de modelo|Não|[Criar uma área de trabalho](#resources) em vez disso.|
|Aprendizagem automática CLI e SDK|Não|Utilizar a nova [CLI](reference-azure-machine-learning-cli.md) e [SDK](https://aka.ms/aml-sdk) para projetos novos.|


Saiba mais sobre [o que foi alterado nesta versão](overview-what-happened-to-workbench.md)?

>[!Warning]
>Este artigo não é para os utilizadores do Azure Machine Learning Studio. É para os clientes do serviço do Azure Machine Learning que instalaram a aplicação Workbench (pré-visualização) e/ou tem experimentação e contas de pré-visualização de gestão de modelo.

<a name="resources"></a>

## <a name="azure-resources"></a>Recursos do Azure

Recursos, tais como as contas de experimentação, as contas de gestão de modelo e machine learning de computação ambientes não podem ser migrados para a versão mais recente do serviço Azure Machine Learning. Consulte a [linha cronológica](overview-what-happened-to-workbench.md#timeline) no quanto seus ativos continuarão a funcionar.

Comece com a versão mais recente através da criação de uma área de trabalho do serviço do Azure Machine Learning no [portal do Azure](quickstart-get-started.md). O dashboard de área de trabalho do portal é suportado apenas nos browsers Microsoft Edge, Chrome e Firefox.

Esta nova área de trabalho é o recurso de serviço de nível superior e permite-lhe utilizar todas as funcionalidades mais recentes do serviço Azure Machine Learning. Saiba mais sobre isso [área de trabalho e arquitetura](concept-azure-machine-learning-architecture.md).

<a name="projects"></a>

## <a name="projects"></a>Projetos

Em vez de ter seus projetos numa área de trabalho na cloud, os projetos são agora diretórios no seu computador local na versão mais recente. Ver um diagrama do [arquitetura mais recente](concept-azure-machine-learning-architecture.md). 

Para continuar a utilizar o diretório de local que contém os ficheiros e os scripts, especifique o nome do diretório no ['experiment.submit'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) comando do Python ou com o comando da CLI "anexar de projeto de ml az".

Por exemplo:
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

<a name="services"></a>

## <a name="deployed-web-services"></a>Serviços web implementados

Para migrar os serviços da web, volte a implementar seus modelos com o novo SDK ou a CLI para os novos destinos de implementação. Não é necessário para alterar o seu ficheiro de classificação original, ficheiros de dependências de ficheiro de modelo, o ficheiro de ambiente e arquivos de esquema. 

Na versão mais recente, os modelos são implementados como serviços da web para clusters do Azure Container Instances (ACI) ou Azure Kubernetes Service (AKS). 

Saiba mais nestes artigos:
+ [Como implementar e, em que](how-to-deploy-and-where.md)
+ [Tutorial: Implementar modelos com o serviço Azure Machine Learning](tutorial-deploy-models-with-aml.md)

Quando [suporte para as extremidades CLI anteriores](overview-what-happened-to-workbench.md#timeline), não será capaz de gerir os serviços da web que originalmente implementado com a sua conta de gestão de modelos. No entanto, esses serviços web vão continuar a funcionar para, desde que o Azure Container Service (ACS) ainda é suportado.

<a name="history"></a>

## <a name="run-history-records"></a>Registos do histórico de execução

Embora não pode continuar a adicionar à sua históricos de execução existentes na área de trabalho antigo, pode exportar os históricos de que tem com a CLI anterior. Quando [suporte para as extremidades CLI anteriores](overview-what-happened-to-workbench.md#timeline), não será possível exportar esses históricos de execução mais.

Inicie seus modelos de treinamento e ao controlar os históricos de execução usando a nova CLI e SDK. Pode saber como com o [Tutorial: criar modelos com o serviço Azure Machine Learning](tutorial-train-models-with-aml.md).

Para exportar o histórico de execuções com a CLI anterior:

```azurecli
#list all runs
az ml history list

#get details about a particular run
az ml history info

# download all artifacts of a run
az ml history download
```

<a name="dataprep"></a>

## <a name="data-preparation-files"></a>Ficheiros de preparação de dados
Ficheiros de preparação de dados não são portáteis sem a Bancada de trabalho. Mas, ainda pode preparar qualquer conjunto de dados de tamanho para modelar a utilizar o novo SDK do Azure Machine Learning Data Prep ou utilizar o Azure Databricks para grandes conjuntos de dados.  [Saiba como obter a SDK de preparação de dados](how-to-data-prep.md). 

## <a name="next-steps"></a>Passos Seguintes

Para obter um guia de introdução mostra-lhe como criar uma área de trabalho, crie um projeto, executar um script, e explore o histórico de execução do script com a versão mais recente do serviço Azure Machine Learning, experimente [introdução ao serviço Azure Machine Learning](quickstart-get-started.md).

Para uma experiência mais aprofundada para este fluxo de trabalho, siga o tutorial completo que contém passos detalhados para formação e implementação de modelos com o serviço Azure Machine Learning. 

> [!div class="nextstepaction"]
> [Tutorial: Preparar e implementar modelos](tutorial-train-models-with-aml.md)
