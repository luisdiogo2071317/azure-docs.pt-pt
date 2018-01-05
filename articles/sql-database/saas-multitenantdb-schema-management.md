---
title: "Gerir o esquema da Base de Dados SQL numa aplicação multi-inquilino | Microsoft Docs"
description: "Gerir o Esquema para vários inquilinos numa aplicação multi-inquilino que utiliza a Base de Dados SQL do Azure"
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 135764a7d89dcf711ff7fe9416850f1af9329479
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Gerir o esquema para vários inquilinos numa aplicação multi-inquilino que utiliza a SQL Database do Azure

Este tutorial examina desafios da manutenção de uma frota potencialmente grande de bases de dados num Software como uma aplicação de serviço (SaaS) na nuvem. Soluções são demonstradas para gerir os melhoramentos de esquema que são desenvolvidos e implementados durante a vigência de uma aplicação.

À medida que o qualquer aplicação evolui, as alterações poderão ocorrer para as colunas da tabela ou outra esquema, ou para os dados de referência ou ao nível de desempenho relacionados com itens. Com uma aplicação SaaS, estas alterações tem de ser implementadas uma forma coordenada através de vários inquilinos bases de dados existentes. E estas alterações têm de ser incluídas nas bases de dados do inquilino futuras que serão adicionados à aplicação. Por conseguinte, as alterações também tem de ser incorporadas no processo que Aprovisiona novas bases de dados.

#### <a name="two-scenarios"></a>Dois cenários

Este tutorial explicar os seguintes dois cenários:
- Implemente atualizações de dados de referência para todos os inquilinos.
- Retuning um índice na tabela que contém os dados de referência.

O [as tarefas elásticas](sql-database-elastic-jobs-overview.md) funcionalidade da SQL Database do Azure é utilizada para executar estas operações em bases de dados do inquilino. As tarefas também funcionam na base de dados do modelo dourada inquilino. Este modelo é utilizado quando são aprovisionadas novas bases de dados.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie uma conta de tarefa.
> * Consultar em vários inquilinos.
> * Atualize dados em todas as bases de dados do inquilino.
> * Crie um índice numa tabela em todas as bases de dados do inquilino.

## <a name="prerequisites"></a>Pré-requisitos

- A aplicação de bilhetes Wingtip já tem de ser implementada:
    - Para obter instruções, consulte o tutorial primeiro, o que introduz o *Wingtip bilhetes* aplicações de base de dados do multi-inquilino de SaaS:<br />[Implementar e explorar uma a aplicação multi-inquilino que utiliza a base de dados do Azure SQL](saas-multitenantdb-get-started-deploy.md).
        - O processo de implementar é executado para menos de cinco minutos.
    - Tem de ter o *a multi-inquilino* versão do Wingtip instalado. As versões para *autónomo* e *base de dados por inquilino* não suportam o tutorial presente.

- A versão mais recente do SQL Server Management Studio (SSMS) tem de estar instalada. [Transfira e instale o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- O Azure PowerShell tem de estar instalado. Para obter mais informações, consulte [introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Este tutorial utiliza funcionalidades do serviço SQL Database do Azure que se encontrem numa pré-visualização limitada ([tarefas de bases de dados elásticas](sql-database-elastic-database-client-library.md)). Se pretender fazer neste tutorial, forneça o ID de subscrição para  *SaaSFeedback@microsoft.com*  com o requerente = elástico de tarefas de pré-visualização. Após receber a confirmação indicando que a sua subscrição foi ativada, deverá [transferir e instalar os cmdlets mais recentes das tarefas da versão de pré-lançamento](https://github.com/jaredmoo/azure-powershell/releases). Esta pré-visualização é limitada, por isso, contacte  *SaaSFeedback@microsoft.com*  para suporte ou questões relacionados.

## <a name="introduction-to-saas-schema-management-patterns"></a>Introdução aos padrões de gestão do esquema de SaaS

O modelo da base de dados do multi-inquilino utilizado neste exemplo permite que uma base de dados de inquilinos conter um ou mais inquilinos. Este exemplo explicar o potencial de utilizar uma combinação de um inquilino muitos e um inquilino bases de dados, ativar uma *híbrida* modelo de gestão de inquilino. É complicado gerir estas bases de dados. As [Tarefas Elásticas](sql-database-elastic-jobs-overview.md) facilitam a administração e a gestão da camada de dados do SQL. As tarefas permitem-lhe de forma segura e fiável executar scripts de Transact-SQL como tarefas, em relação a um grupo de bases de dados do inquilino. As tarefas são independentes da interação do utilizador ou de uma entrada. Este método pode ser utilizado para implementar as alterações ao esquema ou dados de referência comuns, em todos os inquilinos numa aplicação. As tarefas elásticas também podem ser utilizadas para manter uma cópia do modelo dourada da base de dados. O modelo é utilizado para criar novos inquilinos, garantindo sempre o esquema mais recente e os dados de referência estão em utilização.

![ecrã](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Pré-visualização limitada das Tarefas Elásticas

Há uma nova versão do tarefas elásticas que é agora uma funcionalidade integrada da SQL Database do Azure. Por integrada iremos significa que não requer nenhum serviços adicionais ou componentes. Esta nova versão das Tarefas Elásticas está atualmente em pré-visualização limitada. A pré-visualização limitada suporta atualmente o PowerShell para criar contas de tarefa e T-SQL para criar e gerir tarefas.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Obter o código de origem da aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino e os scripts

Os scripts de base de dados do Wingtip bilhetes SaaS multi-inquilino e o código fonte da aplicação, estão disponíveis no [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) repositório no Github. Consulte o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de Wingtip SaaS de pedidos de desbloqueio. 

## <a name="create-a-job-account-database-and-new-job-account"></a>Criar uma base de dados das contas de tarefa e uma nova conta de tarefa

Este tutorial, necessita que utilizar o PowerShell para criar a base de dados de conta de tarefas e a conta de tarefa. Como a base de dados MSDB utilizado pelo agente do SQL Server, as tarefas elásticas utiliza uma base de dados SQL do Azure para armazenar definições de tarefas, o estado de tarefa e o histórico. Depois da conta de tarefa é criada, pode criar e monitorizar tarefas imediatamente.

1. No **ISE do PowerShell**, abra *... \\Learning módulos\\esquema gestão\\demonstração SchemaManagement.ps1*.
2. Prima **F5** para executar o script.

O *demonstração SchemaManagement.ps1* script chamadas a *implementar SchemaManagement.ps1* script para criar uma camada *S2* com o nome da base de dados **jobaccount** no servidor de catálogo. O script, em seguida, cria a conta de tarefa a ser transmitidos a base de dados jobaccount como um parâmetro para a chamada de criação de conta de tarefa.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Criar uma tarefa para implementar novos dados de referência para todos os inquilinos

#### <a name="prepare"></a>Preparação

Cada base de dados de inquilinos inclui um conjunto de tipos de venue o **VenueTypes** tabela. Os tipos de venue definem o tipo de eventos que estão alojados num venue. Neste exercício, implementar uma atualização para todas as bases de dados para adicionar dois tipos de venue adicionais: *da sua motocicleta Racing* e *Swimming Club*. Estes tipos de local correspondem à imagem de fundo que vê na aplicação de eventos do inquilino.

Antes de implementar os novos dados de referência, tenha em atenção o número de tipos de venue que já existe, que poderá ser 10. Tome nota, clicando na hiperligação seguinte para a IU da web de Wingtip e, em seguida, clicando a **Venue tipo** menu pendente:
- http://events.Wingtip-MT.<USER>. trafficmanager.net

Agora pode contabilizar o número de tipos de venue original. Em particular, tenha em atenção que nem *da sua motocicleta Racing* nem *Swimming Club* ainda existe.

#### <a name="steps"></a>Passos

Agora criar uma tarefa para atualizar o **VenueTypes** tabela em cada base de dados de inquilinos, adicionando os dois tipos de venue de novo.

Para criar uma nova tarefa, utilize o conjunto de procedimentos armazenados de sistema tarefas que foram criados no *jobaccount* base de dados. Os procedimentos foram criados quando a conta de tarefa foi criada.

1. No SSMS, ligue ao servidor do inquilino: tenants1-mt -\<utilizador\>. database.windows.net

2. Navegue para o *tenants1* da base de dados no *tenants1-mt -\<utilizador\>. database.windows.net* servidor.

3. Consulta o *VenueTypes* tabela para confirmar que *da sua motocicleta Racing* e *Swimming Club* não estão ainda na lista de resultados.

4. Ligar ao servidor de catálogo, o que é *catálogo-mt -\<utilizador\>. database.windows.net*.

5. Ligar para o *jobaccount* base de dados no servidor de catálogo.

6. No SSMS, abra o ficheiro *... \\Learning módulos\\esquema gestão\\DeployReferenceData.sql*.

7. Modificar a instrução: definir @User = &lt;utilizador&gt; e substitua o valor de utilizador utilizado quando implementou a aplicação de base de dados do Wingtip bilhetes SaaS multi-inquilino.

8. Prima **F5** para executar o script.

#### <a name="observe"></a>Observar

Observar os seguintes itens no *DeployReferenceData.sql* script:

- **SP\_adicionar\_destino\_grupo** cria o nome do grupo de destino *DemoServerGroup*, e adiciona membros de destino ao grupo.

- **SP\_adicionar\_destino\_grupo\_membro** adiciona os seguintes itens:
    - A *servidor* como alvo o tipo de membro.
        - Este é o *tenants1-mt -&lt;utilizador&gt;*  servidor que contém as bases de dados de inquilinos.
        - Assim todas as bases de dados no servidor estão incluídos na tarefa quando executa a tarefa.
    - A *base de dados* como alvo o tipo de membro para a base de dados dourada (*basetenantdb*) que reside no *catálogo-mt -&lt;utilizador&gt;*  servidor,
    - A *base de dados* como alvo o tipo de membro para incluir o *adhocreporting* base de dados que é utilizado um tutorial posterior.

- **SP\_adicionar\_tarefa** cria uma tarefa denominada *implementação de dados de referência*.

- **SP\_adicionar\_passo** cria o passo de tarefa que contém o texto do comando T-SQL ao atualizar a tabela de referência, VenueTypes.

- As vistas restantes no script mostram a existência dos objetos e monitorizam a execução da tarefa. Utilize estas consultas para consultar o valor de estado no **ciclo de vida** coluna para determinar quando a tarefa foi concluída com êxito. A tarefa atualiza a base de dados de inquilinos e atualiza os adicionais e duas bases de dados que contêm a tabela de referência.

No SSMS, navegue para a base de dados do inquilino no *tenants1-mt -&lt;utilizador&gt;*  servidor. Consulta o *VenueTypes* tabela para confirmar que *da sua motocicleta Racing* e *Swimming Club* agora são adicionadas à tabela. A contagem total de tipos de venue deve ter aumentado por dois.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Criar uma tarefa para gerir o índice de tabela de referência

Neste exercício é semelhante à exercício anterior. Neste exercício cria uma tarefa para reconstruir o índice na chave primária da tabela de referência. Uma reconstrução do índice é uma operação de gestão de base de dados típicas que um administrador poderá executar após um carregamento de dados de grandes dimensões para uma tabela para melhorar o desempenho.

1. No SSMS, ligue ao *jobaccount* da base de dados no *catálogo-mt -&lt;utilizador&gt;. database.windows.net* servidor.

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
> - Crie uma conta de tarefa de consultar em vários inquilinos.
> - Atualize dados em todas as bases de dados do inquilino.
> - Crie um índice numa tabela em todas as bases de dados do inquilino.

Em seguida, tente o [tutorial relatórios Ad-hoc](saas-multitenantdb-adhoc-reporting.md).

