---
title: Gerir e monitorizar bases de dados do SQL Server numa VM do Azure, cópias de segurança de cópia de segurança do Azure | Documentos da Microsoft
description: Este artigo descreve como restaurar bancos de dados SQL em execução numa VM do Azure que são submetidos a backup com o Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 1c2ce0ba42f0bc3efd1dcc951113b05ab6941b98
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430802"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Gerir e monitorizar backup de bancos de dados do SQL Server 


Este artigo descreve as tarefas comuns para gerir e monitorizar bases de dados em execução numa VM do Azure que são copiadas para uma cópia de segurança do Azure Recovery Services cofre o [Azure Backup](backup-overview.md) serviço. Tarefas, incluindo tarefas e alertas de monitorização, parar e retomar a proteção de base de dados, executar tarefas de cópia de segurança e anular o registo de uma VM a partir de cópia de segurança.


> [!NOTE]
> Cópia de segurança de bases de dados do SQl Server em execução numa VM do Azure com o Azure Backup está atualmente em pré-visualização pública.


TIf ainda não ainda a cópia de segurança configurada para as bases de dados do SQL Server, siga as instruções em [neste artigo](backup-azure-sql-database.md)

## <a name="monitor-backup-jobs"></a>Monitorizar tarefas de cópia de segurança

###  <a name="monitor-ad-hoc-jobs-in-the-portal"></a>Monitorizar tarefas ad hoc no portal

Cópia de segurança do Azure mostra todas as tarefas manualmente acionadas no **tarefas de cópia de segurança** portal, incluindo a detetar e registar as bases de dados e operações de cópia de segurança e restauro.

![Portal de cópias de segurança](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Tarefas de cópia de segurança agendadas não são apresentadas no **tarefas de cópia de segurança** portal. Utilize SQL Server Management Studio para monitorizar tarefas de cópia de segurança agendadas, conforme descrito na secção seguinte.
>

### <a name="monitor-backup-jobs-with-sql-server-management-studio"></a>Monitorizar trabalhos de cópia de segurança com o SQL Server Management Studio 

O Azure Backup utiliza as APIs nativas do SQL para todas as operações de cópia de segurança.

Utilizar as APIs nativas para obter todas as informações de tarefa do [tabela de backupset SQL](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) na base de dados msdb.

O exemplo seguinte é uma consulta que obtém todas as tarefas de cópia de segurança para uma base de dados com o nome **DB1**. Personalize a consulta para a monitorização avançada.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

## <a name="view-backup-alerts"></a>Ver alertas de cópia de segurança

Porque os backups de log ocorrem a cada 15 minutos, a monitorizar tarefas de cópia de segurança pode ser enfadonho. Cópia de segurança do Azure facilita a monitorização com alertas de e-mail.

- Alertas são acionados para todas as falhas de cópia de segurança.
- Alertas são consolidados ao nível da base de dados por código de erro.
- Um alerta de e-mail é enviado apenas para a primeira falha de cópia de segurança para uma base de dados. 

Para monitorizar alertas de cópia de segurança:

1. Inicie sessão na sua subscrição do Azure no [portal do Azure](https://portal.azure.com) para monitorizar alertas de base de dados.

2. No dashboard do cofre, selecione **alertas e eventos**.

   ![Selecione os alertas e eventos](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Na **eventos e alertas**, selecione **alertas de cópia de segurança**.

   ![Selecione os alertas de cópia de segurança](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Parar a proteção de uma base de dados do SQL Server

É possível parar o backup de uma base de dados do SQL Server de duas formas:

* Parar todas as tarefas de cópia de segurança futuras e eliminar todos os pontos de recuperação.
* Parar todas as tarefas de cópia de segurança futuras, mas deixar os pontos de recuperação intacto.

Tenha em atenção que:

Se deixar os pontos de recuperação, serão possível limpar os pontos de acordo com a política de cópia de segurança. Que incorra em custos para a instância protegida e o armazenamento consumido, até que todos os pontos de recuperação são limpos. [Saiba mais](https://azure.microsoft.com/pricing/details/backup/) sobre os preços.
- Quando sair pontos de recuperação intactos, embora que ocorra de acordo com a política de retenção, cópia de segurança do Azure sempre mantém um último ponto de recuperação até eliminar explicitamente os dados de cópia de segurança.
- Se eliminar uma origem de dados sem parar cópia de segurança, novas cópias de segurança começam a falhar. Novamente, os pontos de recuperação antigos irão expirar acordo com a política, mas um último ponto de recuperação sempre será mantido até parar cópia de segurança e eliminar os dados.
- Não é possível parar cópia de segurança para uma base de dados ativada para a proteção automática, até que a proteção automática está desativada.

Para parar a proteção para uma base de dados:

1. No dashboard do cofre, sob **utilização**, selecione **itens de cópia de segurança**.

    ![Abra o menu de itens de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

2. Na **tipo de gestão de cópia de segurança**, selecione **SQL na VM do Azure**.

    ![Selecione o SQL numa VM do Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)


3. Selecione a base de dados para o qual pretende parar a proteção.

    ![Selecione a base de dados para parar a proteção](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


5. No menu da base de dados, selecione **parar cópia de segurança**.

    ![Selecione parar cópia de segurança](./media/backup-azure-sql-database/stop-db-button.png)


6. Na **parar cópia de segurança** menu, selecione se pretende manter ou eliminar os dados. Opcionalmente, forneça um motivo e o comentário.

    ![Parar o menu de cópia de segurança](./media/backup-azure-sql-database/stop-backup-button.png)

7. Clique em **parar cópia de segurança** .

  

### <a name="resume-protection-for-a-sql-database"></a>Retomar a proteção para uma base de dados SQL

Se o **reter dados de cópia de segurança** opção tiver sido selecionada quando a proteção para a base de dados do SQL foi parada, pode retomar a proteção. Se os dados de cópia de segurança não era retidos, não é possível retomar a proteção.

1. Para retomar a proteção da base de dados SQL, abra o item de cópia de segurança e selecione **retomar cópia de segurança**.

    ![Selecione a retomar a cópia de segurança para retomar a proteção de base de dados](./media/backup-azure-sql-database/resume-backup-button.png)

2. Sobre o **política de cópia de segurança** menu, selecione uma política e, em seguida, selecione **guardar**.

## <a name="run-an-on-demand-backup"></a>Executar cópias de segurança a pedido

Pode executar diferentes tipos de cópias de segurança a pedido:

* Cópia de segurança completa
* Cópia de segurança completa apenas de cópia
* Cópia de segurança diferencial
* Cópia de segurança de registo

[Saiba mais](backup-architecture.md#sql-server-backup-types) sobre o SQL Server tipos de cópia de segurança.

## <a name="unregister-a-sql-server-instance"></a>Anular o registo de uma instância do SQL Server

Depois de ter desabilitado o proteção, mas antes de eliminar o cofre, anular o registo uma instância do SQL Server:

1. No dashboard do cofre, sob **Manage**, selecione **infraestrutura de cópia de segurança**.  

   ![Selecione a infraestrutura de cópia de segurança](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Sob **servidores de gestão**, selecione **servidores protegidos**.

   ![Selecione os servidores protegidos](./media/backup-azure-sql-database/protected-servers.png)


3. Na **servidores protegidos**, selecione o servidor ao anular o registo. Para eliminar o cofre, tem de anular o registo de todos os servidores.

4. O servidor protegido com o botão direito > **eliminar**.

   ![Selecione Delete](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Passos Seguintes

[Revisão](backup-sql-server-azure-troubleshoot.md) informações para cópia de segurança do SQL Server da base de dados de resolução de problemas.
