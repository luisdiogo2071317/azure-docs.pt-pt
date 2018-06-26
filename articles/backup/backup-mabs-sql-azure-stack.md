---
title: Fazer cópias de segurança de cargas de trabalho do SQL Server na pilha do Azure
description: Utilize o servidor de cópia de segurança do Azure para proteger a carga de trabalho do SQL Server na pilha do Azure.
services: backup
author: pvrk
manager: Shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: pullabhk
ms.openlocfilehash: ca7da7ab048b6f7bfdba81aac9bc7702b20ff967
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751802"
---
# <a name="back-up-sql-server-on-stack"></a>Criar cópias de segurança do SQL Server na pilha
Utilize este artigo para configurar o Microsoft Azure cópia de segurança do servidor (MABS) para proteger as bases de dados do SQL Server na pilha do Azure.

A gestão da cópia de segurança do SQL Server da base de dados para recuperação a partir do Azure e Azure envolve três passos:

1. Criar uma política de cópia de segurança para proteger as bases de dados do SQL Server
2. Criar cópias de segurança a pedido
3. Recuperar a base de dados de discos e do Azure

## <a name="before-you-start"></a>Antes de começar

[Instalar e preparar o servidor de cópia de segurança do Azure](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Criar uma política de cópia de segurança para proteger as bases de dados do SQL Server para o Azure
1. Na IU de servidor de cópia de segurança do Azure, clique em de **proteção** área de trabalho.

2. No Friso, clique em **novo** para criar um novo grupo de proteção.

    ![Criar grupo de proteção](./media/backup-azure-backup-sql/protection-group.png)

    Servidor de cópia de segurança do Azure inicia o Assistente do grupo de proteção, o que orienta-o ao longo da criação de um **grupo de proteção**. Clique em **Seguinte**.

3. No **selecionar tipo de grupo de proteção** ecrã, selecione **servidores**.

    ![Selecione o tipo de grupo de proteção - 'Servidores'](./media/backup-azure-backup-sql/pg-servers.png)

4. No **selecionar membros do grupo** ecrã, a lista de membros disponíveis apresenta várias origens de dados. Clique em **+** expanda uma pasta e revelar as subpastas. Clique na caixa de verificação para selecionar um item.

    ![Selecione a base de dados SQL](./media/backup-azure-backup-sql/pg-databases.png)

    Todos os itens selecionados aparecem na lista de membros selecionados. Depois de selecionar os servidores ou bases de dados que pretende proteger, clique em **seguinte**.

5. No **selecionar método de proteção de dados** ecrã, forneça um nome para o grupo de proteção e selecione o **pretendo proteção online** caixa de verificação.

    ![Método de proteção de dados - disco de curto prazo e Online do Azure](./media/backup-azure-backup-sql/pg-name.png)

6. No **especificar objetivos a curto prazo** ecrã, incluir entradas necessárias para criar pontos de cópia de segurança para o disco e clique em **seguinte**.

    No exemplo, **período de retenção** é **5 dias**, **frequência de sincronização** uma vez cada **15 minutos**, que é a cópia de segurança frequência. **Cópia de segurança completa rápida** está definido como **8:00 P.M**.

    ![Objetivos de curto prazo](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > No exemplo mostrado, às 8:00 PM diariamente um ponto de cópia de segurança é criado através da transferência de dados modificados de ponto de cópia de segurança de 8:00 PM o dia anterior. Este processo é denominado **cópia de segurança completa rápida**. Os registos de transações são sincronizados a cada 15 minutos. Se precisar de recuperar a base de dados ao 9:00 PM, o ponto é criado a partir de registos a partir do último express ponto de cópia de segurança completa (de 8 horas neste caso).
   >
   >

7. No **rever alocação do disco** ecrã, verifique o espaço de armazenamento geral disponível e o espaço em disco potencial. Clique em **Seguinte**.

8. No **escolher método de criação de réplica**, escolha como pretende criar o primeiro ponto de recuperação. Pode transferir a cópia de segurança inicial manualmente (desativado rede) para evitar congestionamento de largura de banda ou através da rede. Se escolher a aguardar para transferir a primeira cópia de segurança, pode especificar o tempo para a transferência inicial. Clique em **Seguinte**.

    ![Método de replicação inicial](./media/backup-azure-backup-sql/pg-manual.png)

    A cópia de segurança inicial necessita de transferir a origem de dados completo (base de dados do SQL Server) do servidor de produção (máquina do SQL Server) para o servidor de cópia de segurança do Azure. Estes dados poderão ser grandes e transferir os dados através da rede foi exceder a largura de banda. Por este motivo, pode optar por transferir a cópia de segurança inicial: **manualmente** (utilizando o suporte de dados amovível) para evitar congestionamento de largura de banda, ou **automaticamente através da rede** (a uma determinada hora).

    Assim que a cópia de segurança inicial estiver concluída, o resto das cópias de segurança são as cópias de segurança incrementais a cópia de segurança inicial. Cópias de segurança incrementais tendem a ser pequeno e são facilmente transferidas através da rede.

9. Escolha se pretende que a verificação de consistência para executar e clique em **seguinte**.

    ![Verificação de consistência](./media/backup-azure-backup-sql/pg-consistent.png)

    Servidor de cópia de segurança do Azure efetua uma verificação de consistência a integridade do ponto de cópia de segurança. Servidor do Backup do Azure calcula a soma de verificação do ficheiro cópia de segurança no servidor de produção (máquina do SQL Server neste cenário) e os dados de cópia de segurança para esse ficheiro. Se houver um conflito, presume-se que o ficheiro de cópia de segurança no servidor de cópia de segurança do Azure está danificado. Servidor de cópia de segurança do Azure rectifies os dados de cópia de segurança, enviando os blocos correspondente para o erro de correspondência de soma de verificação. Dado que as verificações de consistência intensivos em termos de desempenho, pode agendar a verificação de consistência ou execute automaticamente.

10. Para especificar a proteção online das origens de dados, selecione as bases de dados protegidos do Azure, clique em **seguinte**.

    ![Selecione as origens de dados](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Escolha as agendas de cópia de segurança e as políticas de retenção que melhor corresponda às políticas da organização.

    ![Agenda e retenção](./media/backup-azure-backup-sql/pg-schedule.png)

    Neste exemplo, as cópias de segurança são efetuadas uma vez por dia às 12:00 PM e 8 PM (parte inferior do ecrã)

    > [!NOTE]
    > É uma boa prática ter alguns pontos de recuperação de curta duração em disco para a recuperação rápida. Estes pontos de recuperação são utilizados para recuperação operacional. Azure funciona como uma localização de boa retirada do local com SLAs superiores e garantida disponibilidade.
    >
    >

    **Melhor prática**: se agendar cópias de segurança do Azure para iniciar após concluir as cópias de segurança de disco local, as cópias de segurança de disco mais recentes sempre são copiadas para o Azure.

12. Escolha o agendamento de política de retenção. Os detalhes sobre como funciona a política de retenção são fornecidos ao [utilize Azure Backup para substituir o seu artigo de infraestrutura da banda](backup-azure-backup-cloud-as-tape.md).

    ![Política de Retenção](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Neste exemplo:

    * As cópias de segurança são efetuadas uma vez por dia às 12:00 PM e 8 PM (parte inferior do ecrã) e são mantidas durante 180 dias.
    * A cópia de segurança no Sábado às 12:00 são mantidas durante 104 semanas
    * A cópia de segurança no último Sábado às 12:00 são mantidas durante 60 meses
    * A cópia de segurança no último Sábado de Março às 12:00 é mantido para 10 anos
13. Clique em **seguinte** e selecione a opção adequada para transferir a cópia de segurança inicial para o Azure. Pode escolher **automaticamente através da rede**

14. Depois de rever os detalhes da política no **resumo** ecrã, clique em **criar grupo** para concluir o fluxo de trabalho. Pode clicar em **fechar** e monitorizar o progresso da tarefa na área de trabalho de monitorização.

    ![Criação do grupo de proteção em curso](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Cópia de segurança a pedido de uma base de dados do SQL Server
Enquanto os passos anteriores criada uma política de cópia de segurança, é criado um "ponto de recuperação" apenas quando ocorre a primeira cópia de segurança. Em vez de aguardar que o programador iniciar, os passos abaixo acionador a criação de uma recuperação ponto manualmente.

1. Aguarde até que mostra o estado do grupo de proteção **OK** para a base de dados antes de criar o ponto de recuperação.

    ![Membros do grupo de proteção](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Clique com o botão direito na base de dados e selecione **criar ponto de recuperação**.

    ![Criar ponto de recuperação Online](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Escolha **proteção Online** no menu pendente e clique **OK** para iniciar a criação de um ponto de recuperação no Azure.

    ![Criar ponto de recuperação](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Ver o progresso da tarefa no **monitorização** área de trabalho.

    ![Consola de monitorização](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Recuperar uma base de dados do SQL Server a partir do Azure
Os seguintes passos são necessários para recuperar uma entidade protegida (base de dados do SQL Server) a partir do Azure.

1. Abra a consola de gestão do servidor de cópia de segurança do Azure. Navegue para **recuperação** área de trabalho onde pode ver os servidores protegidos. Procure a base de dados necessária (neste cenário ReportServer$ MSDPM2012). Selecione um **recuperação a partir de** hora em que está especificada como um **Online** ponto.

    ![Selecione o ponto de recuperação](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. O nome de base de dados com o botão direito e clique em **recuperar**.

    ![Recuperar a partir do Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS mostra os detalhes dos pontos de recuperação. Clique em **Seguinte**. Para substituir a base de dados, selecione o tipo de recuperação **recuperar na instância original do SQL Server**. Clique em **Seguinte**.

    ![Recuperar para a localização Original](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Neste exemplo, MABS recupera a base de dados noutra instância do SQL Server ou para uma pasta de rede autónomo.

4. No **opções especificar recuperação** ecrã, pode selecionar as opções de recuperação, como a limitação da utilização de largura de banda de rede para limitar a largura de banda utilizada pela recuperação. Clique em **Seguinte**.

5. No **resumo** ecrã, pode ver todas as configurações de recuperação até ao momento fornecidas. Clique em **recuperar**.

    O estado de recuperação mostra a base de dados a ser recuperada. Pode clicar em **fechar** para fechar o assistente e ver o progresso no **monitorização** área de trabalho.

    ![Iniciar o processo de recuperação](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Após a conclusão da recuperação, a base de dados restaurada é consistente de aplicação.

## <a name="next-steps"></a>Próximos Passos

Consulte o [ficheiros e aplicações de cópia de segurança](backup-mabs-files-applications-azure-stack.md) artigo.
Consulte o [SharePoint de cópia de segurança no Azure pilha](backup-mabs-sharepoint-azure-stack.md) artigo.
