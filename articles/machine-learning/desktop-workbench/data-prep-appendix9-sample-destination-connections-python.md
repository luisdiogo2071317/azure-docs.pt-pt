---
title: Destinos/saídas de exemplo possíveis com a preparação de dados do Azure Machine Learning | Microsoft Docs
description: Este documento fornece uma um conjunto de exemplos de dados personalizados destinos/saídas com a preparação de dados do Azure Machine Learning
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
ms.openlocfilehash: a9476b9ac3a5cc1f82ab1c09678972fb6b78b894
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="sample-of-destination-connections-python"></a>Exemplo de ligações de destino (Python) 
Antes de ler este anexo, leia [descrição geral de extensibilidade do Python](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Escrever para o Excel 


Escrever para o Excel necessita de uma biblioteca adicional. Adicionar novas bibliotecas é descrita na descrição geral extensibilidade. `openpyxl` é a biblioteca que tem de adicionar.

Antes de escrever para o Excel, poderão ser necessária, algumas das alterações. Alguns dos tipos de dados que são utilizados em preparação de dados não são suportados em alguns formatos de destino. Por exemplo, se existirem objetos "Error", eles não irão serializar corretamente para o Excel. Assim, antes de tentar escrever para o Excel, tem uma transformação de "Substituir valores de erro", que remove os erros de quaisquer colunas.

Se a todo o trabalho anterior estiver concluída, a seguinte linha escreve a tabela de dados para uma folha de única num documento Excel. Adicione uma transformação de fluxo de transformação dados (Script). Em seguida, introduza o seguinte código na secção de uma expressão.


### <a name="on-windows"></a>No Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>No macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
