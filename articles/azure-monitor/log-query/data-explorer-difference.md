---
title: Azure diferenças de linguagem de consulta do Monitor log | Documentos da Microsoft
description: Informações de referência de linguagem de consulta do Explorador de dados utilizada pelo Azure Monitor. Inclui elementos adicionais específicos para o Azure Monitor e elementos que não é suportados em consultas de registo do Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2018
ms.author: bwren
ms.openlocfilehash: 9c58796fa19ffb6d38582c809f7bb6ca948bd92c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003638"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure diferenças de linguagem de consulta do Monitor log

Enquanto [inicia sessão no Azure Monitor](log-query-overview.md) baseia [Explorador de dados do Azure](/azure/data-explorer) e utiliza o [mesmo idioma de consulta](/azure/kusto/query), a versão da linguagem tem algumas diferenças. Este artigo identifica os elementos que são diferentes entre a versão do idioma usado para o Data Explorer e a versão utilizada para consultas de registo do Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="data-explorer-elements-not-supported-in-azure-monitor"></a>Elementos de Explorador de dados não suportados no Azure Monitor
As secções seguintes descrevem os elementos de linguagem de consulta o Data Explorer que não são suportados pelo Azure Monitor.

### <a name="statements-not-supported-in-azure-monitor"></a>Instruções não são suportadas no Azure Monitor

* [Alias](/azure/kusto/query/aliasstatement)
* [Parâmetros de consulta](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Funções não suportadas no Azure Monitor

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Operadores não suportados no Azure Monitor

* [Associação cruzada do Cluster](/azure/kusto/query/joincrosscluster)
* [operador de externaldata](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-azure-monitor"></a>Plug-ins não suportado no Azure Monitor

* [sql_request plugin](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Operadores adicionais no Azure Monitor
Os seguintes operadores suportam funcionalidades específicas do Azure Monitor e não estão disponíveis fora do Azure Monitor.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Passos Seguintes

- Obter referências a diferentes [consultas de registo de recursos para a escrita do Azure Monitor](query-language.md).
- Aceder a completa [documentação para a linguagem de consulta do Explorador de dados de referência](/azure/kusto/query/).
