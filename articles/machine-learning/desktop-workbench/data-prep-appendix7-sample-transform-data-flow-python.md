---
title: Fluxo de dados de transformação de exemplo transformações possíveis com a preparação de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece um conjunto de exemplos de transformações de fluxo de dados de transformação possível com a preparação de dados do Azure Machine Learning
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
ms.openlocfilehash: 82295e412c70065ff8ce3a686aec790614f39f2e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947205"
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Exemplo de transformações de fluxo de dados personalizados (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


É o nome da transformação no menu **transformar fluxo de dados (Script)**. Antes de ler este apêndice, leia [visão geral da extensibilidade de Python](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Transformar quadro
### <a name="create-a-new-column-dynamically"></a>Criar uma nova coluna dinamicamente 
Cria uma coluna dinamicamente (**cidade2**) e concilie várias versões diferentes do San Francisco para um da coluna city existente.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Adicionar novas agregações
Cria um novo quadro com os primeiros e últimos agregados computados para a coluna de pontuação. Estes são agrupados pela **risk_category** coluna.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize uma coluna 
Reformulates os dados de acordo com uma fórmula para reduzir os valores atípicos numa coluna.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Transformar o fluxo de dados
### <a name="fill-down"></a>Preencher para baixo 

Preencher para baixo requer duas transformações. Parte do princípio de dados que se parece com a tabela seguinte:

|Estado         |Localidade       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|Califórnia    |Los Angeles|
|              |San Diego  |
|              |San Jose   |
|Texas         |Dallas     |
|              |San Antonio|
|              |Houston    |

1. Crie uma transformação de "Adicionar coluna (Script)" usando o seguinte código:
```python
    row['State'] if len(row['State']) > 0 else None
```

2. Crie uma transformação de "Fluxo de dados de transformação (Script)" que contém o seguinte código:
```python
    df = df.fillna( method='pad')
```

Os dados agora é parecida com a tabela seguinte:

|Estado         |newState         |Localidade       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Issaquah   |
|              |Washington    |Seattle    |
|Califórnia    |Califórnia    |Los Angeles|
|              |Califórnia    |San Diego  |
|              |Califórnia    |San Jose   |
|Texas         |Texas         |Dallas     |
|              |Texas         |San Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>Normalização mínima-máxima
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```