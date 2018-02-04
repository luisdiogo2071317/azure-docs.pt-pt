---
title: "Expressões de filtro de exemplo possíveis com a preparação de dados do Azure Machine Learning | Microsoft Docs"
description: "Este documento fornece uma um conjunto de exemplos de expressões de filtro possíveis com a preparação de dados do Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: df78e871625f4de406de5ba4ae0fea327b87b01e
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
---
# <a name="sample-of-filter-expressions-python"></a>Exemplo de expressões de filtro (Python) 
Antes de ler este anexo, leia [descrição geral de extensibilidade do Python](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Com o teste de equivalência de filtro
Filtrar em apenas as linhas em que o valor de Col2 (numérico) é superior a 4. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Filtrar com várias colunas 
Filtrar em apenas as linhas onde Col1 contém o valor **boa** e Col2 contém o valor (numérico) 1. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Filtro de teste em relação a valor nulo
Filtrar em apenas as linhas onde Col1 tem um valor nulo. 
```python
    pd.isnull(row["Col1"])
```
