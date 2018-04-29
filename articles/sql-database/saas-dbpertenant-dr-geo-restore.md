---
title: Utilizar cópias de segurança georredundante SQL Database do Azure para a recuperação de desastre de aplicações SaaS | Microsoft Docs
description: Saiba como utilizar as cópias de segurança georredundante SQL Database do Azure para recuperar uma aplicação SaaS multi-inquilino em caso de indisponibilidade
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: 8fd25e13f6796b8be99ad3efd425bcde7bca3905
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Utilize georrestauro para recuperar uma aplicação SaaS multi-inquilino de cópias de segurança da base de dados

Este tutorial explicar um cenário de recuperação de desastre completa para uma aplicação SaaS multi-inquilino implementado com a base de dados por modelo de inquilino. Utilizar [georrestauro](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups) para recuperar as bases de dados de inquilino e de catálogo de cópias de segurança georredundante manter automaticamente para uma região de recuperação alternativo. Após a interrupção estiver resolvida, utilize [georreplicação](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) para repatriate bases de dados alterados para as respetivas região original.

![arquitetura de restauro georreplicação](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Georrestauro é a solução de recuperação de desastre menor custo para a SQL Database do Azure. No entanto, o restauro de cópias de segurança georredundante pode resultar na perda de dados de uma hora. Pode demorar um tempo considerável, dependendo do tamanho de cada base de dados. 

> [!NOTE]
> Recupere as aplicações com o RPO e RTO mais baixa possível utilizando georreplicação em vez de georrestauro.

Este tutorial explicar restauro e repatriation fluxos de trabalho. Saiba como:
> [!div class="checklist"]

>* Base de dados de sincronização e informações de configuração do conjunto elástico para o catálogo de inquilino.
>* Configure um ambiente de imagem espelhada numa região de recuperação que inclui a aplicação, servidores e agrupamentos.   
>* Recupere bases de dados do catálogo e de inquilino utilizando georrestauro.
>* Utilize a georreplicação para repatriate o catálogo de inquilino e bases de dados do inquilino alterados após a interrupção estiver resolvida.
>* O catálogo de atualização, como cada base de dados restaurada (ou repatriated) para controlar a localização atual do Active Directory de cópia da base de dados de cada inquilino.
>* Certifique-se de que a aplicação e a base de dados do inquilino estão sempre colocalizados na mesma região do Azure para reduzir a latência. 
 

Antes de começar este tutorial, conclua os seguintes pré-requisitos:
* Implemente a base de dados de SaaS de bilhetes Wingtip por aplicação do inquilino. Para implementar em menos de cinco minutos, consulte [implementar e explorar a base de dados de SaaS de bilhetes Wingtip por aplicação inquilino](saas-dbpertenant-get-started-deploy.md). 
* Instale o Azure PowerShell. Para obter mais informações, consulte [introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Introdução ao padrão de recuperação georrestauro

Recuperação após desastre (DR) é uma consideração importante para muitas aplicações, para motivos de conformidade ou de continuidade do negócio. Se existir uma interrupção do serviço prolongado, um plano de DR bem preparado pode minimizar a interrupção de negócios. Plano de DR da com base no georrestauro tem realizar vários objetivos:
 * Reserve capacidade todas as necessária na região de recuperação escolhido mais rapidamente possível para garantir que está disponível para restaurar bases de dados do inquilino.
 * Estabelece um ambiente de recuperação da imagem espelhada que reflete a configuração de agrupamento e da base de dados original. 
 * Permite o cancelamento do processo de restauro em trânsito intermédio se a região original volta a ficar online.
 * Ative inquilino aprovisionamento rapidamente, para a integração do novo inquilino pode ser reiniciados logo que possível.
 * Ser otimizada para restaurar os inquilinos na ordem de prioridade.
 * Ser otimizada para obter inquilinos online logo que possível, siga os passos em paralelo onde práticas.
 * Sejam resilientes a falhas, reiniciável e idempotent.
 * Repatriate bases de dados para os respetivos região original com um impacto mínimo para os inquilinos quando a interrupção estiver resolvida.  

> [!NOTE]
> A aplicação é recuperada para a região emparelhada da região na qual a aplicação é implementada. Para obter mais informações, consulte [Azure emparelhado regiões](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).   

Este tutorial utiliza funcionalidades do SQL Database do Azure e a plataforma do Azure para resolver estes desafios:

* [Modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), para reservar capacidade necessária de todos os mais rapidamente possível. Modelos Azure Resource Manager são utilizados para Aprovisionar uma imagem de espelho dos servidores originais e conjuntos elásticos na região de recuperação. Um servidor separado e o agrupamento também são criados para o aprovisionamento de novos inquilinos.
* [Biblioteca de clientes de base de dados elásticas](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library) (EDCL), para criar e manter um catálogo de base de dados do inquilino. O catálogo expandido inclui periodicamente atualizadas informações de configuração de agrupamento e da base de dados.
* [Funcionalidades de recuperação de gestão de partições horizontais](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) de EDCL, para manter as entradas de localização de base de dados no catálogo durante a recuperação e repatriation.  
* [Georrestauro](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), para recuperar as bases de dados de inquilino e de catálogo de cópias de segurança georredundante manter automaticamente. 
* [Operações de restauro assíncrona](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations), enviados por ordem de prioridade de inquilino, são colocados em fila para cada conjunto pelo sistema e processamento em lotes para o conjunto não está sobrecarregado. Estas operações podem ser canceladas antes ou durante a execução, se necessário.   
* [Replicação geográfica](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), para repatriate bases de dados para a região original após a falha. Não há nenhuma perda de dados e um impacto mínimo no inquilino ao utilizar a georreplicação.
* [Aliases DNS do SQL server](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)para permitir que o processo de sincronização do catálogo ligar ao catálogo do Active Directory, independentemente da respetiva localização.  

## <a name="get-the-disaster-recovery-scripts"></a>Obter os scripts de recuperação de desastre

Os scripts de DR utilizados neste tutorial estão disponíveis no [Wingtip bilhetes SaaS a base de dados por inquilino repositório do GitHub](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de gestão Wingtip pedidos de desbloqueio.

> [!IMPORTANT]
> Como todos os pedidos de Wingtip gestão scripts, os scripts de DR são qualidade de exemplo e não são deve ser utilizada na produção.

## <a name="review-the-healthy-state-of-the-application"></a>Reveja o estado de funcionamento da aplicação
Antes de começar o processo de recuperação, reveja o estado de bom estado de funcionamento normal da aplicação.

1. No seu browser, abra o hub de eventos de pedidos de Wingtip (http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net, substitua &lt;utilizador&gt; com o valor de utilizador da sua implementação).
    
   Desloque-se na parte inferior da página e repare o nome do servidor de catálogo e a localização no rodapé. A localização é a região no qual implementou a aplicação.    

   > [!TIP]
   > Paire o rato sobre a localização para aumentar a apresentação.

   ![Estado de bom estado de funcionamento de hub de eventos na região original](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Selecione o inquilino Contoso Concert Hall e abra a página de eventos.

   No rodapé, tenha em atenção o nome do servidor do inquilino. A localização é igual à localização do servidor de catálogo.

   ![Região original contoso Concert Hall](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. No [portal do Azure](https://portal.azure.com), reveja e abra o grupo de recursos no qual implementou a aplicação.

   Tenha em atenção que os recursos e a região na qual os servidores de base de dados do SQL Server e componentes do serviço de aplicações são implementadas.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>A configuração de inquilino para o catálogo de sincronização

Nesta tarefa, inicie um processo de sincronização a configuração de servidores, conjuntos elásticos e bases de dados para o catálogo de inquilino. Estas informações são posteriormente utilizadas para configurar um ambiente de imagem espelhada na região de recuperação.

> [!IMPORTANT]
> De simplicidade, o processo de sincronização e outra recuperação de longa execução e processos de repatriation são implementados nestes exemplos como tarefas de PowerShell locais ou sessões que são executados com o início de sessão de utilizador do cliente. Os tokens de autenticação emitidos quando iniciar sessão expirarem após várias horas e, em seguida, as tarefas falhará. Num cenário de produção, os processos de execução longa devem ser implementados como serviços do Azure fiáveis tipo, a ser executado um principal de serviço. Consulte [utilize o Azure PowerShell para criar um principal de serviço com um certificado](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. No ISE do PowerShell, abra o ficheiro de Modules\UserConfig.psm1 ...\Learning. Substitua `<resourcegroup>` e `<user>` em linhas 10 e 11 com o valor utilizado quando implementou a aplicação. Guarde o ficheiro.

2. No ISE do PowerShell, abra o ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script.

    Neste tutorial, executar cada um dos cenários neste script do PowerShell, por isso, manter este ficheiro aberto.

3. Defina o seguinte:

    $DemoScenario = 1: iniciar uma tarefa em segundo plano que sincroniza-se o servidor de inquilino e informações de configuração de conjunto para o catálogo.

4. Para executar o script de sincronização, selecione F5. 

    Estas informações são posteriormente utilizadas para se certificar de que a recuperação cria uma imagem de espelho de servidores, conjuntos e as bases de dados na região de recuperação.  

    ![Processo de sincronização](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Deixe a janela do PowerShell em execução em segundo plano e continuar com o resto deste tutorial.

> [!NOTE]
> O processo de sincronização estabelece ligação ao catálogo de através de um alias de DNS. O alias é modificado durante o restauro e repatriation para apontar para o catálogo de Active Directory. O processo de sincronização mantém o catálogo atualizados com a base de dados ou um conjunto de alterações de configuração efetuadas na região de recuperação. Durante a repatriation, estas alterações são aplicadas aos recursos equivalentes na região original.

## <a name="geo-restore-recovery-process-overview"></a>Descrição geral do processo de recuperação georrestauro

O processo de recuperação georrestauro implementa a aplicação e restaura as bases de dados de cópias de segurança para a região de recuperação.

O processo de recuperação faz o seguinte:

1. Desativa o ponto de final do Gestor de tráfego do Azure para a aplicação web na região original. Desativar o ponto final impede que os utilizadores a ligar a aplicação num estado inválido deve a região original ficar online durante a recuperação.

2. Aprovisiona uma recuperação catálogo servidor na região de recuperação, georreplicação restaura a base de dados do catálogo e atualiza o alias activecatalog para apontar para o servidor de catálogo restaurada. Alterar o alias de catálogo assegura que o processo de sincronização do catálogo sempre sincroniza-se ao catálogo do Active Directory.

3. Marca todos os inquilinos existentes no catálogo de recuperação como offline para impedir o acesso às bases de dados do inquilino antes de estes são restaurados.

4. Uma instância da aplicação na região de recuperação de Aprovisiona e configura-a para utilizar o catálogo restaurado nessa região. Para manter a latência pelo menos, a aplicação de exemplo foi concebida para sempre ligar a uma base de dados do inquilino na mesma região.

5. Aprovisiona um conjunto de servidor e elástico aprovisionados novos inquilinos. Criar estes recursos assegura que o aprovisionamento novos inquilinos não interfere com a recuperação de inquilinos existentes.

6. Atualiza o novo alias de inquilino para apontar para o servidor para novas bases de dados do inquilino na região de recuperação. Alterar este alias assegura que as bases de dados para os novos inquilinos são aprovisionados na região de recuperação.
        
7. Aprovisiona os servidores e conjuntos elásticos na região de recuperação para restaurar as bases de dados do inquilino. Estes servidores e os conjuntos são uma imagem de espelho da configuração na região original. Aprovisionamento agrupamentos adiantado reserva-se a capacidade necessária para restaurar todas as bases de dados.

    Uma falha numa região poderá colocar pressão significativo em recursos disponíveis na região emparelhado. Se confiar na georrestauro para DR, em seguida, é recomendada a reservar recursos rapidamente. Considere a georreplicação se é fundamental que uma aplicação é recuperada numa região específica. 

8. Permite que o ponto de final do Gestor de tráfego para a aplicação web na região de recuperação. Ativar este ponto final permite que a aplicação aprovisionar novos inquilinos. Nesta fase, os inquilinos existentes são ainda offline.

9. Submete lotes de pedidos de restauro de bases de dados por ordem de prioridade. 

    * Lotes estão organizados de modo a que são restaurar bases de dados em paralelo em todos os agrupamentos.  

    * Os pedidos de restauro são submetidas no modo assíncrono, pelo que são submetidos rapidamente e colocados em fila para execução em cada agrupamento.

    * Porque os pedidos de restauro são processados em paralelo em todos os agrupamentos, é melhor distribuir inquilinos importantes entre vários conjuntos. 

10. Monitoriza o serviço de base de dados SQL para determinar quando são restaurar bases de dados. Após o restauro de uma base de dados do inquilino, está marcado no catálogo online e é registada uma soma rowversion para a base de dados do inquilino. 

    * Bases de dados de inquilino podem ser acedidos pela aplicação, assim que estiver marcadas no catálogo online.

    * A soma dos valores de rowversion na base de dados do inquilino é armazenada no catálogo. Este soma atua como uma identificação digital que permite que o processo de repatriation para determinar se a base de dados foi atualizada na região de recuperação.       

## <a name="run-the-recovery-script"></a>Execute o script de recuperação

> [!IMPORTANT]
> Este tutorial restaura as bases de dados de cópias de segurança georredundante. Embora estas cópias de segurança estão normalmente disponíveis dentro de 10 minutos, pode demorar até uma hora. O script interrompe até que estiverem disponíveis.

Imagine houver uma falha na região na qual a aplicação é implementada e execute o script de recuperação:

1. No ISE do PowerShell, no script ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina o valor seguinte:

    $DemoScenario = 2: recuperar a aplicação para uma região de recuperação restaurando de cópias de segurança georredundante.

2. Para executar o script, selecione F5.  

    * O script abre-se numa nova janela do PowerShell e, em seguida, inicia um conjunto de tarefas do PowerShell que são executadas em paralelo. Estas tarefas restaurar servidores, os conjuntos e as bases de dados para a região de recuperação.

    * A região de recuperação é a região emparelhada associada com a região do Azure no qual implementou a aplicação. Para obter mais informações, consulte [Azure emparelhado regiões](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. Monitorize o estado do processo de recuperação na janela do PowerShell.

    ![Processo de recuperação](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Para explorar o código para as tarefas de recuperação, reveja os scripts do PowerShell na pasta ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\RecoveryJobs.

## <a name="review-the-application-state-during-recovery"></a>Reveja o estado da aplicação durante a recuperação
Enquanto o ponto final da aplicação estiver desativado no Traffic Manager, a aplicação não está disponível. O catálogo é restaurado e todos os inquilinos estão marcados offline. O ponto final da aplicação na região de recuperação, em seguida, é ativado e a aplicação está novamente online. Apesar da aplicação está disponível, os inquilinos aparecem como offline no hub de eventos até as bases de dados são restauradas. É importante criar uma aplicação para processar as bases de dados do inquilino offline.

* Depois da base de dados de catálogo que tenha sido recuperada, mas antes dos inquilinos estiverem novamente online, atualize o hub de eventos de pedidos de Wingtip no seu browser.

    * No rodapé, tenha em atenção que o nome do servidor de catálogo tem agora um - sufixo de recuperação e está localizado na região de recuperação.

    * Tenha em atenção que os inquilinos que ainda não foram restaurados estão marcados como offline e não sejam selecionáveis.   
 
    ![Processo de recuperação](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Se abrir a página de eventos de um inquilino diretamente ao inquilino estiver offline, a página é apresentada uma notificação de offline de inquilino. Por exemplo, se Contoso Concert Hall estiver offline, tentar abrir http://events.wingtip-dpt.&lt; utilizador&gt;.trafficmanager.net/contosoconcerthall.

    ![Processo de recuperação](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Aprovisionar um novo inquilino na região de recuperação
Mesmo antes de o restauro de bases de dados do inquilino, pode aprovisionar novos inquilinos na região de recuperação. Bases de dados do inquilino novo aprovisionados na região de recuperação são repatriated com as bases de dados recuperados mais tarde.   

1. No ISE do PowerShell, no script ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina a propriedade seguinte:

    $DemoScenario = 3: aprovisionar um novo inquilino na região de recuperação.

2. Para executar o script, selecione F5.

3. A página de eventos Hawthorn Hall abre no browser quando o aprovisionamento estiver concluído. 

    Tenha em atenção que a base de dados Hawthorn Hall está localizada numa região de recuperação.

    ![Hall hawthorn aprovisionados na região de recuperação](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. No browser, atualize a página de hub de eventos de pedidos Wingtip para ver que hawthorn Hall incluídos. 

    Se aprovisionado Hawthorn Hall sem aguardar por outros inquilinos restaurar outros inquilinos poderão ainda estar offline.

## <a name="review-the-recovered-state-of-the-application"></a>Reveja o estado da aplicação recuperado

Quando o processo de recuperação estiver concluída, a aplicação e todos os inquilinos são totalmente funcionais na região de recuperação. 

1. Depois do ecrã na janela de consola do PowerShell indica que todos os inquilinos são recuperados, atualize o hub de eventos. 

    Os inquilinos todos os aparecem online, incluindo ao novo inquilino, Hawthorn Hall.

    ![recuperada e novos inquilinos no hub de eventos](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Clique no Contoso Concert Hall e abra a página de eventos. 

    No rodapé, tenha em atenção que a base de dados está localizada no servidor de recuperação localizado na região de recuperação.

    ![Contoso na região de recuperação](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. No [portal do Azure](https://portal.azure.com), abra a lista de grupos de recursos.  

    Observe o grupo de recursos que implementou, além de recursos de recuperação de grupo, com-sufixo de recuperação. O grupo de recursos de recuperação contém todos os recursos criados durante o processo de recuperação plus novos recursos criados durante a interrupção. 

4. Abra o grupo de recursos de recuperação e tenha em atenção os seguintes itens:

    * As versões de recuperação dos servidores de catálogo e tenants1-sufixo de recuperação. Os restaurada inquilino e de catálogo de bases de dados nestes servidores todas as tem nomes utilizados na região original.

    * O tenants2-dpt -&lt;utilizador&gt;-servidor SQL de recuperação. Este servidor é utilizado para aprovisionar novos inquilinos durante a interrupção.

    * O serviço de aplicações com o nome de eventos-wingtip-dpt -&lt;recoveryregion&gt;-&lt;utilizador&gt;, que é a instância de recuperação da aplicação de eventos.

    ![Contoso recursos na região de recuperação](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Abra o tenants2-dpt -&lt;utilizador&gt;-servidor SQL de recuperação. Repare que contém o hawthornhall de base de dados e o conjunto elástico Pool1. A base de dados hawthornhall está configurado como uma base de dados elástica no conjunto elástico Pool1.

## <a name="change-the-tenant-data"></a>Alterar os dados de inquilino 
Nesta tarefa, atualizar uma das bases de dados restaurada do inquilino. As cópias de processo repatriation restaurar bases de dados que foram alterados para a região original. 

1. No seu browser, encontrar a lista de eventos para Hall de Concert Contoso, percorra os eventos e repare o último evento, é importante que Strauss.

2. No ISE do PowerShell, no script ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina o valor seguinte:

    $DemoScenario = 4: eliminar um evento de um inquilino na região de recuperação.

3. Ao executar o script, selecione F5.

4. Atualize a página de eventos de Contoso Concert Hall (http://events.wingtip-dpt.&lt; utilizador&gt;.trafficmanager.net/contosoconcerthall) e tenha em atenção que o evento é importante que Strauss está em falta.

Neste momento no tutorial, tiver recuperado a aplicação, agora está em execução na região de recuperação. Ter aprovisionado um novo inquilino na região de recuperação e modificar os dados de um dos inquilinos restaurados.  

> [!NOTE]
> Outros tutoriais do exemplo não são concebidos para serem executados com a aplicação no estado de recuperação. Se pretende explorar outros tutoriais, lembre-se de que repatriate primeiro a aplicação.

## <a name="repatriation-process-overview"></a>Descrição geral do processo repatriation

O processo de repatriation reverte a aplicação e respetivas bases de dados para a região original após uma interrupção estiver resolvida.

![Georrestauro repatriation](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

O processo:

1. Para qualquer atividade de restauro em curso e cancela quaisquer pedidos de restauro da base de dados em trânsito ou pendentes.

2. Reactivates nas original região inquilino bases de dados que não foram alterados desde a interrupção. Estas bases de dados incluem aqueles não recuperar ainda e os recuperada, mas não é alterada posteriormente. As bases de dados reactivated são exatamente como último acedidos pelos seus inquilinos.

3. Aprovisiona uma imagem de espelho de agrupamento de servidor e elástico ao novo inquilino na região original. Após a conclusão desta ação, o novo alias de inquilino é atualizado para apontar para este servidor. Atualizar o alias faz com que a integração do inquilino novo ocorrer na região original em vez da região de recuperação.

3. Utiliza a georreplicação para mover o catálogo para a região original da região de recuperação.

4. Configuração de conjunto de atualizações na região original para que sejam consistentes com as alterações efetuadas na região de recuperação durante a interrupção.

5. Cria os servidores necessários e os agrupamentos para alojar quaisquer novas bases de dados criadas durante a interrupção.

6. Utiliza a georreplicação inquilino repatriate restaurada bases de dados que tenham sido atualizados pós-restauro e todas as novas bases de dados inquilino aprovisionados durante a interrupção. 

7. Limpa os recursos criados na região de recuperação durante o processo de restauro.

Para limitar o número de bases de dados do inquilino que necessitam de ser repatriated, os passos 1 a 3 são efetuados retomadas rapidamente.  

Passo 4 só é feito se o catálogo na região de recuperação foi modificado durante a interrupção. O catálogo é atualizado se forem criados novos inquilinos ou se qualquer configuração de base de dados ou o conjunto for alterada na região de recuperação.

É importante que o passo 7 faz com que a mínima interrupção aos inquilinos e não se tenha perdido nenhum dado. Para atingir este objetivo, o processo utiliza a georreplicação.

Antes de cada base de dados de georreplicação, a base de dados correspondente na região original é eliminado. A base de dados na região de recuperação é, em seguida, georreplicação, criar uma réplica secundária na região original. Após a conclusão da replicação, o inquilino está marcado como offline no catálogo, o que interrompe a todas as ligações à base de dados na região de recuperação. A base de dados é, em seguida, a ativação pós-falha, fazendo com que quaisquer pendentes transações processar no secundário, por isso, não há dados perdido. 

Na ativação pós-falha, as funções de base de dados são inversa. Secundário na região original torna-se a base de dados de leitura e escrita primária e a base de dados na região recuperação torna-se uma secundária só de leitura. A entrada de inquilino no catálogo é atualizada para referenciar a base de dados na região original e o inquilino está marcado como online. Neste momento, repatriation da base de dados está concluída. 

As aplicações devem ser escritas com a lógica de repetição para se certificar de que estes voltar a ligar automaticamente quando as ligações sejam interrompidas. Quando utilizarem o catálogo para o Mediador de restabelecimento de ligação, estabelecem a base de dados repatriated na região original. Embora muitas vezes, não é reparado desligará os breve, poderá optar por repatriate bases de dados fora do horário de expediente.

Depois de uma base de dados é repatriated, é possível eliminar a base de dados secundária na região de recuperação. A base de dados na região original, em seguida, depende novamente georrestauro para proteção de DR.

Passo 8, os recursos na região de recuperação, incluindo os servidores de recuperação e agrupamentos, são eliminados.

## <a name="run-the-repatriation-script"></a>Execute o script de repatriation
Vamos imagine a interrupção estiver resolvida e execute o script de repatriation.

Se tiver seguiu o tutorial, o script reactivates imediatamente Fabrikam Jazz Club e Dogwood Dojo na região original porque estão contidos inalterados. Em seguida, repatriates ao novo inquilino Hawthorn Hall e Contoso Concert Hall porque este foi modificado. O script repatriates também o catálogo, que foi atualizado quando Hawthorn Hall foi aprovisionado.
  
1. No ISE do PowerShell, no ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script, certifique-se de que o processo de sincronização do catálogo ainda está em execução na sua instância do PowerShell. Se necessário, reinicie-o através da definição:

    $DemoScenario = 1: iniciar a sincronização do servidor de inquilino, agrupamento e informações de configuração de base de dados para o catálogo.

    Para executar o script, selecione F5.

2.  Em seguida, para iniciar o processo de repatriation, defina:

    $DemoScenario = 5: repatriate a aplicação para sua região original.

    Para executar o script de recuperação numa nova janela do PowerShell, selecione F5. Repatriation demora vários minutos e podem ser monitorizados na janela do PowerShell.

3. Enquanto estiver a executar o script, atualize a página de hub de eventos (http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net).

    Tenha em atenção de que todos os inquilinos estão online e acessível ao longo deste processo.

4. Selecione o Club de Jazz Fabrikam para abri-lo. Se não modificar este inquilino, tenha em atenção de rodapé que o servidor já for revertido para o servidor original.

5. Abrir ou Atualize a página de eventos de Contoso Concert Hall. Tenha em atenção do rodapé, inicialmente, a base de dados é ainda no-servidor de recuperação. 

6. Atualize a página de eventos de Contoso Concert Hall quando termina o processo de repatriation e tenha em atenção que a base de dados está agora na sua região original.

7. Atualize novamente o hub de eventos e abra Hawthorn Hall. Tenha em atenção que a respetiva base de dados também está localizado na região original. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Limpar os recursos de região de recuperação após repatriation
Depois de concluída a repatriation, é seguro eliminar os recursos na região de recuperação. 

> [!IMPORTANT]
> Elimine estes recursos retomadas rapidamente, para parar todos os faturação para os mesmos.

O processo de restauro cria todos os recursos de recuperação de um grupo de recursos de recuperação. O processo de limpeza elimina este grupo de recursos e remove todas as referências a recursos do catálogo. 

1. No ISE do PowerShell, no script ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina:
    
    $DemoScenario = 6: eliminar recursos obsoletos da região de recuperação.

2. Para executar o script, selecione F5.

Depois de limpar os scripts, a aplicação está de volta, em que foi iniciado. Neste momento, pode executar novamente o script ou experimente outros tutoriais.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>Conceber a aplicação para garantir que a aplicação e a base de dados localizadas conjuntamente 
A aplicação foi concebida para ligar sempre a partir de uma instância na mesma região que a base de dados do inquilino. Esta conceção reduz a latência entre a aplicação e a base de dados. Esta otimização assume que a interação da aplicação-para-base de dados é chattier que a interação do utilizador para aplicações.  

Bases de dados de inquilino poderão ser distribuídos por recuperação e regiões originais durante algum tempo durante repatriation. Para cada base de dados, a aplicação procura a região na qual a base de dados está localizada efetuando uma pesquisa DNS no nome do servidor de inquilino. Na base de dados do SQL Server, o nome do servidor é um alias. O nome do servidor de um alias contém o nome de região. Se a aplicação não estiver na mesma região que a base de dados, será redirecionado para a instância na mesma região que o servidor de base de dados. Redirecionar para a instância na mesma região que a base de dados minimiza a latência entre a aplicação e a base de dados.  

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:
> [!div class="checklist"]

>* Utilize o catálogo de inquilino para armazenar informações de configuração atualizados periodicamente, o que permite que um ambiente de recuperação da imagem espelhada a serem criadas noutra região.
>* Recupere bases de dados SQL do Azure para a região de recuperação utilizando georrestauro.
>* Atualize o catálogo de inquilino para refletir as localizações de base de dados restaurada do inquilino. 
>* Utilize um alias de DNS para ativar uma aplicação ligar ao catálogo de inquilino ao longo sem uma reconfiguração.
>* Utilize a georreplicação para repatriate bases de dados recuperados para as respetivas região original após uma interrupção estiver resolvida.

Repita o [recuperação após desastre para uma aplicação SaaS multi-inquilino, utilizar a base de dados-georreplicação](saas-dbpertenant-dr-geo-replication.md) tutorial para saber como utilizar a georreplicação para reduzir significativamente o tempo necessário para recuperar uma aplicação multi-inquilino em grande escala.

## <a name="additional-resources"></a>Recursos adicionais

[Tutoriais adicionais que tirar partido da aplicação Wingtip SaaS](https://docs.microsoft.com/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
