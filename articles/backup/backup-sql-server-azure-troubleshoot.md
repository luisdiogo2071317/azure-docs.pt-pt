---
title: Guia para VMs de SQL Server de resolução de problemas de Backup do Azure | Microsoft Docs
description: Informações de resolução de problemas para cópia de segurança das VMs de SQL Server para o Azure.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 1c87382c2aae70b022fb391f80f7c75b0a4e5fe6
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296212"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Resolver problemas de cópia de segurança do SQL Server no Azure

Este artigo fornece informações de resolução de problemas de proteção de VMs de SQL Server no Azure (pré-visualização).

## <a name="public-preview-limitations"></a>Limitações de pré-visualização públicas

Para ver as limitações de pré-visualização pública, consulte o artigo [cópia de segurança da base de dados do SQL Server no Azure](backup-azure-sql-database.md#public-preview-limitations).

## <a name="sql-server-permissions"></a>Permissões do SQL Server

Para configurar a proteção para uma base de dados do SQL Server numa máquina virtual, o **AzureBackupWindowsWorkload** extensão tem de ser instalada em que a máquina virtual. Se receber o erro, **UserErrorSQLNoSysadminMembership**, significa que a instância do SQL Server não tem as permissões de cópia de segurança necessárias. Para corrigir este erro, siga os passos no [definir as permissões para as VMs do SQL Server de tipo não mercado](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

## <a name="troubleshooting-errors"></a>Resolução de problemas de erros

Utilize as informações nas tabelas seguintes para resolver problemas e erros encontrados ao proteger o SQL Server para o Azure.

## <a name="backup-failures"></a>Falhas de cópia de segurança

As tabelas seguintes estão organizadas por código de erro.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Esta base de dados do SQL Server não suporta o tipo de cópia de segurança de pedido. | Ocorre quando o modelo de recuperação de base de dados não permite que o tipo de cópia de segurança de pedido. O erro pode ocorrer nas seguintes situações: <br/><ul><li>Uma base de dados através de um modelo de recuperação simples não permite a cópia de segurança do registo.</li><li>As cópias de segurança do registo e valor diferencial não são permitidas para um modelo global de base de dados.</li></ul>Para obter mais detalhes, consulte o [modelos de recuperação do SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) documentação. | Se falhar a cópia de segurança do registo para a base de dados no modelo de recuperação simples, experimente uma destas opções:<ul><li>Se a base de dados está no modo de recuperação simples, desative as cópias de segurança do registo.</li><li>Utilize o [documentação do SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) alterar o modelo de recuperação de base de dados para completo ou registadas em massa. </li><li> Se não pretender alterar o modelo de recuperação e tiver uma política padrão para criar cópias de segurança várias bases de dados que não podem ser alteradas, ignore o erro. As cópias de segurança completas e diferenciais irão funcionar por agenda. As cópias de segurança do registo serão ignoradas, que é esperado neste caso.</li></ul>Se se tratar de um modelo global de base de dados e de configurou utilização de cópia de segurança, registo ou valor diferencial qualquer um dos seguintes passos:<ul><li>Utilize o portal para alterar a agenda de cópia de segurança para o mestre de base de dados, como Full.</li><li>Se tiver uma política padrão para criar cópias de segurança várias bases de dados que não podem ser alteradas, ignore o erro. A cópia de segurança completa irá funcionar por agenda. As cópias de segurança do registo ou valor diferencial não acontecem, o que é esperado neste caso.</li></ul> |
| Operação cancelada porque uma operação em conflito já estava a ser executado na mesma base de dados. | Consulte o [entrada de blogue sobre a cópia de segurança e restaurar limitações](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) que são executados em simultâneo.| [Utilize o SQL Server Management Studio (SSMS) para monitorizar as tarefas de cópia de segurança.](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms) Depois da operação em conflito falha, reinicie a operação.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Base de dados SQL não existe. | A base de dados foi eliminado ou mudado. | <ul><li>Verifique se a base de dados foi acidentalmente eliminado ou mudado.</li><li>Se a base de dados tiver sido eliminado acidentalmente, para continuar a cópias de segurança, restaure a base de dados para a localização original.</li><li>Se eliminar a base de dados e não precisa de cópias de segurança futuras, em seguida, no cofre dos serviços de recuperação, clique em [parar a cópia de segurança com "Eliminar/manter dados"](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms).</li>|

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Cadeia de registos está danificada. | A base de dados ou a VM é feita utilizando a solução de cópia de segurança outra, o que trunca a cadeia de registos.|<ul><li>Verifique se outra solução de cópia de segurança ou de script está a ser utilizado. Se assim for, pare a solução de cópia de segurança. </li><li>Se a cópia de segurança foi uma cópia de segurança do registo do ad-hoc, acione uma cópia de segurança completa para iniciar uma nova cadeia de registos. Para cópias de segurança do registo agendada, é necessária nenhuma ação porque o serviço de cópia de segurança do Azure será automaticamente acionada uma cópia de segurança completa para corrigir este problema.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Cópia de segurança do Azure não é possível estabelecer ligação à instância do SQL Server. | Cópia de segurança do Azure não consegue ligar à instância do SQL Server. | Utilize os detalhes adicionais no menu do erro portal do Azure para restringir as causas raiz. Consulte [SQL cópia de segurança de resolução de problemas](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) para corrigir o erro.<br/><ul><li>Se as predefinições de SQL não permitir ligações remotas, altere as definições. Consulte o abaixo as ligações para alterar as definições.<ul><li>[https://msdn.microsoft.com/library/bb326495.aspx](https://msdn.microsoft.com/library/bb326495.aspx)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Se existirem problemas de início de sessão, consulte o abaixo as ligações para o corrigir:<ul><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Primeira cópia de segurança completa está em falta para esta origem de dados. | Cópia de segurança completa está em falta para a base de dados. Registo e valor diferencial principal de cópias de segurança para uma cópia de segurança completa, por isso cópias de segurança completas devem ser colocadas antes de acionar diferenciais ou cópias de segurança de registo. | Acione uma cópia de segurança completa de ad-hoc.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Não é possível fazer cópia de segurança à medida que registo de transações para a origem de dados está completo. | O espaço de registo transacional de base de dados está completo. | Para corrigir este problema, consulte o [documentação do SQL Server](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| Esta base de dados do SQL Server não suporta o tipo de cópia de segurança de pedido. | Réplicas secundárias sempre AG não suportam cópias de segurança completas e diferenciais. | <ul><li>Se acionada uma cópia de segurança ad-hoc, acione as cópias de segurança no nó principal.</li><li>Se a cópia de segurança foi agendada pela política, certifique-se que o nó principal está registado. Para registar o nó [siga os passos para detetar uma base de dados do SQL Server](backup-azure-sql-database.md#discover-sql-server-databases).</li></ul> | 

## <a name="restore-failures"></a>Restaurar falhas

Os seguintes códigos de erro são apresentados quando a falha das tarefas de restauro.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite 

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Base de dados com o mesmo nome já existe na localização de destino | O destino de restauro de destino já tem uma base de dados com o mesmo nome.  | <ul><li>Altere o nome de base de dados de destino</li><li>Em alternativa, utilizar a opção de substituição de imposição na página de restauro</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Falha no restauro porque não foi possível colocar offline a base de dados. | Ao efetuar um restauro, a base de dados de destino tem de ser colocados offline. Cópia de segurança do Azure não é capaz de colocar offline estes dados. | Utilize os detalhes adicionais no menu do erro portal do Azure para restringir as causas raiz. Para obter mais informações, consulte o [documentação do SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |


###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Não é possível localizar o certificado de servidor com o thumbprint no destino. | O mestre de base de dados na instância de destino não tem um thumbprint de encriptação válidos. | Importe o thumbprint de certificado válido utilizado na instância de origem, para a instância de destino. |

## <a name="registration-failures"></a>Falhas de registo

Os códigos de erro seguintes destinam-se a falhas de registo.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError 

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Preferência de cópia de segurança para sempre no grupo de disponibilidade SQL não pode ser satisfeita como alguns nós do grupo de disponibilidade não estão registados. | Nós necessários para efetuar cópias de segurança forem inacessíveis ou não estão registados. | <ul><li>Certifique-se de que todos os nós necessários para efetuar cópias de segurança desta base de dados registados e bom estado de funcionamento e, em seguida, repita a operação.</li><li>Preferência de cópia de segurança de alteração sempre no grupo de disponibilidade SQL.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| No VM do SQL server está a encerrar e não está acessível ao serviço de cópia de segurança do Azure. | VM está a ser encerrada | Certifique-se de que o SQL server está em execução. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Mensagem de erro | Causas possíveis | Ação recomendada |
|---|---|---|
| Serviço de cópia de segurança do Azure utiliza o agente convidado da VM do Azure para fazer a cópia de segurança, mas o agente convidado não está disponível no servidor de destino. | O agente convidado não está ativado ou está danificado | [Instalar o agente convidado da VM](../virtual-machines/extensions/agent-windows.md) manualmente. |

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre o Backup do Azure para VMs de SQL Server (pré-visualização pública), consulte [cópia de segurança do Azure para as VMs do SQL Server (pré-visualização pública)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).