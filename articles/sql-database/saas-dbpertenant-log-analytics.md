---
title: Utilizar o Log Analytics com uma aplicação multi-inquilino de base de dados SQL | Documentos da Microsoft
description: Configurar e utilizar o Log Analytics com uma aplicação SaaS de base de dados SQL do Azure multi-inquilino
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 3ca2f811ff0ac81ea70ec0b22d7429cdc5604171
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420187"
---
# <a name="set-up-and-use-log-analytics-with-a-multitenant-sql-database-saas-app"></a>Configurar e utilizar o Log Analytics com uma aplicação de SQL da base de dados SaaS multi-inquilino

Neste tutorial, vai configurar e utilizar o Azure [do Log Analytics](/azure/log-analytics/log-analytics-overview) para monitorizar conjuntos elásticos e bases de dados. Este tutorial baseia-se a [tutorial de monitorização e gestão de desempenho](saas-dbpertenant-performance-monitoring.md). Ele mostra como utilizar o Log Analytics para aumentar a monitorização e alertas fornecidos no portal do Azure. O log Analytics oferece suporte a milhares de monitorização de conjuntos elásticos e centenas de milhares de bases de dados. O log Analytics fornece uma única solução de monitorização, que pode integrar a monitorização de diferentes aplicações e serviços do Azure através de várias subscrições do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Instalar e configurar o Log Analytics.
> * Utilize o Log Analytics para monitorizar conjuntos e bases de dados.

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* A aplicação de base de dados por inquilino Wingtip Tickets SaaS é implementada. Para implementar em menos de cinco minutos, veja [implementar e explorar a aplicação de base de dados por inquilino Wingtip Tickets SaaS](saas-dbpertenant-get-started-deploy.md).
* O Azure PowerShell está instalado. Para obter mais informações, consulte [introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Consulte a [tutorial de monitorização e gestão de desempenho](saas-dbpertenant-performance-monitoring.md) para uma discussão sobre cenários SaaS e padrões e como elas afetam os requisitos numa solução de monitorização.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-log-analytics"></a>Monitorizar e gerir o desempenho de agrupamento de base de dados e elástico com o Log Analytics

Para a base de dados SQL do Azure, os alertas e monitorização estão disponível em bases de dados e agrupamentos no portal do Azure. Estes alertas e monitorização incorporada, é conveniente, mas também é recursos específicos. Isso significa que ele é menos adequado para monitorizar grandes instalações ou fornecer uma vista unificada em recursos e subscrições.

Para cenários de volume elevado, pode utilizar o Log Analytics para monitorização e alertas. Log Analytics é um serviço do Azure separado que permite a análise sobre os registos de diagnóstico e telemetria recolhidos numa área de trabalho de potencialmente vários serviços. O log Analytics proporciona uma consulta incorporada, linguagem e dados de ferramentas de visualização que permitem a análise de dados operacionais. A solução de análise de SQL fornece várias predefinido conjunto elástico e da base de dados de monitorização e alerta consultas e vistas. O log Analytics também fornece um estruturador de vistas personalizado.

Soluções de análise e áreas de trabalho de análise do registo abra no portal do Azure e no Operations Management Suite. O portal do Azure é o mais recente ponto de acesso, mas pode ser por trás de algumas áreas do portal do Operations Management Suite.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Criar dados de diagnóstico de desempenho mediante a simulação de uma carga de trabalho em seus inquilinos 

1. No ISE do PowerShell, abra *.... \\WingtipTicketsSaaS-MultiTenantDb-mestre\\módulos de aprendizagem\\monitorização do desempenho e de gestão\\Demo-PerformanceMonitoringAndManagement.ps1*. Mantenha este script aberto, uma vez que pode querer executar vários da carga de cenários de geração durante este tutorial.
1. Se ainda não fez isso, Aprovisione um lote de inquilinos para tornar o contexto de monitorização mais interessantes. Este processo demora alguns minutos.

   a. Definir **$DemoScenario = 1**, _aprovisionar um lote de inquilinos_.

   b. Para executar o script e implementar um 17 inquilinos adicionais, pressione F5.

1. Agora, inicie o gerador de carga para executar uma carga simulada em todos os inquilinos.

    a. Definir **$DemoScenario = 2**, _gerar carga de intensidade normal (aprox. 30 DTU)_.

    b. Para executar o script, pressione F5.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter os scripts de base de dados por inquilino aplicação Wingtip Tickets SaaS

O código de origem do aplicativo e scripts de banco de dados multi-inquilino de Wingtip Tickets SaaS estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Para obter os passos transferir e desbloquear os scripts do PowerShell de bilhetes Wingtip, consulte a [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalar e configurar o Log Analytics e a solução de análise de SQL do Azure

Log Analytics é um serviço separado que tem de ser configurado. O log Analytics recolhe dados de registo, telemetria e métricas numa área de trabalho do Log Analytics. Assim como outros recursos no Azure, tem de ser criada uma área de trabalho do Log Analytics. A área de trabalho não precisa ser criado no mesmo grupo de recursos que as aplicações que monitoriza. Isso quase sempre faz mais sentido no entanto. Para a aplicação Wingtip Tickets, utilize um grupo de recursos para se certificar de que a área de trabalho é eliminada com o aplicativo.

1. No ISE do PowerShell, abra *.... \\WingtipTicketsSaaS-MultiTenantDb-mestre\\módulos de aprendizagem\\monitorização do desempenho e de gestão\\do Log Analytics\\Demo-LogAnalytics.ps1*.
1. Para executar o script, pressione F5.

Agora é possível abrir o Log Analytics no portal do Azure ou do portal do Operations Management Suite. Demora alguns minutos para recolher dados telemétricos na área de trabalho do Log Analytics e para torná-lo visível. Mais tempo deixar o sistema de recolha de dados de diagnóstico, a experiência é mais interessante. 

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Utilizar o Log Analytics e a Solução de Análise de SQL para monitorizar conjuntos e bases de dados


Neste exercício, abra o Log Analytics e o portal do Operations Management Suite para ver a telemetria recolhida para os conjuntos e bases de dados.

1. Navegue para o [portal do Azure](https://portal.azure.com). Selecione **todos os serviços** para abrir o Log Analytics. Em seguida, procure o Log Analytics.

   ![Abra o Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Selecione a área de trabalho com o nome _wtploganalytics -&lt;usuário&gt;_.

1. Selecione **Descrição Geral** para abrir a solução Log Analytics no portal do Azure.

   ![Descrição geral](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Poderá demorar alguns minutos antes da solução ficar ativa. 

1. Selecione o **a análise de SQL do Azure** mosaico para abri-lo.

    ![Mosaico de descrição geral](media/saas-dbpertenant-log-analytics/overview.png)

1. As vistas na solução desloque-se de lado, com sua própria barra de deslocamento interno na parte inferior. Se necessário, atualize a página.

1. Para explorar a página de resumo, selecione os mosaicos ou bases de dados individuais para abrir um Explorador de desagregação.

    ![Dashboard de análise de registo](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Altere a definição de filtro para modificar o intervalo de tempo. Neste tutorial, selecione **última 1 hora**.

    ![Filtro de tempo](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Selecione uma base de dados para explorar a utilização da consulta e métricas para essa base de dados.

    ![Análise de base de dados](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Para ver as métricas de utilização, desloque-se a página de análise para a direita.
 
     ![Métricas de base de dados](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Desloque-se a página de análise para a esquerda e selecione o mosaico de servidor no **informações de recursos** lista.  

    ![Lista de informações de recursos](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    É aberta uma página que mostra os conjuntos e bases de dados no servidor.

    ![Servidor com conjuntos e bases de dados](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Selecione um agrupamento. Na página de agrupamento que se abre, desloque-se para a direita para ver as métricas de conjunto. 

    ![Métricas de conjunto](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


1. Na área de trabalho do Log Analytics, selecione **Portal do OMS** para abrir a área de trabalho para lá.

    ![Mosaico do Operations Management Suite Portal](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

No portal do Operations Management Suite, pode explorar os dados de registo e métrica na área de trabalho ainda mais. 

Monitorização e alertas no Log Analytics são baseadas em consultas sobre os dados na área de trabalho, ao contrário de alertas definidos em cada recurso no portal do Azure. Baseando alertas em consultas, pode definir um único alerta que procura em todas as bases de dados, em vez de definir um por base de dados. Consultas estão limitadas apenas pelos dados disponíveis na área de trabalho.

Para obter mais informações sobre como utilizar o Log Analytics para consultar e definir alertas, consulte [trabalhar com regras de alerta no Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Análise de registos para cobranças de base de dados SQL com base no volume de dados na área de trabalho. Neste tutorial, criou uma área de trabalho gratuita, limitada a 500 MB por dia. Após esse limite for atingido, os dados já não são adicionados à área de trabalho.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Instalar e configurar o Log Analytics.
> * Utilize o Log Analytics para monitorizar conjuntos e bases de dados.

Experimente o [tutorial de análise de inquilinos](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que têm por base a implementação de base de dados por inquilino aplicação Wingtip Tickets SaaS inicial](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Log Analytics do Azure](../log-analytics/log-analytics-azure-sql.md)
