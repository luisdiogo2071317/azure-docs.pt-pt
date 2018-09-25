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
ms.openlocfilehash: 3600fc47a0fb318a49c1b37722cb7fffa51ec6f2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951955"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Noções básicas sobre a linguagem de consulta do gráfico de recursos do Azure

A linguagem de consulta para o gráfico de recursos do Azure suporta um número de operadores e funções. Cada trabalho e operar semelhante para o idioma de consulta do Kusto (KQL). No entanto, é importante entender que, enquanto a linguagem de consulta do gráfico de recursos é semelhante à [KQL](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators), não é o mesmo.

> [!NOTE]
> Ligações para documentação de KQL podem exigir a autenticação.

A melhor forma de saber mais sobre a linguagem de consulta usada pelo gráfico de recursos é começar com a documentação para KQL obter uma compreensão sobre como o idioma é estruturado e como os vários suportados os operadores e funções funcionam em conjunto.

## <a name="supported-tabular-operators"></a>Operadores de tabela suportados

Eis a lista de operadores tabulares suportados no gráfico de recursos:

- [Distintos](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator)
- [Expandir](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator)
- [Limite](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/limit-operator)
- [Ordenar por](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/order-operator)
- [Projeto](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator)
- [fora do projeto](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-away-operator)
- [Exemplo](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sample-operator)
- [exemplo-distintos](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sample-distinct-operator)
- [Ordenar por](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator)
- [resumir](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator)
- [tirar](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
- [parte superior](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator)
- [superior-nested.](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-nested-operator)
- [Top-hitters](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-hitters-operator)
- [onde](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

## <a name="supported-functions"></a>Funções suportadas

Eis a lista de funções no gráfico de recursos:

- [Ago()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago%28%29)
- [buldschema()](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/buildschema%28%29)
- [Contagem)](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/count%28%29)
- [strcat()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/strcat%28%29)
- [isnotempty()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/isnotempty%28%29_-notempty%28%29)
- [ToString)](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/tostring%28%29)

## <a name="next-steps"></a>Passos Seguintes

- Consulte o idioma utilizado na [consultas Starter](../samples/starter.md)
- Consulte avançada usa no [avançadas consultas](../samples/advanced.md)
- Aprenda a [explore os recursos](explore-resources.md)