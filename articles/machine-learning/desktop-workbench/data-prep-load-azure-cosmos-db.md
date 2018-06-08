---
title: A ligar ao Azure Cosmos DB como uma origem de dados no Azure Machine Learning Workbench | Microsoft Docs
description: Este documento fornece um exemplo sobre como ligar à base de dados do Azure Cosmos através do Azure Machine Learning Workbench
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 20e23f41310b90c62eacb7279ea3da0eec376683
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830695"
---
# <a name="connecting-to-azure-cosmos-db-as-a-data-source"></a>Ligar à base de dados do Azure Cosmos como uma origem de dados
Este artigo contém um python exemplo permite-lhe ligar à base de dados do Cosmos no Azure Machine Learning Workbench.

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Carregar dados de base de dados do Azure Cosmos para preparação de dados

Criar um novo fluxo de dados baseados em script e, em seguida, utilize o seguinte script para carregar os dados da base de dados do Azure Cosmos. 

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```

## <a name="other-data-source-connections"></a>Outras ligações à origem de dados
Para outros exemplos, leia o artigo [ligações de dados de origem adicionais de exemplo](data-prep-appendix8-sample-source-connections-python.md)
