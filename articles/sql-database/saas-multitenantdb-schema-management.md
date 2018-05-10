---
title: Gerir o esquema da Base de Dados SQL numa aplicação multi-inquilino | Microsoft Docs
description: Gerir o Esquema para vários inquilinos numa aplicação multi-inquilino que utiliza a Base de Dados SQL do Azure
keywords: tutorial de base de dados sql
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: dc70fe43c63d6b77d4a122f196f59fe51573b7aa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="manage-schema-in-a-saas-application-that-uses-sharded-multi-tenant-sql-databases"></a>Gerir o esquema de uma aplicação SaaS que utiliza a bases de dados SQL de multi-inquilinos

Este tutorial examina desafios da manutenção de uma frota de bases de dados num Software como uma aplicação de serviço (SaaS). Soluções são demonstradas para fanning das alterações de esquema em frota de bases de dados.

Como qualquer aplicação, a aplicação SaaS de bilhetes Wingtip será evoluir ao longo do tempo e irá exigir alterações à base de dados. As alterações poderão afetar os dados de esquema ou de referência ou aplicam-se tarefas de manutenção de base de dados. Com uma aplicação SaaS utilizando uma base de dados por padrão de inquilino, alterações tem de ser coordenadas através de uma frota potencialmente grande de bases de dados do inquilino. Além disso, tem de incorporar estas alterações na base de dados de aprovisionamento de processo para garantir que estão incluídos nas novas bases de dados que são criados.

#### <a name="two-scenarios"></a>Dois cenários

Este tutorial explicar os seguintes dois cenários:
- Implemente atualizações de dados de referência para todos os inquilinos.
- Reconstrua um índice na tabela que contém os dados de referência.

O [as tarefas elásticas](sql-database-elastic-jobs-overview.md) funcionalidade da SQL Database do Azure é utilizada para executar estas operações em bases de dados do inquilino. As tarefas também funcionam na base de dados do inquilino 'template'. Na aplicação de exemplo Wingtip bilhetes, esta base de dados do modelo é copiada para Aprovisionar uma nova base de dados do inquilino.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie um agente de tarefa.
> * Execute uma consulta T-SQL em várias bases de dados do inquilino.
> * Atualize dados de referência em todas as bases de dados do inquilino.
> * Crie um índice numa tabela em todas as bases de dados do inquilino.

## <a name="prerequisites"></a>Pré-requisitos

- A aplicação de base de dados do multi-inquilino de bilhetes Wingtip já tem de ser implementada:
    - Para obter instruções, consulte o primeiro tutorial, o que introduz a aplicação de base de dados do multi-inquilino Wingtip bilhetes SaaS:<br />[Implementar e explorar uma a aplicação multi-inquilino que utiliza a base de dados do Azure SQL](saas-multitenantdb-get-started-deploy.md).
        - O processo de implementar é executado para menos de cinco minutos.
    - Tem de ter o *a multi-inquilino* versão do Wingtip instalado. As versões para *autónomo* e *base de dados por inquilino* não suportam este tutorial.

- A versão mais recente do SQL Server Management Studio (SSMS) tem de estar instalada. [Transfira e instale o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- O Azure PowerShell tem de estar instalado. Para obter mais informações, consulte [introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Este tutorial utiliza funcionalidades do serviço SQL Database do Azure que se encontrem numa pré-visualização limitada ([tarefas de bases de dados elásticas](sql-database-elastic-database-client-library.md)). Se pretender fazer neste tutorial, forneça o ID de subscrição para *SaaSFeedback@microsoft.com* com o requerente = elástico de tarefas de pré-visualização. Após receber a confirmação indicando que a sua subscrição foi ativada, deverá [transferir e instalar os cmdlets mais recentes das tarefas da versão de pré-lançamento](https://github.com/jaredmoo/azure-powershell/releases). Esta pré-visualização é limitada, por isso, contacte *SaaSFeedback@microsoft.com* para suporte ou questões relacionados.

## <a name="introduction-to-saas-schema-management-patterns"></a>Introdução aos padrões de gestão do esquema de SaaS

O modelo da base de dados do multi-inquilino utilizado neste exemplo permite que uma base de dados de inquilinos conter um ou mais inquilinos. Este exemplo explicar o potencial de utilizar uma combinação de um inquilino muitos e um inquilino bases de dados, ativar uma *híbrida* modelo de gestão de inquilino. Gerir as alterações a estas bases de dados pode ser complicada. [As tarefas elásticas](sql-database-elastic-jobs-overview.md) facilita a administração e gestão de grandes quantidades de base de dados. As tarefas permitem-lhe de forma segura e fiável executar scripts de Transact-SQL como tarefas, em relação a um grupo de bases de dados do inquilino. As tarefas são independentes da interação do utilizador ou de uma entrada. Este método pode ser utilizado para implementar as alterações ao esquema ou dados de referência comuns, em todos os inquilinos numa aplicação. As tarefas elásticas também podem ser utilizadas para manter uma cópia do modelo dourada da base de dados. O modelo é utilizado para criar novos inquilinos, garantindo sempre o esquema mais recente e os dados de referência estão em utilização.

![ecrã](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Pré-visualização limitada das Tarefas Elásticas

Há uma nova versão do tarefas elásticas que é agora uma funcionalidade integrada da SQL Database do Azure. Esta nova versão das Tarefas Elásticas está atualmente em pré-visualização limitada. O limitado de pré-visualização atualmente suporta a utilização do PowerShell para criar um agente de tarefa e o T-SQL para criar e gerir tarefas.
> [!NOTE] 
> Este tutorial utiliza funcionalidades do serviço base de dados do SQL Server que estão numa (tarefas de bases de dados elásticas) de pré-visualização limitada. Se pretender fazer neste tutorial, forneça o ID de subscrição para SaaSFeedback@microsoft.com com o requerente = elástico de tarefas de pré-visualização. Depois de receber a confirmação de que a sua subscrição tiver sido ativada, transfira e instale os cmdlets mais recente versão de pré-lançamento tarefas. Esta pré-visualização é limitada, por isso, contacte SaaSFeedback@microsoft.com para suporte ou questões relacionados.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Obter o código de origem da aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino e os scripts

Os scripts de base de dados do Wingtip bilhetes SaaS multi-inquilino e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) repositório no Github. Consulte o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de Wingtip SaaS de pedidos de desbloqueio. 

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Criar um agente de tarefa da base de dados e o novo agente de tarefa

Este tutorial, necessita que utilizar o PowerShell para criar a tarefa agente da base de dados e o agente de tarefa. Como a base de dados MSDB utilizada pelo SQL Server Agent, um agente de trabalho utiliza uma base de dados SQL do Azure para armazenar definições de tarefas, o estado de tarefa e o histórico. Depois do agente de tarefa é criado, pode criar e monitorizar tarefas imediatamente.

1. No **ISE do PowerShell**, abra *... \\Learning módulos\\esquema gestão\\demonstração SchemaManagement.ps1*.
2. Prima **F5** para executar o script.

O *demonstração SchemaManagement.ps1* script chamadas a *implementar SchemaManagement.ps1* script para criar uma base de dados com o nome _jobagent_ no servidor de catálogo. O script cria, em seguida, o agente de tarefa, transmitir o _jobagent_ base de dados como um parâmetro.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Criar uma tarefa para implementar novos dados de referência para todos os inquilinos

#### <a name="prepare"></a>Preparação

Base de dados de cada inquilino inclui um conjunto de tipos de venue o **VenueTypes** tabela. Cada tipo de venue define o tipo de eventos que podem ser alojados num venue. Estes tipos de venue correspondem às imagens em segundo plano que vê na aplicação de eventos do inquilino.  Neste exercício, implementar uma atualização para todas as bases de dados para adicionar dois tipos de venue adicionais: *da sua motocicleta Racing* e *Swimming Club*. 

Em primeiro lugar, reveja os tipos de venue incluídos em cada base de dados do inquilino. Ligar a uma das bases de dados do inquilino no SQL Server Management Studio (SSMS) e Inspecione a tabela de VenueTypes.  Também pode consultar esta tabela em do editor de consultas no portal do Azure, acedido a partir da página de base de dados. 

1. Abra o SSMS e ligar ao servidor do inquilino: *tenants1-dpt -&lt;utilizador&gt;. database.windows.net*
1. Para confirmar que *da sua motocicleta Racing* e *Swimming Club* **não são** atualmente parte, navegue para o *contosoconcerthall* base de dados no *tenants1-dpt -&lt;utilizador&gt;*  servidor e consulta o *VenueTypes* tabela.



#### <a name="steps"></a>Passos

Agora criar uma tarefa para atualizar o **VenueTypes** tabela em cada base de dados de inquilinos, adicionando os dois tipos de venue de novo.

Para criar uma nova tarefa, utilize o conjunto de procedimentos armazenados de sistema tarefas que foram criados no _jobagent_ base de dados. Os procedimentos armazenados criados quando o agente de tarefa foi criado.

1. No SSMS, ligue ao servidor do inquilino: tenants1-mt -&lt;utilizador&gt;. database.windows.net

2. Navegue para o *tenants1* base de dados.

3. Consulta o *VenueTypes* tabela para confirmar que *da sua motocicleta Racing* e *Swimming Club* não estão ainda na lista de resultados.

4. Ligar ao servidor de catálogo, o que é *catálogo-mt -&lt;utilizador&gt;. database.windows.net*.

5. Ligar para o _jobagent_ base de dados no servidor de catálogo.

6. No SSMS, abra o ficheiro *... \\Learning módulos\\esquema gestão\\DeployReferenceData.sql*.

7. Modificar a instrução: definir @User = &lt;utilizador&gt; e substitua o valor de utilizador utilizado quando implementou a aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino.

8. Prima **F5** para executar o script.

#### <a name="observe"></a>Observar

Observar os seguintes itens no *DeployReferenceData.sql* script:

- **SP\_adicionar\_destino\_grupo** cria o nome do grupo de destino *DemoServerGroup*, e adiciona membros de destino ao grupo.

- **SP\_adicionar\_destino\_grupo\_membro** adiciona os seguintes itens:
    - A *servidor* como alvo o tipo de membro.
        - Este é o *tenants1-mt -&lt;utilizador&gt;*  servidor que contém as bases de dados de inquilinos.
        - Incluindo o servidor inclui as bases de dados do inquilino que existe no momento que executa a tarefa.
    - A *base de dados* tipo de membro para a base de dados do modelo de destino (*basetenantdb*) que reside no *catálogo-mt -&lt;utilizador&gt;*  servidor,
    - A *base de dados* como alvo o tipo de membro para incluir o *adhocreporting* base de dados que é utilizado um tutorial posterior.

- **SP\_adicionar\_tarefa** cria uma tarefa denominada *implementação de dados de referência*.

- **SP\_adicionar\_passo** cria o passo de tarefa que contém o texto do comando T-SQL ao atualizar a tabela de referência, VenueTypes.

- As vistas restantes no script mostram a existência dos objetos e monitorizam a execução da tarefa. Utilize estas consultas para consultar o valor de estado no **ciclo de vida** coluna para determinar quando a tarefa foi concluída. A tarefa atualiza a base de dados de inquilinos e atualiza os adicionais e duas bases de dados que contêm a tabela de referência.

No SSMS, navegue para a base de dados do inquilino no *tenants1-mt -&lt;utilizador&gt;*  servidor. Consulta o *VenueTypes* tabela para confirmar que *da sua motocicleta Racing* e *Swimming Club* agora são adicionadas à tabela. A contagem total de tipos de venue deve ter aumentado por dois.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Criar uma tarefa para gerir o índice de tabela de referência

Neste exercício cria uma tarefa para reconstruir o índice na chave primária de tabela de referência em todas as bases de dados do inquilino. Uma reconstrução do índice é uma operação de gestão de base de dados típicas que um administrador possam ser executadas depois de carregar uma grande quantidade de carregamento de dados, para melhorar o desempenho.

1. No SSMS, ligue ao _jobagent_ da base de dados no *catálogo-mt -&lt;utilizador&gt;. database.windows.net* servidor.

2. No SSMS, abra *... \\Learning módulos\\esquema gestão\\OnlineReindex.sql*.

3. Prima **F5** para executar o script.

#### <a name="observe"></a>Observar

Observar os seguintes itens no *OnlineReindex.sql* script:

* **SP\_adicionar\_tarefa** cria uma nova tarefa denominada *reindexar Online PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_adicionar\_passo** cria o passo de tarefa que contém o texto de comando T-SQL para atualizar o índice.

* As vistas restantes no script de monitorizar a execução da tarefa. Utilize estas consultas para consultar o valor de estado no **ciclo de vida** coluna para determinar quando a tarefa tem foi concluído com êxito em todos os membros do grupo de destino.

## <a name="additional-resources"></a>Recursos adicionais

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Managing scaled-out cloud databases (Gerir bases de dados de escalamento horizontal na cloud)](sql-database-elastic-jobs-overview.md)
* [Create and manage scaled-out cloud databases (Criar e gerir bases de dados de escalamento horizontal na cloud)](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
.
> * Criar uma tarefa de agente para executar tarefas de T-SQL em várias bases de dados
> * Dados de referência de atualização em todas as bases de dados do inquilino
> * Criar um índice numa tabela em todas as bases de dados de inquilinos

Em seguida, tente o [tutorial relatórios Ad-hoc](saas-multitenantdb-adhoc-reporting.md) para explorar a execução de consultas distribuídas em inquilino bases de dados.

