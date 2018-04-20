---
title: Recuperação após desastre para aplicações de SaaS com o Azure SQL da base de dados de Georreplicação | Microsoft Docs
description: Saiba como utilizar réplicas geo de SQL Database do Azure para recuperar uma aplicação de SaaS multi-inquilino em caso de indisponibilidade
keywords: tutorial de base de dados sql
services: sql-database
author: AyoOlubeko
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/09/2018
ms.author: ayolubek
ms.openlocfilehash: c6f3da52643caa9aa1172db5b884c5336c409715
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
# <a name="disaster-recovery-for-a-multi-tenant-saas-application-using-database-geo-replication"></a>Recuperação após desastre para uma aplicação de SaaS multi-inquilino através da base de dados georreplicação

Neste tutorial, explore um cenário de recuperação de desastre completa para uma aplicação de SaaS multi-inquilino implementado utilizando o modelo de base de dados por inquilino. Para proteger a aplicação a partir de uma falha, utilize [ _georreplicação_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) criar réplicas de bases de dados do catálogo e de inquilino numa região de recuperação alternativo. Se ocorrer uma falha, pode rapidamente falha a estas réplicas para retomar as operações de negócio normal. Na ativação pós-falha, as bases de dados na região original, tornar-se as réplicas secundárias das bases de dados na região de recuperação. Depois destas réplicas fique novamente online são automaticamente catch cópias de segurança para o estado das bases de dados na região de recuperação. Após a interrupção estiver resolvida, falhar novamente para as bases de dados na região de produção original.

Este tutorial explicar fluxos de trabalho de ativação pós-falha e a reativação pós-falha. Irá aprender como:
> [!div classs="checklist"]

>* Base de dados de sincronização e informações de configuração do conjunto elástico para o catálogo de inquilino
>* Configurar um ambiente de recuperação numa região alternativo, que inclui a aplicação, servidores e agrupamentos
>* Utilize _georreplicação_ para replicar as bases de dados do catálogo e de inquilino para a região de recuperação
>* Efetuar a ativação pós-falha a aplicação e as bases de dados de catálogo e de inquilino para a região de recuperação 
>* Mais tarde, a ativação pós-falha a aplicação, as bases de dados do catálogo e de inquilino de volta para a região original após a interrupção estiver resolvida
>* O catálogo de atualização, como cada base de dados do inquilino é a ativação pós-falha para controlar a localização principal da base de dados de cada inquilino
>* Certifique-se de que a base de dados de aplicação e primário inquilino são sempre colocalizadas na mesma região do Azure para reduzir a latência  
 

Antes de começar este tutorial, certifique-se que os pré-requisitos seguintes são concluídos:
* A base de dados de SaaS de bilhetes Wingtip por inquilino aplicação é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a base de dados de SaaS de bilhetes Wingtip por aplicação do inquilino](saas-dbpertenant-get-started-deploy.md)  
* O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-geo-replication-recovery-pattern"></a>Introdução ao padrão de recuperação georreplicação

![Arquitetura de recuperação](media/saas-dbpertenant-dr-geo-replication/recovery-architecture.png)
 
Recuperação após desastre (DR) é uma consideração importante para muitas aplicações, para motivos de conformidade ou de continuidade do negócio. Deve existir uma interrupção do serviço prolongado, um plano de DR bem preparado pode minimizar a interrupção de negócios. Utilizar a georreplicação fornece ao mais baixo RPO e RTO, mantendo as réplicas de base de dados numa região de recuperação que pode ser falhou a num curto período reparar.

Plano de DR da com base no georreplicação é composto por três partes distintas:
* Configuração-criação e manutenção do ambiente de recuperação
* Recuperação - ativação pós-falha da aplicação e bases de dados para o ambiente de recuperação se ocorrer uma falha,
* Repatriation - ativação pós-falha da aplicação e as bases de dados novamente para a região original assim que a aplicação estiver resolvida 

Todas as partes tem de ser considerada cuidadosamente, especialmente se funcionar à escala. Em geral, o plano deve realizar vários objetivos:

* Configurar
    * Estabelecer e manter um ambiente de imagem espelhada na região de recuperação. Criar os conjuntos elásticos e bases de dados autónomo neste ambiente de recuperação a replicar reserva capacidade na região de recuperação. Manter este ambiente inclui replicar novas bases de dados do inquilino como terem sido aprovisionados.  
* Recuperação
    * Sempre que um ambiente de recuperação pendente ampliada é utilizado para minimizar os custos diárias, conjuntos e as bases de dados autónomo tem ser escalados para cima para adquirir a capacidade total operacional na região de recuperação
    * Ativar o novo inquilino aprovisionamento na região recuperação logo que possível  
    * Otimizado para restaurar os inquilinos na ordem de prioridade
    * Otimizado para obter os inquilinos online mais rapidamente possível efetuando os passos em paralelo onde práticas
    * Sejam resilientes a falhas, reiniciável e idempotent
    * É possível cancelar o processo em trânsito intermédio se a região original vem voltar a estar online.
* Repatriation 
    * Efetuar a ativação pós-falha de bases de dados da região de recuperação para réplicas na região original com um impacto mínimo para os inquilinos: sem perda de dados e um período mínimo offline por inquilino.   

Neste tutorial, estes desafios são resolvidos utilizando as funcionalidades da SQL Database do Azure e a plataforma do Azure:

* [Modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), para reservar capacidade necessária de todos os mais rapidamente possível. Modelos Azure Resource Manager são utilizados para Aprovisionar uma imagem de espelho dos servidores de produção e conjuntos elásticos na região de recuperação.
* [Replicação geográfica](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), para criar as réplicas secundárias no modo assíncrono replicadas só de leitura para todas as bases de dados. Durante uma falha, a ativação pós-falha para as réplicas na região de recuperação.  Após a interrupção estiver resolvida, falhar novamente para as bases de dados na região sem perda de dados original.
* [Assíncrona](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) enviadas por ordem de prioridade de inquilino, para minimizar o tempo de ativação pós-falha para grandes quantidades de bases de dados de operações de ativação pós-falha.
* [Funcionalidades de recuperação de gestão de partições horizontais](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager), para alterar as entradas da base de dados no catálogo durante a recuperação e repatriation. Estas funcionalidades permitem a aplicação se ligue ao inquilino bases de dados, independentemente da localização sem necessidade de reconfigurar a aplicação.
* [Aliases DNS do SQL server](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), para ativar o aprovisionamento totalmente integrada de novos inquilinos, independentemente da que região a aplicação está a funcionar no. Aliases DNS também são utilizadas para permitir que o processo de sincronização do catálogo ligar ao catálogo do Active Directory, independentemente da respetiva localização.

## <a name="get-the-disaster-recovery-scripts"></a>Obter os scripts de recuperação de desastre 

> [!IMPORTANT]
> Como todos os pedidos de Wingtip gestão scripts, os scripts de DR são qualidade de exemplo e não são deve ser utilizada na produção. 

A recuperação scripts utilizadas neste tutorial e o código fonte da aplicação Wingtip estão disponíveis no [Wingtip bilhetes SaaS a base de dados por inquilino repositório do GitHub](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/). Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de gestão Wingtip pedidos de desbloqueio.

## <a name="tutorial-overview"></a>Descrição geral do tutorial
Neste tutorial, utilize primeiro georreplicação para criar as réplicas de aplicação Wingtip pedidos e respetivas bases de dados numa região diferente. Em seguida, a ativação pós-falha para esta região para simular a recuperar a partir de uma falha. Quando terminar, a aplicação é totalmente funcional na região de recuperação.

Mais tarde, um passo repatriation separado, ativação pós-falha as bases de dados do catálogo e de inquilino na região de recuperação para a região original. As aplicações e bases de dados permanecem disponíveis em toda a repatriation. Quando terminar, a aplicação é totalmente funcional na região original.

> [!Note]
> A aplicação é recuperada para o _região emparelhado_ da região na qual a aplicação é implementada. Para obter mais informações, consulte [Azure emparelhado regiões](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).

## <a name="review-the-healthy-state-of-the-application"></a>Reveja o estado de funcionamento da aplicação

Antes de começar o processo de recuperação, reveja o estado de bom estado de funcionamento normal da aplicação.
1. No seu browser, abra o Hub de eventos de pedidos de suporte de Wingtip (http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net - substitua &lt;utilizador&gt; com o valor de utilizador da sua implementação).
    * Desloque-se na parte inferior da página e repare o nome do servidor de catálogo e a localização no rodapé. A localização é a região no qual implementou a aplicação.
    *Sugestão: Paire o rato sobre a localização para aumentar a apresentação. * 
     ![Estado de bom estado de funcionamento de hub de eventos na região original](media/saas-dbpertenant-dr-geo-replication/events-hub-original-region.png)

2. Clique no inquilino Contoso Concert Hall e abra a página de eventos.
    * No rodapé, tenha em atenção o nome do servidor de inquilino. A localização será a mesma localização no servidor de catálogo.

3. No [portal do Azure](https://portal.azure.com), abra o grupo de recursos no qual a aplicação é implementada
    * Tenha em atenção a região na qual os servidores são implementados. 

## <a name="sync-tenant-configuration-into-catalog"></a>Configuração de inquilinos de sincronização no catálogo

Nesta tarefa, inicie um processo que sincroniza-se a configuração de servidores, conjuntos elásticos e bases de dados para o catálogo de inquilino. O processo mantém estas informações atualizadas no catálogo.  O processo funciona com o catálogo do Active Directory, se a região original ou na região de recuperação. As informações de configuração são utilizadas como parte do processo de recuperação para garantir que o ambiente de recuperação é consistente com o ambiente original e, em seguida, mais tarde durante repatriation para garantir a região original é tornada consistente com todas as alterações efetuadas a ambiente de recuperação. O catálogo também é utilizado para controlar o estado de recuperação dos recursos de inquilino

> [!IMPORTANT]
> De simplicidade, o processo de sincronização e outros processos de recuperação e repatriation execução longa são implementados nestes tutoriais de como as tarefas de Powershell locais ou sessões que são executados com o início de sessão de utilizador do cliente. Os tokens de autenticação emitidos quando o início de sessão irá expirar após várias horas e, em seguida, as tarefas falhará. Num cenário de produção, os processos de execução longa devem ser implementados como serviços do Azure fiáveis tipo, a ser executado um principal de serviço. Consulte [utilize o Azure PowerShell para criar um principal de serviço com um certificado](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal).

1. No _ISE do PowerShell_, abra o ficheiro de Modules\UserConfig.psm1 ...\Learning. Substitua `<resourcegroup>` e `<user>` em linhas 10 e 11 com o valor utilizado quando implementou a aplicação.  Guarde o ficheiro!

2. No *ISE do PowerShell*, abra o ...\Learning script Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 e definir:
    * **$DemoScenario = 1**, iniciar uma tarefa em segundo plano que sincroniza-se o servidor de inquilino e agrupamento de informações de configuração para o catálogo

3. Prima **F5** para executar o script de sincronização. Uma nova sessão PowerShell está aberta para sincronizar a configuração dos recursos de inquilino.
![Processo de sincronização](media/saas-dbpertenant-dr-geo-replication/sync-process.png)

Deixe a janela do PowerShell em execução em segundo plano e continuar com o resto do tutorial. 

> [!Note]
> O processo de sincronização estabelece ligação ao catálogo de através de um alias de DNS. Este alias é modificado durante o restauro e repatriation para apontar para o catálogo de Active Directory. O processo de sincronização mantém o catálogo atualizado com a base de dados ou um conjunto de alterações de configuração efetuadas na região de recuperação.  Durante a repatriation, estas alterações são aplicadas aos recursos equivalentes na região original.

## <a name="create-secondary-database-replicas-in-the-recovery-region"></a>Criar réplicas de base de dados secundária na região de recuperação

Nesta tarefa, inicie um processo que implementa uma instância de aplicação duplicado e replica o catálogo e todas as bases de dados do inquilino para uma região de recuperação.

> [!Note]
> Este tutorial adiciona uma proteção de replicação geográfica para a aplicação de exemplo Wingtip pedidos de suporte. Num cenário de produção para uma aplicação que utiliza a georreplicação, cada inquilino teria de ser aprovisionado com uma base de dados de georreplicação proposto. Consulte [conceber elevados serviços utilizando a SQL Database do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)

1. No *ISE do PowerShell*, abra o ...\Learning script Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 e defina os seguintes valores:
    * **$DemoScenario = 2**, criar o ambiente de recuperação da imagem espelhada e replicar as bases de dados do catálogo e de inquilino

2. Prima **F5** para executar o script. É aberta uma nova sessão PowerShell para criar as réplicas.
![Processo de sincronização](media/saas-dbpertenant-dr-geo-replication/replication-process.png)  

## <a name="review-the-normal-application-state"></a>Reveja o estado normal da aplicação
Neste momento, a aplicação está a funcionar normalmente na região original e agora está protegida por georreplicação.  As réplicas secundárias só de leitura, existe na região de recuperação para todas as bases de dados. 
1. No portal do Azure, observe os grupos de recursos e tenha em atenção que foi criado um grupo de recursos com - sufixo de recuperação na região de recuperação. 

1. Explore os recursos no grupo de recursos de recuperação.  

1. Clique na base de dados de Contoso Concert Hall no _tenants1-dpt -&lt;utilizador&gt;-recuperação_ servidor.  Clique em Georreplicação no lado esquerdo. 

    ![Ligação de replicação geográfica Concert de contoso](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication.png) 

No mapa de regiões do Azure, tenha em atenção a ligação de replicação geográfica entre a primária na região original e o elemento secundário na região de recuperação.  

## <a name="fail-over-the-application-into-the-recovery-region"></a>Efetuar a ativação pós-falha a aplicação para a região de recuperação

### <a name="geo-replication-recovery-process-overview"></a>Descrição geral do processo de recuperação georreplicação

O script de recuperação efetua as seguintes tarefas:

1. Desativa o ponto final do Gestor de tráfego para a aplicação web na região original. Desativar o ponto final impede que os utilizadores a ligar a aplicação num estado inválido deve a região original ficar online durante a recuperação.

1. Utiliza uma ativação pós-falha forçada da base de dados do catálogo na região de recuperação para que a base de dados primária e atualiza o _activecatalog_ alias para apontar para o servidor de catálogo de recuperação.

1. Atualizações a _newtenant_ alias para apontar para o servidor de inquilino na região de recuperação. Alterar este alias assegura que as bases de dados para os novos inquilinos são aprovisionados na região de recuperação. 

1. Marca todos os inquilinos existentes no catálogo de recuperação como offline para impedir o acesso às bases de dados do inquilino antes de poderem executar a ativação pós-falha.

1. Atualiza a configuração de todos os conjuntos elásticos e bases de dados autónomo replicados na região de recuperação para espelhar a configuração dos mesmos na região original. (Esta tarefa só é necessário se agrupamentos ou bases de dados replicados no ambiente de recuperação são ampliadas durante as operações normais para reduzir os custos).

1. Permite que o ponto de final do Gestor de tráfego para a aplicação web na região de recuperação. Ativar este ponto final permite que a aplicação aprovisionar novos inquilinos. Nesta fase, os inquilinos existentes são ainda offline.

1. Submete lotes de pedidos para forçar a ativação pós-falha bases de dados por ordem de prioridade.
    * Lotes estão organizados de modo a que as bases de dados estão a ativação pós-falha em paralelo em todos os agrupamentos.
    * Pedidos de ativação pós-falha são enviados através de operações assíncronas, de modo a são submetidas rapidamente e vários pedidos podem ser processados em simultâneo.

   > [!Note]
   > Num cenário de falha, as bases de dados principais na região original estão offline.  Force a ativação pós-falha no quebras de linha secundárias a ligação primária sem tentar aplicar o quaisquer residuais transações em fila. Num cenário de desagregação DR como neste tutorial, se não houver qualquer atividade de atualização no momento da ativação pós-falha pode haver alguma perda de dados. Mais tarde, durante repatriation, quando efetuar a ativação pós-falha de bases de dados na região de recuperação para a região original, uma ativação pós-falha normal é utilizada para garantir que não há nenhuma perda de dados.

1. Monitoriza o serviço de base de dados do SQL Server para determinar quando bases de dados tem sido efetuadas a ativação pós-falha. Depois de uma base de dados do inquilino é a ativação pós-falha, atualiza o catálogo para registar o estado de recuperação da base de dados do inquilino e marcar o inquilino como online.
    * Bases de dados de inquilino podem ser acedidos pela aplicação, assim que estiver marcadas no catálogo online.
    * A soma dos valores de rowversion na base de dados do inquilino é armazenada no catálogo. Este valor atua como uma identificação digital que permite que o processo de repatriation para determinar se a base de dados tiver sido atualizado na região de recuperação.

### <a name="run-the-script-to-fail-over-to-the-recovery-region"></a>Execute o script para efetuar a ativação pós-falha para a região de recuperação

Agora imagine houver uma falha na região na qual a aplicação é implementada e execute o script de recuperação:

1. No *ISE do PowerShell*, abra o ...\Learning script Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 e defina os seguintes valores:
    * **$DemoScenario = 3**, recuperar a aplicação para uma região de recuperação por falhar ao longo para réplicas

2. Prima **F5** para executar o script.  
    * O script abre-se numa nova janela do PowerShell e, em seguida, inicia uma série de tarefas do PowerShell que são executadas em paralelo. Estas tarefas efetuar a ativação pós-falha de bases de dados do inquilino para a região de recuperação.
    * A região de recuperação é a _região emparelhado_ associada com a região do Azure no qual implementou a aplicação. Para obter mais informações, consulte [Azure emparelhado regiões](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

3. Monitorize o estado do processo de recuperação na janela do PowerShell.
    ![Processo de ativação pós-falha](media/saas-dbpertenant-dr-geo-replication/failover-process.png)

> [!Note]
> Para explorar o código para as tarefas de recuperação, reveja os scripts do PowerShell na pasta ...\Learning Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\RecoveryJobs.

### <a name="review-the-application-state-during-recovery"></a>Reveja o estado da aplicação durante a recuperação
Enquanto o ponto final da aplicação estiver desativado no Traffic Manager, a aplicação não está disponível. Depois do catálogo de ativação pós-falha foi efetuada para a região de recuperação e todos os inquilinos marcado offline, a aplicação seja novamente colocada online. Apesar da aplicação está disponível, cada inquilino aparece offline no hub de eventos até que a respetiva base de dados é a ativação pós-falha. É importante criar uma aplicação para processar as bases de dados do inquilino offline.

1. Imediatamente depois da base de dados do catálogo foi recuperado, atualize o Hub de eventos de pedidos de suporte de Wingtip no seu browser.
    * No rodapé, tenha em atenção que o nome do servidor de catálogo tem agora um _-recuperação_ sufixo e está localizado na região de recuperação.
    * Tenha em atenção que os inquilinos que ainda não foram restaurados, estão marcados como offline e não sejam selecionável.  

    > [!Note]
    > Com apenas alguns bases de dados a recuperar, não poderá atualizar o browser antes de recuperação foi concluída, pelo que não poderá ver os inquilinos enquanto estão offline. 
 
    ![Hub de eventos offline](media/saas-dbpertenant-dr-geo-replication/events-hub-offlinemode.png) 

    * Se abrir diretamente o página de eventos de um inquilino offline, apresenta uma notificação 'inquilino offline'. Por exemplo, se Contoso Concert Hall estiver offline, tentar abrir http://events.wingtip-dpt.&lt; utilizador&gt;.trafficmanager.net/contosoconcerthall ![página Contoso Offline](media/saas-dbpertenant-dr-geo-replication/dr-in-progress-offline-contosoconcerthall.png) 

### <a name="provision-a-new-tenant-in-the-recovery-region"></a>Aprovisionar um novo inquilino na região de recuperação
Mesmo antes de todos os os inquilino bases de dados existentes têm a ativação pós-falha, pode aprovisionar novos inquilinos na região de recuperação.  

1. No *ISE do PowerShell*, abra o ...\Learning script Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 e defina a propriedade seguinte:
    * **$DemoScenario = 4**, aprovisionar um novo inquilino na região de recuperação

2. Prima **F5** para executar o script e aprovisionar ao novo inquilino. 

3. A página de eventos Hawthorn Hall abre no browser quando for concluída. Tenha em atenção de rodapé que a base de dados Hawthorn Hall é aprovisionado na região de recuperação.
    ![Página de eventos de hawthorn Hall](media/saas-dbpertenant-dr-geo-replication/hawthornhallevents.png) 

4. No browser, atualize a página de Hub de eventos de pedidos de suporte de Wingtip para ver que hawthorn Hall incluídos. 
    * Se aprovisionado Hawthorn Hall sem aguardar por outros inquilinos restaurar outros inquilinos ainda podem estar offline.


## <a name="review-the-recovered-state-of-the-application"></a>Reveja o estado da aplicação recuperado

Quando tiver concluído o processo de recuperação, a aplicação e todos os inquilinos são totalmente funcionais na região de recuperação. 

1. Depois do ecrã na janela de consola do PowerShell indica que todos os inquilinos são recuperados, atualize o Hub de eventos.  Os inquilinos todas aparecerá online, incluindo ao novo inquilino, Hawthorn Hall.

    ![recuperada e novos inquilinos no hub de eventos](media/saas-dbpertenant-dr-geo-replication/events-hub-with-hawthorn-hall.png)

2. No [portal do Azure](https://portal.azure.com), abra a lista de grupos de recursos.  
    * Tenha em atenção o grupo de recursos que implementou, juntamente com o grupo de recursos de recuperação com o _-recuperação_ sufixo.  O grupo de recursos de recuperação contém todos os recursos criados durante o processo de recuperação plus novos recursos criados durante a interrupção.  

3. Abra o grupo de recursos de recuperação e tenha em atenção os seguintes itens:
    * As versões de recuperação dos servidores de catálogo e tenants1 com _-recuperação_ sufixo.  Os restaurada inquilino e de catálogo de bases de dados nestes servidores todas as tem nomes utilizados na região original.

    * O _tenants2-dpt -&lt;utilizador&gt;-recuperação_ do SQL Server.  Este servidor é utilizado para aprovisionar novos inquilinos durante a interrupção.
    *   O serviço de aplicações com o nome, _eventos-wingtip-dpt -&lt;recoveryregion&gt;-&lt;utilizador & gt_; que é a instância de recuperação da aplicação de eventos. 

    ![Recursos de recuperação do Azure ](media/saas-dbpertenant-dr-geo-replication/resources-in-recovery-region.png)    
    
4. Abra o _tenants2-dpt -&lt;utilizador&gt;-recuperação_ do SQL Server.  Tenha em atenção que contém a base de dados _hawthornhall_ e o conjunto elástico, _Pool1_.  O _hawthornhall_ base de dados é configurada como uma base de dados elástica no _Pool1_ conjunto elástico.

5. Navegue de volta para o grupo de recursos e clique na base de dados de Contoso Concert Hall no _tenants1-dpt -&lt;utilizador&gt;-recuperação_ servidor. Clique em Georreplicação no lado esquerdo.
    
    ![Base de dados de contoso após a ativação pós-falha](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication-after-failover.png)

## <a name="change-tenant-data"></a>Dados de alterações de inquilino 
Nesta tarefa, atualizar uma das bases de dados do inquilino. 

1. No seu browser, localize a lista de eventos para Hall de Concert Contoso e tenha em atenção o último nome do evento.
2. No *ISE do PowerShell*, na ...\Learning script Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1, defina o seguinte valor:
    * **$DemoScenario = 5** eliminar um evento de um inquilino na região de recuperação
3. Prima **F5** ao executar o script
4. Atualize a página de eventos de Contoso Concert Hall (http://events.wingtip-dpt.&lt; utilizador&gt;.trafficmanager.net/contosoconcerthall - substitua &lt;utilizador&gt; com o valor de utilizador da sua implementação) e tenha em atenção que o último evento foi eliminado.

## <a name="repatriate-the-application-to-its-original-production-region"></a>Repatriate a aplicação para a região de produção original

Esta tarefa repatriates a aplicação para sua região original. Num cenário real, teria de iniciar repatriation quando a interrupção estiver resolvida.

### <a name="repatriation-process-overview"></a>Descrição geral do processo repatriation

![Arquitetura de repatriation](media/saas-dbpertenant-dr-geo-replication/repatriation-architecture.png)

O processo de repatriation:
1. Cancela quaisquer pedidos de restauro da base de dados em trânsito ou pendentes.
2. Atualizações a _newtenant_ alias para apontar para o servidor dos inquilinos na região de origem. Alterar este alias assegura que as bases de dados para os novos inquilinos agora serão aprovisionados na região de origem.
3. Implementa quaisquer dados de inquilino alterados para a região original
4. Falha ao longo de bases de dados do inquilino por ordem de prioridade.

Ativação pós-falha move efetivamente a base de dados para a região original. Quando a base de dados a ativação pós-falha, quaisquer ligações abertas são ignoradas e a base de dados não está disponível por alguns segundos. As aplicações devem ser escritas com a lógica de repetição para se certificar de que estabelecer ligação novamente.  Embora este breve desligar muitas vezes, não é reparado, pode optar por repatriate bases de dados fora do horário de expediente. 


### <a name="run-the-repatriation-script"></a>Execute o script de repatriation
Agora vamos imaginar a interrupção estiver resolvida e execute o script de repatriation.

1. No *ISE do PowerShell*, na ...\Learning Modules\Business continuidade e desastre Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script.

2. Certifique-se de que o processo de sincronização do catálogo ainda está em execução na sua instância do PowerShell.  Se necessário, reinicie-o através da definição:
    * **$DemoScenario = 1**, iniciar a sincronização do servidor de inquilino, agrupamento e informações de configuração de base de dados para o catálogo
    * Prima **F5** para executar o script.

3.  Em seguida, para iniciar o processo de repatriation, defina:
    * **$DemoScenario = 6**, repatriate a aplicação para sua região original
    * Prima **F5** para executar o script de recuperação numa nova janela do PowerShell.  Repatriation irá demorar alguns minutos e podem ser monitorizados na janela do PowerShell.
    ![Processo de repatriation](media/saas-dbpertenant-dr-geo-replication/repatriation-process.png)

4. Enquanto estiver a executar o script, atualize a página de Hub de eventos (http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net)
    * Tenha em atenção de que todos os inquilinos estão online e acessível ao longo deste processo.

5. Depois de concluída a repatriation, atualize o hub de eventos e abra a página Eventos Hawthorn Hall. Tenha em atenção que esta base de dados tem sido repatriated para a região original.
    ![Hub de eventos repatriated](media/saas-dbpertenant-dr-geo-replication/events-hub-repatriated.png)


## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Conceber a aplicação para garantir que a aplicação e a base de dados estão colocalizados 
A aplicação foi concebida para que o se liga-se sempre de uma instância na mesma região que a base de dados do inquilino. Esta conceção reduz a latência entre a aplicação e a base de dados. Esta otimização assume que a interação da aplicação-para-base de dados é chattier que a interação do utilizador para aplicações.  

Bases de dados do inquilino podem ser distribuídos por recuperação e regiões originais durante algum tempo durante repatriation. Para cada base de dados, a aplicação procura a região na qual a base de dados está localizada efetuando uma pesquisa DNS no nome do servidor de inquilino. Na base de dados do SQL Server, o nome do servidor é um alias. O nome do servidor de um alias contém o nome de região. Se a aplicação não estiver na mesma região que a base de dados, será redirecionado para a instância na mesma região que o servidor de base de dados.  Redirecionar a instância na mesma região que a base de dados minimiza a latência entre aplicações e a base de dados. 

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:
> [!div classs="checklist"]

>* Base de dados de sincronização e informações de configuração do conjunto elástico para o catálogo de inquilino
>* Configurar um ambiente de recuperação numa região alternativo, que inclui a aplicação, servidores e agrupamentos
>* Utilize _georreplicação_ para replicar as bases de dados do catálogo e de inquilino para a região de recuperação
>* Efetuar a ativação pós-falha a aplicação e as bases de dados de catálogo e de inquilino para a região de recuperação 
>* Falhar novamente as bases de dados de aplicação, de inquilino e de catálogo para a região original, após a interrupção estiver resolvida

Pode saber mais sobre as tecnologias de base de dados SQL do Azure fornece para ativar a continuidade do negócio no [descrição geral da continuidade do negócio](sql-database-business-continuity.md) documentação.

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que tirar partido da aplicação Wingtip SaaS](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
