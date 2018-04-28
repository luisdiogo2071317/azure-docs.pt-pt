---
title: Recuperação após desastre para aplicações SaaS utilizando cópias de segurança georredundante SQL Database do Azure | Microsoft Docs
description: Saiba como utilizar as cópias de segurança georredundante SQL Database do Azure para recuperar uma aplicação de SaaS multi-inquilino em caso de indisponibilidade
keywords: tutorial de base de dados sql
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: a677e6eb583e293f83df824804aa4cd6f8f5d778
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="recover-a-multi-tenant-saas-application-using-geo-restore-from-database-backups"></a>Recuperar uma aplicação de SaaS multi-inquilino utilizando georrestauro de cópias de segurança da base de dados

Neste tutorial, explore um cenário de recuperação de desastre completa para uma aplicação de SaaS multi-inquilino implementado utilizando o modelo de base de dados por inquilino. Utilizar [ _georrestauro_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-recovery-using-backups) para recuperar as bases de dados de inquilino e de catálogo de cópias de segurança georredundante manter automaticamente para uma região de recuperação alternativo. Após a interrupção estiver resolvida, utilize [ _georreplicação_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) para repatriate bases de dados alterados para as respetivas região original.

![arquitetura de restauro georreplicação](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Georrestauro é a solução de recuperação de desastre do menor custo da base de dados do SQL Server.  No entanto, o restauro de cópias de segurança georredundante pode resultar na perda de dados de uma hora e pode demorar um tempo considerável, dependendo do tamanho de cada base de dados. **Para recuperar as aplicações com o menor RPO possíveis e RTO, utilizar a georreplicação em vez de georrestauro**.

Este tutorial explicar restauro e repatriation fluxos de trabalho. Saiba como:
> [!div class="checklist"]

>* Base de dados de sincronização e informações de configuração do conjunto elástico para o catálogo de inquilino
>* Configurar um ambiente de imagem espelhada numa região 'recuperação', que inclui a aplicação, servidores e agrupamentos    
>* Recuperar bases de dados catálogo e de inquilino utilizando _georrestauro_
>* Repatriate o catálogo de inquilino e bases de dados alterados inquilino utilizando _georreplicação_ após a interrupção estiver resolvida
>* O catálogo de atualização, como cada base de dados restaurada (ou repatriated) para controlar a localização atual do Active Directory de cópia da base de dados de cada inquilino
>* Certifique-se de que a base de dados de aplicação e de inquilino são sempre colocalizados na mesma região do Azure para reduzir a latência  
 

Antes de começar este tutorial, certifique-se que os pré-requisitos seguintes são concluídos:
* A base de dados de SaaS de bilhetes Wingtip por inquilino aplicação é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a base de dados de SaaS de bilhetes Wingtip por aplicação do inquilino](saas-dbpertenant-get-started-deploy.md)  
* O Azure PowerShell está instalado. Para obter mais detalhes, veja [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Introdução ao padrão de recuperação georrestauro

Recuperação após desastre (DR) é uma consideração importante para muitas aplicações, para motivos de conformidade ou de continuidade do negócio. Deve existir uma interrupção do serviço prolongado, um plano de DR bem preparado pode minimizar a interrupção de negócios. Plano de DR da com base no georrestauro tem realizar vários objetivos:
 * Capacidade de reserva todas as necessária na região de recuperação escolhido mais rapidamente possível para garantir que está disponível para restaurar bases de dados do inquilino.
 * Estabelecer um ambiente de recuperação de imagem espelhada reflete a configuração original do conjunto e base de dados 
 * Tem de ser possível cancelar o processo de restauro em trânsito intermédio se a região original vem voltar a estar online.
 * Ativar o inquilino aprovisionamento rapidamente, para a integração do novo inquilino pode ser reiniciados logo que possível  
 * Otimizado para restaurar os inquilinos na ordem de prioridade
 * Otimizado para obter os inquilinos online logo que possível, siga os passos em paralelo onde práticas
 * Sejam resilientes a falhas, reiniciável e idempotent
 * Repatriate bases de dados para os respetivos região original com um impacto mínimo para os inquilinos quando a interrupção estiver resolvida.  

> [!Note]
> A aplicação é recuperada para o _região emparelhado_ da região na qual a aplicação é implementada. Para obter mais informações, consulte [Azure emparelhado regiões](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).   



Neste tutorial, estes desafios são resolvidos utilizando as funcionalidades da SQL Database do Azure e a plataforma do Azure:

* [Modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), para reservar capacidade necessária de todos os mais rapidamente possível. Modelos Azure Resource Manager são utilizados para Aprovisionar uma imagem de espelho dos servidores originais e conjuntos elásticos na região de recuperação. Um servidor separado e o agrupamento também são criados para o aprovisionamento de novos inquilinos. 
* [Biblioteca de clientes de base de dados elásticas](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) (EDCL) para criar e manter um catálogo de base de dados do inquilino.  O catálogo é expandido para incluir informações de configuração de agrupamento e da base de dados atualizadas periodicamente.
* [Funcionalidades de recuperação de gestão de partições horizontais](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) de EDCL para manter as entradas de localização de base de dados no catálogo durante a recuperação e repatriation.  
* [Georrestauro](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), para recuperar as bases de dados de inquilino e de catálogo de cópias de segurança georredundante manter automaticamente. 
* [Operações de restauro assíncrona](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) enviados por ordem de prioridade de inquilino, que são colocados em fila para cada conjunto pelo sistema e processamento em lotes para o conjunto não está sobrecarregado. Estas operações podem ser canceladas antes ou durante a execução, se necessário.    
* [Replicação geográfica](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), para repatriate bases de dados para a região original após a falha. Utilizar a georreplicação garante que não há nenhuma perda de dados e um impacto mínimo no inquilino.
* [Aliases DNS do SQL server](https://docs.microsoft.com/azure/sql-database/dns-alias-overview) para permitir que o processo de sincronização do catálogo ligar ao catálogo do Active Directory, independentemente da respetiva localização.  

## <a name="get-the-disaster-recovery--scripts"></a>Obter os scripts de recuperação de desastre 

Os scripts de DR utilizados neste tutorial estão disponíveis no [Wingtip bilhetes SaaS a base de dados por inquilino repositório do GitHub](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Veja o [orientações gerais](saas-tenancy-wingtip-app-guidance-tips.md) para obter os passos transferir e os scripts de gestão Wingtip pedidos de desbloqueio.
> [!IMPORTANT]
> Como todos os pedidos de Wingtip gestão scripts, os scripts de DR são qualidade de exemplo e não são deve ser utilizada na produção.   

## <a name="review-the-healthy-state-of-the-application"></a>Reveja o estado de funcionamento da aplicação
Antes de começar o processo de recuperação, reveja o estado de bom estado de funcionamento normal da aplicação.
1. No seu browser, abra o Hub de eventos de pedidos de suporte de Wingtip (http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net - substitua &lt;utilizador&gt; com o valor de utilizador da sua implementação).
    * Desloque-se na parte inferior da página e repare o nome do servidor de catálogo e a localização no rodapé.  A localização é a região no qual implementou a aplicação.    
        Sugestão: Paire o rato sobre a localização para aumentar a apresentação.

    ![Estado de bom estado de funcionamento de hub de eventos na região original](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

1. Clique no inquilino Contoso Concert Hall e abra a página de eventos.
    * No rodapé, tenha em atenção o nome do servidor de inquilinos. A localização será a mesma localização no servidor de catálogo.

    ![Região original contoso Concert Hall](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png)    
1. No [portal do Azure](https://portal.azure.com), reveja e abra o grupo de recursos no qual implementou a aplicação.
    * Tenha em atenção que os recursos e a região na qual os servidores de base de dados do SQL Server e componentes do serviço de aplicações são implementadas.

## <a name="sync-tenant-configuration-into-catalog"></a>Configuração de inquilinos de sincronização no catálogo

Nesta tarefa, inicie um processo de sincronização a configuração de servidores, conjuntos elásticos e bases de dados para o catálogo de inquilino.  Estas informações são posteriormente utilizadas para configurar um ambiente de imagem espelhada na região de recuperação.

> [!IMPORTANT]
> De simplicidade, o processo de sincronização e outros processos de recuperação e repatriation execução longa são implementados nestes exemplos como tarefas de Powershell locais ou sessões que são executados com o início de sessão de utilizador do cliente. Os tokens de autenticação emitidos quando o início de sessão expire após várias horas e, em seguida, as tarefas falhará. Num cenário de produção, os processos de execução longa devem ser implementados como serviços do Azure fiáveis tipo, a ser executado um principal de serviço. Consulte [utilize o Azure PowerShell para criar um principal de serviço com um certificado](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. No _ISE do PowerShell_, abra o ficheiro de Modules\UserConfig.psm1 ...\Learning. Substitua `<resourcegroup>` e `<user>` em linhas 10 e 11 com o valor utilizado quando implementou a aplicação.  Guarde o ficheiro!

2. No *ISE do PowerShell*, abra o ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script.
    *  Durante este tutorial, irá executar cada um dos cenários neste PowerShell script, por isso, manter este ficheiro aberto.

3. Defina o seguinte:
    * **$DemoScenario = 1**, iniciar uma tarefa em segundo plano que sincroniza-se o servidor de inquilino e agrupamento de informações de configuração para o catálogo

3. Prima **F5** para executar o script de sincronização. 
    *  Estas informações são posteriormente utilizadas para se certificar de que a recuperação cria uma imagem de espelho de servidores, conjuntos e as bases de dados na região de recuperação.  
![Processo de sincronização](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Deixe a janela do PowerShell em execução em segundo plano e continuar com o resto deste tutorial.

> [!Note]
> O processo de sincronização estabelece ligação ao catálogo de através de um alias de DNS. O alias é modificado durante o restauro e repatriation para apontar para o catálogo de Active Directory. O processo de sincronização mantém o catálogo atualizado com a base de dados ou um conjunto de alterações de configuração efetuadas na região de recuperação.  Durante a repatriation, estas alterações são aplicadas aos recursos equivalentes na região original.

## <a name="geo-restore-recovery-process-overview"></a>Descrição geral do processo de recuperação georrestauro

O processo de recuperação georrestauro implementa a aplicação e restaura as bases de dados de cópias de segurança para a região de recuperação.

O processo de recuperação faz o seguinte:

1. Desativa o ponto final do Gestor de tráfego para a aplicação web na região original. Desativar o ponto final impede que os utilizadores a ligar a aplicação num estado inválido deve a região original ficar online durante a recuperação.

1. Aprovisiona um servidor de catálogo de recuperação na região de recuperação, georreplicação restaura a base de dados do catálogo e atualiza o _activecatalog_ alias para apontar para o servidor de catálogo restaurada.  
    * Alterar o alias de catálogo assegura que o processo de sincronização do catálogo sincroniza-se sempre ao catálogo do Active Directory.

1. Marca todos os inquilinos existentes no catálogo de recuperação como offline para impedir o acesso às bases de dados do inquilino antes de estes são restaurados.

1. Uma instância da aplicação na região de recuperação de Aprovisiona e configura-a para utilizar o catálogo restaurado nessa região.
    * Para manter a latência pelo menos, a aplicação de exemplo foi concebida para que este sempre se liga a uma base de dados do inquilino na mesma região.

1. Aprovisiona um conjunto de servidor e elásticos em que serão aprovisionados novos inquilinos. Criar estes recursos assegura que o aprovisionamento novos inquilinos não interfere com a recuperação de inquilinos existentes.

1. Atualiza o alias do novo inquilino para apontar para o servidor para novas bases de dados do inquilino na região de recuperação. Alterar este alias assegura que as bases de dados para os novos inquilinos são aprovisionados na região de recuperação.
        
1. Aprovisiona os servidores e conjuntos elásticos na região de recuperação para restaurar as bases de dados do inquilino. Estes servidores e os conjuntos são uma imagem de espelho da configuração na região original.  Aprovisionamento agrupamentos prévio reserva-se a capacidade necessária para restaurar todas as bases de dados.
    * Uma falha numa região pode colocar pressão significativo nos recursos disponíveis na região emparelhado.  Se confiar na georrestauro para DR, em seguida, é recomendada a reservar recursos rapidamente. Considere utilizar a georreplicação se de que é fundamental que uma aplicação deve ser recuperada numa região específica. 

1. Permite que o ponto de final do Gestor de tráfego para a aplicação Web na região de recuperação. Ativar este ponto final permite que a aplicação aprovisionar novos inquilinos. Nesta fase, os inquilinos existentes são ainda offline.

1. Submete lotes de pedidos de restauro de bases de dados por ordem de prioridade. 
    * Lotes estão organizados de modo a que são restaurar bases de dados em paralelo em todos os agrupamentos.  
    * Os pedidos de restauro são submetidas no modo assíncrono, pelo que são submetidos rapidamente e colocados em fila para execução em cada agrupamento.
    * Porque os pedidos de restauro são processados em paralelo em todos os agrupamentos, é melhor distribuir inquilinos importantes entre vários conjuntos. 

1. Monitoriza o serviço de base de dados do SQL Server para determinar quando são restaurar bases de dados. Depois de uma base de dados do inquilino é restaurada, está marcado online no catálogo e uma soma rowversion para a base de dados do inquilino é registada. 
    * Bases de dados de inquilino podem ser acedidos pela aplicação, assim que estiver marcadas no catálogo online.
    * A soma dos valores de rowversion na base de dados do inquilino é armazenada no catálogo. Este soma atua como uma identificação digital que permite que o processo de repatriation para determinar se a base de dados tiver sido atualizado na região de recuperação.      

## <a name="run-the-recovery-script"></a>Execute o script de recuperação

> [!IMPORTANT]
> Este tutorial restaura as bases de dados de cópias de segurança georredundante. Embora estas cópias de segurança estão normalmente disponíveis dentro de 10 minutos, pode demorar até uma hora até que estejam disponíveis. O script será colocado em pausa até que estejam disponíveis.  Tempo para obter um café!

Agora imagine houver uma falha na região na qual a aplicação é implementada e execute o script de recuperação:

1. No *ISE do PowerShell*, no ...\Learning script Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina os seguintes valores:
    * **$DemoScenario = 2**, recuperar a aplicação para uma região de recuperação restaurando de cópias de segurança georredundante

1. Prima **F5** para executar o script.  
    * O script abre-se numa nova janela do PowerShell e, em seguida, inicia um conjunto de tarefas do PowerShell que são executadas em paralelo.  Estas tarefas restaurar servidores, os conjuntos e as bases de dados para a região de recuperação. 
    * A região de recuperação é a _região emparelhado_ associada com a região do Azure no qual implementou a aplicação. Para obter mais informações, consulte [Azure emparelhado regiões](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

1. Monitorize o estado do processo de recuperação na janela do PowerShell.

    ![Processo de recuperação](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

>Para explorar o código para as tarefas de recuperação, reveja os scripts do PowerShell na pasta ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\RecoveryJobs.

## <a name="review-the-application-state-during-recovery"></a>Reveja o estado da aplicação durante a recuperação
Enquanto o ponto final da aplicação estiver desativado no Traffic Manager, a aplicação não está disponível. O catálogo é restaurado e todos os inquilinos estão marcados offline.  O ponto final da aplicação na região de recuperação, em seguida, é ativado e a aplicação está novamente online. Apesar da aplicação está disponível, os inquilinos aparecem como offline no hub de eventos até as bases de dados são restauradas. É importante criar uma aplicação para processar as bases de dados do inquilino offline.

1. Depois da base de dados de catálogo que tenha sido recuperada, mas antes dos inquilinos estiverem novamente online, atualize o Hub de eventos de pedidos de suporte de Wingtip no seu browser.
    * No rodapé, tenha em atenção que o nome do servidor de catálogo tem agora um _-recuperação_ sufixo e está localizado na região de recuperação.
    * Tenha em atenção que os inquilinos que ainda não foram restaurados estão marcados como offline e não sejam selecionáveis.   
 
    ![Processo de recuperação](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Se abrir diretamente página de eventos de um inquilino enquanto o inquilino estiver offline, a página é apresentada uma notificação 'inquilino offline'. Por exemplo, se Contoso Concert Hall estiver offline, tentar abrir http://events.wingtip-dpt.&lt; utilizador&gt;.trafficmanager.net/contosoconcerthall ![processo de recuperação](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Aprovisionar um novo inquilino na região de recuperação
Mesmo antes de o restauro de bases de dados do inquilino, pode aprovisionar novos inquilinos na região de recuperação. Bases de dados do inquilino novo aprovisionados na região de recuperação serão mais tarde repatriated com as bases de dados recuperados.   

1. No *ISE do PowerShell*, no ...\Learning script Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina a propriedade seguinte:
    * **$DemoScenario = 3**, aprovisionar um novo inquilino na região de recuperação

1. Prima **F5** para executar o script. 

1. A página de eventos Hawthorn Hall abre no browser quando ter concluído o aprovisionamento. 
    * Tenha em atenção que a base de dados Hawthorn Hall está localizada numa região de recuperação.
    ![Hall hawthorn aprovisionados na região de recuperação](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

1. No browser, atualize a página de Hub de eventos de pedidos de suporte de Wingtip para ver que hawthorn Hall incluídos. 
    * Se aprovisionado Hawthorn Hall sem aguardar por outros inquilinos restaurar outros inquilinos ainda podem estar offline.

## <a name="review-the-recovered-state-of-the-application"></a>Reveja o estado da aplicação recuperado

Quando tiver concluído o processo de recuperação, a aplicação e todos os inquilinos são totalmente funcionais na região de recuperação. 

1. Depois do ecrã na janela de consola do PowerShell indica que todos os inquilinos são recuperados, atualize o Hub de eventos.  Os inquilinos todas aparecerá online, incluindo ao novo inquilino, Hawthorn Hall.

    ![recuperada e novos inquilinos no hub de eventos](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

1. Clique no Contoso Concert Hall e abra a página de eventos.
    * No rodapé, repare que a base de dados está localizada no servidor de recuperação localizado na região de recuperação.

    ![Contoso na região de recuperação](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

1. No [portal do Azure](https://portal.azure.com), abra a lista de grupos de recursos.  
    * Tenha em atenção o grupo de recursos que implementou, juntamente com o grupo de recursos de recuperação com o _-recuperação_ sufixo.  O grupo de recursos de recuperação contém todos os recursos criados durante o processo de recuperação plus novos recursos criados durante a interrupção.  

1. Abra o grupo de recursos de recuperação e tenha em atenção os seguintes itens:
    * As versões de recuperação dos servidores de catálogo e tenants1 com _-recuperação_ sufixo.  Os restaurada inquilino e de catálogo de bases de dados nestes servidores todas as tem nomes utilizados na região original.

    * O _tenants2-dpt -&lt;utilizador&gt;-recuperação_ do SQL Server.  Este servidor é utilizado para aprovisionar novos inquilinos durante a interrupção.
    *   O serviço de aplicações com o nome, _eventos-wingtip-dpt -&lt;recoveryregion&gt;-&lt;utilizador & gt_; que é a instância de recuperação da aplicação de eventos. 

    ![Contoso na região de recuperação](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png)   
    
1. Abra o _tenants2-dpt -&lt;utilizador&gt;-recuperação_ do SQL Server.  Tenha em atenção que contém a base de dados _hawthornhall_ e o conjunto elástico, _Pool1_.  O _hawthornhall_ base de dados é configurada como uma base de dados elástica no _Pool1_ conjunto elástico.

## <a name="change-tenant-data"></a>Dados de alterações de inquilino 
Nesta tarefa, atualizar uma das bases de dados restaurada do inquilino. O processo de repatriation copiará restauradas bases de dados que foram alterados para a região original. 

1. No seu browser, localizar os eventos de listam para Hall de Concert Contoso, percorra os eventos e tome nota do último evento _muito a sério Strauss_.

1. No *ISE do PowerShell*, na ...\Learning script Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1, defina o seguinte valor:
    * **$DemoScenario = 4**, eliminar um evento de um inquilino na região de recuperação
1. Prima **F5** ao executar o script.
1. Atualize a página de eventos de Contoso Concert Hall (http://events.wingtip-dpt.&lt; utilizador&gt;.trafficmanager.net/contosoconcerthall) e tenha em atenção que o evento é importante que Strauss, está em falta.

Neste momento no tutorial, tiver recuperado a aplicação, agora está em execução na região de recuperação.  Ter aprovisionado um novo inquilino na região de recuperação e modificar os dados de um dos inquilinos restaurados.  

> [!Note]
> Outros tutoriais do exemplo não são concebidos para serem executados com a aplicação no estado de recuperação. Se pretende explorar outros tutoriais, lembre-se de que repatriate primeiro a aplicação.

## <a name="repatriation-process-overview"></a>Descrição geral do processo repatriation

O processo de repatriation reverte a aplicação e respetivas bases de dados para a região original após uma interrupção estiver resolvida.

![Georrestauro repatriation](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 


O processo:

1. Para qualquer atividade de restauro em curso e cancela quaisquer pedidos de restauro da base de dados em trânsito ou pendentes.

1. Reactivates nas original região inquilino bases de dados que não foram alterados desde a interrupção.  Isto inclui e bases de dados que não tenham sido recuperadas ainda, que foram recuperados, mas não foram alterados depois disso. As bases de dados reactivated são exatamente como último acedidos pelos seus inquilinos.

1. Aprovisiona uma imagem de espelho novo inquilinos servidor e elástico do conjunto de na região original. Após a conclusão desta ação, o alias do novo inquilino é atualizado para apontar para este servidor. Atualizar o alias faz com que a integração do inquilino novo ocorrer na região original em vez da região de recuperação.

1. Utilizar a georreplicação, move o catálogo para a região original de região de recuperação.

1. Configuração de conjunto de atualizações na região original para que sejam consistentes com as alterações efetuadas na região de recuperação durante a interrupção.

1. Cria o servidor (es) necessário e os agrupamentos de bases de dados novas criadas durante a interrupção do anfitrião.

1. Utilizar a georreplicação, repatriates restaurada inquilino bases de dados que tenham sido atualizados pós-restauro e todos os novos inquilinos bases de dados aprovisionados durante a interrupção. 

1. Limpa os recursos criados na região de recuperação durante o processo de restauro.

Para limitar o número de bases de dados do inquilino que necessitam de ser repatriated, os passos 1 a 3 são efetuados retomadas rapidamente.  

Passo 4 só é feito se o catálogo na região de recuperação foi modificado durante a interrupção. O catálogo é atualizado se forem criados novos inquilinos ou se qualquer configuração de base de dados ou o conjunto for alterada na região de recuperação.

É importante que o passo 7 faz com que a mínima interrupção aos inquilinos e não se tenha perdido nenhum dado. Para atingir este objetivo, o processo utiliza _georreplicação_.

Antes de cada base de dados de georreplicação, a base de dados correspondente na região original é eliminado. A base de dados na região de recuperação é, em seguida, georreplicação, criar uma réplica secundária na região original. Após a conclusão da replicação, o inquilino está marcado como offline no catálogo, o que interrompe a todas as ligações à base de dados na região de recuperação. A base de dados é, em seguida, a ativação pós-falha, fazendo com que quaisquer pendentes transações de serem processadas no secundário para que o se tenha perdido nenhum dado. Na ativação pós-falha, as funções de base de dados são inversa.  Secundário na região original torna-se a base de dados de leitura e escrita primária e a base de dados na região recuperação torna-se uma secundária só de leitura. A entrada de inquilino no catálogo é atualizada para referenciar a base de dados na região original e o inquilino está marcada como online.  Neste momento, repatriation da base de dados está concluída. 

As aplicações devem ser escritas com a lógica de repetição para se certificar de que voltar a ligar automaticamente quando as ligações sejam interrompidas.  Quando estes restabeleça a ligação utilizando o catálogo para mediador a ligação, estabelecem a base de dados repatriated na região original. Apesar de muitas vezes, não é reparado desligará os breve, poderá optar por repatriate bases de dados fora do horário de expediente. 

Depois de uma base de dados é repatriated, é possível eliminar a base de dados secundária na região de recuperação. A base de dados na região original baseia-se, em seguida, no georrestauro para DR novamente a proteção.

Passo 8, os recursos na região de recuperação, incluindo os servidores de recuperação e agrupamentos, são eliminados.

## <a name="run-the-repatriation-script"></a>Execute o script de repatriation
Agora vamos imaginar a interrupção estiver resolvida e execute o script de repatriation.

Se seguiu o tutorial, o script reactivates imediatamente Fabrikam Jazz Club e Dogwood Dojo na região original por estarem inalterados. Em seguida, repatriates ao novo inquilino Hawthorn Hall e Contoso Concert Hall porque este foi modificado. O script repatriates também o catálogo, que foi atualizado quando Hawthorn Hall foi aprovisionado.
  
1. No *ISE do PowerShell*, na ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script.

1. Certifique-se de que o processo de sincronização do catálogo ainda está em execução na sua instância do PowerShell.  Se necessário, reinicie-o através da definição:
    * **$DemoScenario = 1**, iniciar a sincronização do servidor de inquilino, agrupamento e informações de configuração de base de dados para o catálogo
    * Prima **F5** para executar o script.
1.  Em seguida, para iniciar o processo de repatriation, defina:
    * **$DemoScenario = 5**, repatriate a aplicação para sua região original
    * Prima **F5** para executar o script de recuperação numa nova janela do PowerShell.  Repatriation irá demorar alguns minutos e podem ser monitorizados na janela do PowerShell.
1. Enquanto estiver a executar o script, atualize a página de Hub de eventos (http://events.wingtip-dpt.&lt; utilizador&gt;. trafficmanager.net)
    * Tenha em atenção de que todos os inquilinos estão online e acessível ao longo deste processo.
1. Clique em Club de Jazz Fabrikam para abri-lo. Se não foi possível modificar este inquilino, tenha em atenção de rodapé que o servidor já for revertido para o servidor original.
1. Abrir ou Atualize a página de eventos de Contoso Concert Hall e repare do rodapé que a base de dados é ainda no _-recuperação_ servidor inicialmente.  
1. Atualize a página de eventos de Contoso Concert Hall quando concluir o processo de repatriation e repare que a base de dados está agora na sua região original.
1. Atualize novamente o hub de eventos e abra Hawthorn Hall e repare que a respetiva base de dados também está localizado na região original. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Limpar os recursos de região de recuperação após repatriation
Assim que repatriation estiver concluída, é seguro eliminar os recursos na região de recuperação. 

> [!IMPORTANT]
> Elimine estes recursos retomadas rapidamente, para parar todos os faturação para os mesmos.

O processo de restauro cria todos os recursos de recuperação de um grupo de recursos de recuperação. O processo de limpeza irá eliminar este grupo de recursos e remova todas as referências a recursos do catálogo. 

1. No *ISE do PowerShell*, na ...\Learning Modules\Business continuidade e desastre Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script, conjunto de:
    * **$DemoScenario = 6**, eliminar recursos obsoletos da região de recuperação

1. Prima **F5** para executar o script.

Depois de limpar os scripts, a aplicação está de volta, em que foi iniciado.  Foi possível executar novamente o script ou experimente outros tutoriais neste momento.

## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Conceber a aplicação para garantir que a aplicação e a base de dados estão colocalizados 
A aplicação foi concebida para que o se liga-se sempre de uma instância na mesma região que a base de dados do inquilino. Esta conceção reduz a latência entre a aplicação e a base de dados. Esta otimização assume que a interação da aplicação-para-base de dados é chattier que a interação do utilizador para aplicações.  

Bases de dados do inquilino podem ser distribuídos por recuperação e regiões originais durante algum tempo durante repatriation.  Para cada base de dados, a aplicação procura a região na qual a base de dados está localizada efetuando uma pesquisa DNS no nome do servidor de inquilino. Na base de dados do SQL Server, o nome do servidor é um alias. O nome do servidor de um alias contém o nome de região. Se a aplicação não estiver na mesma região que a base de dados, será redirecionado para a instância na mesma região que o servidor de base de dados.  Redirecionar a instância na mesma região que a base de dados minimiza a latência entre aplicações e a base de dados.  

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:
> [!div class="checklist"]

>* Utilizar o catálogo de inquilino para armazenar informações de configuração atualizados periodicamente, permitindo que um ambiente de recuperação da imagem espelhada a serem criadas noutra região
>* Utilize georrestauro para recuperar bases de dados SQL do Azure para a região de recuperação
>* Atualizar o catálogo de inquilino para refletir as localizações de base de dados restaurada do inquilino  
>* Utilize um alias de DNS para ativar uma aplicação ligar ao catálogo de inquilino ao longo sem reconfiguração
>* Utilizar a georreplicação para repatriate bases de dados recuperados para as respetivas região original após uma interrupção estiver resolvida

Agora, repita o [recuperação após desastre para uma aplicação de SaaS multi-inquilino, utilizar a base de dados-georreplicação](saas-dbpertenant-dr-geo-replication.md) para saber como reduzir significativamente o tempo necessário para recuperar uma aplicação em grande escala de multi-inquilino ao utilizar a georreplicação.

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que tirar partido da aplicação Wingtip SaaS](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
