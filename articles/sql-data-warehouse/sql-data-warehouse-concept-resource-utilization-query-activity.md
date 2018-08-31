---
title: Capacidade de gestão do armazém de dados SQL do Azure e monitorização - atividade, a utilização de recursos de consulta | Documentos da Microsoft
description: Saber que capacidades estão disponíveis para gerir e monitorizar o Azure SQL Data Warehouse. Utilize o portal do Azure e a vistas de gestão dinâmica (DMVs) para compreender a atividade de consulta e a utilização de recursos do seu armazém de dados.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/26/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: c783045d242725ee19dfe7e0baee13625d986312
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246499"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Monitorizar atividade de consulta e de utilização de recursos no Azure SQL Data Warehouse
O Azure SQL Data Warehouse oferece uma Rica experiência de monitorização no portal do Azure para obter informações para a sua carga de trabalho do armazém de dados. O portal do Azure é a ferramenta recomendada quando o armazém de dados de monitorização, pois fornece períodos de retentions configuráveis, alertas, recomendações e personalizáveis gráficos e dashboards para métricas e registos. O portal também lhe permite integrar com outro serviços, como o Operations Management Suite (OMS) de monitorização do Azure / Log Analytics e o Azure Monitor para fornecer uma monitorização holística experiência para não apenas seu armazém de dados, mas também sua do Azure inteira plataforma de análise para uma experiência de monitorização integrada. Esta documentação descreve as capacidades de monitorização estão disponíveis para otimizar e gerir a sua plataforma de análise com o SQL Data Warehouse. 

## <a name="resource-utilization"></a>Utilização de recursos 
As métricas seguintes estão disponíveis no portal do Azure.

| Nome da Métrica                           | Descrição     | Tipo de Agregação |
| --------------------------------------- | ---------------- | --------------------------------------- |
| Percentagem de CPU                          | Utilização da CPU de todos os nós para o armazém de dados | Máximo      |
| Percentagem de ES de Dados                      | Utilização de e/s em todos os nós para o armazém de dados | Máximo   |
| Ligações com êxito                  | Número de conexões com êxito para os dados | Total            |
| Ligações Falhadas                      | Número de ligações falhadas ao armazém de dados | Total            |
| Bloqueado pela Firewall                     | Número de inícios de sessão para o armazém de dados que foi bloqueado | Total            |
| Limite de DWU                              | Objetivo de nível de serviço do data warehouse | Máximo   |
| Percentagem DWU                          | Máximo entre a percentagem de CPU e percentagem de es de dados | Máximo   |
| DWU utilizado                                | Limite de DWU * percentagem DWU | Máximo   |
| Percentagem de acertos na cache | (colocar em cache pedidos com êxito na / falha de acerto em cache) * erros de 100 onde acertos na cache é a soma de todos os acessos de segmentos de columnstore na cache SSD local e os segmentos de columnstore de é de falha de acerto na cache na cache SSD local somado em todos os nós | Máximo |
| Percentagem de cache utilizada | (o cache utilizada / capacidade em cache) * 100 onde cache utilizada é a soma de todos os bytes no cache SSD local em todos os nós e a capacidade de cache é a soma da capacidade de local SSD de armazenamento em cache em todos os nós | Máximo |

## <a name="query-activity"></a>Atividade de Consulta
Para uma experiência de programação ao monitorizar o SQL Data Warehouse através de T-SQL, o serviço fornece um conjunto de vistas de gestão dinâmica (DMVs). Estas vistas são úteis quando ativamente a solução de problemas e identificar afunilamentos de desempenho com a carga de trabalho.

Para ver a lista de DMVs que fornece o SQL Data Warehouse, consulte este [documentação](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Métricas e registo de diagnósticos
Métricas e registos podem ser exportados para [Operations Management Suite](https://azure.microsoft.com/resources/videos/operations-management-suite-oms-overview/) (OMS), especificamente a [do Log analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) componente e pode ser acessada programaticamente através de [depesquisaderegistos](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata).


> [!NOTE]
> A partir de Agosto de 2018, os registos atualmente estão sendo implementados para o SQL Data Warehouse

## <a name="next-steps"></a>Passos Seguintes
Os guias de procedimentos seguintes descrevem cenários comuns e casos quando monitorizar e gerir o armazém de dados de utilização:

- [Monitorizar a sua carga de trabalho do armazém de dados com DMVs](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

