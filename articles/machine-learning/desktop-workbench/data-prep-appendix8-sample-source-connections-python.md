---
title: "Ligações de dados de origem adicionais de exemplo possíveis com a preparação de dados do Azure Machine Learning | Microsoft Docs"
description: "Este documento fornece um conjunto de exemplos de ligações de dados de origem que são possíveis com a preparação de dados do Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 27d72f1b49a89929098c0cc35f92d3915fc8b0b9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="sample-of-custom-source-connections-python"></a>Exemplo de ligações à origem personalizada (Python) 
Antes de ler este anexo, leia [descrição geral de extensibilidade do Python](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Carregar dados do data.world

### <a name="prerequisites"></a>Pré-requisitos

#### <a name="register-yourself-at-dataworld"></a>Registar-se em data.world
Precisa de um token de API do Web site data.world.

#### <a name="install-dataworld-library"></a>Instalar data.world biblioteca

Abrir a interface de linha de comandos do Azure Machine Learning Workbench selecionando **ficheiro** > **interface de linha de comandos aberta**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Em seguida, execute `dw configure` na linha de comandos, que lhe solicita o token. Quando introduzir o seu token, um .dw / é criado um diretório no seu diretório raiz e o token é armazenado não existe.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Agora, deve conseguir importar data.world bibliotecas.

#### <a name="load-data-into-data-preparation"></a>Carregar dados para a preparação de dados

Crie uma transformação de transformação do fluxo de dados (Script). Em seguida, utilize o seguinte script para carregar os dados de data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>BD do Azure do Cosmos como uma ligação à origem de dados
Para obter um exemplo de base de dados do Azure Cosmos como uma ligação de dados, leia [BD de Cosmos carga do Azure como uma ligação de dados de origem](data-prep-load-azure-cosmos-db.md)
