---
title: Guia detalhado sobre como utilizar a API de execução de preparações de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece detalhes sobre a execução anteriormente criado pacotes de origens de dados e as preparações de dados
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 5280ed4143ef2ee980300cebe234de7f2020a14f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968106"
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Executar pacotes de origens de dados e as preparações de dados a partir de Python

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Para utilizar um pacote de origens de dados do Azure Machine Learning ou de preparações de dados do Azure Machine Learning em Python:

1. Vá para o **dados** separador do seu projeto.

2. Com o botão direito a origem adequada.

3. Escolha **gerar ficheiro de código de acesso de dados.**

Esta ação cria um script de Python curto que executa o pacote e retorna um pacote de dados.

## <a name="data-sources"></a>Origens de Dados

O `azureml.dataprep.datasource` módulo contém uma função única para executar uma origem de dados e retornar um dataframe: `load_datasource(path, secrets=None, spark=None)`.
- `path` é o caminho para a origem de dados (ficheiro .dsource).
- `secrets` é um dicionário opcional que mapeia as chaves para segredos.
- `spark` é um booleano opcional que especifica se pretende devolver um dataframe do Spark ou um Pandas dataframe. Por predefinição, o Azure Machine Learning Workbench determina qual tipo de dataframe para voltar no tempo de execução com base no contexto.

## <a name="data-preparations-packages"></a>Pacotes de preparações de dados

O `azureml.dataprep.package` módulo contém três funções que executar um fluxo de dados a partir de um pacote de preparações de dados.

### <a name="execution-functions"></a>Funções de execução

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)` submete o fluxo de dados especificada para a execução, mas não devolve um dataframe.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)` executa o fluxo de dados especificada e devolve os resultados como um pacote de dados.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)` executa o fluxo de dados especificado com base numa origem de dados na memória e retorna os resultados como um pacote de dados. O `user_config` argumento é um dicionário que mapeia o caminho absoluto de uma origem de dados (ficheiro .dsource) a uma origem de dados na memória representada como uma lista de listas.

### <a name="common-arguments"></a>Argumentos comuns

- `package_path` é o caminho para o pacote de preparações de dados (ficheiro .dprep).
- `dataflow_idx` é o índice baseado em zero que do fluxo de dados no pacote para executar. Se o fluxo de dados especificada fizer referência a outros fluxos de dados ou origens de dados, eles serão executados também.
- `secrets` é um dicionário opcional que mapeia as chaves para segredos.
- `spark` é um booleano opcional que especifica se pretende devolver um dataframe do Spark ou um Pandas dataframe. Por predefinição, o Workbench determina o tipo de pacote de dados para voltar no tempo de execução com base no contexto.
