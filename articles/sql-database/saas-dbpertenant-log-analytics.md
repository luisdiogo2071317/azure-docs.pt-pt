---
title: Utilize a análise de registos com uma aplicação multi-inquilino de base de dados SQL | Microsoft Docs
description: Configurar e utilizar a análise de registos com uma aplicação SaaS de base de dados do SQL Azure multi-inquilino
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 02c380c78fa773b56a3c8b666e890836a3d8e54b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="set-up-and-use-log-analytics-with-a-multitenant-sql-database-saas-app"></a>Configurar e utilizar a análise de registos com uma aplicação SaaS de base de dados do SQL Server multi-inquilino

Neste tutorial, configurar e utilizar o Azure [Log Analytics](/azure/log-analytics/log-analytics-overview) para monitorizar os conjuntos elásticos e bases de dados. Este tutorial baseia-se a [tutorial de gestão e monitorização de desempenho](saas-dbpertenant-performance-monitoring.md). Mostra como utilizar a análise de registos para aumentar a monitorização e alertas fornecido no portal do Azure. Análise de registos suporta milhares de monitorização de conjuntos elásticos e centenas de milhares de bases de dados. Análise de registos fornece uma solução de monitorização único, que pode integrar a monitorização de diferentes aplicações e serviços do Azure através de várias subscrições do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Instale e configure a análise de registos.
> * Utilize a análise de registos para monitorizar os conjuntos e as bases de dados.

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* A aplicação de base de dados por inquilino Wingtip bilhetes SaaS é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a aplicação de base de dados por inquilino Wingtip bilhetes SaaS](saas-dbpertenant-get-started-deploy.md).
* O Azure PowerShell está instalado. Para obter mais informações, consulte [começar com o Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Consulte o [tutorial de gestão e monitorização de desempenho](saas-dbpertenant-performance-monitoring.md) para um debate dos cenários de SaaS e padrões e como estas afetam os requisitos de uma solução de monitorização.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-log-analytics"></a>Monitorizar e gerir o desempenho de agrupamento de base de dados e elástico com a análise de registos

Para a base de dados SQL do Azure, monitorização e alertas estão disponível em bases de dados e agrupamentos no portal do Azure. Esta monitorização incorporada e os alertas são conveniente, mas também é específica do recurso. Isto significa que menos também é adequada para monitorizar grandes instalações de ou para fornecer uma vista unificada através de recursos e as subscrições.

Para cenários de volume elevado, pode utilizar a análise de registos para monitorização e alertas. Análise de registos é um serviço do Azure separado que lhe permite a análise através de registos de diagnóstico e de telemetria recolhidos numa área de trabalho de potencialmente vários serviços. Análise de registos fornece uma consulta incorporada idioma e dados visualização as ferramentas que permite a análise de dados operacional. A solução de análise do SQL Server fornece vários predefinido conjunto elástico e base de dados de monitorização e alerta vistas e consultas. Análise de registos também fornece um estruturador de vistas personalizadas.

Soluções de áreas de trabalho e a análise de análise do registo abrir no portal do Azure e no Operations Management Suite. O portal do Azure é o mais recente ponto de acesso, mas poderá ser por trás do portal do Operations Management Suite em algumas áreas.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Criar dados de diagnóstico de desempenho simulando uma carga de trabalho nos seus inquilinos 

1. No ISE do PowerShell, abra *... \\Mestre de MultiTenantDb WingtipTicketsSaaS\\Learning módulos\\monitorização de desempenho e gestão\\demonstração PerformanceMonitoringAndManagement.ps1*. Mantenha este script aberto porque poderá querer executar a carga de vários cenários de geração durante este tutorial.
2. Se ainda não o feito já, Aprovisione um lote de inquilinos para tornar o contexto de monitorização mais interessante. Este processo demora alguns minutos.

   a. Definir **$DemoScenario = 1**, _aprovisionar um lote de inquilinos_.

   b. Para executar o script e implementar uma inquilinos 17 adicionais, prima F5.

3. Inicie agora o gerador de carga para executar uma carga simulada em todos os inquilinos.

    a. Definir **$DemoScenario = 2**, _carga normal intensidade de gerar (approx. 30 DTU)_.

    b. Para executar o script, prima F5.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter os scripts de aplicação de base de dados por inquilino Wingtip bilhetes SaaS

O código fonte da aplicação e scripts de base de dados multi-inquilino Wingtip bilhetes SaaS estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Para obter os passos transferir e desbloquear os scripts do PowerShell de bilhetes Wingtip, consulte o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalar e configurar a análise de registos e a solução de análise de SQL do Azure

Análise de registos é um serviço separado que deve ser configurado. Análise de registos recolhe dados de registo, telemetria e métricas numa área de trabalho de análise de registos. Tal como outros recursos no Azure, tem de ser criada uma área de trabalho de análise de registos. A área de trabalho não tem de ser criada no mesmo grupo de recursos, como as aplicações que monitoriza. Se o fizer, por isso, muitas vezes, mais adequado apesar. Para a aplicação de bilhetes Wingtip, utilize um grupo de recursos única para se certificar de que a área de trabalho é eliminada com a aplicação.

1. No ISE do PowerShell, abra *... \\Mestre de MultiTenantDb WingtipTicketsSaaS\\Learning módulos\\monitorização de desempenho e gestão\\Iniciar análise\\demonstração LogAnalytics.ps1*.
2. Para executar o script, prima F5.

Agora pode abrir a análise de registos no portal do Azure ou no portal do Operations Management Suite. Demora alguns minutos para recolher a telemetria na área de trabalho de análise de registos e torná-lo visível. Mais tempo deixar o sistema de recolha de dados de diagnóstico, a experiência é mais interessante. 

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Utilizar o Log Analytics e a Solução de Análise de SQL para monitorizar conjuntos e bases de dados


Neste exercício, abra o portal de Operations Management Suite e análise de registos para ver a telemetria recolhida para as bases de dados e agrupamentos.

1. Navegue para o [portal do Azure](https://portal.azure.com). Selecione **todos os serviços** para abrir a análise de registos. Em seguida, procure a análise de registos.

   ![Análise de registo abertos](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

2. Selecione a área de trabalho com o nome _wtploganalytics -&lt;utilizador&gt;_.

3. Selecione **Descrição Geral** para abrir a solução Log Analytics no portal do Azure.

   ![Descrição geral](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Poderá demorar alguns minutos antes da solução está ativa. 

4. Selecione o **análise do Azure SQL** mosaico para abri-lo.

    ![Mosaico de descrição geral](media/saas-dbpertenant-log-analytics/overview.png)

5. As vistas na solução lado, desloque com as seus próprios barra de deslocamento interna na parte inferior. Se necessário, atualize a página.

6. Para explorar a página de resumo, selecione os mosaicos ou bases de dados individuais para abrir o Explorador de desagregação.

    ![Dashboard de análise do registo](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

7. Altere a definição de filtro para modificar o intervalo de tempo. Para este tutorial, selecione **última 1 hora**.

    ![filtro de tempo](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

8. Selecione uma base de dados para explorar a utilização de consulta e métricas para essa base de dados.

    ![análise de base de dados](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

9. Para ver as métricas de utilização, desloque-se a página de análise para a direita.
 
     ![métricas de base de dados](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

10. Desloque-se a página de análise para a esquerda e selecione o mosaico de servidor no **informações de recurso** lista.  

    ![Lista de informações de recursos](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    É aberta uma página que mostra os conjuntos e as bases de dados no servidor.

    ![servidor com conjuntos e as bases de dados](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

11. Selecione um conjunto. Na página de agrupamento que se abre, desloque-se para a direita para ver as métricas de agrupamento. 

    ![métricas de conjunto](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


12. Na área de trabalho de análise de registos, selecione **Portal do OMS** para abrir a área de trabalho existe.

    ![Mosaico do Operations Management Suite Portal](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

No portal do Operations Management Suite, pode explorar os dados de registo e a métrica na área de trabalho adicional. 

Monitorização e alertas na análise de registos são baseadas em consultas sobre os dados na área de trabalho, ao contrário de alertas definido em cada recurso no portal do Azure. Por basing alertas em consultas, pode definir um único alerta que se pareça através de todas as bases de dados, em vez de uma definição por base de dados. As consultas estão limitadas apenas pelos dados disponíveis na área de trabalho.

Para obter mais informações sobre como utilizar a análise de registos para consultar e definir alertas, consulte [trabalhar com regras de alertas na análise de registos](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Análise de registos para custos de base de dados SQL com base no volume de dados na área de trabalho. Neste tutorial, vai criar uma área de trabalho gratuita, que é limitado a 500 MB por dia. Após esse limite é atingido, os dados já não são adicionados à área de trabalho.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Instale e configure a análise de registos.
> * Utilize a análise de registos para monitorizar os conjuntos e as bases de dados.

Repita o [tutorial de análise do inquilino](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que criar na implementação da aplicação de base de dados por inquilino inicial do Wingtip bilhetes SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Log Analytics do Azure](../log-analytics/log-analytics-azure-sql.md)
