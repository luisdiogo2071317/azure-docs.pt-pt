---
title: Ativação pós-falha de grupos e a georreplicação ativa - base de dados SQL do Azure | Documentos da Microsoft
description: Utilizar grupos de ativação pós-falha automática com a georreplicação ativa e ativar a ativação pós-falha de autoomatic em caso de interrupção.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 2c744866bdec3ebc3ebb336d42c2b837fc888149
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39093129"
---
# <a name="overview-failover-groups-and-active-geo-replication"></a>Descrição geral: Grupos de ativação pós-falha e georreplicação ativa
Georreplicação ativa permite-lhe configurar até quatro legíveis bases de dados secundárias nos locais de centro de dados iguais ou diferentes (regiões). Bases de dados secundárias estão disponíveis para consulta e ativação pós-falha, se ocorrer uma indisponibilidade do Centro de dados ou a incapacidade de ligar à base de dados primária. A ativação pós-falha tem de ser iniciada manualmente pela aplicação do utilizador. Após a ativação pós-falha, a nova principal tem um ponto de final de ligação diferente. 

> [!NOTE]
> Replicação geográfica activa está disponível para todas as bases de dados em todos os escalões de serviço em todas as regiões.
>  

Grupos de ativação pós-falha automática da base de dados SQL do Azure é uma funcionalidade de base de dados SQL concebida para gerir automaticamente a relação de replicação geográfica, conectividade e ativação pós-falha em escala. Com ele, os clientes obtém a capacidade de recuperar automaticamente de várias bases de dados relacionados na região secundária após falhas catastróficas de regionais ou outros eventos não planeados que resultam na perda total ou parcial de disponibilidade do serviço de base de dados SQL no região primária. Além disso, podem utilizar as bases de dados secundárias legíveis para a descarga de cargas de trabalho só de leitura.  Uma vez que os grupos de ativação pós-falha automática envolvem várias bases de dados, tem de ser configurados no servidor primário. Os servidores primários e secundários têm de estar na mesma subscrição. Grupos de ativação pós-falha automática suportam a replicação de todas as bases de dados no grupo de apenas um servidor secundário numa região diferente. Replicação geográfica activa, sem grupos de ativação pós-falha automática, permite até quatro bases de dados secundárias em qualquer região.

Se estiver a utilizar georreplicação ativa e por qualquer motivo a falha da base de dados primária ou simplesmente precisam de ficar offline, pode iniciar ativação pós-falha para qualquer uma das suas bases de dados secundários. Quando a ativação pós-falha é ativada para uma das bases de dados secundárias, todas as outras bases de dados secundárias são automaticamente associados para a nova principal. Se estiver a utilizar grupos de ativação pós-falha automática para gerir a recuperação de base de dados e de qualquer falha que afeta uma ou várias das bases de dados os resultados de grupo na ativação pós-falha automática. Pode configurar a política de ativação pós-falha automática que melhor atenda às necessidades da sua aplicação, ou pode optar por sair e usar a ativação manual. Além disso, os grupos de ativação pós-falha automática fornecem leitura / escrita e pontos finais de serviço de escuta só de leitura que permanecem inalterados durante as ativações pós-falha. Se usa a ativação de ativação pós-falha manual ou automática, ativação pós-falha muda todas as bases de dados secundárias no grupo principal. Depois de concluída a ativação pós-falha de base de dados, o registo DNS é atualizado automaticamente para redirecionar os pontos finais para a nova região.

Pode gerir a replicação e ativação pós-falha de uma base de dados individual ou um conjunto de bases de dados num servidor ou num conjunto elástico com georreplicação ativa. Pode fazê-lo 

- O [portal do Azure](sql-database-geo-replication-portal.md)
- [PowerShell: Base de dados individual](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Conjunto elástico](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Grupo de ativação pós-falha](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: Base de dados ou conjunto elástico](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API: Base de dados](/rest/api/sql/replicationlinks/failover)
- [REST API: Grupo de ativação pós-falha](/rest/api/sql/failovergroups/failover). 
 
Após a ativação pós-falha, certifique-se de que os requisitos de autenticação para o seu servidor e base de dados são configurados na nova principal. Para obter detalhes, consulte [segurança de base de dados SQL após a recuperação após desastre](sql-database-geo-replication-security-config.md). Isto aplica-se tanto a grupos de georreplicação e ativação pós-falha automática Active Directory.

Tira partido da georreplicação ativa a [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) ler de tecnologia do SQL Server para a replicação assíncrona de transações consolidadas na base de dados primária para uma base de dados secundário com o isolamento de instantâneo confirmada (RCSI). Os grupos de ativação pós-falha automática fornecem a semântica de grupo por cima de georreplicação ativa, mas o mesmo mecanismo de replicação assíncrona é utilizado. Embora num determinado período, a base de dados secundária pode ser um pouco por trás da base de dados primária, os dados secundários são garantidos que nunca terá transações parciais. Redundância entre regiões habilita aplicativos para recuperar rapidamente de uma perda permanente de todo o datacenter ou partes de um datacenter causado por desastres naturais, catastróficos erros humanos ou atos mal-intencionados. Os dados específicos de RPO que podem ser encontrados em [descrição geral da continuidade do negócio](sql-database-business-continuity.md).

A figura seguinte mostra um exemplo de georreplicação ativa configurada com um site primário na região e.u.a. Centro-Norte e secundárias na região Centro-Sul.

![relação de replicação geográfica](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Como as bases de dados secundárias legíveis, pode servir para a descarga de cargas de trabalho de só de leitura como tarefas de relatório. Se estiver a utilizar georreplicação ativa, é possível criar a base de dados secundário na mesma região com o principal, mas não aumenta maior resiliência o aplicativo às falhas catastróficas. Se estiver a utilizar grupos de ativação pós-falha automática, a sua base de dados secundária é sempre criado numa região diferente.

Além de desastre, recuperação georreplicação ativa pode ser usada nos seguintes cenários:

* **Migração de base de dados**: pode utilizar a georreplicação ativa para migrar uma base de dados de um servidor para outro online com tempo de inatividade mínimo.
* **As atualizações de aplicações**: pode criar um secundário extra como uma cópia de back-falhar durante as atualizações de aplicações.

Para alcançar a continuidade do negócio real, a adição de redundância da base de dados entre datacenters é apenas uma parte da solução. Recuperação de uma aplicativo (serviço)-a-ponto após uma falha catastrófica requer a recuperação de todos os componentes que constituem o serviço e quaisquer serviços dependentes. Exemplos desses componentes incluem o software de cliente (por exemplo, um navegador com um JavaScript personalizada), o front-ends web, o armazenamento e o DNS. É fundamental que todos os componentes são resilientes a falhas do mesmo e fiquem disponíveis dentro de objetivo de tempo de recuperação (RTO) da sua aplicação. Portanto, precisa identificar todos os serviços dependentes e compreender as garantias e capacidades que fornecem. Em seguida, tem de seguir passos adequados para garantir que as suas funções de serviço durante a ativação pós-falha dos serviços do qual depende. Para obter mais informações sobre as soluções de conceção para recuperação após desastre, veja [conceber soluções de Cloud para recuperação de desastres usando replicação geográfica activa](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Capacidades de georreplicação ativa
A funcionalidade de georreplicação ativa proporciona as seguintes capacidades essenciais:
* **A replicação assíncrona automática**: só pode criar uma base de dados secundária ao adicionar a base de dados existente. A secundária pode ser criada em qualquer servidor de base de dados do Azure SQL. Depois de criado, a base de dados secundária é preenchido com os dados copiados da base de dados primária. Este processo é conhecido como seeding. Depois de base de dados secundária foi criado e implantado, as atualizações para a base de dados primário são replicadas de forma assíncrona a base de dados secundária automaticamente. Replicação assíncrona significa que as transações são consolidadas no banco de dados primário antes de eles serem replicados para a base de dados secundária. 
* **Bases de dados secundárias legíveis**: uma aplicação pode aceder a uma base de dados secundária para operações só de leitura, usando os princípios de segurança de idêntica ou diferente usados para acessar o banco de dados primário. As bases de dados secundários operam no modo de isolamento do instantâneo para garantir a replicação das atualizações dos principais (repetição do log) não é atrasada por consultas executadas no secundário.

   > [!NOTE]
   > A repetição do log está atrasada no banco de dados secundário se existem atualizações de esquema na primária. A segunda opção requer um bloqueio de esquema na base de dados secundário. 
   > 

* **Vários secundários legíveis**: duas ou mais bases de dados secundárias aumentam a redundância e a nível de proteção para a base de dados primária e a aplicação. Se existirem várias bases de dados secundários, o aplicativo permanece protegido mesmo que uma das bases de dados secundários falhar. Se há apenas uma base de dados secundária, e este falhar, o aplicativo é exposto ao risco mais alto até que seja criada uma nova base de dados secundário.

   > [!NOTE]
   > Se estiver a utilizar georreplicação ativa para criar uma aplicação distribuída globalmente e tem de fornecer acesso só de leitura aos dados em mais de quatro regiões, pode criar secundário de uma secundária (um processo conhecido como encadeamento). Desta forma, que pode atingir dimensionamento praticamente ilimitado de replicação de base de dados. Além disso, o encadeamento reduz a sobrecarga da replicação da base de dados primário. A desvantagem é o desfasamento de aumento nas bases de dados mais folha secundários. 
   >

* **Suporte de bases de dados do conjunto elástico**: cada réplica pode participar em separado de um conjunto elástico ou não estar em qualquer conjunto elástico em todos os. A opção de agrupamento para cada réplica está separada e não dependem da configuração de qualquer outra réplica (seja primário ou secundário). Cada conjunto elástico está contido numa única região, portanto, várias réplicas na mesma topologia nunca podem partilhar um conjunto elástico.
* **Nível de desempenho configurável de base de dados secundária**: bases de dados primários e secundários têm de ter a mesma camada de serviço. Uma base de dados secundária pode ser criada com o nível de desempenho inferior (DTUs) que o primário. Esta opção não é recomendada para aplicações com a atividade de escrita de base de dados elevada porque o desfasamento de maior aumenta o risco de perda de dados substancial após uma ativação pós-falha. Além disso, após a ativação pós-falha do desempenho do aplicativo é afetado até que a nova principal é atualizada para um nível de desempenho mais elevado. O gráfico de percentagem de e/s de registo no portal do Azure fornece uma boa forma de estimar o nível de desempenho mínimos da secundária que é necessário para sustentar a carga de replicação. Por exemplo, se a sua base de dados primária é P6 (1000 DTUS) e o respetivo registo de % de e/s é de 50% secundário tem de ser, pelo menos, P4 (500 DTU). Também pode obter os dados de e/s de registo usando [resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) ou [DM db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) vistas de base de dados.  Para obter mais informações sobre os níveis de desempenho da base de dados SQL, consulte [quais são os escalões de serviço de base de dados SQL](sql-database-service-tiers.md). 
* **Ativação pós-falha controlada pelo utilizador e a reativação pós-falha**: uma base de dados secundária pode explicitamente de mudar para a função primária em qualquer altura o aplicativo ou o utilizador. Durante uma falha real a opção "não planeada" deve ser usada, que promove imediatamente uma secundária para ser o primário. Quando o primário falhou recupera e estiver novamente disponível, o sistema marca primário como um secundário recuperado automaticamente e colocá-lo atualizado com a nova principal. Devido à natureza assíncrona de replicação, uma pequena quantidade de dados pode ser perdida durante as ativações pós-falha não planeadas se falhar de um site primário antes de ser replicado as alterações mais recentes para o secundário. Quando um site primário com várias bases de dados secundárias falha, o sistema é automaticamente reconfigura as relações de replicação e ligações restantes secundários para primária recentemente promovida sem exigir qualquer intervenção do utilizador. Assim que a falha que causou a ativação pós-falha é minimizada, talvez seja desejável para a aplicação volte para a região primária. Para tal, o comando de ativação pós-falha deve ser invocado com a opção "planeada". 
* **Mantendo credenciais e as regras de firewall em sincronia**: Recomendamos que utilize [regras de firewall de base de dados](sql-database-firewall-configure.md) para georreplicado bases de dados para que estas regras podem ser replicadas com a base de dados para garantir que todas as bases de dados secundárias têm o regras de firewall do mesmo como principal. Essa abordagem elimina a necessidade dos clientes manualmente configurar e manter regras de firewall nos servidores que alojam as bases de dados primárias e secundárias. Da mesma forma, usando [utilizadores de base de dados contidos](sql-database-manage-logins.md) para dados de acesso garante que as bases de dados primários e secundários têm sempre a mesma as credenciais de utilizador durante uma ativação pós-falha, não há nenhum interrupções devido a incompatibilidades com inícios de sessão e palavras-passe. Com a adição da [do Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), os clientes podem gerir o acesso de utilizador para bases de dados primárias e secundárias e eliminando a necessidade de gerenciamento de credenciais em bases de dados completamente.

## <a name="auto-failover-group-capabilities"></a>Capacidades do grupo de ativação pós-falha automática

Funcionalidade de grupos de ativação pós-falha automática fornece uma abstração eficaz de georreplicação ativa, suporte de replicação de nível de grupo e a ativação pós-falha automática. Além disso, ele remove a necessidade de alterar a cadeia de ligação de SQL após a ativação pós-falha, fornecendo os pontos finais de serviço de escuta adicionais. 

* **Grupo de ativação pós-falha**: um ou vários grupos de ativação pós-falha podem ser criados entre dois servidores em diferentes regiões (servidores primário e secundário). Cada grupo pode conter uma ou várias bases de dados que são recuperados como uma unidade no caso de todos ou alguns bases de dados primárias ficam indisponíveis devido a uma falha na região primária.  
* **Servidor primário**: um servidor que aloja as bases de dados primárias do grupo de ativação pós-falha.
* **Servidor secundário**: um servidor que aloja as bases de dados secundárias no grupo de ativação pós-falha. O servidor secundário não pode estar na mesma região que o servidor principal.
* **Adicionar bases de dados ao grupo de ativação pós-falha**: pode colocar vários bancos de dados dentro de um servidor ou dentro de um conjunto elástico no mesmo grupo de ativação pós-falha. Se adicionar uma base de dados autónoma para o grupo, este cria automaticamente uma base de dados secundária, usando a mesma edição e o nível de desempenho. Se a base de dados primária num conjunto elástico, secundário é criado automaticamente no conjunto elástico com o mesmo nome. Se adicionar uma base de dados que já tenha uma base de dados secundária no servidor secundário, essa replicação geográfica é herdada pelo grupo.

   > [!NOTE]
   > Ao adicionar uma base de dados que já tenha uma base de dados secundário num servidor que não faz parte do grupo de ativação pós-falha, um secundário novo é criado no servidor secundário. 
   >

* **Serviço de escuta de leitura / escrita de grupo de ativação pós-falha**: registo CNAME no DNS um formado como  **&lt;nome do grupo de ativação pós-falha&gt;. database.windows.net** que aponta para o URL do servidor primário atual. Ele permite que os aplicativos de SQL de leitura / escrita de forma transparente voltar a ligar à base de dados primária quando primário é alterado após a ativação pós-falha. 
* **Escuta de só de leitura do grupo de ativação pós-falha**: registo CNAME no DNS um formado como  **&lt;nome do grupo de ativação pós-falha&gt;. secondary.database.windows.net** que aponta para o URL do servidor secundário. Ele permite que os aplicativos de SQL só de leitura ligar de forma transparente para a base de dados secundária com as regras de balanceamento de carga especificadas. Opcionalmente, pode especificar se pretende que o tráfego de só de leitura para ser redirecionado automaticamente para o servidor primário quando o servidor secundário não está disponível.
* **Política de ativação pós-falha automática**: por predefinição, o grupo de ativação pós-falha é configurado com uma política de ativação pós-falha automática. O sistema aciona ativação pós-falha, assim que a falha é detectada. Se pretender controlar o fluxo de trabalho de ativação pós-falha da aplicação, pode desativar a ativação pós-falha automática. 
* **Ativação pós-falha manual**: pode iniciar ativação pós-falha manualmente a qualquer momento, independentemente da configuração de ativação pós-falha automática. Se não estiver configurada a política de ativação pós-falha automática, a ativação pós-falha manual é necessário para recuperar bases de dados no grupo de ativação pós-falha. Pode iniciar a ativação pós-falha forçada ou amigável (com a sincronização de dados completa). A última opção pode ser utilizada para reposicionar o servidor Active Directory para a região primária. Quando concluir a ativação pós-falha, os registos DNS são automaticamente atualizados para garantir a conectividade ao servidor correto.
* **Período de tolerância de perda de dados**: uma vez que as bases de dados primárias e secundárias são sincronizadas com a replicação assíncrona, a ativação pós-falha pode resultar em perda de dados. Pode personalizar a política de ativação pós-falha automática para refletir a tolerância de seu aplicativo à perda de dados. Configurando **GracePeriodWithDataLossHours**, pode controlar o tempo que o sistema aguarda antes de iniciar a ativação pós-falha que é provável que a perda de dados de resultado. 

   > [!NOTE]
   > Quando o sistema detetar que os bancos de dados no grupo ainda estão online (por exemplo, a falha apenas afetado o plano de controlo de serviço), ela imediatamente é ativada a ativação pós-falha com a sincronização de dados completa (amigável ativação pós-falha), independentemente do valor definido pelo  **GracePeriodWithDataLossHours**. Esse comportamento garante que não existe nenhuma perda de dados durante a recuperação. O período de tolerância entra em vigor apenas quando uma ativação pós-falha amigável não é possível. Se a falha é atenuada antes do período de tolerância expirar, a ativação pós-falha não está ativada.
   >

* **Vários grupos de ativação pós-falha**: pode configurar vários grupos de ativação pós-falha para o mesmo par de servidores para controlar a escala de ativações pós-falha. Cada grupo faz a ativação pós-falha independentemente. Se a sua aplicação multi-inquilino utiliza conjuntos elásticos, pode utilizar esta capacidade misturar bases de dados primários e secundários em cada agrupamento. Dessa forma pode reduzir o impacto de uma indisponibilidade para apenas metade dos inquilinos.

## <a name="best-practices-of-building-highly-available-service"></a>Melhores práticas de criação do serviço de elevada disponibilidade

Para criar um serviço de elevada disponibilidade que utiliza a base de dados SQL do Azure, deve seguir estas diretrizes:

- **Grupo de ativação pós-falha de utilização**: um ou vários grupos de ativação pós-falha podem ser criados entre dois servidores em diferentes regiões (servidores primário e secundário). Cada grupo pode conter uma ou várias bases de dados que são recuperados como uma unidade no caso de todos ou alguns bases de dados primárias ficam indisponíveis devido a uma falha na região primária. O grupo de ativação pós-falha cria a base de dados geo-secundária com o mesmo objetivo de serviço como principal. Se adicionar uma relação de georreplicação existente para o grupo de ativação pós-falha, certifique-se de que GEO-secundária está configurado com o mesmo objetivo de nível de serviço como principal.
- **Utilizar o serviço de escuta de leitura / escrita para a carga de trabalho OLTP**: quando executar operações de OLTP, utilize  **&lt;nome do grupo de ativação pós-falha&gt;. database.windows.net** que o servidor de URL e as ligações são direcionado automaticamente para o primário. Este URL não é alterada após a ativação pós-falha. Tenha em atenção de que a ativação pós-falha envolve a atualizar que o registo DNS para que as ligações de cliente são redirecionadas para a nova principal apenas depois do cache DNS do cliente é atualizado.
- **Utilizar o serviço de escuta só de leitura para a carga de trabalho só de leitura**: Se tiver uma isolada logicamente só de leitura carga de trabalho que forem tolerantes a determinados limitada de dados, pode utilizar a base de dados secundária no aplicativo. Para sessões de só de leitura, utilize  **&lt;nome do grupo de ativação pós-falha&gt;. secondary.database.windows.net** que o servidor o URL e a ligação é automaticamente direcionado para o secundário. Também é recomendável que indique na cadeia de ligação ler intenção através de **ApplicationIntent = só de leitura**. 
- **Esteja preparado para degradação de desempenho**: decisão de ativação pós-falha SQL é independente do restante do aplicativo ou outros serviços utilizados. O aplicativo pode ser "misturar" com alguns componentes numa região e alguns em outro. Para evitar a degradação, certifique-se a implementação da aplicação redundante na região DR e siga as diretrizes neste artigo. Tenha em atenção a aplicação na região DR não tem de utilizar uma cadeia de ligação diferente.  
- **Preparar para a perda de dados**: se for detetada uma falha, o SQL aciona automaticamente ativação pós-falha de leitura e escrita se houver perda de dados para o melhor dos nossos dados de conhecimento. Caso contrário, ele aguarda o período especificado pelo **GracePeriodWithDataLossHours**. Se tiver especificado **GracePeriodWithDataLossHours**, estar preparado para perda de dados. Em geral, durante as falhas, o Azure favorece disponibilidade. Se não podem estar perda de dados, certifique-se definir **GracePeriodWithDataLossHours** para um número grande o suficiente, por exemplo, 24 horas. 

> [!IMPORTANT]
> Conjuntos elásticos com DTUs 800 ou menos e mais de 250 bancos de dados com georreplicação poderão encontrar problemas, incluindo já as ativações pós-falha planeadas e degradação do desempenho.  Esses problemas têm maior probabilidade de ocorrer para cargas de trabalho intensivas de escrita, quando os pontos finais de georreplicação amplamente são separados por geografia, ou quando vários pontos de extremidade secundários são utilizados para cada base de dados.  Os sintomas desses problemas são indicados quando o desfasamento de georreplicação aumenta ao longo do tempo.  Este atraso pode ser monitorizado com [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Se ocorrer estes problemas, atenuações incluem aumento do número de DTUs do conjunto, ou reduzir o número de bases de dados georreplicada no mesmo conjunto.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Atualizar ou fazer downgrade uma base de dados primária
Pode atualizar ou mudar a versão de uma base de dados principal para um nível de desempenho diferente (dentro da mesma camada de serviço) sem desligar a quaisquer bases de dados secundários. Ao atualizar, recomendamos que Atualize primeiro o banco de dados secundário e, em seguida, atualizar o primário. Ao fazer downgrade, inverter a ordem: mudar para a versão principal primeiro e, em seguida, mudar o secundário. Quando atualizar ou mudar para a versão da base de dados para um escalão de serviço diferente, esta recomendação é imposta. 

> [!NOTE]
> Se criou a base de dados secundária como parte da configuração do grupo de ativação pós-falha não é recomendado para mudar para a versão da base de dados secundário. Isso é para garantir que sua camada de dados tem capacidade suficiente para processar a carga de trabalho regular depois de ativação pós-falha está ativado. 
>

## <a name="preventing-the-loss-of-critical-data"></a>Impedir a perda de dados críticos
Devido a alta latência das redes de longa distância, a cópia contínua utiliza um mecanismo de replicação assíncrona. Replicação assíncrona faz alguma perda de dados inevitável se ocorrer uma falha. No entanto, alguns aplicativos podem exigir sem perda de dados. Para proteger estas atualizações críticas, um desenvolvedor de aplicativos pode chamar o [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) procedimento de sistema imediatamente após a confirmação da transação. A invocar **sp_wait_for_database_copy_sync** bloqueia o thread de chamada até que a última transação ocorrida foi transmitida para a base de dados secundário. No entanto, não espere para as transações transmitidas para serem reproduzidos e confirmada no secundário. **sp_wait_for_database_copy_sync** tem um âmbito para uma ligação de cópia contínua específico. Qualquer utilizador com os direitos de ligação para base de dados primária pode chamar este procedimento.

> [!NOTE]
> **sp_wait_for_database_copy_sync** impede que a perda de dados após a ativação pós-falha, mas não garante a sincronização completa para acesso de leitura. O atraso causado por um **sp_wait_for_database_copy_sync** chamada de procedimento pode ser significativa e depende do tamanho do registo de transação no momento da chamada. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Gerir programaticamente os grupos de ativação pós-falha e a georreplicação ativa
Como discutido anteriormente, grupos de ativação pós-falha automática e o Active Directory georreplicação também pode ser gerida através de programação com o Azure PowerShell e a API REST. As tabelas seguintes descrevem o conjunto de comandos disponíveis.

**A API do Azure Resource Manager e a segurança baseada em funções**: replicação geográfica activa inclui um conjunto de APIs do Azure Resource Manager para a gestão, incluindo o [API REST da base de dados SQL do Azure](https://docs.microsoft.com/rest/api/sql/) e [Azure Cmdlets do PowerShell](https://docs.microsoft.com/powershell/azure/overview). Essas APIs requerem a utilização de grupos de recursos e suportam a segurança baseada em funções (RBAC). Para obter mais informações sobre como implementar funções de acesso, consulte [controlo de acesso](../role-based-access-control/overview.md).

## <a name="manage-sql-database-failover-using-transact-sql"></a>Gerir a ativação pós-falha de base de dados SQL com o Transact-SQL

| Comando | Descrição |
| --- | --- |
| [Alterar base de dados (banco de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Utilize o argumento de servidor para adicionar SECUNDÁRIO ON para criar uma base de dados secundária para uma base de dados existente e começa a replicação de dados |
| [Alterar base de dados (banco de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Utilize a ativação pós-falha ou FORCE_FAILOVER_ALLOW_DATA_LOSS para mudar de uma base de dados secundária para primária para iniciar a ativação pós-falha |
| [Alterar base de dados (banco de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Utilize remover no servidor SECUNDÁRIO para terminar uma replicação de dados entre uma base de dados SQL e a base de dados secundária especificada. |
| [sys.geo_replication_links (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Devolve informações sobre todas as ligações de replicação existente para cada base de dados no servidor lógico de base de dados do Azure SQL. |
| [sys.dm_geo_replication_link_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Obtém a última vez que a replicação, última desfasamento e outras informações sobre a ligação de replicação para uma determinada base de dados do SQL. |
| [dm operation_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Mostra o estado de todas as operações de banco de dados, incluindo o estado das ligações de replicação. |
| [sp_wait_for_database_copy_sync (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |faz com que o aplicativo esperar até que todas as transações consolidadas são replicadas e confirmadas pela base de dados secundário Active Directory. |
|  | |

## <a name="manage-sql-database-failover-using-powershell"></a>Gerir a ativação pós-falha de base de dados SQL com o PowerShell

| Cmdlet | Descrição |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Obtém uma ou mais bases de dados. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Cria uma base de dados secundária para uma base de dados existente e começa a replicação de dados. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Muda uma base de dados secundária para primária, para iniciar a ativação pós-falha. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Termina a replicação de dados entre uma Base de Dados SQL e a base de dados secundária especificada. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Obtém as ligações de georreplicação entre uma Base de Dados SQL do Azure e um grupo de recursos ou o SQL Server. |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Este comando cria um grupo de ativação pós-falha e regista-o nos servidores primário e secundários|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Remove o grupo de ativação pós-falha do servidor e elimina todas as bases de dados secundárias incluído o grupo |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Obtém a configuração do grupo de ativação pós-falha |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Modifica a configuração do grupo de ativação pós-falha |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Ativação pós-falha de acionadores de grupo de ativação pós-falha para o servidor secundário |
|  | |

> [!IMPORTANT]
> Para scripts de exemplo, consulte [configurar e ativação pós-falha de uma base de dados com georreplicação ativa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [configurar e ativação pós-falha de uma base de dados agrupada com a georreplicação ativa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), e [ Configurar e ativação pós-falha de uma ativação pós-falha de grupo para uma base de dados](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

## <a name="manage-sql-database-failover-using-the-rest-api"></a>Gerir a ativação pós-falha de base de dados SQL com a API REST
| API | Descrição |
| --- | --- |
| [Criar ou atualizar banco de dados (createMode = restauro)](/rest/api/sql/Databases/CreateOrUpdate) |Cria, atualiza ou restaura a base de dados secundário ou de um site primário. |
| [Começar a criar ou atualizar o estado da base de dados](/rest/api/sql/Databases/CreateOrUpdate) |Devolve o estado durante uma operação de criação. |
| [Definir a base de dados secundária como principal (ativação pós-falha planeada)](/rest/api/sql/replicationlinks/failover) |Conjuntos de qual banco de dados de réplica é principal ao realizar a ativação pós-falha da base de dados de réplica primária atual. |
| [Definir a base de dados secundária como principal (ativação pós-falha não planeada)](/rest/api/sql/replicationlinks/failoverallowdataloss) |Conjuntos de qual banco de dados de réplica é principal ao realizar a ativação pós-falha da base de dados de réplica primária atual. Esta operação poderá resultar em perda de dados. |
| [Obter a ligação de replicação](/rest/api/sql/replicationlinks/get) |Obtém uma ligação de replicação específicos para um determinado banco de dados do SQL numa parceria de replicação geográfica. Obtém as informações visíveis na vista de catálogo sys.geo_replication_links. |
| [Ligações de replicação - lista pela base de dados](/rest/api/sql/replicationlinks/listbydatabase) | Obtém todas as ligações de replicação para uma determinada base de dados do SQL numa parceria de replicação geográfica. Obtém as informações visíveis na vista de catálogo sys.geo_replication_links. |
| [Eliminar ligação de replicação](/rest/api/sql/databases/delete) | Elimina uma ligação de replicação de base de dados. Não pode ser feito durante a ativação pós-falha. |
| [Criar ou atualizar o grupo de ativação pós-falha](/rest/api/sql/failovergroups/createorupdate) | Cria ou atualiza um grupo de ativação pós-falha |
| [Eliminar grupo de ativação pós-falha](/rest/api/sql/failovergroups/delete) | Remove o grupo de ativação pós-falha do servidor |
| [Ativação pós-falha (planeada)](/rest/api/sql/failovergroups/failover) | Efetua a ativação pós-falha do servidor primário atual para este servidor. |
| [Permitir a ativação pós-falha forçada perda de dados](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |aflige através do servidor primário atual para este servidor. Esta operação poderá resultar em perda de dados. |
| [Obter grupo de ativação pós-falha](/rest/api/sql/failovergroups/get) | Obtém um grupo de ativação pós-falha. |
| [Lista de grupos de ativação pós-falha por servidor](/rest/api/sql/failovergroups/listbyserver) | Lista os grupos de ativação pós-falha num servidor. |
| [Grupo de ativação pós-falha de atualização](/rest/api/sql/failovergroups/update) | Atualiza um grupo de ativação pós-falha. |
|  | |

## <a name="next-steps"></a>Passos Seguintes
* Para scripts de exemplo, consulte:
   - [Configurar e efetuar a ativação pós-falha de uma base de dados através de georreplicação ativa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [Configurar e efetuar a ativação pós-falha de uma base de dados agrupada através de georreplicação ativa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Configurar e grupo de ativação pós-falha, uma ativação pós-falha para uma base de dados](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* Para uma visão geral de continuidade de negócio e cenários, consulte [descrição geral da continuidade de negócio](sql-database-business-continuity.md)
* Para saber mais sobre SQL do Azure, base de dados automatizada de cópias de segurança, consulte [cópias de segurança automatizadas de base de dados SQL](sql-database-automated-backups.md).
* Para saber mais sobre a utilização de cópias de segurança automatizadas para recuperação, veja [restaurar uma base de dados a partir de cópias de segurança iniciadas pelo serviço](sql-database-recovery-using-backups.md).
* Para saber mais sobre os requisitos de autenticação para um novo servidor primário e a base de dados, veja [segurança de base de dados SQL após a recuperação após desastre](sql-database-geo-replication-security-config.md).

