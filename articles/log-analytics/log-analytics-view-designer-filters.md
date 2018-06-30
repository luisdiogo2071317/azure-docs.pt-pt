---
title: Filtros nas vistas do Log Analytics do Azure | Microsoft Docs
description: Um filtro numa vista de análise de registos permite aos utilizadores filtrar os dados na vista pelo valor de uma propriedade específica sem modificar a vista de si próprio.  Este artigo descreve como utilizar um filtro e adicione uma a uma vista personalizada.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 0ad22562bd1f36bba7c0ab99fe504e82645033d3
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131415"
---
# <a name="filters-in-log-analytics-views"></a>Filtros nas vistas de análise de registos
A **filtro** num [ver análise de registos](log-analytics-view-designer.md) permite aos utilizadores filtrar os dados na vista pelo valor de uma propriedade específica sem modificar a vista de si próprio.  Por exemplo, pode permitir que os utilizadores da sua vista para filtrar a vista de dados apenas a partir de um computador específico ou um conjunto de computadores.  Pode criar vários filtros numa única vista para permitir aos utilizadores filtrar por várias propriedades.  Este artigo descreve como utilizar um filtro e adicione uma a uma vista personalizada.

## <a name="using-a-filter"></a>Utilizando um filtro
Clique no intervalo de tempo de dados na parte superior de uma vista para abrir a lista pendente onde pode alterar o intervalo de tempo de dados para a vista.

![Exemplo de filtro](media/log-analytics-view-designer/filters-example-time.png)

Clique em de **+** para adicionar um filtro utilizando filtros personalizados que estão definidos para a vista. Selecionar um valor para o filtro na lista pendente ou digitar um valor. Continue a adicionar filtros clicando a **+**. 


![Exemplo de filtro](media/log-analytics-view-designer/filters-example-custom.png)

Se remover todos os valores para um filtro, esse filtro já não será aplicado.


## <a name="creating-a-filter"></a>Criar um filtro

Criar um filtro do **filtros** separador quando [editar uma vista](log-analytics-view-designer.md).  O filtro é global para a vista e aplica-se a todas as partes na vista.  

![Definições de filtro](media/log-analytics-view-designer/filters-settings.png)

A tabela seguinte descreve as definições para um filtro.

| Definição | Descrição |
|:---|:---|
| Nome do Campo | Nome do campo utilizado para filtragem.  Isto deve corresponder ao campo summarize em **consulta para os valores**. |
| Consulta para os valores | Consulta seja executada para preencher a lista pendente de filtro para o utilizador.  Este tem de utilizar um [resumir](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) ou [distintos](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator) para fornecer valores exclusivos para um determinado campo e têm de corresponder a **o nome do campo**.  Pode utilizar [ordenação](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator) para ordenar os valores que são apresentados ao utilizador. |
| Etiqueta | Nome para o campo que é utilizado em consultas que suporta o filtro e também é apresentado ao utilizador. |

### <a name="examples"></a>Exemplos

A tabela seguinte inclui alguns exemplos de filtros comuns.  

| Nome do Campo | Consulta para os valores | Etiqueta |
|:--|:--|:--|
| Computador   | Heartbeat &#124; computador distinto &#124; ordenar por computador asc | Computadores |
| EventLevelName | Evento &#124; EventLevelName distinto | Gravidade |
| Nível de gravidade | Syslog &#124; distinto nível de gravidade | Gravidade |
| SvcChangeType | ConfigurationChange &#124; svcChangeType distinto | ChangeType |


## <a name="modify-view-queries"></a>Modificar consultas de vista

Para um filtro têm qualquer efeito, tem de modificar todas as consultas na vista para filtrar valores selecionados.  Se não modificar todas as consultas na vista de quaisquer valores que o utilizador seleciona terá qualquer efeito.

A sintaxe para utilizar um valor de filtro numa consulta é: 

    where ${filter name}  

Por exemplo, se a vista com uma consulta os eventos de devolve e utiliza um filtro chamado computadores, pode utilizar o seguinte.

    Event | where ${Computers} | summarize count() by EventLevelName

Se tiver adicionado filtro outro chamado gravidade, pode utilizar a seguinte consulta para utilizar ambos os filtros.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre o [partes de visualização](log-analytics-view-designer-parts.md) pode adicionar à sua vista personalizada.