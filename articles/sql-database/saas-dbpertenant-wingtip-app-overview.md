---
title: Exemplo de aplicação multi-inquilino de base de dados SQL do Azure - Wingtip SaaS | Microsoft Docs
description: Aprender utilizando um exemplo de aplicação multi-inquilino que utiliza o SQL Database do Azure, o exemplo de Wingtip SaaS
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: cf54c789d766c4bd3d353028e75e34c961470070
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="introduction-to-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Introdução a uma aplicação SaaS multi-inquilino que utiliza o padrão de base de dados por inquilino com a base de dados SQL

Aplicação Wingtip SaaS é uma aplicação multi-inquilino de exemplo. A aplicação utiliza o padrão de aplicações de SaaS de base de dados por inquilino para servir vários inquilinos. A aplicação showcases funcionalidades da SQL Database do Azure que ativar cenários de SaaS, utilizando vários padrões de conceção e gestão de SaaS. Para obter rapidamente e em execução, a aplicação Wingtip SaaS implementa em menos de cinco minutos.

Scripts de gestão e código de origem de aplicação estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Antes de começar, consulte o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de gestão Wingtip pedidos de desbloqueio.

## <a name="application-architecture"></a>Arquitetura da aplicação

A aplicação Wingtip SaaS utiliza o modelo de base de dados por inquilino. Utiliza conjuntos elásticos SQL para maximizar a eficiência. Para o aprovisionamento e os inquilinos de mapeamento para os seus dados, é utilizada uma base de dados do catálogo. As principais aplicação Wingtip SaaS utiliza um conjunto com três inquilinos de exemplo, para além da base de dados do catálogo. Os servidores de catálogo e o inquilino foram aprovisionados com aliases DNS. Estes aliases são utilizados para manter uma referência para os recursos de Active Directory utilizada pela aplicação Wingtip. Estes aliases são atualizadas para apontar para recursos de recuperação nos tutoriais de recuperação após desastre. A concluir muitos dos resultados de tutoriais Wingtip SaaS na suplementos para a implementação inicial. Suplementos, como bases de dados analíticos e de gestão de esquema de base de dados em vários locais são introduzidos.


![Arquitetura de SaaS Wingtip](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


À medida que percorrer os tutoriais e trabalha com a aplicação, concentre-se nos padrões de SaaS como se relacionam com a camada de dados. Por outras palavras, focar-se na camada de dados e não overanalyze aplicação em si. Noções sobre a implementação destas SaaS padrões é a chave para implementar estes padrões nas suas aplicações. Considere também a quaisquer modificações necessárias para necessidades comerciais específicas.

## <a name="sql-database-wingtip-saas-tutorials"></a>Tutoriais do SQL Server da base de dados Wingtip SaaS

Depois de implementar a aplicação, explore os tutoriais seguintes que criar na implementação inicial. Estes tutoriais explorar padrões comuns da SaaS que tirar partido das funcionalidades integradas de base de dados SQL, o Azure SQL Data Warehouse e outros serviços do Azure. Tutoriais incluem scripts do PowerShell com explicações detalhadas. As explicações simplificam a compreensão e a implementação de padrões de gestão de SaaS mesmos nas suas aplicações.


| Tutorial | Descrição |
|:--|:--|
| [Orientações e sugestões para o exemplo de aplicação de SaaS multi-inquilino da base de dados SQL](saas-tenancy-wingtip-app-guidance-tips.md) | Transferir e executar scripts do PowerShell para preparar as partes da aplicação. |
|[Implementar e explorar a aplicação Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)|  Implementar e explorar a aplicação Wingtip SaaS com a sua subscrição do Azure. |
|[Aprovisionamento e o catálogo de inquilinos](saas-dbpertenant-provision-and-catalog.md)| Saiba como a aplicação se liga ao inquilinos através da utilização de uma base de dados do catálogo e como o catálogo de mapas de inquilinos para os seus dados. |
|[Monitorizar e gerir o desempenho](saas-dbpertenant-performance-monitoring.md)| Saiba como utilizar funcionalidades de monitorização da base de dados do SQL Server e definir alertas quando os limiares de desempenho for excedidos. |
|[Monitor de análise de registos do Azure](saas-dbpertenant-log-analytics.md) | Saiba como utilizar [Log Analytics](../log-analytics/log-analytics-overview.md) monitorizar grandes quantidades de recursos entre vários conjuntos. |
|[Restaurar um inquilino único](saas-dbpertenant-restore-single-tenant.md)| Saiba como restaurar uma base de dados do inquilino para um ponto anterior no tempo. Além disso, saiba como restaurar uma base de dados paralelas, deixando a base de dados existente do inquilino online. |
|[Gerir o esquema de base de dados do inquilino](saas-tenancy-schema-management.md)| Saiba como atualizar o esquema e atualizar os dados de referência em todas as bases de dados do inquilino. |
|[Executar consultas distribuídas cross-inquilino](saas-tenancy-cross-tenant-reporting.md) | Criar uma base de dados de análise do ad-hoc e executar consultas distribuídas em tempo real em todos os inquilinos.  |
|[Executar a análise nos dados de inquilino extraídos](saas-tenancy-tenant-analytics.md) | Extrair dados do inquilino para um armazém de dados ou base de dados de análise para consultas de análise offline. |


## <a name="next-steps"></a>Passos Seguintes

- [Orientações gerais e sugestões quando implementar e utilizar o exemplo de aplicação Wingtip bilhetes SaaS](saas-tenancy-wingtip-app-guidance-tips.md)
- [Implementar a aplicação Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)
