---
title: Monitorização e otimização de desempenho - base de dados SQL do Azure | Documentos da Microsoft
description: Sugestões para ajuste na base de dados do Azure SQL através de avaliação e melhoria de desempenho.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 10/23/2018
ms.openlocfilehash: 0d728d81a29c5520938c8553c026727c0f94cc43
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957008"
---
# <a name="monitoring-and-performance-tuning"></a>Ajuste de monitorização e desempenho

Base de dados SQL do Azure é gerida automaticamente e serviço de dados flexível, onde pode facilmente monitorizar a utilização, adicionar ou remover recursos (CPU, memória, e/s), localizar recomendações que podem melhorar o desempenho da base de dados ou permitir que a base de dados se adaptar a sua carga de trabalho e automaticamente a otimizar o desempenho.

## <a name="monitoring-database-performance"></a>Monitorização do desempenho de base de dados

A monitorização do desempenho de uma base de dados SQL do Azure é iniciada com a monitorização da utilização de recursos em relação ao nível de desempenho de base de dados que escolher. Base de dados SQL do Azure permite-lhe identificar oportunidades para melhorar e otimizar o desempenho de consulta sem alterar recursos ao rever [recomendações de otimização de desempenho](sql-database-advisor.md). Os índices em falta e as consultas mal otimizadas são razões comuns para um desempenho fraco da base de dados. Pode aplicar estas recomendações de otimização para melhorar o desempenho da carga de trabalho. Também pode permitir que banco de dados SQL do Azure para [automaticamente otimizar o desempenho das suas consultas](sql-database-automatic-tuning.md) aplicando todos identificados, recomendações e verificar o que melhorar o desempenho da base de dados.

Tem as seguintes opções de monitorização e resolução de problemas de desempenho de base de dados:

- Na [portal do Azure](https://portal.azure.com), clique em **bases de dados SQL**, selecione a base de dados e, em seguida, utilize o gráfico de monitorização para procurar recursos máximas a aproximar-se. Consumo de DTU, é apresentado por predefinição. Clique em **editar** para alterar o intervalo de tempo e os valores mostrados.
- Uso [Query Performance Insight](sql-database-query-performance.md) para identificar as consultas que gastar o máximo de recursos.
- Uso [Assistente de base de dados SQL](sql-database-advisor-portal.md) para ver as recomendações para criar e remover índices, parametrização consultas e corrigir problemas de esquema.
- Uso [informações inteligentes do SQL Azure](sql-database-intelligent-insights.md) para a monitorização automática de desempenho da sua base de dados. Depois de um problema de desempenho é detetado, é gerado um registo de diagnóstico com detalhes e análise de causa raiz (RCA) do problema. Recomendação de melhoria do desempenho é fornecida sempre que possível.
- [Ativar o ajuste automático](sql-database-automatic-tuning-enable.md) e permita que o SQL do Azure, base de dados automaticamente problemas de desempenho de correção identificada.
- Uso [vistas de gestão dinâmica (DMVs)](sql-database-monitoring-with-dmvs.md), [eventos expandidos](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr)e o [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) para obter mais dos problemas de desempenho de resolução de problemas.

> [!TIP]
> Ver [orientação de desempenho](sql-database-performance-guidance.md) para descobrir técnicas que pode utilizar para melhorar o desempenho da base de dados do Azure SQL depois de identificar o problema de desempenho com um ou mais dos métodos acima.

## <a name="monitor-databases-using-the-azure-portal"></a>Monitorizar bases de dados com o portal do Azure

Na [portal do Azure](https://portal.azure.com/), pode monitorizar a uma utilização de base de dados individual s selecionando a base de dados e clicando no **monitorização** gráfico. É apresentada a janela **Métricas** que pode alterar ao clicar no botão **Editar gráfico**. Adicione as métricas seguintes:

- Percentagem de CPU
- Percentagem de DTU
- Percentagem de ES de Dados
- Percentagem de tamanho da Base de Dados

Depois de adicionar estas métricas, pode continuar a visualizá-los no **monitorização** do gráfico com mais informações sobre o **métrica** janela. As quatro métricas mostram a percentagem de utilização média relativa à **DTU** da base de dados. Consulte a [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) artigos para obter mais informações sobre escalões de serviço.  

![Monitorização da camada de serviços do desempenho da base de dados.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Também pode configurar alertas para as métricas de desempenho. Clique no botão **Adicionar alerta** na janela **Métricas**. Siga o assistente para configurar o alerta. Tem a opção de alertar se as métricas excederem um determinado limiar ou se a métrica descer abaixo de um determinado limiar.

Por exemplo, se espera que a carga de trabalho na sua base de dados aumente, pode optar por configurar um alerta por e-mail sempre que a base de dados atingir 80% em qualquer uma das métricas de desempenho. Pode usar isso como um aviso inicial para calcular quando poderá ter de mudar para o próximo tamanho de computação mais elevado.

As métricas de desempenho também podem ajudar a determinar se é possível mudar para um tamanho de computação mais baixo. Parta do princípio de que está a utilizar uma base de dados Standard S2 e que todas as métricas de desempenho mostram que, em média, a base de dados não utiliza mais de 10% em qualquer momento. É provável que a base de dados funcione corretamente no Standard S1. No entanto, tenha em atenção de cargas de trabalho que aumentam ou flutuam, antes de tomar a decisão de migrar para um tamanho de computação mais baixo.

## <a name="troubleshoot-performance-issues"></a>Resolver problemas de desempenho

Para diagnosticar e resolver problemas de desempenho, comece por compreender o estado de cada consulta do Active Directory e as condições que causam problemas de desempenho relevantes para o estado de cada carga de trabalho. Para melhorar o desempenho da base de dados do Azure SQL, compreenda a que cada pedido de consulta do Active Directory da sua aplicação está num Estado de espera ou de uma execução. Ao solucionar um problema de desempenho na base de dados do Azure SQL, tenha o gráfico a seguir em mente enquanto for lendo este artigo para diagnosticar e resolver problemas de desempenho.

![Estados de carga de trabalho](./media/sql-database-monitor-tune-overview/workload-states.png)

Para uma carga de trabalho com problemas de desempenho, o problema de desempenho pode ser devido a contenção de CPU (um **relacionados com a execução** condição) ou consultas individuais estão a aguardar em algo (um **relacionados com a espera** condição ).

## <a name="running-related-performance-issues"></a>Problemas de desempenho relacionados com a execução

Como orientação geral, se a sua utilização da CPU for consistentemente igual ou superior a 80%, tem um problema de desempenho relacionados com a execução. Se tiver um problema relacionado com a execução, ele pode ser causado por insuficiência de recursos de CPU ou podem estar relacionado a uma das seguintes condições:

- Demasiadas consultas em execução
- Demasiadas consultas compiling
- Um ou mais consultas em execução estiver a utilizar um plano de consulta abaixo do ideal

Se determinar que tem um problema de desempenho relacionados com a execução, o seu objetivo é identificar o problema exato a utilizar um ou mais métodos. Os métodos mais comuns para identificar problemas relacionados com a execução são:

- Utilize o [portal do Azure](#monitor-databases-using-the-azure-portal) para monitorizar a utilização de percentagem de CPU.
- Utilize o seguinte procedimento [exibições de gerenciamento dinâmico](sql-database-monitoring-with-dmvs.md):

  - [dm db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devolve o consumo de CPU, IO e memória para uma base de dados de base de dados do Azure SQL. Não existe uma linha para cada 15 segundos, mesmo que não existe nenhuma atividade na base de dados. Dados do histórico são mantidos durante uma hora.
  - [resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) retorna dados de armazenamento e utilização de CPU para uma base de dados do SQL do Azure. Os dados são recolhidos e agregados em intervalos de cinco minutos.

> [!IMPORTANT]
> Para um conjunto um consultas de T-SQL com esses DMVs para resolver problemas de utilização de CPU, consulte [problemas de desempenho de CPU identificar](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

Depois de identificar o problema, pode ajustar as consultas de problema ou atualizar o tamanho de computação ou camada para aumentar a capacidade da base de dados SQL do Azure que visam absorver os requisitos de CPU de serviço. Para obter informações sobre como dimensionar recursos de bases de dados individuais, consulte [Dimensionar recursos de base de dados individual no Azure SQL Database](sql-database-single-database-scale.md) e, para dimensionar os recursos para conjuntos elásticos, veja [Dimensionar recursos de conjunto elástico de SQL do Azure Base de dados](sql-database-elastic-pool-scale.md). Para obter informações sobre como dimensionar uma instância gerida, veja [limites de recursos de nível de instância](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

## <a name="waiting-related-performance-issues"></a>Problemas de desempenho relacionados com a espera

Assim que tenha a certeza de que não está a enfrentar uma CPU de alta, o problema de desempenho relacionados com a execução, está a enfrentar um problema de desempenho relacionados com a espera. Ou seja, seus recursos de CPU não estão sendo usados com eficiência porque está a aguardar a CPU em algum outro tipo de recurso. Neste caso, sua próxima etapa é identificar o que os recursos da CPU estão a aguardar. Categorias de tipo de espera de métodos mais comuns para mostrar parte superior:

- O [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) fornece estatísticas de espera por consulta ao longo do tempo. No Query Store, os tipos de espera são combinados em categorias de espera. O mapeamento das categorias de espera de espera tipos está disponível no [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql?view=sql-server-2017#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) devolve informações sobre todas as esperas encontrados por threads que executados durante a operação. Pode utilizar esta vista agregada para diagnosticar problemas de desempenho com a base de dados do Azure SQL e também com consultas específicas e lotes.
- [os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) retorna informações sobre a fila de espera de tarefas que estão a aguardar em algum recurso.

Conforme mostrado no gráfico anterior, as esperas mais comuns são:

- Bloqueios (bloqueio)
- E/S
- `tempdb`-relacionados com a contenção
- Esperas de concessão de memória

> [!IMPORTANT]
> Para um conjunto um consultas de T-SQL com esses DMVs para resolver estes problemas relacionados com a espera, consulte:
>
> - [Identificar problemas de desempenho de e/s](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar `tempdb` problemas de desempenho](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar as esperas de concessão de memória](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)

## <a name="improving-database-performance-with-more-resources"></a>Melhorando o desempenho de base de dados com mais recursos

Por fim, se não existirem não existem itens passíveis de ação que podem melhorar o desempenho da base de dados, pode alterar a quantidade de recursos disponíveis no banco de dados do Azure SQL. Pode atribuir mais recursos ao alterar o [escalão de serviço DTU](sql-database-service-tiers-dtu.md) de um único banco de dados ou aumentar as eDTUs de um conjunto elástico em qualquer altura. Em alternativa, se estiver a utilizar o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md), pode alterar a camada de serviço ou aumentar os recursos alocados à base de dados.

1. Para obter bases de dados individuais, pode [alterar escalões de serviço](sql-database-service-tiers-dtu.md) ou [recursos de computação](sql-database-service-tiers-vcore.md) sob demanda para melhorar o desempenho da base de dados.
2. Para várias bases de dados, considere a utilização [conjuntos elásticos](sql-database-elastic-pool-guidance.md) Dimensionar recursos automaticamente.

## <a name="tune-and-refactor-application-or-database-code"></a>Otimizar e um aplicativo do refactor ou código de base de dados

Pode alterar o código do aplicativo de maneira ideal, utilizar a base de dados, alterar os índices, forçar planos ou utilizar sugestões para adaptar manualmente a base de dados para a sua carga de trabalho. Encontrar algumas orientações e sugestões para ajuste manual e reescrever o código na [tópico de documentação de orientação de desempenho](sql-database-performance-guidance.md) artigo.

## <a name="next-steps"></a>Passos Seguintes

- Para ativar a otimização automática na base de dados do Azure SQL e permitir que a funcionalidade de otimização automática gerenciar totalmente a sua carga de trabalho, consulte [ativar o ajuste automático](sql-database-automatic-tuning-enable.md).
- Para utilizar a ajuste manual, pode rever [recomendações no portal do Azure de otimização](sql-database-advisor-portal.md) e aplicar manualmente os que melhorar o desempenho das suas consultas.
- Alterar os recursos que estão disponíveis na sua base de dados ao alterar [escalões de serviço de base de dados do Azure SQL](sql-database-performance-guidance.md)
