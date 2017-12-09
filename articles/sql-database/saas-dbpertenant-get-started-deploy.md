---
title: Tutorial de SaaS de base de dados por inquilino - SQL Database do Azure | Microsoft Docs
description: "Implementar e explorar a Wingtip bilhetes SaaS multi-inquilino de aplicação, que demonstra a base de dados por inquilino e outros padrões de SaaS a utilizar a SQL Database do Azure."
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: genemi
ms.openlocfilehash: 5342b5290fab9826a2b38cd7ada63a6736c77601
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Implementar e explorar uma aplicação SaaS multi-inquilino que utiliza a base de dados por padrão do inquilino com a SQL Database do Azure

Neste tutorial, implementar e explorar o SaaS de bilhetes Wingtip *base de dados por inquilino* aplicação (Wingtip). A aplicação utiliza uma base de dados por padrão de inquilino, para armazenar os dados de vários inquilinos. A aplicação foi concebida para demonstram as funcionalidades da SQL Database do Azure que simplificam a ativar cenários de SaaS.

Cinco minutos depois de clicar no botão azul etiquetado **implementar no Azure**, que tem uma aplicação de SaaS multi-inquilino. A aplicação inclui uma SQL Database do Azure em execução na nuvem do Microsoft Azure. A aplicação é implementada com três inquilinos de exemplo, cada uma com a sua própria base de dados. Todas as bases de dados são implementadas para um SQL *conjunto elástico*. A aplicação é implementada a sua subscrição do Azure. Tem acesso total a explorar e trabalhar com os componentes individuais da aplicação. O código de origem de c# da aplicação e os scripts de gestão, estão disponíveis no [repositório do GitHub WingtipTicketsSaaS DbPerTenant][github-wingtip-dpt].

Neste tutorial, ficará a saber:

> [!div class="checklist"]
> - Como implementar a aplicação Wingtip SaaS.
> - Onde obter o código fonte da aplicação e os scripts de gestão.
> - Sobre os servidores, os conjuntos e as bases de dados que constituem a aplicação.
> - Como os inquilinos estão mapeados para os seus dados com o *catálogo*.
> - Como aprovisionar um novo inquilino.
> - Como monitorizar a atividade de inquilino na aplicação.

A [série de tutoriais relacionados](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) oferece explorar diversas padrões de conceção e gestão de SaaS. Os tutoriais de compilação para além desta implementação inicial.
Quando utiliza os tutoriais, pode ver como os diferentes padrões de SaaS são implementados, examinando os scripts fornecidos.
Os scripts demonstram como as funcionalidades da base de dados do SQL Server simplificam o desenvolvimento de aplicações SaaS.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, confirme que conclui os pré-requisitos seguintes:

* O Azure PowerShell está instalado. Para obter mais informações, consulte [introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Implementar a aplicação de Wingtip bilhetes SaaS

Implemente a aplicação:

1. Escolha e lembre-se os valores que necessitará para os seguintes parâmetros:

    - **Utilizador**: Escolha um valor curto, tais como as suas iniciais seguido de um dígito. Por exemplo, *af1*. O parâmetro de utilizador pode conter apenas letras, números e hífenes (sem espaços). O primeiro e último caráter tem de ser uma letra ou um dígito. Recomendamos que todas as letras estar em minúsculas.
    - **Grupo de recursos**: sempre que implementar a aplicação de Wingtip, tem de escolher outro nome exclusivo para o novo grupo de recursos. Recomendamos que acrescentar o nome de utilizador para um nome de base para o grupo de recursos. Pode ser um nome de grupo de recursos de exemplo *wingtip af1*. Novamente, recomendamos que todas as letras estar em minúsculas.

2. Abrir Wingtip pedidos de suporte de SaaS base de dados por modelo de implementação de inquilino no portal do Azure, clicando a azul **implementar no Azure** botão.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

3. Para o modelo, introduza valores para os parâmetros obrigatórios:

    > [!IMPORTANT]
    > Algumas autenticações e firewalls do servidor são intencionalmente não seguras para fins de demonstração. Recomendamos que lhe *criar um novo grupo de recursos*. Não utilize grupos de recursos, servidores ou conjuntos existente. Não utilize esta aplicação, scripts ou quaisquer recursos implementados para produção. Elimine este grupo de recursos quando tiver terminado com a aplicação para parar a faturação relacionada.

    - **Grupo de recursos** - selecione **criar nova** e forneça o exclusivo **nome** que selecionou anteriormente para o grupo de recursos. 
    - **Localização** - selecione um **localização** na lista pendente.
    - **Utilizador** -utilize o valor de nome de utilizador que selecionou anteriormente.

4. Implemente a aplicação.

    - Clique para aceitar os termos e condições.
    - Clique em **Comprar**.

5. Monitorizar o estado de implementação, clicando em **notificações**, que é o ícone de campainha à direita da caixa de pesquisa. Implementar a aplicação SaaS de bilhetes Wingtip demora cerca de cinco minutos.

   ![implementação concluída com êxito](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Transferir e os scripts de gestão Wingtip pedidos de desbloqueio

Enquanto a aplicação está a implementar, transfira os scripts de gestão e o código de origem.

> [!IMPORTANT]
> Conteúdo executável (scripts, DLLs) pode estar bloqueado pelo Windows quando os ficheiros. zip transferidos a partir de uma origem externa e extraiu. Quando extrair os scripts a partir de um ficheiro zip, utilize os seguintes passos para desbloquear o ficheiro. zip antes de a extrair. O desbloqueio garante que os scripts estão autorizados a executar.

1. Navegue para o [repositório do GitHub WingtipTicketsSaaS DbPerTenant][github-wingtip-dpt].
2. Clique em **clonar ou transferir**.
3. Clique em **transferir ZIP**e, em seguida, guarde o ficheiro.
4. Clique com botão direito do **WingtipTicketsSaaS-DbPerTenant-master.zip** de ficheiros e, em seguida, selecione **propriedades**.
5. No **geral** separador, selecione **desbloqueio**e, em seguida, clique em **aplicar**.
6. Clique em **OK**.
7. Extraia os ficheiros.

Scripts estão localizados no *... \\Mestre de DbPerTenant WingtipTicketsSaaS\\Learning módulos* pasta.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Atualize o ficheiro de configuração de utilizador para esta implementação

Antes de executar quaisquer scripts, atualize o *grupo de recursos* e *utilizador* valores na **UserConfig.psm1**. Defina estas variáveis para os valores utilizados durante a implementação.

1. No *ISE do PowerShell*, abra... \\Learning módulos\\*UserConfig.psm1* 
2. Atualização *ResourceGroupName* e *nome* com os valores específicos para a sua implementação (em linhas 10 e 11 apenas).
3. Guarde as alterações!

Estes valores são referenciados quase todos os script.

## <a name="run-the-application"></a>Executar a aplicação

A aplicação showcases venues que alojam os eventos. Os tipos de venue incluem concert halls, jazz clubs e clubs desporto. Em permissões Wingtip, venues estão registados como os inquilinos. A ser um inquilino proporciona um venue uma forma fácil para eventos de lista e para vender permissões aos seus clientes. Cada venue obtém um site personalizado para listar os respetivos eventos e para vender pedidos de suporte.

Internamente na aplicação, cada inquilino obtém uma base de dados do SQL implementada para um conjunto elástico de SQL.

Um centro **Hub de eventos** página fornece uma lista de ligações para os inquilinos na sua implementação.

1. Abra o *Hub de eventos* no seu browser: http://events.wingtip-dpt.&lt;utilizador&gt;. trafficmanager.net (substituir &lt;utilizador&gt; com o valor de utilizador da sua implementação):

    ![hub de eventos](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Clique em **Fabrikam Jazz Club** no *Hub de Eventos*.

    ![Eventos](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Traffic Manager do Azure

A aplicação de Wingtip utiliza [ *Traffic Manager do Azure* ](../traffic-manager/traffic-manager-overview.md) para controlar a distribuição de pedidos recebidos. O URL para aceder ao hub de eventos para um inquilino tem obedecer o seguinte formato:

- http://events.Wingtip-DPT.&lt;utilizador&gt;.trafficmanager.net/fabrikamjazzclub

As partes do formato anterior são explicados com a tabela seguinte.

| Parte do URL | Descrição |
| :------- | :---------- |
| http://events.Wingtip-DPT | As partes de eventos da aplicação Wingtip.<br /><br />o ***-dpt*** parte distingue a *da base de dados por inquilino* implementação Wingtip partir de outras implementações ligeiramente diferentes. Por exemplo, os outros artigos de documentação oferecem Wingtip para *Standalong* (*-sa*), ou para *DB de multi-inquilino*. |
| .  *&lt;UTILIZADOR&gt;* | *af1* no nosso exemplo. |
| .trafficmanager.NET/ | Traffic Manager do Azure, base de URL. |
| fabrikamjazzclub | Para o inquilino com o nome *Fabrikam Jazz Club*. |
| &nbsp; | &nbsp; |

1. O nome do inquilino é analisado a partir do URL de pela aplicação de eventos.
2. O nome do inquilino é utilizado para criar uma chave.
3. A chave é utilizada para aceder ao catálogo, para obter a localização da base de dados do inquilino.
    - O catálogo é implementado utilizando *gestão de mapa de partições horizontais*.
4. O *Hub de eventos* utiliza metadados expandido no catálogo para obter uma lista dos URLs de eventos.

Num ambiente de produção, normalmente, cria um registo CNAME DNS para [ *apontar um domínio de internet da empresa* ](../traffic-manager/traffic-manager-point-internet-domain.md) para o perfil do Gestor de tráfego.

## <a name="start-generating-load-on-the-tenant-databases"></a>Começar a gerar carga nas bases de dados de inquilinos

Agora que a aplicação é implementada, vamos colocá-la para funcionar!
O *demonstração LoadGenerator* script do PowerShell inicia uma carga de trabalho em execução em todas as bases de dados do inquilino.
A carga do mundo real em muitas aplicações de SaaS é esporádicas e imprevisíveis.
Para simular este tipo de carga, o gerador de produz uma carga com picos aleatório ou bursts da atividade em cada inquilino.
Os bursts ocorrem em intervalos aleatório.
Demora alguns minutos até o padrão de carga surgir. Por isso, é melhor permitir que o gerador de execução para, pelo menos, três ou quatro minutos antes da carga de monitorização.

1. No *ISE do PowerShell*, abra a... \\Learning módulos\\utilitários\\*demonstração LoadGenerator.ps1* script.
2. Prima **F5** para executar o script e iniciar o gerador de carga. (Mantenha a predefinição os valores de parâmetros por agora.)

Não reutilize a mesma instância de ISE do PowerShell para qualquer coisa, que não sejam talvez uma reexecução de *demonstração LoadGenerator.ps1*. Se precisar de executar outros scripts do PowerShell, inicie um separado ISE do PowerShell.

#### <a name="rerun-with-different-parameters"></a>Volte a executar com diferentes parâmetros

Se pretender voltar a executar o teste de carga de trabalho com diferentes parâmetros, siga estes passos:

1. Parar *LoadGenerator.ps1*.
    - Utilize **Ctrl + C**, ou clique em de **parar** botão.
    - Este interrupção não parar ou afetam as tarefas de incompleta em segundo plano que ainda estão em execução.

2. Volte a executar *demonstração LoadGenerator.ps1*.
    - Volte a executar este deixa de qualquer uma das tarefas em segundo plano que podem ainda estar a executar primeiro *sp_CpuLoadGenerator*.

Ou pode terminar a instância do ISE do PowerShell, o que deixa de todas as tarefas em segundo plano. Em seguida, iniciar uma nova instância do ISE do PowerShell e execute novamente *demonstração LoadGenerator.ps1*.

#### <a name="monitor-the-background-jobs"></a>Monitorizar as tarefas em segundo plano

Se pretender controlar e monitorizar as tarefas em segundo plano, pode utilizar os seguintes cmdlets:

- Get-Job
- Tarefa receber
- Tarefa de paragem

#### <a name="demo-loadgeneratorps1-actions"></a>Ações de demonstração LoadGenerator.ps1

*Demonstração LoadGenerator.ps1* mimics uma carga de trabalho ativa de transações de cliente. Os passos seguintes descrevem a sequência de ações que *demonstração LoadGenerator.ps1* inicia:

1. *Demonstração LoadGenerator.ps1* inicia *LoadGenerator.ps1* em primeiro plano.
    - Ambos estes ficheiros. ps1 são armazenados em pastas *Learning módulos\\utilitários\\*.

2. *LoadGenerator.ps1* ciclos através de todas as bases de dados do inquilino que são registadas no catálogo.

3. Para cada base de dados do inquilino, *LoadGenerator.ps1* inicia uma execução de Transact-SQL armazenados com o nome de procedimento *sp_CpuLoadGenerator*.
    - As execuções são iniciadas em segundo plano, chamando o *Invoke-SqlAzureWithRetry* cmdlet.
    - *sp_CpuLoadGenerator* ciclos em torno de uma instrução SQL SELECT para uma duração predefinida de 60 segundos. O intervalo de tempo entre problemas a SELECIONAR varia de acordo com os valores de parâmetros.
    - Este ficheiro. SQL é armazenado em *WingtipTenantDB\\dbo\\StoredProcedures\\*.

4. Para cada base de dados do inquilino, *LoadGenerator.ps1* também inicia o *Start-Job* cmdlet.
    - *Tarefa de início* mimics uma carga de trabalho de vendas de permissão.

5. *LoadGenerator.ps1* continua em execução, a monitorização de quaisquer novos inquilinos aprovisionados.

&nbsp;

Antes de avançar para a secção seguinte, deixe o gerador de carga em execução no estado de invocar a tarefa.

## <a name="provision-a-new-tenant"></a>Aprovisionar um inquilino novo

A implementação inicial cria três inquilinos de exemplo. Agora pode criar outro inquilino para ver o impacto na aplicação implementada. Na aplicação Wingtip, o fluxo de trabalho para aprovisionar novos inquilinos é explicado no [tutorial de aprovisionamento e o catálogo](saas-dbpertenant-provision-and-catalog.md). Nesta fase, crie um novo inquilino, demora menos de um minuto.

1. No *ISE do PowerShell*, abra... \\Learning Modules\Provision e catálogo\\*demonstração ProvisionAndCatalog.ps1* .
2. Prima **F5** para executar o script. (Mantenha os valores predefinidos por agora).

   > [!NOTE]
   > Utilizam scripts de Wingtip SaaS muitos *$PSScriptRoot* para navegar pastas para chamar as funções em outros scripts. Esta variável só é avaliada quando o script completo é executado, premindo **F5**.  Realce e em execução uma seleção com **F8** pode resultar em erros. Por isso, execute os scripts, premindo **F5**.

A nova base de dados do inquilino é:

- Criados num agrupamento elástico de SQL.
- Foi inicializado.
- Registado no catálogo.

Após o aprovisionamento com êxito, o *eventos* local do inquilino novo é apresentada no browser:

![Novo inquilino](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Atualize o *Hub de eventos* efetuar ao novo inquilino aparecer na lista.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Explorar os servidores, os conjuntos e as bases de dados de inquilinos

Agora que já iniciou a execução de carga na coleção de inquilinos, vamos ver alguns dos recursos que foram implementados:

1. No [portal do Azure](http://portal.azure.com), navegue até à sua lista de servidores SQL e, em seguida, abra o **catálogo-dpt -&lt;utilizador&gt;**  servidor.
    - O servidor de catálogo contém duas bases de dados, o **tenantcatalog** e **basetenantdb** (modelo base de dados que é copiado para criar novos inquilinos).

   ![bases de dados](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. Regressar à sua lista de servidores SQL.

3. Abra o **tenants1-dpt -&lt;utilizador&gt;**  servidor que contém as bases de dados do inquilino.

4. Consulte os seguintes itens:
    - Cada base de dados do inquilino é um *elástico Standard* base de dados num conjunto standard 50 eDTU.
    - O *Red Maple Racing* base de dados, o que é a base de dados do inquilino aprovisionados anteriormente.

   ![servidor](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Monitorizar o conjunto

Depois de *LoadGenerator.ps1* é executado durante vários minutos, existem dados suficientes devem estar disponíveis para começar a procurar algumas capacidades de monitorização. Estas capacidades incorporadas em conjuntos e as bases de dados.

Navegue para o servidor **tenants1-dpt -&lt;utilizador&gt;**e clique em **Pool1** para ver a utilização de recursos para o conjunto. Os gráficos seguintes, o gerador de carga foi executada durante uma hora.

   ![monitorização do conjunto](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- O gráfico primeiro, com a etiqueta **utilização de recursos**, mostra a utilização de eDTU do conjunto.
- Segundo gráfico mostra a utilização de eDTU das bases de dados de cinco superiores no conjunto.

Os dois gráficos mostram que são adequados para cargas de trabalho de aplicações de SaaS conjuntos elásticos e base de dados SQL.
Os gráficos mostram que 4 bases de dados são cada segurança para quanto 40 eDTUs e ainda todas as bases de dados comfortably são suportados por um conjunto de 50 eDTU. O conjunto de 50 eDTU pode suportar as cargas de trabalho mesmo mais pesadas.
Se estes tenham sido aprovisionados como bases de dados autónomo, fariam cada têm de ser um S2 (50 DTU) para suportar os bursts.
O custo de 4 bases de dados do autónomo S2 seria quase 3 vezes o preço do conjunto.
Em situações de mundo real, os clientes de base de dados do SQL Server estão atualmente em execução até 500 bases de dados em 200 conjuntos de eDTU.
Para obter mais informações, veja o [tutorial de monitorização do desempenho](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Recursos adicionais

- Adicionais [tutoriais que criar no Wingtip pedidos de suporte de SaaS base de dados por aplicação do inquilino](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
- Para saber mais sobre os conjuntos elásticos, veja [ *O que é um conjunto elástico do SQL do Azure?*](sql-database-elastic-pool.md)
- Para saber mais sobre as tarefas elásticas, veja [*Managing scaled-out cloud databases (Gerir bases de dados de escalamento horizontal na cloud)*](sql-database-elastic-jobs-overview.md)
- Para saber mais sobre as aplicações SaaS multi-inquilino, veja [*Design patterns for multi-tenant SaaS applications (Padrões de conceção para aplicações SaaS multi-inquilino)*](saas-tenancy-app-design-patterns.md)


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber:

> [!div class="checklist"]
> - Como implementar a aplicação de Wingtip bilhetes SaaS
> - Mais informações sobre os servidores, os conjuntos e as bases de dados que constituem a aplicação
> - Como os inquilinos são mapeados para os dados com o *catálogo*
> - Como aprovisionar inquilinos novos
> - Como ver a utilização do conjunto para monitorizar a atividade dos inquilinos
> - Como eliminar os recursos de exemplo para parar a faturação relacionada

Em seguida, tente o [tutorial de aprovisionamento e o catálogo](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 

