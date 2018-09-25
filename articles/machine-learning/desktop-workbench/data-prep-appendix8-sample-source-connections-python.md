---
title: Ligações de dados de origem adicionais de exemplo possíveis com a preparação de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece um conjunto de exemplos de ligações de dados de origem que são possíveis com a preparação de dados do Azure Machine Learning
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
ms.openlocfilehash: ed0e6bc4c506535fcb679fc2b4015a61274a77f3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967698"
---
# <a name="sample-of-custom-source-connections-python"></a>Exemplo de ligações de origem de dados personalizada (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Antes de ler este apêndice, leia [visão geral da extensibilidade de Python](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Carregar dados de data. World

### <a name="prerequisites"></a>Pré-requisitos

#### <a name="register-yourself-at-dataworld"></a>Registre-se em data. World
Precisa de um token de API a partir do site da data. World.

#### <a name="install-dataworld-library"></a>Instalar a biblioteca de data. World

Abra a interface de linha de comandos do Azure Machine Learning Workbench, selecionando **arquivo** > **interface de linha de comandos aberta**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Em seguida, execute `dw configure` na linha de comando, que pede-lhe o token. Quando inserir o seu token, um .dw / diretório é criado no diretório raiz e o token é armazenado nele.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Agora deve ser capaz de data. World bibliotecas de importação.

#### <a name="load-data-into-data-preparation"></a>Carregar dados para preparação de dados

Crie uma transformação transformar fluxo de dados (Script). Em seguida, utilize o seguinte script para carregar os dados de data. World.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>O Azure Cosmos DB como uma ligação de origem de dados
Para obter um exemplo do Azure Cosmos DB como uma ligação de dados, leia [carga do Azure Cosmos DB como uma ligação de dados de origem](data-prep-load-azure-cosmos-db.md)
