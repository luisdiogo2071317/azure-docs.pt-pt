---
title: Compreender a linguagem de consulta
description: Descreve como funciona a linguagem de consulta para o gráfico de recursos do Azure.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 62f61bfea3896fd3828253f5ec16cc38fe3ca007
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316679"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Noções básicas sobre a linguagem de consulta do gráfico de recursos do Azure

A linguagem de consulta para o gráfico de recursos do Azure suporta um número de operadores e funções. Cada trabalho e operasse com base em [Explorador de dados do Azure](../../../data-explorer/data-explorer-overview.md).

A melhor forma de saber mais sobre a linguagem de consulta usada pelo gráfico de recursos é começar com a documentação para o Azure Data Explorer [linguagem de consulta](/azure/kusto/query/index). Ele fornece uma compreensão sobre como o idioma é estruturado e como os vários suportados os operadores e funções funcionam em conjunto.

## <a name="supported-tabular-operators"></a>Operadores de tabela suportados

Eis a lista de operadores tabulares suportados no gráfico de recursos:

- [count](/azure/kusto/query/countoperator)
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
- [ZIP()](/azure/kusto/query/zipfunction)

## <a name="next-steps"></a>Passos Seguintes

- Consulte o idioma utilizado na [consultas Starter](../samples/starter.md)
- Consulte avançada usa no [avançadas consultas](../samples/advanced.md)
- Aprender a [explorar recursos](explore-resources.md)
