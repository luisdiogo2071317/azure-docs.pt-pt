---
title: Restaurar uma cópia de segurança bases de dados do SQL Server numa VM do Azure com o Azure Backup | Documentos da Microsoft
description: Este artigo descreve como restaurar bancos de dados SQL em execução numa VM do Azure que são submetidos a backup com o Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 3571eb2471f9b3f06eb509937fd11866b4e0caa8
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430809"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Restaurar bases de dados do SQL Server em VMs do Azure 


Este artigo descreve como restaurar uma base de dados do SQL Server em execução numa VM do Azure que é uma cópia de segurança para um cofre de serviços de recuperação de cópia de segurança do Azure com o [Azure Backup](backup-overview.md) serviço.


> [!NOTE]
> Cópia de segurança de bases de dados do SQl Server em execução numa VM do Azure com o Azure Backup está atualmente em pré-visualização pública.


Este artigo descreve como restaurar bases de dados do SQL Server. Se ainda não tiver configurado a cópia de segurança das bases de dados, siga as instruções em [neste artigo](backup-azure-sql-database.md)



## <a name="about-restoring-databases"></a>Sobre o restauro de bases de dados

Cópia de segurança do Azure, pode restaurar bases de dados do SQL Server em execução em VMs do Azure da seguinte forma:

- Restaure para uma data específica ou a hora (para o segundo), usando backups de log de transação. O Azure Backup determina automaticamente a cópia de segurança completa, diferencial adequada e a cadeia de backups de log, que são necessários para restaurar com base no tempo selecionado.
- Restaurar um um backup completo ou diferencial específico para restaurar para um ponto de recuperação específico, em vez de uma hora específica.


### <a name="prerequisites"></a>Pré-requisitos

Antes de restaurar uma base de dados tenha em atenção o seguinte:

- Pode restaurar a base de dados para uma instância do SQL Server na mesma região do Azure.
- O servidor de destino tem de estar registado no mesmo cofre como origem.
- Para restaurar uma base de dados encriptado do TDE para outro SQL Server, precisa primeiro [restaurar o certificado para o servidor de destino](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- Antes de acionar um restauro de base de dados "Mestra", iniciar a instância do SQL Server no modo de utilizador único com a opção de arranque **-m AzureWorkloadBackup**.
    - O valor para **-m** é o nome do cliente.
    - Apenas o nome de cliente especificado tem permissão para abrir a ligação.
- Para todas as bases de dados do sistema (modelo, mestra, msdb), pare o serviço SQL Agent antes de acionar o restauro.
- Feche todas as aplicações que podem tentar roubar uma ligação a qualquer uma destas bases de dados.

## <a name="restore-a-database"></a>Restaurar uma base de dados

Para restaurar necessita das seguintes permissões:

* **Operador de cópia de segurança** permissões no cofre o que estiver fazendo o restauro.
* **Contribuidor (escrita)** acesso à origem de VM que é uma cópia de segurança.
* **Contribuidor (escrita)** acesso para a VM de destino:
    - Se estiver a restaurar para a mesma VM esta será a VM de origem.
    - Se estiver a restaurar para uma localização alternativa será a nova VM de destino. 

Restaure da seguinte forma:
1. Abra o Cofre no qual a VM do SQL Server está registado.
2. No dashboard do cofre, sob **utilização**, selecione **itens de cópia de segurança**.

    ![Abra o menu de itens de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Na **itens de cópia de segurança**, em **tipo de gestão de cópia de segurança**, selecione **SQL na VM do Azure**.

    ![Selecione o SQL numa VM do Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Selecione a base de dados para restaurar.

    ![Selecione a base de dados para restaurar](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Reveja o menu de base de dados. Fornece informações sobre a cópia de segurança da base de dados, incluindo: 

    * Pontos de restauro mais antigo e mais recentes.
    * Registo de estado de cópia de segurança nas últimas 24 horas, para bases de dados no modo de recuperação completa e registadas em massa, se configurado para cópias de segurança do registo transacional.

6. Clique em **restaurar DB**. 

    ![Selecione restaurar DB](./media/backup-azure-sql-database/restore-db-button.png)

7. Na **restaurar configuração**, especifique onde pretende restaurar os dados para:
    - **Alternativo de localização**: Restaurar a base de dados para uma localização alternativa e manter a base de dados de origem original.
    - **Substituir a BD**: Restaure os dados para a mesma instância do SQL Server como a origem original. O efeito desta opção é substituir a base de dados original.

    > [!Important]
    > Se a base de dados selecionada pertence a um grupo de sempre disponibilidade, o SQL Server não permite que a base de dados sejam substituídas. Apenas **localização alternativa** está disponível.
    >

    ![Restaurar o menu de configuração](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restaurar para uma localização alternativa

1. Na **restaurar Configuration** menu, em **restaurar onde**, selecione **localização alternativa**.
2. Selecione o nome do SQL Server e a instância à qual pretende restaurar a base de dados.
3. Na **nome de BD restaurada** , introduza o nome da base de dados de destino.
4. Se aplicável, selecione **substituir se a BD com o mesmo nome já existe na instância SQL selecionada**.
5. Clique em **OK**.

    ![Fornecer valores para o menu de restaurar a configuração](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Na **selecionar ponto de restauro**, selecione se pretende [restaurar para um ponto específico no tempo](#restore-to-a-specific-point-in-time), ou restaurar para um [ponto de recuperação específico](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > O restauro de ponto no tempo só está disponível para backups de log para bases de dados com o modo de recuperação completa e registadas em massa. 


### <a name="restore-and-overwrite"></a>Restaurar e substituir

1. Na **restaurar Configuration** menu, em **restaurar onde**, selecione **substituir DB** > **OK**.

    ![Selecione substituir a BD](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. Na **selecionar ponto de restauro**, selecione * * registos (ponto no tempo) para [restaurar para um ponto específico no tempo](#restore-to-a-specific-point-in-time), ou **completa e diferencial** restaurar para um [específico ponto de recuperação](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > O restauro de ponto no tempo só está disponível para backups de log para bases de dados com um modelo de recuperação completa e registadas em massa. 




    
### <a name="restore-to-a-specific-point-in-time"></a>Restaurar para um ponto específico no tempo

Se tiver selecionado **registos (ponto no tempo)** como o tipo de restauro, efetue o seguinte procedimento:

1.  Sob **restaurar data/hora**, selecione o calendário mini. Sobre o **calendário**, as datas a negrito tem pontos de recuperação e a data atual é realçada.
2. Selecione uma data com pontos de recuperação. Não não possível selecionar datas sem pontos de recuperação.

    ![Abrir o calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

3. Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis num intervalo contínuo.
4. Especificar uma hora para a recuperação utilizando o gráfico de linha de tempo ou selecione uma hora. Em seguida, clique em **OK**.

    ![Abrir o calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

 
4. Sobre o **configuração avançada** menu, se pretender manter a base de dados não operacional após o restauro, ative **restaurar com NORECOVERY**.
5. Se pretender alterar a localização de restauro no servidor de destino, introduza um novo caminho de destino.
6. Clique em **OK**.

    ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)


7. Sobre o **restaurar** menu, selecione **restaurar** para iniciar a tarefa de restauro.
8. Acompanhar o progresso de restauro no **notificações** área ou selecione **restaurar trabalhos** no menu da base de dados.

    ![Restaurar o progresso da tarefa](./media/backup-azure-sql-database/restore-job-notification.png)



### <a name="restore-to-a-specific-restore-point"></a>Restaurar para um ponto de restauro específico

Se tiver selecionado **completa e diferencial** como o tipo de restauro, efetue o seguinte procedimento:


1. Selecione um ponto de recuperação da lista e clique em **OK** para concluir o procedimento de ponto de restauro.

    ![Escolha um ponto de recuperação completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)
        
2. Sobre o **configuração avançada** menu, se pretender manter a base de dados não operacional após o restauro, ative **restaurar com NORECOVERY**.
3. Se pretender alterar a localização de restauro no servidor de destino, introduza um novo caminho de destino. 
4. Clique em **OK**.

    ![Menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

7. Sobre o **restaurar** menu, selecione **restaurar** para iniciar a tarefa de restauro.
8. Acompanhar o progresso de restauro no **notificações** área ou selecione **restaurar trabalhos** no menu da base de dados.

    ![Restaurar o progresso da tarefa](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="next-steps"></a>Passos Seguintes

[Gerir e monitorizar](manage-monitor-sql-database-backup.md) bases de dados do SQL Server cópias de segurança de cópia de segurança do Azure.
