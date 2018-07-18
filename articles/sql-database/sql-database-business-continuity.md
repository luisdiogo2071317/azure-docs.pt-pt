---
title: Continuidade empresarial na nuvem - recuperação de base de dados - Base de Dados SQL | Microsoft Docs
description: Saiba como a Base de Dados SQL do Azure suporta a continuidade empresarial na nuvem e a recuperação de base de dados, e ajuda a manter as aplicações fundamentais na nuvem em execução.
keywords: continuidade empresarial, continuidade empresarial na nuvem, recuperação de base de dados após desastre, recuperação de base de dados
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: On Demand
ms.date: 07/16/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: dfea1587cddbf7440771ca7007928f7e4054f61a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092295"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Descrição geral da continuidade empresarial com a Base de Dados SQL do Azure

Esta descrição geral descreve as capacidades que a Base de Dados SQL do Azure fornece para a continuidade empresarial e a recuperação após desastre. Saiba mais sobre as opções, recomendações e tutoriais para recuperar de eventos disruptivos que podem causar perda de dados ou fazer com que a sua base de dados e a aplicação fiquem indisponíveis. Saiba o que fazer quando um erro de utilizador ou aplicação afeta a integridade dos dados, uma região do Azure fica indisponível ou a aplicação requerer manutenção.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funcionalidades da Base de Dados SQL que pode utilizar para proporcionar continuidade empresarial

A Base de Dados SQL fornece várias funcionalidades de continuidade empresarial, incluindo cópias de segurança automáticas e a replicação opcional da base de dados. Cada uma possui características diferentes para o tempo de recuperação estimado (ERT) e a potencial perda de dados de transações recentes. Assim que compreender estas opções, pode escolher entre elas - e, na maioria dos cenários, utilizá-las em conjunto para cenários diferentes. À medida que elabora o seu plano de continuidade empresarial, tem de saber qual o tempo máximo aceitável antes de a aplicação recuperar totalmente após o evento problemático – este é o objetivo de tempo de recuperação (RTO). Também precisa de saber a quantidade máxima de dados recentes atualizações (intervalo de tempo) da aplicação pode tolerar perder ao recuperar após o evento problemático – este é o objetivo de ponto de recuperação (RPO).

A tabela seguinte compara o ERT e o RPO para cada camada de serviço para os três cenários mais comuns.

| Capacidade | Básica | Standard | Premium  | Fins Gerais | Crítico para a Empresa
| --- | --- | --- | --- |--- |--- |
| Restauro para um Ponto Anterior no Tempo a partir de cópia de segurança |Qualquer ponto de restauro num período de 7 dias |Qualquer ponto de restauro num período de 35 dias |Qualquer ponto de restauro num período de 35 dias |Qualquer ponto de restauro no período configurado (até 35 dias)|Qualquer ponto de restauro no período configurado (até 35 dias)|
| Georrestauro a partir de cópias de segurança georreplicado |ERT < 12h, RPO < 1h |ERT < 12h, RPO < 1h |ERT < 12h, RPO < 1h |ERT < 12h, RPO < 1h|ERT < 12h, RPO < 1h|
| Restaurar a partir do Azure Backup Vault |ERT < 12h, RPO < 1 sem |ERT < 12h, RPO < 1 sem |ERT < 12h, RPO < 1 sem |ERT < 12h, RPO < 1 sem|ERT < 12h, RPO < 1 sem|
| Georreplicação ativa |ERT < 30s, RPO < 5s |ERT < 30s, RPO < 5s |ERT < 30s, RPO < 5s |ERT < 30s, RPO < 5s|ERT < 30s, RPO < 5s|

### <a name="use-point-in-time-restore-to-recover-a-database"></a>Utilize o restauro de ponto no tempo para recuperar uma base de dados

Base de dados SQL efetua automaticamente uma combinação de cópias de segurança da base de dados completa semanalmente, uma vez por hora cópias de segurança de base de dados diferencial e transação backups de log a cada cinco - dez minutos para proteger a sua empresa contra perda de dados. Se estiver a utilizar o [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md), em seguida, estas cópias de segurança são armazenadas no armazenamento RA-GRS durante 35 dias para bases de dados nos escalões de serviço Standard e Premium e 7 dias para bases de dados na camada de serviço básico. Se o período de retenção da camada de serviços não for de encontro aos seus requisitos empresariais, pode aumentar o período de retenção ao [alterar a camada de serviços](sql-database-single-database-scale.md). Se estiver a utilizar o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md), a retenção de cópias de segurança é configurável até uma máximo de 35 dias nos fins gerais e camadas de críticas de negócios. As cópias de segurança completas e diferenciais da base de dados também são replicadas para um [centro de dados emparelhado](../best-practices-availability-paired-regions.md), para proteção contra indisponibilidade do centro de dados. Para obter mais informações, consulte [cópias de segurança da base de dados automática](sql-database-automated-backups.md).

Se o máximo suportado ponto anterior no tempo restaurar (PITR), o período de retenção não é suficiente para a sua aplicação, pode expandi-lo ao configurar uma política de retenção (LTR) de longo prazo para as bases de dados. Para obter mais informações, consulte [cópias de segurança automatizada](sql-database-automated-backups.md) e [retenção de cópia de segurança de longo prazo](sql-database-long-term-retention.md).

Pode utilizar estas cópias de segurança automáticas da base de dados para recuperar uma base de dados após vários eventos problemáticos, tanto no seu centro de dados como para outro centro de dados. Ao utilizar cópias de segurança automáticas de bases de dados, o tempo estimado de recuperação depende de vários fatores, incluindo o número total de bases de dados a recuperar na mesma região ao mesmo tempo, o tamanho da base de dados, o tamanho do registo de transações e a largura de banda de rede. O tempo de recuperação é normalmente inferior a 12 horas. Pode demorar mais tempo para recuperar uma base de dados muito grande ou Active Directory. Para obter mais detalhes sobre o tempo de recuperação, consulte [hora da recuperação de base de dados](sql-database-recovery-using-backups.md#recovery-time). Ao recuperar para outra região de dados, a potencial perda de dados está limitada a 1 hora pelo armazenamento georredundante de cópias de segurança de bases de dados diferenciais de hora a hora.

> [!IMPORTANT]
> Para recuperar através de cópias de segurança automáticas, tem de ser um membro da função de Contribuinte do SQL Server ou o proprietário da subscrição - veja [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md). Pode recuperar através do portal do Azure, do PowerShell ou da API REST. Não é possível utilizar Transact-SQL.
>

Utilize cópias de segurança automáticas como o mecanismo de continuidade empresarial e de recuperação se a aplicação:

* Não for considerada fundamental para a atividade crítica.
* Não tem um SLA de enlace - um período de indisponibilidade de 24 horas ou já não resulta em encargos financeiros.
* Tiver uma taxa reduzida de alteração de dados (poucas transações por hora) e perder até uma hora de alterações é uma perda de dados aceitável.
* For sensível aos custos.

Se precisar de uma recuperação mais rápida, utilize [georreplicação ativa](sql-database-geo-replication-overview.md) (abordada a seguir). Se tiver de ser capaz de recuperar dados de um período com mais de 35 dias, utilize [retenção a longo prazo](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Utilizar grupos Active Directory, georreplicação e ativação pós-falha automática para reduzir o tempo de recuperação e limitar a perda de dados associada a uma recuperação

Além de utilizar cópias de segurança da base de dados para recuperação de base de dados se ocorrer uma interrupção do negócio, pode usar [georreplicação ativa](sql-database-geo-replication-overview.md) para configurar um banco de dados para até quatro secundárias bases de dados legíveis em regiões da sua preferência. Estas bases de dados secundárias são mantidas sincronizadas com a base de dados primária através de um mecanismo de replicação assíncrona. Esta funcionalidade é utilizada para proteger contra interrupções de negócio se ocorrer uma indisponibilidade do Centro de dados ou durante uma atualização da aplicação. Georreplicação ativa também pode ser utilizada para fornecer um melhor desempenho de consulta para consultas só de leitura aos utilizadores geograficamente dispersos.

Para ativar automatizado e ativação pós-falha transparente deve organizar seus bancos de dados de georreplicação em grupos com o [grupo de ativação pós-falha automática](sql-database-geo-replication-overview.md) funcionalidade de base de dados SQL.

Se a base de dados primária fica offline inesperadamente ou se tiver de colocá-la offline para atividades de manutenção, pode promover rapidamente uma secundária para primária (também chamado de ativação pós-falha) e configurar aplicações para ligar para o primário promovido. Se a sua aplicação está a ligar às bases de dados com o serviço de escuta do grupo de ativação pós-falha, não precisa de alterar a configuração de cadeia de ligação SQL após a ativação pós-falha. Com uma ativação pós-falha planeada, não existe nenhuma perda de dados. Com uma ativação pós-falha não planeada, poderá existir uma pequena quantidade de perda de dados para transações muito recentes devido à natureza da replicação assíncrona. Utilizar grupos de ativação pós-falha automática, pode personalizar a política de ativação pós-falha para minimizar a potencial perda de dados. Após uma ativação pós-falha, pode fazer a reativação pós-falha posteriormente - de acordo com um plano ou quando o centro de dados voltar a ficar online. Em todos os casos, os utilizadores passam por um pequeno período de indisponibilidade e têm de restabelecer a ligação.

> [!IMPORTANT]
> Para utilizar a georreplicação ativa e grupos de ativação pós-falha automática, tem de ser o proprietário da subscrição ou ter permissões administrativas no SQL Server. Pode configurar e efetuar a ativação pós-falha utilizando o Azure portal, PowerShell ou a API de REST com permissões de subscrição do Azure ou com o Transact-SQL com permissões do SQL Server.
> 

Utilize grupos de georreplicação e ativação pós-falha automática Active Directory se a sua aplicação cumprir qualquer um dos seguintes critérios:

* É fundamental para a atividade crítica.
* Tem um contrato de nível de serviço (SLA) que não permite 24 horas ou mais de períodos de indisponibilidade.
* Tempo de inatividade pode resultar em encargos financeiros.
* Tem uma taxa elevada de alteração de dados e perder uma hora não é aceitável.
* O custo adicional da georreplicação ativa é inferior aos potenciais encargos financeiros e perda empresarial associada.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Recuperar uma base de dados após um erro de utilizador ou aplicação

Ninguém é perfeito! Um utilizador poderá acidentalmente eliminar alguns dados, inadvertidamente remover uma tabela importante ou até mesmo remover uma base de dados completa. Ou uma aplicação poderá substituir acidentalmente dados corretos por dados incorretos devido a um defeito da aplicação.

Neste cenário, estas são as opções de recuperação.

### <a name="perform-a-point-in-time-restore"></a>Efetuar um restauro para um ponto anterior no tempo
Pode utilizar as cópias de segurança automáticas para recuperar uma cópia da base de dados para um ponto anterior no tempo válido e conhecido, desde que esse ponto esteja dentro do período de retenção da base de dados. Depois de restaurar a base de dados, pode substituir a base de dados original pela base de dados restaurada ou copiar os dados necessários a partir dos dados restaurados para a base de dados original. Se a base de dados utilizar a georreplicação ativa, é recomendável copiar os dados necessários da cópia restaurada para a base de dados original. Se substituir a base de dados original pela base de dados restaurada, terá de reconfigurar e ressincronizar a georreplicação ativa (o que pode demorar algum tempo para uma base de dados grandes). Embora isto restaura uma base de dados para o último ponto disponível no tempo, restaurar geo-secundária para qualquer ponto anterior no tempo não é atualmente suportado.

Para obter mais informações e passos detalhados para restaurar uma base de dados para um ponto anterior no tempo através do portal do Azure ou através do PowerShell, veja [restauro para um ponto anterior no tempo](sql-database-recovery-using-backups.md#point-in-time-restore). Não é possível recuperar com o Transact-SQL.

### <a name="restore-a-deleted-database"></a>Restaurar uma base de dados eliminada
Se a base de dados tiver sido eliminada, mas o servidor lógico não tiver sido eliminado, pode restaurar a base de dados eliminada para o ponto anterior no tempo em que foi eliminada. Isto restaura uma cópia de segurança da base de dados para o mesmo servidor SQL lógico do qual foi eliminada. Pode restaurá-la com o nome original ou fornecer um novo nome para a base de dados restaurada.

Para obter mais informações e passos detalhados para restaurar uma base de dados eliminada através do portal do Azure ou através do PowerShell, veja [restaurar uma base de dados eliminada](sql-database-recovery-using-backups.md#deleted-database-restore). Não é possível restaurar com o Transact-SQL.

> [!IMPORTANT]
> Se o servidor lógico tiver sido eliminado, não é possível recuperar uma base de dados eliminada.


### <a name="restore-backups-from-long-term-retention"></a>Restaurar cópias de segurança de retenção de longa duração

Se a perda de dados ocorreu fora do período de retenção atual para cópias de segurança automáticas e a base de dados está configurada para retenção de longa duração, pode restaurar a partir de uma cópia de segurança completa no armazenamento de LTR para uma nova base de dados. Nesse momento, pode substituir a base de dados original pela base de dados restaurada ou copiar os dados necessários da base de dados restaurada para a base de dados original. Se precisar de obter uma versão antiga da base de dados antes de uma atualização principal da aplicação, responder a um pedido de auditores ou uma ordem legal, que pode criar uma base de dados com uma cópia de segurança completa guardada no Cofre de cópia de segurança do Azure.  Para obter mais informações, veja [Retenção de longa duração](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Recuperar uma base de dados para outra região a partir de uma indisponibilidade do centro de dados regional do Azure
<!-- Explain this scenario -->

Embora seja raro, um centro de dados do Azure pode ficar indisponível. Quando ocorre uma indisponibilidade, esta causa uma interrupção do negócio que poderá demorar apenas alguns minutos ou pode durar horas.

* Uma opção é aguardar que a base de dados volte a ficar online quando a indisponibilidade do centro de dados terminar. Isto funciona para as aplicações que podem dar-se ao luxo de ter a base de dados offline. Por exemplo, um projeto de desenvolvimento ou uma versão de avaliação gratuita nos quais não tem de trabalhar constantemente. Quando um centro de dados fica indisponível, não sabe quanto pode durar a indisponibilidade, pelo que esta opção só funciona se não precisar de sua base de dados durante algum tempo.
* Outra opção é a reprovação ao longo para outra região de dados se estiver a utilizar a georreplicação ativa ou a recuperar uma base de dados com cópias de segurança da base de dados georredundantes (georrestauro). A ativação pós-falha demora apenas alguns segundos enquanto a recuperação de base de dados de cópias de segurança demora horas.

Quando tomar medidas, o período de tempo que demora a recuperar e a perda de dados incorrida depende de como optar por utilizar estas funcionalidades de continuidade de negócio na sua aplicação. Na verdade, pode optar por utilizar uma combinação de cópias de segurança da base de dados e a georreplicação ativa consoante os requisitos da aplicação. Para um debate das considerações de estrutura de aplicações para bases de dados autónomas e para conjuntos elásticos com estas funcionalidades de continuidade empresarial, veja [Estruturar uma aplicação para recuperação após desastre na nuvem](sql-database-designing-cloud-solutions-for-disaster-recovery.md) e [Estratégias de recuperação após desastre do Conjunto Elástico](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

As secções seguintes fornecem uma visão geral dos passos para recuperar através de cópias de segurança da base de dados ou a georreplicação ativa. Para passos detalhados, incluindo o planejamento de requisitos, passos após recuperação e informações sobre como simular uma falha para executar um teste de recuperação após desastre, veja [recuperar uma base de dados SQL a partir de um período de indisponibilidade](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Preparar para uma indisponibilidade
Independentemente da funcionalidade de continuidade empresarial utilizada, tem de:

* Identificar e preparar o servidor de destino, incluindo as regras de firewall ao nível do servidor, os inícios de sessão e as permissões ao nível da base de dados mestra.
* Determinar como redirecionar os clientes e as aplicações de cliente para o novo servidor
* Documentar outras dependências, tais como definições e alertas de auditoria

Se não preparar corretamente, colocar as aplicações online após uma ativação pós-falha ou uma recuperação de base de dados demora mais tempo e provavelmente também exigem solução de problemas numa altura tensa - uma má combinação.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Ativação pós-falha para um georreplicada secundária da base de dados
Se estiver a utilizar grupos de ativação pós-falha automática e de georreplicação ativa como mecanismo de recuperação, pode configurar uma política de ativação pós-falha automática ou utilizar [ativação pós-falha manual](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Uma vez iniciada, a ativação pós-falha faz com que o secundário para a primária e está preparado para registar novas transações e responder a consultas - com perda mínima de dados para os dados ainda não replicadas. Para informações sobre como estruturar o processo de ativação pós-falha, consulte [estruturar uma aplicação para recuperação de desastres em nuvem](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Quando o Centro de dados online novamente as cores primárias antigas restabelecer a ligação para a nova principal automaticamente e tornar-se do bancos de dados secundários. Se precisar de alteração da localização a cópia principal para a região original, pode iniciar manualmente uma ativação pós-falha planeada (reativação pós-falha). 
> 

### <a name="perform-a-geo-restore"></a>Efetuar um georrestauro
Se estiver a utilizar cópias de segurança automáticas com a replicação de armazenamento georredundante como mecanismo de recuperação, [iniciar uma recuperação de base de dados com restauro geográfico](sql-database-disaster-recovery.md#recover-using-geo-restore). A recuperação normalmente decorre no prazo de 12 horas - com perda de dados até uma hora, determinada por quando a última cópia de segurança diferencial de hora a hora foi efetuada e replicada. Até concluir a recuperação, a base de dados não consegue registar quaisquer transações nem responder a consultas. Embora isto restaura uma base de dados para o último ponto disponível no tempo, restaurar geo-secundária para qualquer ponto anterior no tempo não é atualmente suportado.

> [!NOTE]
> Se o Centro de dados estiver online novamente antes de passar seu aplicativo para a base de dados recuperada, pode cancelar a recuperação.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Efetuar a ativação pós-falha/tarefas de recuperação
Após a recuperação a partir de qualquer mecanismo de recuperação, tem de efetuar as seguintes tarefas adicionais antes de os seus utilizadores e aplicações ficarem funcionais:

* Redirecionar os clientes e as aplicações de cliente para o novo servidor e base de dados restaurada
* Certificar-se de que estão implementadas regras de firewall ao nível do servidor adequadas para que os utilizadores liguem (ou utilizar [firewalls ao nível da base de dados](sql-database-firewall-configure.md#creating-and-managing-firewall-rules))
* Certificar-se de que estão implementados inícios de sessão e permissões ao nível da base de dados mestra no local adequados (ou utilizar [utilizadores contidos](https://msdn.microsoft.com/library/ff929188.aspx))
* Configurar auditorias, conforme adequado
* Configurar alertas, conforme adequado

## <a name="upgrade-an-application-with-minimal-downtime"></a>Atualizar uma aplicação com um período de indisponibilidade mínimo
Por vezes, um aplicativo deve ser colocado offline devido a manutenção planeada, como uma atualização da aplicação. [Gerir atualizações de aplicações](sql-database-manage-application-rolling-upgrade.md) descreve como utilizar a georreplicação ativa para permitir a implementação de atualizações da sua aplicação na cloud para minimizar o período de indisponibilidade durante as atualizações e fornecer um caminho de recuperação caso algo corra mal. 

## <a name="next-steps"></a>Passos Seguintes
Para um debate das considerações de estrutura de aplicações para bases de dados autónomas e conjuntos elásticos, veja [Estruturar uma aplicação para recuperação após desastre na nuvem](sql-database-designing-cloud-solutions-for-disaster-recovery.md) e [Estratégias de recuperação após desastre do Conjunto Elástico](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
