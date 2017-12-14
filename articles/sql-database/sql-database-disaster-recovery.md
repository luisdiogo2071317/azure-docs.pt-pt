---
title: "Recuperação após desastre de base de dados SQL | Microsoft Docs"
description: "Saiba como recuperar uma base de dados a partir de uma falha de centro de dados regionais ou falha com a SQL Database do Azure Active Directory georreplicação e capacidades de restauro de georreplicação."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 4800960e-3f9d-40ce-9e55-fb7f2784c067
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 224c0b9f12595ec6cdc65e3d397fb62dba504d06
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Restaurar um SQL Database do Azure ou a ativação pós-falha para uma secundária
Base de dados SQL do Azure oferece as seguintes capacidades para recuperar a partir de uma falha:

* [Grupos de replicação geográfica e ativação pós-falha Active Directory](sql-database-geo-replication-overview.md)
* [Georrestauro](sql-database-recovery-using-backups.md#point-in-time-restore)

Para saber mais sobre cenários de continuidade do negócio e as funcionalidades destes cenários de suporte, consulte [continuidade do negócio](sql-database-business-continuity.md).

> [!NOTE]
> Se estiver a utilizar ou agrupamentos de bases de dados Premium com redundância de zona, o processo de recuperação é um processo automatizado o resto deste material, não sendo aplicável. 

### <a name="prepare-for-the-event-of-an-outage"></a>Preparar para o evento de indisponibilidade
Para o êxito da recuperação noutra região de dados através de grupos de ativação pós-falha ou as cópias de segurança georredundante, terá de preparar um servidor no Centro de dados noutro interrupção para se tornar o novo servidor primário deve a necessidade de surgir, bem como tem bem definidos passos documentados e testar para garantir uma recuperação sem problemas. Estes passos de preparação incluem:

* Identifica o servidor lógico noutra região para se tornar o novo servidor primário. Para georrestauro, este geralmente será um servidor no [região emparelhado](../best-practices-availability-paired-regions.md) para a região onde está localizada a base de dados. Isto irá eliminar o custo de tráfego adicionais durante as operações de restauro de georreplicação.
* Identificar e, opcionalmente, defina, as regras de firewall ao nível do servidor necessárias para os utilizadores acederem a nova base de dados primária.
* Determine como vai para redirecionar os utilizadores para o novo servidor primário, tal como alterando as cadeias de ligação ou alterando as entradas de DNS.
* Identifique e, opcionalmente, criar, os inícios de sessão que tem de estar presente na base de dados mestra no novo servidor primário e certifique-se de que estes inícios de sessão tem as permissões adequadas na base de dados mestra, se aplicável. Para obter mais informações, consulte [segurança da base de dados do SQL Server após a recuperação de desastre](sql-database-geo-replication-security-config.md)
* Identifica as regras de alertas que terão de ser atualizado para mapear para a nova base de dados primária.
* A configuração da auditoria na base de dados primária atual do documento
* Efetuar uma [exercício de recuperação após desastre](sql-database-disaster-recovery-drills.md). Para simular a uma falha do restauro georreplicação, pode eliminar ou mudar o nome da base de dados de origem para fazer com que a falha de conectividade da aplicação. Para simular indisponibilidade através de grupos de ativação pós-falha, pode desativar a aplicação web ou a máquina virtual ligada à base de dados ou a ativação pós-falha da base de dados para provocar falhas de conectividade da aplicação.

## <a name="when-to-initiate-recovery"></a>Quando iniciar a recuperação
A operação de recuperação tem impacto sobre a aplicação. É também exige a alteração a cadeia de ligação do SQL Server ou o redirecionamento com DNS e pode resultar em perda de dados permanente. Por conseguinte, deve ser feita apenas quando a interrupção estiver provável até ao último há mais do que o objetivo de tempo de recuperação da sua aplicação. Quando a aplicação for implementada para produção deve efetuar a monitorização regular do Estado de funcionamento de aplicação e utilizar os seguintes pontos de dados para que a recuperação é garantida de asserção:

1. Falha de conectividade permanente da camada de aplicação para a base de dados.
2. O portal do Azure mostra um alerta sobre um incidente na região com impacto abrangente.

> [!NOTE]
> Se estiver a utilizar grupos de ativação pós-falha e escolher a ativação pós-falha automática, o processo de recuperação é automática e transparente para a aplicação. 

Dependendo da sua tolerância de aplicação para o período de indisponibilidade e possíveis responsabilidade de negócio pode considerar as seguintes opções de recuperação.

Utilize o [obter base de dados recuperáveis](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) para obter o ponto de restauro georreplicação mais recente.

## <a name="wait-for-service-recovery"></a>Aguarde pela recuperação de serviço
O trabalho do Azure equipas diligently para restaurar a disponibilidade do serviço como rapidamente quanto possível, mas, dependendo de raiz com que pode demorar horas ou dias.  Se a sua aplicação pode tolerar o período de indisponibilidade significativo que simplesmente pode aguardar a recuperação concluir. Neste caso, não é necessária qualquer ação da sua parte. Pode ver o estado atual do serviço no nosso [Dashboard de estado de funcionamento do serviço de Azure](https://azure.microsoft.com/status/). Após a recuperação da região de disponibilidade da aplicação será restaurada.

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>Ativação pós-falha para o servidor secundário georreplicação no grupo de ativação pós-falha
Se o período de indisponibilidade da sua aplicação pode resultar em responsabilidade de empresas devem estar a utilizar grupos de ativação pós-falha. -Irá ativar a aplicação para restauro rápido disponibilidade numa região diferente em caso de indisponibilidade. Saiba como [configurar grupos de ativação pós-falha](sql-database-geo-replication-portal.md).

Para restaurar a disponibilidade das bases de dados tem de iniciar a ativação pós-falha para o servidor secundário utilizando um dos métodos suportados.

Utilize um dos seguintes guias para efetuar a ativação pós-falha um georreplicação base de dados secundária:

* [Ativação pós-falha para um servidor secundário georreplicação no portal do Azure](sql-database-geo-replication-portal.md)
* [Efetuar a ativação pós-falha para o servidor secundário com o PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)

## <a name="recover-using-geo-restore"></a>Recuperar utilizando georrestauro
Se o período de indisponibilidade da sua aplicação não resulta numa responsabilidade de empresas podem utilizar [georrestauro](sql-database-recovery-using-backups.md) como um método para recuperar as bases de dados de aplicação. Cria uma cópia da base de dados de cópia de segurança georredundante mais recente.

## <a name="configure-your-database-after-recovery"></a>Configurar a base de dados após a recuperação
Se estiver a utilizar georrestauro para recuperar a partir de uma falha, tem de se certificar de que a conectividade com as novas bases de dados está configurada corretamente para que a função de normal da aplicação pode ser retomada. Esta é uma lista de verificação de tarefas para preparar a produção de base de dados recuperada.

### <a name="update-connection-strings"></a>Atualizar cadeias de ligação
Porque a base de dados recuperada irá residir num servidor diferente, terá de atualizar a cadeia de ligação da sua aplicação para apontar para esse servidor.

Para obter mais informações sobre como alterar cadeias de ligação, consulte a linguagem de desenvolvimento adequado para a sua [biblioteca de ligação](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Configurar regras de Firewall
Tem de certificar-se de que as regras de firewall configuradas no servidor e na base de dados correspondem às que foram configurados no servidor primário e base de dados primária. Para obter mais informações, consulte [como: configurar as definições da Firewall (base de dados do Azure SQL)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Configurar utilizadores de base de dados e inícios de sessão
Tem de certificar-se de que todos os inícios de sessão utilizados pela sua aplicação existem no servidor que está a alojar a base de dados recuperada. Para obter mais informações, consulte [configuração de segurança a georreplicação](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Deve configurar e testar as regras de firewall do servidor e inícios de sessão (e as respetivas permissões) durante um exercício de recuperação após desastre. Estes objetos ao nível do servidor e a respetiva configuração poderão não estar disponíveis durante a interrupção.
> 
> 

### <a name="setup-telemetry-alerts"></a>Configurar alertas de telemetria
Tem de certificar-se de que as definições de regra de alerta existentes são atualizadas para mapear para a base de dados recuperada e o outro servidor.

Para obter mais informações sobre regras de alerta de base de dados, consulte [receber notificações de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) e [controlar serviço de integridade](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Ativar a auditoria
Se a auditoria é necessária para aceder à sua base de dados, terá de ativar a auditoria após a recuperação de base de dados. Para obter mais informações, consulte [auditoria de base de dados](sql-database-auditing.md).

## <a name="next-steps"></a>Passos seguintes
* Para saber mais sobre o SQL do Azure, base de dados automática de cópias de segurança, consulte [cópias de segurança automatizadas de base de dados SQL](sql-database-automated-backups.md)
* Para saber mais sobre cenários de design e de recuperação de continuidade do negócio, consulte [cenários de continuidade](sql-database-business-continuity.md)
* Para saber mais sobre a utilização de cópias de segurança automatizadas para recuperação, consulte [restaurar uma base de dados de cópias de segurança iniciou o serviço](sql-database-recovery-using-backups.md)

