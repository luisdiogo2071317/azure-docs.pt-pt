---
title: Referência de linguagem de Monitor Log Analytics do Azure | Documentos da Microsoft
description: Informações de referência para o idioma de Kusto usado pelo Log Analytics. Inclui elementos adicionais específicos para o Log Analytics e os elementos que não é suportados em consultas do Log Analytics.
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
ms.openlocfilehash: 5173790436a29fa9947346d711da1a2ddb32bf62
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451073"
---
# <a name="log-analytics-query-language-reference"></a>Referência de linguagem de consulta de análise de registo
[Consultas de análise de registo](../log-analytics-queries.md) utilizar o mesmo idioma de consulta e o mecanismo utilizado pelo [Explorador de dados do Azure](/azure/data-explorer/). Pode acessar a referência de linguagem e outros detalhes sobre a linguagem da seguinte localização: [referência de linguagem de Kusto](/azure/kusto/query)



## <a name="kusto-elements-not-support-in-log-analytics"></a>Elementos de Kusto não suporte no Log Analytics
Apesar de consultas do Log Analytics utilizam uma implementação do Kusto, existem alguns elementos de Kusto não suporta conforme descrito nas seções a seguir.

### <a name="statements-not-supported-in-log-analytics"></a>Instruções não são suportadas no Log Analytics

* [Alias](/kusto/query/aliasstatement)
* [Parâmetros de consulta](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>Funções não suportadas no Log Analytics

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [Database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>Operadores não suportadas no Log Analytics

* [Associação cruzada do Cluster](/azure/kusto/query/joincrosscluster)
* [operador de externaldata](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>Plug-ins não suportado no Log Analytics

* [Plug-in do sql_request](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Operadores adicionais no Log Analytics
Para suportar funcionalidades específicas do Log Analytics, os seguintes operadores de Kusto adicionais são desde que não estão disponíveis fora do Log Analytics. 

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre consultas no [do Log Analytics](../log-analytics-queries.md).
- Percorrer uma lição sobre escrita num [consulta do Log Analytics](/log-analytics/query-language/get-started-queries.md).
- Aceder a completa [fazer referência a documentação do Kusto](/azure/kusto/query/).