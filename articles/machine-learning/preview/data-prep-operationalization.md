---
title: Guia aprofundado sobre como utilizar o Azure Machine Learning dados preparativos Operationalization | Microsoft Docs
description: Este documento fornece detalhes sobre como executar previamente concebidos pacotes origens de dados e preparativos de dados
services: machine-learning
author: hughz
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/13/2018
ms.openlocfilehash: 0849747fe6d66d55d11c131b51b07d8f689774e1
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="data-preparation-operationalization"></a>Operationalization de preparação de dados 

## <a name="operationalization-scenario"></a>Cenário de operationalization

Seguem-se a diferentes cenários de dados preparação Operationalization provir através de um utilizador.

### <a name="develop-your-model-based-on-training-data"></a>Desenvolver o seu modelo com base nos dados de formação

Suponha que procura para criar e implementar um serviço de API de classificação em tempo real. O primeiro passo é a desenvolver um modelo de classificação com base em algumas conjunto de dados de preparação. Preparação de dados importa uma amostra dos seus dados de formação como uma nova origem de dados. Depois dos dados são preparados, o pacote de DataPrep contém os passos de preparação. Utilizar uma conta de experimentação do AzureML, o utilizador pode iterar um modelo de machine learning para a geração de etiquetas para cada entrada os dados de preparação.

Como as funcionalidades da necessidade de dados a ser atualizado, o utilizador devolve para o pacote de DataPrep para preparar os dados de forma a nova e guardar esses passos. Este processo iteração de gerar novas funcionalidades e ajuste o seu modelo do machine learning continua até o modelo pontuações com precisão os seus dados de teste. 

### <a name="deploy-your-model-to-the-azure-model-management-service"></a>Implementar o seu modelo para o serviço de gestão de modelo do Azure

Agora, tem dados de cliente que gostaria de pontuação em tempo real. Utilizar o serviço de gestão de modelo do Azure, pode implementar este modelo a partir da CLI do Python do AzureML como um serviço. O serviço implementado expõe um ponto final de REST para receber dados de cliente em tempo real e devolver correspondente saída etiquetas do modelo treinado.

Facilidade de utilização, o ponto final de modelo aceita dados no formato JSON. A entrada deve ser uma cadeia JSON que representam uma matriz unidimensional de 2 de dados, que irão passar a transformação ReadJSONLiteral e convertidas para uma origem de dados DataPrep. O pacote de preparação de dados criada durante a pode de fase de experimentação e ser executado contra os dados JSON transmissão em fluxo. O dataframe resultante, em seguida, pode ser transferido para o modelo treinado para classificação.

## <a name="read-json-literal-transformation"></a>Ler transformação Literal JSON

### <a name="description"></a>Descrição

Para efeitos de operationalization, preparação de dados contém um **ReadJsonLiteral** transformação para executar uma atividade e gerar um Pandas ou Dataframe de Spark. Esta transformação exclusivamente aceita como entrada um dados existentes preparação do pacote e uma origem de dados JSON. Esta transformação está exposta através da CLI do Python DataPrep.

### <a name="instructions"></a>Instruções

#### <a name="pythoncli"></a>PythonCLI

O Azure Machine Learning Workbench, abra a Interface de linha de comandos (ficheiro > abrir a linha de comandos).

Neste exemplo, o pacote de preparação de dados de TrainingPreparationSteps é utilizado para preparar os dados e adicionar a funcionalidade de volume para cada entrada.

```
>>> import azureml.dataprep.package as azdp

>>> azdp.run_on_data.__doc__
"Generate a dataframe based on a selected dataflow in a package using in-memory data sources.
'user_config' is expected as a dictionary that maps the absolute path of a dsource file to an in-memory data source represented as a list of lists."

>>> real_time_customer_data = [[1.0, 1.5, 2.5], [2.5, 1.5, 3]]
>>> azdp.run_on_data({ "C:\\TrainingSampleData.dsource": real_time_customer_data}, "C:\\TrainingPreparationSteps.dprep")
    Height   Width   Depth  Volumne
0   1.0       1.5    2.5    3.75
1   2.5       1.5    3.0    11.25
```

`run_on_data()` tem os seguintes parâmetros opcionais
 - `dataflow_idx`: Especifique o índice de fluxo de dados pretendido para executar no pacote
 - `secrets`: arquivo secreta dicionário (chave: secretId, valor: segredo armazenado)
 - `spark`: forneça uma sessão de spark para a execução do spark

#### <a name="input"></a>Input

Uma entrada da matriz unidimensional-2 ("matriz de matrizes"). No Python, a entrada deve ser uma lista de listas. Uma vez que JSON tem um tipo de data nativo, quaisquer objetos de datetime.datetime Python serão convertidos para cadeias de ISO-formatado data. Para pontos finais REST, a entrada deve ser uma cadeia de literal JSON que representam uma matriz JSON 2-D.

#### <a name="output"></a>Saída

Um Pandas ou Spark DataFrame. O tipo de DataFrame é determinado pelo framework selecionado na configuração de execução (`.runconfig`). O dataframe resultante pode ser transmitido como entrada para o modelo treinado para classificação.
