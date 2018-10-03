---
title: Noções básicas sobre a linguagem de consulta do gráfico de recursos do Azure
description: Descreve como funciona a linguagem de consulta para o gráfico de recursos do Azure.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 6fcf3087feb7f208ea46d0e08824e965160a61d4
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48236455"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Noções básicas sobre a linguagem de consulta do gráfico de recursos do Azure

A linguagem de consulta para o gráfico de recursos do Azure suporta um número de operadores e funções. Cada trabalho e operasse com base em [Explorador de dados do Azure](../../../data-explorer/data-explorer-overview.md).

A melhor forma de saber mais sobre a linguagem de consulta usada pelo gráfico de recursos é começar com a documentação para o Azure Data Explorer [linguagem de consulta](/azure/kusto/query/index). Isto proporciona uma compreensão sobre como o idioma é estruturado e como os vários suportados os operadores e funções funcionam em conjunto.

## <a name="supported-tabular-operators"></a>Operadores de tabela suportados

Eis a lista de operadores tabulares suportados no gráfico de recursos:

- [Contagem](/azure/kusto/query/countoperator)
- [Distintos](/azure/kusto/query/distinctoperator)
- [Expandir](/azure/kusto/query/extendoperator)
- [Limite](/azure/kusto/query/limitoperator)
- [Ordenar por](/azure/kusto/query/orderoperator)
- [Projeto](/azure/kusto/query/projectoperator)
- [fora do projeto](/azure/kusto/query/projectawayoperator)
- [Exemplo](/azure/kusto/query/sampleoperator)
- [exemplo-distintos](/azure/kusto/query/sampledistinctoperator)
- [Ordenar por](/azure/kusto/query/sortoperator)
- [resumir](/azure/kusto/query/summarizeoperator)
- [tirar](/azure/kusto/query/takeoperator)
- [parte superior](/azure/kusto/query/topoperator)
- [superior-nested.](/azure/kusto/query/topnestedoperator)
- [Top-hitters](/azure/kusto/query/tophittersoperator)
- [onde](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Funções suportadas

Eis a lista de funções no gráfico de recursos:

- [Ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [ToString)](/azure/kusto/query/tostringfunction)

## <a name="next-steps"></a>Passos Seguintes

- Consulte o idioma utilizado na [consultas Starter](../samples/starter.md)
- Consulte avançada usa no [avançadas consultas](../samples/advanced.md)
- Aprenda a [explore os recursos](explore-resources.md)
