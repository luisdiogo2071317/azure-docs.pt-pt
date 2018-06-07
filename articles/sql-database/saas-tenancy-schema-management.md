---
title: Gerir o esquema da Base de Dados SQL numa aplicação multi-inquilino | Microsoft Docs
description: Gerir o Esquema para vários inquilinos numa aplicação multi-inquilino que utiliza a Base de Dados SQL do Azure
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
ms.openlocfilehash: 2e4af3e3e1ef1d9da7c66b929885e3ec749b462f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646277"
---
# <a name="manage-schema-in-a-saas-application-using-the-database-per-tenant-pattern-with-azure-sql-database"></a>Gerir o esquema de uma aplicação SaaS com o padrão de base de dados por inquilino a SQL Database do Azure

À medida que uma aplicação de base de dados evolui, as alterações inevitavelmente precisam de ser efetuadas aos dados da base de dados de esquema ou de referência.  Tarefas de manutenção de base de dados também são necessários periodicamente. Gerir uma aplicação que utiliza a base de dados por padrão de inquilino necessita de aplicar estas alterações ou tarefas de manutenção através de uma frota de bases de dados do inquilino.

Este tutorial explicar dois cenários - implementação de atualizações de dados de referência para todos os inquilinos e reconstruir um índice na tabela que contém os dados de referência. O [as tarefas elásticas](sql-database-elastic-jobs-overview.md) funcionalidade é utilizada para executar estas ações todas as bases de dados do inquilino e a base de dados do modelo utilizado para criar bases de dados de novo inquilino.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]

> * Criar uma tarefa de agente
> * Fazer com que as tarefas de T-SQL ser executado em todas as bases de dados do inquilino
> * Dados de referência de atualização em todas as bases de dados do inquilino
> * Criar um índice numa tabela em todas as bases de dados de inquilinos


Para concluir este tutorial, devem ser cumpridos os seguintes pré-requisitos:

* A aplicação de Wingtip bilhetes SaaS da base de dados por inquilino é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a base de dados de SaaS de bilhetes Wingtip por aplicação do inquilino](saas-dbpertenant-get-started-deploy.md)
* O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Está instalada a versão mais recente do SQL Server Management Studio (SSMS). [Transferir e instalar o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Este tutorial utiliza funcionalidades do serviço base de dados do SQL Server que estão numa (tarefas de bases de dados elásticas) de pré-visualização limitada. Se pretender fazer neste tutorial, forneça o ID de subscrição para SaaSFeedback@microsoft.com com o requerente = elástico de tarefas de pré-visualização. Após receber a confirmação indicando que a sua subscrição foi ativada, deverá [transferir e instalar os cmdlets mais recentes das tarefas da versão de pré-lançamento](https://github.com/jaredmoo/azure-powershell/releases). Esta pré-visualização é limitada, por isso, contacte SaaSFeedback@microsoft.com para suporte ou questões relacionados.

## <a name="introduction-to-saas-schema-management-patterns"></a>Introdução aos padrões de gestão do esquema de SaaS

A base de dados por padrão de inquilino isola efetivamente os dados de inquilino, mas aumenta o número de bases de dados para gerir e manter. [As tarefas elásticas](sql-database-elastic-jobs-overview.md) facilita a administração e gestão de bases de dados do SQL Server. As tarefas permitem-lhe segura e fiável, executar tarefas (scripts T-SQL) em relação a um grupo de bases de dados. As tarefas podem implementar o esquema e de alterações de dados de referência comuns em todas as bases de dados do inquilino de uma aplicação. As tarefas elásticas também podem ser utilizadas para manter um *modelo* base de dados utilizado para criar novos inquilinos, garantindo sempre tem os dados mais recentes do esquema e de referência.

![ecrã](media/saas-tenancy-schema-management/schema-management-dpt.png)


## <a name="elastic-jobs-limited-preview"></a>Pré-visualização limitada das Tarefas Elásticas

Há uma nova versão do tarefas elásticas que é agora uma funcionalidade integrada da SQL Database do Azure. Esta nova versão das Tarefas Elásticas está atualmente em pré-visualização limitada. Esta pré-visualização limitada suporta atualmente a utilizar o PowerShell para criar um agente de tarefa e o T-SQL para criar e gerir tarefas.

> [!NOTE]
> Este tutorial utiliza funcionalidades do serviço base de dados do SQL Server que estão numa (tarefas de bases de dados elásticas) de pré-visualização limitada. Se pretender fazer neste tutorial, forneça o ID de subscrição para SaaSFeedback@microsoft.com com o requerente = elástico de tarefas de pré-visualização. Após receber a confirmação indicando que a sua subscrição foi ativada, deverá [transferir e instalar os cmdlets mais recentes das tarefas da versão de pré-lançamento](https://github.com/jaredmoo/azure-powershell/releases). Esta pré-visualização é limitada, por isso, contacte SaaSFeedback@microsoft.com para suporte ou questões relacionados.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obter a base de dados de SaaS de bilhetes Wingtip por scripts de aplicação do inquilino

Os scripts de código e gestão da origem de aplicação estão disponíveis no [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repositório do GitHub. Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de Wingtip SaaS de pedidos de desbloqueio.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Criar um agente de tarefa da base de dados e o novo agente de tarefa

Neste tutorial necessita de que utilizar o PowerShell para criar um agente de tarefa e a respetiva base de dados de agente de tarefas de cópia de segurança. A base de dados do agente de tarefa contém definições de tarefas, o estado de tarefa e o histórico. Depois do agente de tarefa e a respetiva base de dados são criados, pode criar e monitorizar tarefas imediatamente.

1. **No ISE do PowerShell**, abra... \\Learning módulos\\esquema gestão\\*demonstração SchemaManagement.ps1*.
1. Prima **F5** para executar o script.

O *demonstração SchemaManagement.ps1* script chamadas a *implementar SchemaManagement.ps1* script para criar uma base de dados do SQL Server com o nome *osagent* no servidor de catálogo. Em seguida, cria o agente de tarefa, utilizando a base de dados como um parâmetro.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Criar uma tarefa para implementar novos dados de referência para todos os inquilinos

Na aplicação Wingtip bilhetes, cada base de dados do inquilino inclui um conjunto de tipos de venue suportados. Cada venue tem um tipo de venue específico, que define o tipo de eventos que podem ser alojados e determina a imagem de fundo utilizada na aplicação. Para a aplicação suportar novos tipos de eventos, estes dados de referência tem de ser tipos venue novas e atualizadas adicionados.  Neste exercício, vai implementar uma atualização para todas as bases de dados de inquilinos para adicionar dois tipos de local adicionais: *Corrida de Motos* e *Clube de Natação*.

Em primeiro lugar, reveja os tipos de venue incluídos em cada base de dados do inquilino. Ligar a uma das bases de dados do inquilino no SQL Server Management Studio (SSMS) e Inspecione a tabela de VenueTypes.  Também pode consultar esta tabela em do editor de consultas no portal do Azure, acedido a partir da página de base de dados. 

1. Abra o SSMS e ligar ao servidor do inquilino: *tenants1-dpt -&lt;utilizador&gt;. database.windows.net*
1. Para confirmar que *da sua motocicleta Racing* e *Swimming Club* **não são** atualmente parte, navegue para o _contosoconcerthall_ base de dados no *tenants1-dpt -&lt;utilizador&gt;*  servidor e consulta o *VenueTypes* tabela.

Agora vamos criar uma tarefa para atualizar o *VenueTypes* tabela em todos os inquilinos bases de dados para adicionar os novos tipos de venue.

Para criar uma nova tarefa, utilize um conjunto de sistema de tarefas que criar procedimentos armazenados no _jobagent_ da base de dados quando o agente de tarefa foi criado.

1. No SSMS, ligue para o servidor de catálogo: *catálogo-dpt -&lt;utilizador&gt;. database.windows.net* servidor 
1. No SSMS, abra o ficheiro... \\Learning módulos\\esquema gestão\\DeployReferenceData.sql
1. Modificar a instrução: definir @wtpUser = &lt;utilizador&gt; e substitua o valor de utilizador utilizado quando implementou a aplicação de Wingtip bilhetes SaaS da base de dados por inquilino
1. Certifique-se de que está ligado ao _jobagent_ base de dados e prima **F5** para executar o script

Observar os seguintes elementos no *DeployReferenceData.sql* script:
* **SP\_adicionar\_destino\_grupo** cria o nome do grupo de destino DemoServerGroup.
* **SP\_adicionar\_destino\_grupo\_membro** é utilizado para definir o conjunto de bases de dados de destino.  Primeiro o _tenants1-dpt -&lt;utilizador&gt;_  servidor é adicionado.  Adicionar o servidor como um destino faz com que as bases de dados em que o servidor no momento da execução da tarefa a ser incluído na tarefa. Em seguida, a _basetenantdb_ base de dados e o *adhocreporting* base de dados (que é utilizado um tutorial posteriores) são adicionados como destinos.
* **SP\_adicionar\_tarefa** cria uma tarefa com o nome _implementação de dados de referência_.
* **SP\_adicionar\_passo** cria o passo de tarefa que contém o texto do comando T-SQL ao atualizar a tabela de referência, VenueTypes.
* As vistas restantes no script mostram a existência dos objetos e monitorizam a execução da tarefa. Utilize estas consultas para consultar o valor de estado no **ciclo de vida** coluna para determinar quando a tarefa foi concluída em todas as bases de dados de destino.

Após a conclusão do script, pode verificar que os dados de referência foi atualizados.  No SSMS, navegue para o *contosoconcerthall* da base de dados no *tenants1-dpt -&lt;utilizador&gt;*  servidor e consulta o *VenueTypes* tabela.  Verifique se *da sua motocicleta Racing* e *Swimming Club* **são** agora está presente.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Criar uma tarefa para gerir o índice de tabela de referência

Neste exercício utiliza uma tarefa para reconstruir o índice na chave primária da tabela de referência.  Esta é uma operação de manutenção de base de dados típicas que pode ser feita depois de carregar grandes quantidades de dados.

Crie uma tarefa com os mesmos procedimentos armazenados no “sistema” das tarefas.

1. Abra o SSMS e ligue ao _catálogo-dpt -&lt;utilizador&gt;. database.windows.net_ servidor
1. Abra o ficheiro _... \\Learning módulos\\esquema gestão\\OnlineReindex.sql_
1. Clique com o botão direito, selecione a ligação e estabelecer ligação com o _catálogo-dpt -&lt;utilizador&gt;. database.windows.net_ servidor, se ainda não estiver ligado
1. Certifique-se de que está ligado ao _jobagent_ base de dados e prima **F5** para executar o script

Observar os seguintes elementos no _OnlineReindex.sql_ script:
* **SP\_adicionar\_tarefa** cria uma nova tarefa denominada "Online PK reindexar\_\_VenueTyp\_\_265E44FD7FD4C885"
* **SP\_adicionar\_passo** cria o passo de tarefa que contém o texto de comando T-SQL para atualizar o índice
* As vistas restantes no script de monitorizar a execução da tarefa. Utilize estas consultas para consultar o valor de estado no **ciclo de vida** coluna para determinar quando a tarefa tem foi concluído com êxito em todos os membros do grupo de destino.



## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]

> * Criar uma tarefa de agente para executar nas tarefas de T-SQL várias bases de dados
> * Dados de referência de atualização em todas as bases de dados do inquilino
> * Criar um índice numa tabela em todas as bases de dados de inquilinos

Em seguida, tente o [tutorial relatórios Ad-hoc](saas-tenancy-cross-tenant-reporting.md) para explorar a execução de consultas distribuídas em inquilino bases de dados.


## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que criar após a implementação da aplicação Wingtip bilhetes SaaS da base de dados por inquilino](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Managing scaled-out cloud databases (Gerir bases de dados de escalamento horizontal na cloud)](sql-database-elastic-jobs-overview.md)
* [Create and manage scaled-out cloud databases (Criar e gerir bases de dados de escalamento horizontal na cloud)](sql-database-elastic-jobs-create-and-manage.md)