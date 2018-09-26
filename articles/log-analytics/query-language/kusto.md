---
title: Diferenças de linguagem de Monitor de Log Analytics e Kusto do Azure | Documentos da Microsoft
description: Descreve as diferenças entre consultas do Log Analytics e núcleo de idioma do Kusto.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 109ffa6abb34dad6a00210a5c2c726bdfdde094f
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184919"
---
# <a name="log-analytics-and-kusto-language-differences"></a>Diferenças de idioma do log Analytics e Kusto
[Consultas de análise de registo](../log-analytics-queries.md) são escritos com o [Kusto linguagem](/azure/kusto/query). No entanto, tal como descrito neste artigo, existem algumas diferenças de idioma padrão e a implementação do Log Analytics.


## <a name="statements-not-supported-in-log-analytics"></a>Instruções não são suportadas no Log Analytics
As seguintes instruções não são suportadas no Log Analytics.

* [Alias](/kusto/query/aliasstatement)
* [Parâmetros de consulta](/azure/kusto/query/queryparametersstatement)

## <a name="functions-not-supported-in-log-analytics"></a>Funções não suportadas no Log Analytics
As seguintes funções não são suportadas no Log Analytics.

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [Database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

## <a name="operators-not-supported-in-log-analytics"></a>Operadores não suportadas no Log Analytics
Os seguintes operadores não são suportados no Log Analytics.

* [Associação cruzada do Cluster](/azure/kusto/query/joincrosscluster)
* [operador de externaldata](/azure/kusto/query/externaldata-operator)

## <a name="plugins-not-supported-in-log-analytics"></a>Plug-ins não suportado no Log Analytics
Os plug-ins seguintes não são suportados no Log Analytics.
* [Plug-in do sql_request](/azure/kusto/query/sqlrequestplugin)


## <a name="log-analytics-specific-operators"></a>Operadores específicos do log Analytics
* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre consultas no [do Log Analytics](../log-analytics-queries.md).
- Percorrer uma lição sobre escrita num [consulta do Log Analytics](/log-analytics/query-language/get-started-queries.md).
- Aceder a completa [fazer referência a documentação do Kusto](/azure/kusto/query/).