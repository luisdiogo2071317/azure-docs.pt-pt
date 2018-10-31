---
title: Referência de linguagem de Monitor Log Analytics do Azure | Documentos da Microsoft
description: Informações de referência de linguagem de consulta do Explorador de dados usada pelo Log Analytics. Inclui elementos adicionais específicos para o Log Analytics e os elementos que não é suportados em consultas do Log Analytics.
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
ms.date: 10/29/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e6d097749dae49cf6f1d710bcf01cf99dcd98a4c
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244123"
---
# <a name="log-analytics-query-language-differences"></a>Diferenças de linguagem de consulta de análise de registo

Enquanto [do Log Analytics](../log-analytics-queries.md) baseia [Explorador de dados do Azure](/azure//data-explorer) e utiliza o [mesmo idioma de consulta](/azure/kusto/query), a versão da linguagem tem algumas diferenças. Este artigo identifica os elementos que são diferentes entre a versão do idioma usado para o Data Explorer e a versão utilizada para consultas do Log Analytics.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Elementos de Explorador de dados não suportados no Log Analytics
As secções seguintes descrevem os elementos de linguagem de consulta o Data Explorer que não são suportados pelo Log Analytics.

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
Os seguintes operadores suportam funcionalidades específicas do Log Analytics e não estão disponíveis fora do Log Analytics.

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Passos Seguintes

<<<<<<< HEAD:articles/log-analytics/query-language/data-explorer-difference.md
- Obter referências a diferentes [recursos para escrever consultas do Log Analytics](kusto.md).
- Aceder a completa [documentação para a linguagem de consulta do Explorador de dados de referência](/azure/kusto/query/).
=======
- Saiba mais sobre consultas no [do Log Analytics](../log-analytics-queries.md).
- Percorrer uma lição sobre escrita num [consulta do Log Analytics](/log-analytics/query-language/get-started-queries.md).
- Aceder a completa [fazer referência a documentação do Kusto](/azure/kusto/query/).
>>>>>>> 4bccab5ecb17c887658a4d2ed1bab6b22bf29ffd:articles/log-Analytics/Query-Language/kusto.md
