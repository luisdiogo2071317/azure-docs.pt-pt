---
title: Linguagem de consulta do Log Analytics do Azure Monitor | Documentos da Microsoft
description: Referências a recursos para aprender a escrever consultas do Log Analytics.
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
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: f8bef8c6ab5c0639f9a99eb2c0443c33d7b6d84e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244067"
---
# <a name="log-analytics-query-language"></a>Linguagem de consulta de análise de registo
Do log Analytics fornece análise e recolha de registos para o Azure Monitor. Ele se baseia no Explorador de dados do Azure e utiliza uma versão da mesma linguagem de consulta. O [documentação de linguagem de consulta do Explorador de dados do Azure](/azure/kusto/query) tem todos os detalhes para o idioma e deve ser o principal recurso para escrever consultas do Log Analytics. Esta página fornece links para outros recursos para aprender a escrever consultas e nas diferenças com a implementação do Log Analytics da linguagem.

## <a name="getting-started"></a>Introdução

- [Introdução ao Log Analytics no portal do Azure](get-started-analytics-portal.md) é uma lição para escrever consultas e trabalhar com os resultados no portal do Azure.
-  [Introdução às consultas no Log Analytics](get-started-queries.md) é algo para escrever consultas usando os dados do Log Analytics.

## <a name="concepts"></a>Conceitos
- [Analisar dados do Log Analytics no Azure Monitor](../log-analytics-queries.md) fornece uma breve descrição geral do registo de consulta e descreve a forma como os dados do Log Analytics estão estruturados.
- [Visualizando e analisando os dados no Log Analytics](../log-analytics-log-search-portals.md) explica os portais onde criar e executar consultas do Log Analytics.

## <a name="reference"></a>Referência

- [Referência de linguagem de consulta](/azure/kusto/query) é a referência de linguagem completa para a linguagem de consulta do Data Explorer.
- [Diferenças de linguagem de consulta de análise de registo](data-explorer-difference.md) descreve as diferenças entre as versões da linguagem de consulta Data Explorer.
- [Propriedades padrão nos registos do Log Analytics](../log-analytics-standard-properties.md) Descreve propriedades padrão para todos os dados do Log Analytics.
- [Efetuar pesquisas de registos de entre recursos do Log Analytics](../log-analytics-cross-workspace-search.md) descreve como escrever consultas que usam dados de várias áreas de trabalho do Log Analytics e aplicações do Application Insights.


## <a name="examples"></a>Exemplos

- [Exemplos de consulta de análise de registo](examples.md) fornece consultas de exemplo com dados do Log Analytics.



## <a name="lessons"></a>Lições

- [Trabalhar com cadeias de caracteres em consultas do Log Analytics](string-operations.md) descreve como trabalhar com dados de cadeia de caracteres.
- [Trabalhar com valores de hora da data em consultas do Log Analytics](datetime-operations.md) descreve como trabalhar com dados de data e hora. 
- [As agregações em consultas do Log Analytics](aggregations.md) e [Advanced agregações em consultas do Log Analytics](advanced-aggregations.md) descrevem como agregam e resumem os dados.
- [Ingressa em consultas do Log Analytics](joins.md) descreve como associar dados de várias tabelas.
- [Trabalhar com dados e de JSON estruturas em consultas do Log Analytics](json-data-structures.md) descreve como analisar dados json.
- [Escrever consultas do Log Analytics de advanced](advanced-query-writing.md) descreve estratégias para criar consultas complexas e reutilização de código.
- [Criação de gráficos e diagramas de consultas do Log Analytics](charts.md) descreve como visualizar dados de uma consulta.

## <a name="cheatsheets"></a>Truques e dicas

-  [SQL para referência rápida do Log Analytics query language](sql-cheatsheet.md) auxilia os usuários que já estejam familiarizados com o SQL.
-  [Splunk para referência rápida do Log Analytics query language](sql-cheatsheet.md) auxilia os usuários que já estão familiarizados com Splunk.
 
## <a name="next-steps"></a>Passos Seguintes

- Aceder a completa [documentação para o idioma de consulta do Explorador de dados de referência](/azure/kusto/query/).