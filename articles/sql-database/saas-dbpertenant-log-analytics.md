---
title: "Utilizar o Log Analytics com uma aplicação multi-inquilino da Base de Dados SQL | Microsoft Docs"
description: "Configurar e utilizar a análise de registos (OMS) com uma aplicação de SaaS de base de dados do SQL Azure multi-inquilino"
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: billgib; sstein
ms.openlocfilehash: 90510520e5f6bbfa8aea4026d7437a4a4881984f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Configurar e utilizar a análise de registos (OMS) com uma aplicação de SaaS de base de dados do SQL Azure multi-inquilino

Neste tutorial, configurar e utilizar *Log Analytics ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* para conjuntos elásticos e bases de dados de monitorização. Este tutorial baseia-se a [tutorial de gestão e monitorização do desempenho](saas-dbpertenant-performance-monitoring.md). Mostra como utilizar *Log Analytics* para melhorar a monitorização e alertas fornecido no portal do Azure. Análise de registos suporta milhares de monitorização de conjuntos elásticos e centenas de milhares de bases de dados. Análise de registos fornece uma solução de monitorização único, que pode integrar a monitorização de diferentes aplicações e serviços do Azure, através de várias subscrições do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Instalar e configurar o Log Analytics (OMS)
> * Utilizar o Log Analytics para monitorizar conjuntos e bases de dados

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* A aplicação de Wingtip bilhetes SaaS da base de dados por inquilino é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a aplicação de Wingtip bilhetes SaaS da base de dados por inquilino](saas-dbpertenant-get-started-deploy.md)
* O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

Veja o [Tutorial de Monitorização e Gestão do Desempenho](saas-dbpertenant-performance-monitoring.md) para ver um debate sobre padrões e cenários SaaS e como afetam os requisitos numa solução de monitorização.

## <a name="monitoring-and-managing-database-and-elastic-pool-performance-with-log-analytics-or-operations-management-suite-oms"></a>Monitorizar e gerir o desempenho de agrupamento de base de dados e elástico com o Log Analytics ou o Operations Management Suite (OMS)

Para a base de dados do SQL Server, monitorização e alertas estão disponível em bases de dados e agrupamentos no portal do Azure. Esta monitorização incorporada e os alertas são conveniente, mas a recursos específicos, é menos também adequada para monitorização grandes instalações de ou para fornecer uma vista unificada através de recursos e as subscrições.

Para cenários de volume elevado, análise de registos pode ser utilizado para monitorização e alertas. Análise de registos é um serviço separada do Azure que permite a análise através de registos de diagnóstico e telemetria recolhidos numa área de trabalho de potencialmente vários serviços. Análise de registos fornece uma consulta incorporada ferramentas de visualização de dados e de idioma permitir análise de dados operacional. A solução de análise do SQL Server fornece vários predefinido conjunto elástico e base de dados de monitorização e alerta vistas e consultas. O OMS também fornece um estruturador de vistas personalizado.

As áreas de trabalho do Log Analytics e as soluções de análise podem ser abertas no portal do Azure e no OMS. O portal do Azure é o mais recente ponto de acesso, mas pode ficar atrás em relação ao portal do OMS em algumas áreas.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Criar dados de diagnóstico de desempenho simulando uma carga de trabalho nos seus inquilinos 

1. No **ISE do PowerShell**, abra *... \\Mestre de MultiTenantDb WingtipTicketsSaaS\\Learning módulos\\monitorização de desempenho e gestão\\* * PerformanceMonitoringAndManagement.ps1*** de demonstração. Mantenha este script aberto, uma vez que poderá querer executar vários cenários de geração de carga durante este tutorial.
1. Se ainda não o tiver o feito, Aprovisione um lote de inquilinos para fornecer um contexto de monitorização mais interessante. Esta ação demora alguns minutos:
   1. Definir **$DemoScenario = 1**, _aprovisionar um lote de inquilinos_
   1. Para executar o script e implementar uma inquilinos 17 adicionais, prima **F5**.  

1. Inicie agora o gerador de carga para executar uma carga simulada em todos os inquilinos.  
    1. Definir **$DemoScenario = 2**, _carga normal intensidade de gerar (approx. 30 DTU)_.
    1. Para executar o script, prima **F5**.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter os scripts de aplicação Wingtip bilhetes SaaS da base de dados por inquilino

Os scripts de base de dados do Wingtip bilhetes SaaS multi-inquilino e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts do PowerShell de bilhetes Wingtip de desbloqueio.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalar e configurar o Log Analytics e a solução Análise de SQL do Azure

O Log Analytics é um serviço em separado que tem de ser configurado. Análise de registos recolhe dados de registo, telemetria e métricas numa área de trabalho de análise de registo. Uma área de trabalho de análise do registo é um recurso, tal como outros recursos no Azure e tem de ser criada. Enquanto a área de trabalho não tem de ser criada no mesmo grupo de recursos de aplicação (ões) que está a monitorizar, se o fizer, por isso, muitas vezes, faz com que a maioria dos sentido. Para a aplicação de bilhetes Wingtip, utilizar um grupo de recursos única garante que a área de trabalho é eliminada com a aplicação.

1. No **ISE do PowerShell**, abra *... \\Mestre de MultiTenantDb WingtipTicketsSaaS\\Learning módulos\\monitorização de desempenho e gestão\\Iniciar análise\\* * LogAnalytics.ps1*** de demonstração.
1. Para executar o script, prima **F5**.

Nesta fase, deve ser capaz de análise de registos de abrir o portal do Azure (ou o portal do OMS). Demora alguns minutos para que a telemetria recolhida na área de trabalho de análise de registos e ficar visível. Mais tempo deixar o sistema de recolha de dados de diagnóstico a experiência é mais interessante. Agora é o momento indicado para tomar um café. Só tem de verificar se o gerador de carga ainda está em execução!

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Utilizar o Log Analytics e a Solução de Análise de SQL para monitorizar conjuntos e bases de dados


Neste exercício, abra o portal do OMS e de análise de registo para ver a telemetria a ser recolhida para as bases de dados e agrupamentos.

1. Navegue para o [portal do Azure](https://portal.azure.com) e abra a análise de registos, clicando em **todos os serviços**, em seguida, procure a análise de registos:

   ![abrir o Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Selecione a área de trabalho com o nome _wtploganalytics -&lt;utilizador&gt;_.

1. Selecione **Descrição Geral** para abrir a solução Log Analytics no portal do Azure.

   ![overview-link](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Pode demorar alguns minutos antes da solução está ativa. Seja paciente!

1. Clique no mosaico Análise de SQL do Azure para o abrir.

    ![descrição geral](media/saas-dbpertenant-log-analytics/overview.png)

    ![análise](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. As vistas na solução lado, desloque com as seus próprios barra de deslocamento interna na parte inferior (Atualize a página se necessário).

1. Explore a página de resumo clicando nos mosaicos ou uma base de dados individual para abrir o Explorador de desagregação.

1. Alterar o filtro de definição para modificar o intervalo de tempo - para este tutorial escolha _última 1 hora_

    ![filtro de tempo](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Selecione uma base de dados para explorar a utilização de consulta e métricas para essa base de dados.

    ![análise de base de dados](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Para ver a utilização de métricas, desloque-se a página de análise para a direita.
 
     ![métricas de base de dados](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Desloque-se a página de análise para a esquerda e clique no mosaico de servidor na lista de informações de recurso. Esta ação abre uma página que mostra os conjuntos e as bases de dados no servidor. 

     ![informações de recurso](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

 
     ![servidor com conjuntos e as bases de dados](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. No servidor de página que abre-se de que mostra os conjuntos e bases de dados no servidor, clique no conjunto.  Na página de agrupamento que se abre, desloque-se para a direita para ver as métricas de agrupamento.  

     ![métricas de conjunto](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)



1. Novamente na área de trabalho de análise de registos, selecione **Portal do OMS** para abrir a área de trabalho existe.

    ![oms](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

No portal do OMS, pode explorar os dados de registo e a métrica na área de trabalho adicional.  

A monitorização e alertas na análise de registos e OMS baseada em consultas sobre os dados na área de trabalho, ao contrário de alertas definido em cada recurso no portal do Azure. Por basing alertas em consultas, pode definir um único alerta que se pareça através de todas as bases de dados, em vez de uma definição por base de dados. As consultas são apenas limitadas pelos dados disponíveis na área de trabalho.

Para obter mais informações sobre a utilização do OMS para consultar e definir alertas, consulte, [trabalhar com regras de alertas na análise de registos](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-alerts-creating).

O Log Analytics da Base de Dados SQL é cobrado com base no volume de dados na área de trabalho. Neste tutorial, vai criar uma área de trabalho gratuita, que é limitado a 500 MB por dia. Assim que esse limite é atingido, os dados já não são adicionados à área de trabalho.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Instalar e configurar o Log Analytics (OMS)
> * Utilizar o Log Analytics para monitorizar conjuntos e bases de dados

[Tutorial de análise de inquilinos](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que criar após a implementação de aplicação Wingtip bilhetes SaaS da base de dados por inquilino inicial](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Log Analytics do Azure](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
