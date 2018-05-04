---
title: Fluxo de dados de transformação de exemplo transformações possíveis com a preparação de dados do Azure Machine Learning | Microsoft Docs
description: Este documento fornece um conjunto de exemplos de transformações de fluxo de dados de transformação possível com a preparação de dados do Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: aa213a3b1a8949f0fca5e4bbb7ec5a6a775ae6ec
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Exemplo de transformações de fluxo de dados personalizados (Python) 
O nome da transformação no menu é **transformar fluxo de dados (scripts)**. Antes de ler este anexo, leia [descrição geral de extensibilidade do Python](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Transformar moldura
### <a name="create-a-new-column-dynamically"></a>Criar uma nova coluna dinâmica 
Cria uma coluna dinamicamente (**city2**) e reconcilia várias versões diferentes do frente a uma partir da coluna cidade existente.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Adicionar novo agregados
Cria um novo pacote com os primeiro e últimos agregados calculados para a coluna de modelo de pontuação. Estes são agrupados pelo **risk_category** coluna.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize uma coluna 
Reformulates os dados para cumprir uma fórmula para reduzir os valores atípicos numa coluna.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Transformar o fluxo de dados
### <a name="fill-down"></a>Preencher para baixo 

Preenchimento baixo requer dois transformações. Pressupõe que os dados que se assemelha a tabela seguinte:

|Estado         |Cidade       |
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

1. Crie uma transformação "Adicionar a coluna (Script)" utilizando o seguinte código:
```python
    row['State'] if len(row['State']) > 0 else None
```

2. Crie uma transformação de "Fluxo de dados de transformação (Script)" que contém o seguinte código:
```python
    df = df.fillna( method='pad')
```

Os dados aspeto agora a tabela seguinte:

|Estado         |newState         |Cidade       |
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


### <a name="min-max-normalization"></a>Normalização de min-máx.
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```