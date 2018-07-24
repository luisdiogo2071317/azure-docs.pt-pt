---
title: Destinos/saídas de exemplo possíveis com a preparação de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece um conjunto de exemplos de destinos de dados personalizados/saídas preparação de dados do Azure Machine Learning
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
ms.openlocfilehash: 2173ff15906940b8628aba3615f31e3f7137e3e2
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216536"
---
# <a name="sample-of-destination-connections-python"></a>Exemplo de ligações de destino (Python) 
Antes de ler este apêndice, leia [visão geral da extensibilidade de Python](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Escrever para o Excel 


Escrever para o Excel requer uma biblioteca adicional. Adicionar novas bibliotecas está documentado na descrição de geral de extensibilidade. `openpyxl` é a biblioteca que precisa adicionar.

Antes de escrever para o Excel, algumas outras alterações podem ser necessários. Alguns dos tipos de dados que são utilizados na preparação de dados não são suportados em alguns formatos de destino. Por exemplo, se existirem objetos "Error", eles não serializar corretamente para o Excel. Portanto, antes de tentar escrever para o Excel, precisa uma transformação de "Substituir valores de erro", que remove os erros de quaisquer colunas.

Se todo o trabalho anterior estar concluída, a seguinte linha escreve a tabela de dados numa folha de cálculo único num documento Excel. Adicione uma transformação de transformar fluxo de dados (Script). Em seguida, introduza o código a seguir numa seção de expressão.


### <a name="on-windows"></a>No Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>No macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
