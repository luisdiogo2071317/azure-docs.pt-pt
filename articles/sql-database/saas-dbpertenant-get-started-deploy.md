---
title: Tutorial de SaaS de base de dados por inquilino - SQL Database do Azure | Microsoft Docs
description: Implementar e explorar a aplicação de multi-inquilino de SaaS de bilhetes Wingtip demonstra o padrão de base de dados por inquilino e outros padrões de SaaS ao utilizar a SQL Database do Azure.
keywords: tutorial de base de dados sql
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: 84e6f2303bcfd6a2c91f29e9e9afdc2b84f990ca
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-and-explore-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Implementar e explorar uma aplicação SaaS multi-inquilino que utiliza o padrão de base de dados por inquilino com a base de dados SQL

Neste tutorial, implementar e explorar a aplicação de base de dados por inquilino Wingtip bilhetes SaaS (Wingtip). A aplicação utiliza um padrão de base de dados por inquilino para armazenar os dados de vários inquilinos. A aplicação foi concebida para demonstram as funcionalidades da SQL Database do Azure que simplificam como ativar cenários de SaaS.

Cinco minutos depois de selecionar **implementar no Azure**, que tem uma aplicação SaaS multi-inquilino. A aplicação inclui uma base de dados do SQL Server que é executado na nuvem. A aplicação é implementada com três inquilinos de exemplo, cada uma com a sua própria base de dados. Todas as bases de dados são implementadas num agrupamento elástico de SQL. A aplicação é implementada a sua subscrição do Azure. Tem acesso total a explorar e trabalhar com os componentes individuais da aplicação. O código de origem de c# da aplicação e os scripts de gestão estão disponíveis no [repositório do GitHub WingtipTicketsSaaS DbPerTenant][github-wingtip-dpt].

Neste tutorial, ficará a saber:

> [!div class="checklist"]
> - Como implementar a aplicação Wingtip SaaS.
> - Onde obtê a origem da aplicação de scripts de gestão e de código.
> - Sobre os servidores, os conjuntos e as bases de dados que constituem a aplicação.
> - Como os inquilinos estão mapeados para os seus dados com o *catálogo*.
> - Como aprovisionar um novo inquilino.
> - Como monitorizar a atividade de inquilino na aplicação.

A [série de tutoriais relacionados](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) oferece explorar diversas padrões de conceção e gestão de SaaS. Os tutoriais de compilação para além desta implementação inicial. Quando utiliza os tutoriais, pode examinar os scripts fornecidos para ver como os diferentes padrões de SaaS são implementados. Os scripts demonstram como as funcionalidades da base de dados do SQL Server simplificam o desenvolvimento de aplicações SaaS.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certificar-se de que o Azure PowerShell está instalado. Para obter mais informações, consulte [começar com o Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Implementar a aplicação de Wingtip bilhetes SaaS

#### <a name="plan-the-names"></a>Planear os nomes

Os passos desta secção, deve fornecer um valor de utilizador que é utilizado para efetuar os nomes de recursos se são globalmente exclusivo. Também fornecem um nome para o grupo de recursos que contém todos os recursos criados por uma implementação da aplicação. Para uma personagem fictícia denominada Ann Finley, sugerimos que:

- **Utilizador**: *af1* é constituído por iniciais de Ann Finley mais um dígito. Se implementar a aplicação uma segunda vez, utilize um valor diferente. Um exemplo é af2.
- **Grupo de recursos**: *wingtip-dpt-af1* indica que esta é a aplicação de base de dados por inquilino. Acrescente o af1 de nome de utilizador para correlacionar o nome do grupo de recursos com os nomes dos recursos que nele contidos.

Escolha os nomes agora e escrevê-las para baixo. 

#### <a name="steps"></a>Passos

1. Para abrir o modelo de implementação de base de dados por inquilino Wingtip bilhetes SaaS no portal do Azure, selecione **implementar no Azure**.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Introduza valores no modelo para os parâmetros obrigatórios.

    > [!IMPORTANT]
    > Alguns firewalls de autenticação e o servidor são intencionalmente protegidas para fins de demonstração. Recomendamos que crie um novo grupo de recursos. Não utilize grupos de recursos, servidores ou conjuntos existente. Não utilize esta aplicação, scripts ou quaisquer recursos implementados para produção. Elimine este grupo de recursos quando tiver terminado com a aplicação para parar a faturação relacionada.

    - **Grupo de recursos**: selecione **criar nova**e forneça o nome exclusivo que escolheu anteriormente para o grupo de recursos. 
    - **Localização**: selecione uma localização da lista pendente.
    - **Utilizador**: Utilize o valor de nome de utilizador que selecionou anteriormente.

3. Implemente a aplicação.

    a. Selecione a concordar com os termos e condições.

    b. Selecione **Compra**.

4. Para monitorizar o estado de implementação, selecione **notificações** (no ícone de campainha para a direita da caixa de pesquisa). Implementar a aplicação SaaS de bilhetes Wingtip demora cerca de cinco minutos.

   ![Implementação efetuada com êxito](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Transferir e os scripts de gestão Wingtip pedidos de desbloqueio

Quando implementa a aplicação, transfira os scripts de gestão e o código de origem.

> [!IMPORTANT]
> Conteúdo executável (scripts e DLLs) poderá ser bloqueado pelo Windows quando os ficheiros. zip transferidos a partir de uma origem externa e extraiu. Siga os passos para desbloquear o ficheiro. zip antes de extrair os scripts. Desbloqueio certifica-se dos que scripts estão autorizados a executar.

1. Navegue para o [repositório do GitHub WingtipTicketsSaaS DbPerTenant][github-wingtip-dpt].
2. Selecione **clonar ou transferir**.
3. Selecione **transferir ZIP**e, em seguida, guarde o ficheiro.
4. Clique com botão direito do **WingtipTicketsSaaS-DbPerTenant-master.zip** de ficheiros e, em seguida, selecione **propriedades**.
5. No **geral** separador, selecione **desbloqueio** > **aplicar**.
6. Selecione **OK**e extraia os ficheiros

Scripts estão localizados na... \\Mestre de DbPerTenant WingtipTicketsSaaS\\pasta de módulos de aprendizagem.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Atualize o ficheiro de configuração de utilizador para esta implementação

Antes de executar quaisquer scripts, atualize os valores de utilizador e grupo de recursos no ficheiro de configuração de utilizador. Defina estas variáveis para os valores utilizados durante a implementação.

1. No ISE do PowerShell, abra... \\Learning módulos\\**UserConfig.psm1** 
2. Atualização **ResourceGroupName** e **nome** com os valores específicos para a sua implementação (em linhas 10 e 11 apenas).
3. Guarde as alterações.

Estes valores são referenciados quase todos os script.

## <a name="run-the-application"></a>Executar a aplicação

A aplicação showcases venues que alojam os eventos. Os tipos de venue incluem concert halls, jazz clubs e clubs desporto. Em permissões Wingtip, venues estão registados como os inquilinos. A ser um inquilino proporciona um venue uma forma fácil para eventos de lista e para vender permissões aos seus clientes. Cada venue obtém um Web site personalizado para listar os respetivos eventos e para vender pedidos de suporte.

Internamente na aplicação, cada inquilino obtém uma base de dados do SQL implementada para um agrupamento elástico de SQL.

Um centro **Hub de eventos** página fornece uma lista de ligações para os inquilinos na sua implementação.

1. Utilizar o URL para abrir o Hub de eventos no seu browser: http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net. Substitute &lt;utilizador&gt; com o valor de utilizador da sua implementação.

    ![Hub de eventos](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Selecione **Fabrikam Jazz Club** no Hub de eventos.

    ![Eventos](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Traffic Manager do Azure

A aplicação de Wingtip utiliza [ *Traffic Manager do Azure* ](../traffic-manager/traffic-manager-overview.md) para controlar a distribuição de pedidos recebidos. O URL para aceder à página Eventos para um inquilino específico utiliza o seguinte formato:

- http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/fabrikamjazzclub

    As partes do formato anterior são explicados com a tabela seguinte.

    | Parte do URL        | Descrição       |
    | :-------------- | :---------------- |
    | http://events.wingtip-dpt | As partes de eventos da aplicação Wingtip.<br /><br /> *-dpt* distingue a *da base de dados por inquilino* implementação Wingtip permissões de outras implementações. São exemplos de *autónomo* aplicação por inquilino (*-sa*) ou *base de dados multi-inquilino* (*- mt*) implementações. |
    | .*&lt;user&gt;* | *af1* no exemplo. |
    | .trafficmanager.net/ | Gestor de tráfego, base de URL. |
    | fabrikamjazzclub | Identifica o inquilino com o nome Fabrikam Jazz Club. |
    | &nbsp; | &nbsp; |

* O nome do inquilino é analisado a partir do URL pela aplicação de eventos.
* O nome do inquilino é utilizado para criar uma chave.
* A chave é utilizada para aceder ao catálogo de obter a localização da base de dados do inquilino.
    - O catálogo é implementado utilizando *gestão de mapa de partições horizontais*.
* O Hub de eventos utiliza metadados expandido no catálogo para construir os URLs de página lista de eventos para cada inquilino.

Num ambiente de produção, normalmente, cria um registo CNAME DNS para [ *apontar um domínio de internet da empresa* ](../traffic-manager/traffic-manager-point-internet-domain.md) ao nome DNS do Gestor de tráfego.

## <a name="start-generating-load-on-the-tenant-databases"></a>Começar a gerar carga nas bases de dados de inquilinos

Agora que a aplicação é implementada, vamos colocá-la para trabalhar.

O *demonstração LoadGenerator* script do PowerShell inicia uma carga de trabalho que é executada relativamente a todas as bases de dados do inquilino. A carga do mundo real em muitas aplicações de SaaS é esporádicas e imprevisíveis. Para simular este tipo de carga, o gerador de produz uma carga com picos aleatório ou bursts da atividade em cada inquilino. Os bursts ocorrem em intervalos aleatório. Demora alguns minutos até o padrão de carga surgir. Permitir que o gerador de execução para, pelo menos, três ou quatro minutos para monitorizar a carga.

1. No ISE do PowerShell, abra a... \\Learning módulos\\utilitários\\*demonstração LoadGenerator.ps1* script.
2. Prima F5 para executar o script e iniciar o gerador de carga. Deixe a predefinição os valores de parâmetros por agora.
3. Inicie sessão na sua conta do Azure e selecione a subscrição que pretende utilizar, se necessário.

O script de gerador de carga inicia uma tarefa em segundo plano para cada base de dados no catálogo e, em seguida, interrompe. Se voltar a executar o script de gerador de carga, interrompe as tarefas em segundo plano que estejam a executar antes de iniciar novos.

#### <a name="monitor-the-background-jobs"></a>Monitorizar as tarefas em segundo plano

Se pretender controlar e monitorizar as tarefas em segundo plano, utilize os seguintes cmdlets:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

#### <a name="demo-loadgeneratorps1-actions"></a>Ações de demonstração LoadGenerator.ps1

*Demonstração LoadGenerator.ps1* mimics uma carga de trabalho ativa de transações de cliente. Os passos seguintes descrevem a sequência de ações que *demonstração LoadGenerator.ps1* inicia:

1. *Demonstração LoadGenerator.ps1* inicia *LoadGenerator.ps1* em primeiro plano.

    - Ambos os ficheiros. ps1 são armazenados em pastas de módulos de aprendizagem\\utilitários\\.

2. *LoadGenerator.ps1* ciclos através de todas as bases de dados do inquilino no catálogo.

3. *LoadGenerator.ps1* inicia um trabalho do PowerShell em segundo plano para cada base de dados do inquilino:

    - Por predefinição, as tarefas em segundo plano é executada para 120 minutos.
    - Cada tarefa faz com que uma carga de CPU baseada na base de dados de um inquilino executando *sp_CpuLoadGenerator*. A intensidade e a duração da carga do varia consoante `$DemoScenario`. 
    - *sp_CpuLoadGenerator* ciclos em torno de uma instrução SQL SELECT que faz com que uma elevada carga de CPU. O intervalo de tempo entre problemas a SELECIONAR varia de acordo com os valores de parâmetros para criar uma carga de CPU controllable. Níveis de carga e os intervalos são aleatória para simular cargas mais realistas.
    - Este ficheiro. SQL é armazenado em *WingtipTenantDB\\dbo\\StoredProcedures\\*.

4. Se `$OneTime = $false`, o gerador de carga inicia as tarefas em segundo plano e, em seguida, continua a ser executado. Cada 10 segundos, que monitoriza os novos inquilinos aprovisionados. Se definir `$OneTime = $true`, o LoadGenerator inicia as tarefas em segundo plano e, em seguida, deixa de ser executada em primeiro plano. Para este tutorial, deixe `$OneTime = $false`.

  Utilize Ctrl-C ou Ctrl-Break de operação de parar para interromper ou reiniciar o gerador de carga. 

  Se deixar o gerador de carga em execução em primeiro plano, utilize outra instância do ISE do PowerShell para executar outros scripts do PowerShell.

&nbsp;

Antes de continuar com a secção seguinte, deixe o gerador de carga em execução no estado de invocar a tarefa.

## <a name="provision-a-new-tenant"></a>Aprovisionar um inquilino novo

A implementação inicial cria três inquilinos de exemplo. Agora pode criar outro inquilino para ver o impacto na aplicação implementada. Na aplicação Wingtip, o fluxo de trabalho para aprovisionar novos inquilinos é explicado no [tutorial de aprovisionamento e o catálogo](saas-dbpertenant-provision-and-catalog.md). Nesta fase, crie um novo inquilino, demora menos de um minuto.

1. Abra uma nova ISE do PowerShell.
2. Abra... \\Learning Modules\Provision e catálogo\\*demonstração ProvisionAndCatalog.ps1*.
3. Para executar o script, prima F5. Deixe os valores predefinidos por agora.

   > [!NOTE]
   > Utilizam scripts de Wingtip SaaS muitos *$PSScriptRoot* procurar nas pastas para chamar as funções em outros scripts. Esta variável é avaliada apenas quando o script completo é executado ao premir F5. Realce e em execução uma seleção com F8 podem resultar em erros. Para executar os scripts, prima F5.

A nova base de dados do inquilino é:

- Criados num agrupamento elástico de SQL.
- Foi inicializado.
- Registado no catálogo.

Após o aprovisionamento com êxito, o *eventos* local do inquilino novo aparece no seu browser.

![Novo inquilino](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Atualize o Hub de eventos para tornar o novo inquilino aparecer na lista.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Explorar os servidores, os conjuntos e as bases de dados de inquilinos

Agora que já iniciou a execução de carga na coleção de inquilinos, vamos ver alguns dos recursos que foram implementados.

1. No [portal do Azure](http://portal.azure.com), navegue até à sua lista de servidores SQL. Em seguida, abra o **catálogo-dpt -&lt;utilizador&gt;**  servidor.
    - O servidor de catálogo contém duas bases de dados, **tenantcatalog** e **basetenantdb** (modelo base de dados que é copiado para criar novos inquilinos).

   ![Bases de Dados](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. Regressar à sua lista de servidores SQL.

3. Abra o **tenants1-dpt -&lt;utilizador&gt;**  servidor que contém as bases de dados do inquilino.

4. Consulte os seguintes itens:

    - Cada base de dados do inquilino é um **elástico Standard** base de dados num conjunto standard 50 eDTU.
    - A base de dados Red Maple Racing é a base de dados do inquilino aprovisionados anteriormente.

   ![Servidor de bases de dados](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Monitorizar o conjunto

Depois de *LoadGenerator.ps1* é executado durante vários minutos, existem dados suficientes devem estar disponíveis para começar a procurar algumas capacidades de monitorização. Estas capacidades incorporadas em conjuntos e as bases de dados.

Navegue para o servidor **tenants1-dpt -&lt;utilizador&gt;**e selecione **Pool1** para ver a utilização de recursos para o conjunto. Os gráficos seguintes, o gerador de carga foi executada durante uma hora.

   ![Conjunto de monitor](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- O gráfico primeiro, com a etiqueta **utilização de recursos**, mostra a utilização de eDTU do conjunto.
- Segundo gráfico mostra a utilização de eDTU das bases de dados mais ativos cinco no conjunto.

Os dois gráficos mostram que são adequados para cargas de trabalho de aplicação imprevisíveis SaaS conjuntos elásticos e base de dados SQL. Os gráficos mostram que quatro bases de dados são cada segurança para quanto 40 eDTUs e ainda todas as bases de dados comfortably são suportados por um conjunto de 50 eDTU. O conjunto de eDTU de 50 pode suportar as cargas de trabalho mesmo mais pesadas. Se as bases de dados são aprovisionados como bases de dados autónomo, cada um deles tem de ser um S2 (50 DTU) para suportar os bursts. O custo de bases de dados de S2 quatro autónomo é quase três vezes o preço do conjunto. Em situações de mundo real, os clientes de base de dados SQL executam até 500 bases de dados em 200 conjuntos de eDTU. Para obter mais informações, consulte o [tutorial de monitorização de desempenho](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Recursos adicionais

- Para obter mais informações, consulte o artigo adicionais [tutoriais que criar na aplicação de base de dados por inquilino Wingtip bilhetes SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
- Para mais informações sobre conjuntos elásticos, consulte o artigo [o que é um agrupamento elástico de SQL do Azure?](sql-database-elastic-pool.md).
- Para saber mais sobre as tarefas elásticas, consulte [bases de dados de nuvem de escalamento horizontal de gerir](sql-database-elastic-jobs-overview.md).
- Para saber mais sobre as aplicações de SaaS multi-inquilino, consulte [padrões para aplicações de SaaS multi-inquilino de conceção](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber:

> [!div class="checklist"]
> - Como implementar a aplicação de Wingtip bilhetes SaaS.
> - Sobre os servidores, os conjuntos e as bases de dados que constituem a aplicação.
> - Como os inquilinos estão mapeados para os seus dados com o *catálogo*.
> - Como aprovisionar novos inquilinos.
> - Como ver utilização de agrupamento para monitorizar a atividade de inquilino.
> - Como eliminar os recursos de exemplo para parar a faturação relacionada.

Em seguida, tente o [tutorial de aprovisionamento e o catálogo](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 

