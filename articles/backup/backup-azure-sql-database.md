---
title: Cópia de segurança da base de dados do SQL Server para o Azure | Documentos da Microsoft
description: Este tutorial explica a cópia de segurança do SQL Server para o Azure. O artigo também explica a recuperação do SQL Server.
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
ms.date: 7/6/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 32f45b66c4b1d22da3ffc4310a8a47c17319301f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38302828"
---
# <a name="back-up-sql-server-database-in-azure"></a>Criar cópias de segurança da base de dados do SQL Server no Azure

Bases de dados do SQL Server são cargas de trabalho críticas que requerem baixa objetivo de ponto de recuperação (RPO) e a retenção de longa duração. O Azure Backup fornece uma solução de cópia de segurança do SQL Server que requer zero infraestrutura, o que significa que nenhum servidor de cópia de segurança complexa, nenhum agente de gestão ou armazenamento de cópia de segurança para gerir. O Azure Backup Fornece gerenciamento centralizado para as cópias de segurança em todos os servidores SQL, ou até mesmo diferentes cargas de trabalho.

 Neste artigo, vai aprender:

> [!div class="checklist"]
> * Pré-requisitos para criar cópias de segurança do SQL Server para o Azure
> * Criar e utilizar um cofre dos serviços de recuperação
> * Configurar cópias de segurança do SQL Server da base de dados
> * Definir uma política de cópia de segurança (ou de retenção) para os pontos de recuperação
> * Como restaurar a base de dados

Antes de iniciar os procedimentos neste artigo, deve ter um SQL Server em execução no Azure. [Pode utilizar máquinas virtuais do SQL marketplace para criar rapidamente um servidor SQL](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Limitações da pré-visualização públicas

Os itens seguintes são as limitações conhecidas na pré-visualização pública.

- A máquina virtual do SQL requer ligação à internet para acessar os endereços IP públicos do Azure. Para obter mais detalhes, consulte a secção [estabelecer conectividade de rede](backup-azure-sql-database.md#establish-network-connectivity).
- Pode proteger até 2000 bases de dados SQL num cofre dos serviços de recuperação. Bases de dados SQL adicionais devem ser armazenados num cofre dos serviços de recuperação separado.
- [Cópia de segurança de grupos de disponibilidade distribuída tem limitações](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- Instâncias de Cluster de ativação pós-falha (FCI) do SQL não são suportadas.
- Utilize o portal do Azure para configurar o Azure Backup para proteger bases de dados do SQL Server. Suporte para o Azure PowerShell, CLI e as APIs REST não está atualmente disponível.

## <a name="supported-azure-geos"></a>Suportado GEOs do Azure

- Sudeste da Austrália (ASE) 
- Sul do Brasil (BRS)
- Canadá Central (CNC)
- Leste do Canadá (CE)
- EUA Central (CUS)
- Ásia Oriental (EA)
- Leste da Austrália (AE) 
- EUA Leste (EUS)
- EUA Leste 2 (EUS2)
- Leste do Japão (JPE)
- Oeste do Japão (JPW)
- Índia Central (INC) 
- Índia do Sul (INS)
- Coreia Central (KRC)
- Sul da Coreia (KRS)
- EUA Centro-Norte (NCUS) 
- Europa do Norte (NE) 
- EUA Centro-Sul (SCUS) 
- Sudeste Asiático (SEA)
- Sul do Reino Unido (UKS) 
- Oeste do Reino Unido (UKW) 
- Europa Ocidental (WE) 
- EUA Oeste (WUS)
- EUA Centro-Oeste (WCUS)
- EUA Oeste 2 (WUS 2) 

## <a name="supported-operating-systems-and-versions-of-sql-server"></a>Sistemas operativos e versões do SQL server

Os seguintes sistemas operativos são suportados. SQL marketplace máquinas virtuais do Azure e máquinas de virtuais externas (em que o SQL Server manualmente está instalado), são suportados.

### <a name="supported-operating-systems"></a>Sistemas operativos suportados

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux não é atualmente suportado.

### <a name="supported-versionseditions-of-sql-server"></a>Versões/edições suportadas do SQL Server

- SQL 2012 Enterprise, Standard, Web, desenvolvedor, Express
- SQL 2014 Enterprise, Standard, Web, desenvolvedor, Express
- SQL 2016 Enterprise, Standard, Web, desenvolvedor, Express
- SQL 2017 Enterprise, Standard, Web, desenvolvedor, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>Pré-requisitos para utilizar o Azure Backup para proteger o SQL Server 

Antes de pode criar cópias de segurança da base de dados do SQL Server, verifique as seguintes condições. :

- Identificar ou [criar um cofre dos serviços de recuperação](backup-azure-sql-database.md#create-a-recovery-services-vault) na mesma região ou localidade, como a máquina virtual alojar o SQL Server.
- [Verifique as permissões na máquina virtual](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) necessários para fazer cópias de segurança de bases de dados SQL.
- [Máquina virtual do SQL tem conectividade de rede](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> Pode ter apenas uma solução de cópia de segurança em simultâneo para bases de dados de cópia de segurança do SQL Server. Desative a outra cópia de segurança SQL antes de utilizar esta funcionalidade, as cópias de segurança mais irão interferir e falhar. Pode ativar a cópia de segurança do Azure para a VM de IaaS, juntamente com a cópia de segurança SQL sem qualquer conflito 
>

Se estas condições existem no seu ambiente, avance para a secção [configurar o cofre para proteger uma base de dados do SQL](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Se qualquer um dos pré-requisitos não existir, continue a ler esta secção.


## <a name="establish-network-connectivity"></a>Estabelecer conectividade de rede

Para todas as operações, a máquina virtual do SQL necessita de conectividade para os endereços IP públicos do Azure. Operações de máquina virtual do SQL (por exemplo, a deteção de base de dados, configuração de cópia de segurança, cópias de segurança agendadas, restaurar pontos de recuperação e assim por diante) falharem sem conectividade com os endereços IP públicos. Utilize qualquer uma das seguintes opções para fornecer um caminho claro para o tráfego de cópia de segurança:

- Lista de permissões de intervalos de IP do datacenter do Azure – intervalos de IP de datacenter à lista de permissões do Azure, utilize o [center página para obter detalhes sobre os intervalos de IP e as instruções de transferência](https://www.microsoft.com/download/details.aspx?id=41653). 
- Implantar um servidor de proxy HTTP para o encaminhamento de tráfego - quando estiver a criar a segurança uma base de dados SQL numa VM, a extensão de cópia de segurança na VM usa APIs de HTTPS para enviar comandos de gestão para a cópia de segurança do Azure e os dados ao armazenamento do Azure. A extensão de cópia de segurança também utiliza o Azure Active Directory (AAD) para autenticação. Encaminhe o tráfego de extensão de cópia de segurança para estes três serviços através do proxy HTTP, porque é o único componente configurado para acesso à Internet pública.

As relações vantajosas entre as opções são: capacidade de gerenciamento, um controle granular e custo.

> [!NOTE]
>Etiquetas de serviço para o Azure Backup devem estar disponíveis por disponibilidade geral.
>

| Opção | Vantagens | Desvantagens |
| ------ | ---------- | ------------- |
| Intervalos de IP da lista branca | Sem custos adicionais. <br/> Para abrir o acesso num NSG, utilize **Set-AzureNetworkSecurityRule** cmdlet. | Complexo de gerir o afetado como intervalos de IP mudam ao longo do tempo. <br/>Fornece acesso para a totalidade do, não apenas o armazenamento do Azure.|
| Utilizar um proxy de HTTP   | Um controle granular no proxy sobre o armazenamento é permitida a URLs. <br/>Único ponto de acesso à internet para VMs. <br/> Não sujeitas a alterações ao endereço IP do Azure. | Custos adicionais para executar uma VM com o software de proxy. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Definir permissões para as VMs de SQL externas

Para fazer backup de uma máquina virtual, o Azure Backup requer o **AzureBackupWindowsWorkload** extensão ser instalada. Se estiver a utilizar máquinas de virtuais do Azure marketplace, avançar diretamente para [bases de dados do SQL detetar server](backup-azure-sql-database.md#discover-sql-server-databases). Se a máquina virtual que aloja as bases de dados SQL não foi criada no Azure marketplace, conclua a secção seguinte para instalar a extensão e definir as permissões apropriadas. Para além da **AzureBackupWindowsWorkload** extensão, o Azure Backup requer privilégios de sysadmin do SQL para proteger as bases de dados SQL. Ao detetar as bases de dados na máquina virtual, o Azure Backup cria uma conta, NT Service\AzureWLBackupPluginSvc. Para o Azure Backup detetar as bases de dados SQL, a conta NT Service\AzureWLBackupPluginSvc tem de ter SQL e SQL permissões de administrador do sistema. O procedimento seguinte explica como fornecer estas permissões.

Para configurar permissões:

1. Na [portal do Azure](https://portal.azure.com), abra o Cofre de serviços de recuperação está a utilizar para proteger as bases de dados SQL.
2. No menu do dashboard do cofre, clique em **+ cópia de segurança** para abrir o **objetivo de cópia de segurança** menu.

   ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

3. Sobre o **objetivo de cópia de segurança** menu, no **em que a sua carga de trabalho é executado?** menu, deixe **Azure** como predefinição.

4. Na **o que fazer quiser a cópia de segurança** menu, expanda o menu pendente e selecione **do SQL Server na VM do Azure**.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    O **objetivo de cópia de segurança** menu apresenta dois novos passos: **detetar DBs em VMs** e **configurar a cópia de segurança**. **Detetar DBs em VMs** inicia uma pesquisa de máquinas virtuais do Azure.

    ![Mostra os novos passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Clique em **Iniciar deteção** para procurar as máquinas virtuais não protegidas na subscrição. Dependendo do número de máquinas virtuais não protegidas na subscrição, pode demorar algum tempo para passar por todas as máquinas virtuais.

    ![Cópia de segurança pendente](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Quando uma máquina virtual não protegida for detetada, aparecerá na lista. Máquinas virtuais não protegidas são listadas pelo respetivo grupo de recursos e o nome da máquina virtual. É possível que várias máquinas virtuais para ter o mesmo nome. No entanto, as máquinas virtuais com o mesmo nome pertencer aos grupos de recursos diferentes. Se uma máquina virtual esperada não aparecer na lista, ver se a máquina virtual já está protegida para um cofre.

6. Na lista de máquinas virtuais, selecione a VM que contenha a base de dados do SQL que pretende criar cópias de segurança e clique em **detetar bds**. 

    O processo de deteção instala o **AzureBackupWindowsWorkload** extensão na máquina virtual. A extensão permite que o serviço de cópia de segurança do Azure comunicar com a máquina virtual, pelo que pode fazer backup de bancos de dados SQL. Uma vez instala a extensão, o Azure Backup cria a conta de serviço virtual do Windows, **NT Service\AzureWLBackupPluginSvc**, na máquina virtual. A conta de serviço virtual requer permissão de sysadmin do SQL. Durante o serviço virtual conta processo de instalação, se vir o erro **UserErrorSQLNoSysadminMembership**, consulte a secção [permissões de administrador do sistema SQL corrigir](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    A área de notificações mostra o progresso da deteção da base de dados. Consoante o número de bases de dados estão na máquina virtual, pode demorar algum tempo para que a tarefa seja concluída. Quando as bases de dados selecionadas foram descobertas, é apresentada uma mensagem de êxito.

    ![mensagem de notificação de implementação com êxito](./media/backup-azure-sql-database/notifications-db-discovered.png)

Depois de associar a base de dados com o Cofre dos serviços de recuperação, a próxima etapa é [configurar a cópia de segurança](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>Corrigir permissões de administrador do sistema do SQL

Durante o processo de instalação, se vir o erro **UserErrorSQLNoSysadminMembership**, utilizar uma conta com permissões de administrador do sistema do SQL para iniciar sessão para o SQL Server Management Studio (SSMS). A menos que precise permissões especiais, autenticação do Windows deverá funcionar.

1. No SQL Server, abra a **inícios de sessão/segurança** pasta.

    ![Abra as pastas do SQL Server e de segurança e de início de sessão para ver as contas](./media/backup-azure-sql-database/security-login-list.png)

2. Na pasta de inícios de sessão, clique com botão direito e selecione **novo início de sessão**e no início de sessão - nova caixa de diálogo, clique em **pesquisa**

    ![Abrir caixa de diálogo de pesquisa no início de sessão - novo](./media/backup-azure-sql-database/new-login-search.png)

3. Desde a conta de serviço virtual do Windows, **NT Service\AzureWLBackupPluginSvc** já foi criado durante o registo de máquina virtual e a fase de deteção SQL, introduza o nome da conta como é apresentado no  **Introduza o nome de objeto a selecionar** caixa de diálogo. Clique em **verificar nomes** para resolver o nome. 

    ![Clique em verificar nomes para resolver o nome do serviço desconhecido](./media/backup-azure-sql-database/check-name.png)

4. Clique em **OK** para fechar a caixa de diálogo Selecionar utilizador ou grupo.

5. Na **funções de servidor** caixa de diálogo, certifique-se a **sysadmin** função está selecionada. Em seguida, clique em **OK** para fechar **início de sessão - novo**.

    ![Certifique-se de que a função de servidor sysadmin](./media/backup-azure-sql-database/sysadmin-server-role.png)

    As permissões necessárias agora devem existir.

6. Embora a corrigir o erro de permissões, ainda tem de associar a base de dados com o Cofre dos serviços de recuperação. No portal do Azure **servidores protegidos** lista, o servidor no erro com o botão direito e selecione **Redeteção de DBs**.

    ![Verifique se que o servidor tem as permissões adequadas](./media/backup-azure-sql-database/check-erroneous-server.png)

    A área de notificações mostra o progresso da deteção da base de dados. Consoante o número de bases de dados estão na máquina virtual, pode demorar algum tempo para que a tarefa seja concluída. Quando foram encontradas bases de dados selecionadas, é apresentada uma mensagem de êxito na área de notificações.

    ![mensagem de notificação de implementação com êxito](./media/backup-azure-sql-database/notifications-db-discovered.png)

Depois de associar a base de dados com o Cofre dos serviços de recuperação, a próxima etapa é [configurar a cópia de segurança](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detetar as bases de dados do SQL Server

Cópia de segurança do Azure pode detetar todas as bases de dados numa instância do SQL Server, para que possa protegê-los por seus requisitos de cópia de segurança. Utilize o procedimento seguinte para identificar a máquina virtual que aloja as bases de dados SQL. Depois de identificar a máquina virtual, o Azure Backup instala uma extensão simples para detetar as bases de dados do SQL server.

1. Inicie sessão na sua subscrição no [portal do Azure](https://portal.azure.com/).
2. No menu da esquerda, selecione **todos os serviços**.

    ![Escolha a opção de todos os serviços no menu principal](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Na caixa de diálogo todos os serviços, escreva *serviços de recuperação*. Quando começar a escrever, sua entrada filtra a lista de recursos. Quando o vir, selecione **cofres dos serviços de recuperação**.

    ![Escreva os serviços de recuperação na caixa de diálogo todos os serviços](./media/backup-azure-sql-database/all-services.png) <br/>

    É apresentada a lista de cofres de serviços de recuperação na subscrição. 

4. Na lista de cofre dos serviços de recuperação, selecione o cofre que pretende utilizar para proteger as bases de dados SQL.

5. No menu do dashboard do cofre, clique em **+ cópia de segurança** para abrir o **objetivo de cópia de segurança** menu.

   ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

6. Sobre o **objetivo de cópia de segurança** menu, no **em que a sua carga de trabalho é executado?** menu, deixe **Azure** como predefinição.

7. Na **o que fazer quiser a cópia de segurança** menu, expanda o menu pendente e selecione **do SQL Server na VM do Azure**.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Uma vez selecionada, o **objetivo de cópia de segurança** menu apresenta dois passos: detetar DBs em VMs e configurar a cópia de segurança. 

    ![Mostra os novos passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Clique em **Iniciar deteção** para procurar as máquinas virtuais não protegidas na subscrição. Dependendo do número de máquinas virtuais não protegidas na subscrição, pode demorar algum tempo para passar por todas as máquinas virtuais.

    ![Cópia de segurança pendente](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Quando uma máquina virtual não protegida for detetada, aparecerá na lista. Várias máquinas virtuais podem ter o mesmo nome. No entanto, várias máquinas virtuais com o mesmo nome pertencer aos grupos de recursos diferentes. As máquinas virtuais desprotegidas são listadas pelo respetivo grupo de recursos e o nome da máquina virtual. Se uma máquina virtual esperada não estiver listada, veja se que a máquina virtual já está protegida para um cofre.

9. Na lista de máquinas virtuais, selecione a caixa de verificação da máquina virtual que contém as bases de dados do SQL que pretende proteger e clique em **detetar bds**.

    Cópia de segurança do Azure Deteta todas as bases de dados do SQL na máquina virtual. Para obter informações sobre o que acontece durante a fase de deteção de base de dados, consulte a secção [operações de back-end quando detetar as bases de dados SQL](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). Após a deteção de bancos de dados SQL, está pronto para [configurar a tarefa de cópia de segurança](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>Operações de back-end quando detetar as bases de dados SQL

Quando utiliza a **detetar bds** ferramenta, cópia de segurança do Azure executa as seguintes operações em segundo plano:

- regista a máquina virtual com o Cofre de serviços de recuperação para cópia de segurança da carga de trabalho. Todas as bases de dados na máquina virtual registada apenas podem ser copiadas para este Cofre de serviços de recuperação. 

- instala o **AzureBackupWindowsWorkload** extensão na máquina virtual. Backup de uma base de dados SQL é uma solução sem agente, ou seja, com a extensão instalada na máquina virtual, sem agente está instalado na base de dados SQL.

- cria a conta de serviço, **NT Service\AzureWLBackupPluginSvc**, na máquina virtual. Todas as operações de cópia de segurança e restauro, utilize a conta de serviço. **NT Service\AzureWLBackupPluginSvc** necessita de permissões de administrador do sistema do SQL. Todas as máquinas virtuais do SQL Marketplace são fornecidos com o SqlIaaSExtension instalado e AzureBackupWindowsWorkload utiliza SQLIaaSExtension para obter automaticamente as permissões necessárias. Se a sua máquina virtual não tem SqlIaaSExtension instalado, a operação de detetar DB falhar e receberá a mensagem de erro, **UserErrorSQLNoSysAdminMembership**. Para adicionar a permissão de administrador do sistema para cópia de segurança, siga as instruções em [como configurar permissões de cópia de segurança do Azure para VMs do SQL externas](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![Selecione a vm e a base de dados](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>Configurar cópia de segurança da base de dados do SQL Server

Cópia de segurança do Azure fornece serviços de gestão para proteger as bases de dados do SQL Server e gerir tarefas de cópia de segurança. As capacidades de monitorização e gestão dependem do seu Cofre dos serviços de recuperação. 

> [!NOTE]
> Pode ter apenas uma solução de cópia de segurança em simultâneo para bases de dados de cópia de segurança do SQL Server. Desative a outra cópia de segurança SQL antes de utilizar esta funcionalidade, as cópias de segurança mais irão interferir e falhar. Pode ativar a cópia de segurança do Azure para a VM de IaaS, juntamente com a cópia de segurança SQL sem qualquer conflito 
>

Para configurar a proteção de base de dados SQL:

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL.

2. No menu do dashboard do cofre, clique em **+ cópia de segurança** para abrir o **objetivo de cópia de segurança** menu.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

3. Sobre o **objetivo de cópia de segurança** menu, no **em que a sua carga de trabalho é executado?** menu, deixe **Azure** como predefinição.

4. Na **o que fazer quiser a cópia de segurança** menu, expanda o menu pendente e selecione **do SQL Server na VM do Azure**.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Uma vez selecionada, o **objetivo de cópia de segurança** menu apresenta dois passos: detetar DBs em VMs e configurar a cópia de segurança. Se tiver tido através deste artigo por ordem, descobertos já as máquinas virtuais desprotegidas e este Cofre está registado com uma máquina virtual. Agora, está pronto para configurar a proteção para as bases de dados SQL.

5. No menu do objetivo de cópia de segurança, clique em **configurar a cópia de segurança**.

    ![Mostra os novos passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    O serviço de cópia de segurança do Azure apresenta todas as instâncias SQL com bases de dados autónomas, bem como grupos de disponibilidade SQL AlwaysOn. Para ver as bases de dados autónomo na instância do SQL, clique na divisa junto ao nome da instância para ver as bases de dados. As imagens seguintes mostram exemplos de uma instância autónoma e um grupo de disponibilidade Always On.

    > [!NOTE]
    > Em caso de SQL grupo de Disponibilidade AlwaysOn, que Respeitamos a preferência de cópia de segurança de SQL. Mas, devido a uma limitação de plataforma do SQL, cópias de segurança completas e diferenciais têm de ocorrer a partir do nó principal. Cópia de segurança do registo pode ser efetuadas com base em sua preferência de cópia de segurança. Devido a essa limitação, o nó principal sempre deve ser registrado para grupos de disponibilidade.
    >

    ![Lista de bases de dados na instância do SQL](./media/backup-azure-sql-database/discovered-databases.png)

    Clique na divisa junto a grupos de Disponibilidade AlwaysOn para ver a lista de bases de dados.

    ![Lista de bases de dados no grupo de Disponibilidade AlwaysOn](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Na lista de bases de dados, Selecionar tudo o que pretende proteger e clique em **OK**.

    ![Selecione várias bases de dados para protegê-los](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Pode selecionar até 50 bases de dados de uma só vez. Se pretender proteger mais de 50 bases de dados, fazer várias passagens. Depois de proteger as 50 primeiras bases de dados, repita este passo para proteger o próximo conjunto de bases de dados.
    > [!Note] 
    > Para otimizar cargas de cópia de segurança, cópia de segurança do Azure divide grandes trabalhos de cópia de segurança em vários lotes. O número máximo de bases de dados numa tarefa de cópia de segurança é 50.
    >
    >

7. Para criar ou escolha uma política de cópia de segurança, no **cópia de segurança** menu, selecione **política de cópia de segurança**, para abrir o menu.

    ![Selecione a opção da política de cópia de segurança](./media/backup-azure-sql-database/select-backup-policy.png)

8. Partir do **escolher política de cópia de segurança** menu pendente, escolha uma política de cópia de segurança e clique em **OK**. Para informações sobre como criar sua própria política de cópia de segurança, consulte a secção [definir uma política de cópia de segurança](backup-azure-sql-database.md#define-a-backup-policy).

    ![Escolha uma política de cópia de segurança a partir do menu pendente](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    No menu de política de cópia de segurança, do **escolher política de cópia de segurança** menu pendente, pode escolher: 
    - a política de HourlyLogBackup predefinida, 
    - uma política de cópia de segurança existente que criou anteriormente para o SQL,
    - para [definir uma nova política](backup-azure-sql-database.md#define-a-backup-policy) com base no seu objetivo de ponto de recuperação (RPO) e o período de retenção. 

    > [!Note]
    > O Azure Backup suporta a retenção de longo prazo com base no esquema de cópia de segurança avô-pai-filho para otimizar o consumo de armazenamento de back-end, satisfazendo as necessidades de conformidade.
    >

9. Depois de escolher uma política de cópia de segurança, no **menu de cópia de segurança** clique em **ativar cópia de segurança**.

    ![Ativar a política de cópia de segurança escolhida](./media/backup-azure-sql-database/enable-backup-button.png)

    Pode controlar o progresso da configuração na área de notificações do portal.

    ![área de notificação do Vista](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Definir uma política de cópia de segurança

Uma política de cópia de segurança define uma matriz de quando são executadas as cópias de segurança e quanto tempo as cópias de segurança são retidas. Pode utilizar o Azure Backup para agendar a três tipos de cópia de segurança de bases de dados SQL:

* Cópia de segurança completa - uma cópia de segurança completa da base de dados faz backup de todo o banco de dados. Uma cópia de segurança completa contém todos os dados numa base de dados específica ou conjunto de grupos de ficheiros ou ficheiros e suficiente registo para recuperar esses dados. No máximo, pode acionar uma cópia de segurança completa por dia. Pode optar por realizar um backup completo num intervalo diário ou semanal. 
* Cópia de segurança diferencial - um backup diferencial baseia-se a cópia de segurança de dados completa mais recente, anterior. Uma cópia de segurança diferencial captura apenas os dados que foram alterados desde a cópia de segurança completa. No máximo, pode acionar uma cópia de segurança diferencial por dia. Não é possível configurar uma cópia de segurança completa e uma cópia de segurança diferencial no mesmo dia.
* Backup de log de transação - uma cópia de segurança do registo permite restauro de ponto no tempo até específicas de um segundo. No máximo, pode configurar cópias de segurança do registo transacional a cada 15 minutos.

Política é criada no cofre dos serviços de recuperação de nível. Se tiver vários cofres, os cofres, podem utilizar a mesma política de cópia de segurança, mas tem de aplicar a política de cópia de segurança para cada cofre. Ao criar uma política de cópia de segurança, diária, cópia de segurança completa é a predefinição. Pode adicionar um Backup diferencial, mas apenas se mudar as cópias de segurança completas semanais de ocorrer. O procedimento seguinte explica como criar uma política de cópia de segurança para um SQL server numa máquina virtual do Azure.

Para criar uma política de cópia de segurança

1. No menu de política de cópia de segurança, do **escolher política de cópia de segurança** menu pendente, selecione **criar nova**.

   ![Criar nova política de cópia de segurança](./media/backup-azure-sql-database/create-new-backup-policy.png)

    O menu de política de cópia de segurança muda para fornecer os campos necessários para qualquer política de cópia de segurança de servidor SQL.

   ![novos campos de política de cópia de segurança](./media/backup-azure-sql-database/blank-new-policy.png)

2. Na **nome da política**, forneça um nome. 

3. Um Backup completo é obrigatório. Pode aceitar os valores predefinidos para a cópia de segurança completa ou clique em **cópia de segurança completa** para editar a política.

    ![novos campos de política de cópia de segurança](./media/backup-azure-sql-database/full-backup-policy.png)

    Na diretiva de cópia de segurança completa, escolha diária ou semanalmente para a frequência. Se optar por diariamente, escolha a hora e o fuso horário, quando a tarefa de cópia de segurança começa. Se optar por cópias de segurança completas diárias, não é possível criar cópias de segurança diferencial.

   ![definição de intervalo diário](./media/backup-azure-sql-database/daily-interval.png)

    Se optar por semanalmente, escolha o dia da semana, hora e fuso horário quando começa a tarefa de cópia de segurança.

   ![definição de intervalo semanal](./media/backup-azure-sql-database/weekly-interval.png)

4. Por predefinição, são selecionadas todas as opções de período de retenção (diárias, semanais, mensais e anuais). Desmarcar qualquer limite de intervalo de retenção que não pretende e definir os intervalos de usar. No menu de política de cópia de segurança completa, clique em **OK** para aceitar as configurações.

   ![definições de intervalo de intervalo de retenção](./media/backup-azure-sql-database/retention-range-interval.png)

    Pontos de recuperação são marcados para a retenção, com base no seu período de retenção. Por exemplo, se selecionar um valor diário, cópia de segurança completa de cópia de segurança completa apenas um é acionada por dia. Dependendo de seu período de retenção semanal, cópia de segurança do dia específico é marcada e mantida com base no período de retenção semanal. O período de retenção mensais e anuais tem um comportamento semelhante.

5. Para adicionar uma política de cópia de segurança diferencial, clique em **cópia de segurança diferencial** para abrir respetivo menu. 

   ![abrir a política diferencial](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    No menu de política de cópia de segurança diferencial, selecione **ativar** para abrir os controles de frequência e retenção. Pode acionar, no máximo, uma cópia de segurança diferencial por dia.
    > [!Important] 
    > No máximo, backups diferenciais podem ser mantidos durante 180 dias. Se precisar de retenção mais longa, tem de utilizar cópias de segurança completas, não é possível utilizar cópias de segurança diferenciais.
    >

   ![Editar política diferencial](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Clique em **OK** para guardar a política e volte ao menu principal da política de cópia de segurança.

6. Para adicionar uma política de cópia de segurança do registo transacional, clique em **registo de cópia de segurança** para abrir respetivo menu. No menu de cópia de segurança do registo, selecione **ativar**e defina os controlos de frequência e retenção. Backups de log podem ocorrer com a frequência de 15 minutos e podem ser mantidos até 35 dias. Clique em **OK** para guardar a política e volte ao menu principal da política de cópia de segurança.

   ![Editar política de cópia de segurança do registo](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. Escolha se pretende ativar a compressão de cópia de segurança de SQL. Compressão está desativada por predefinição.

    Back-end, o Azure Backup utiliza compressão de cópias de segurança nativa SQL.

8. Quando efetuar todas as edições para a política de cópia de segurança, clique em **OK**. 

   ![aceite a nova política](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Restaurar uma base de dados SQL

Cópia de segurança do Azure fornece funcionalidades para restaurar bases de dados individuais para uma data específica ou a hora, até um específico em segundo lugar, usando backups de log de transação. Com base em tempos de restauro fornecer, cópia de segurança do Azure automaticamente determina o adequado completas, diferenciais e a cadeia de backups de log necessário para restaurar os dados.

Em alternativa, pode selecionar um backup completo ou diferenciais específico para restaurar para um ponto de recuperação específico, em vez de uma hora específica.
 > [!Note]
 > Antes de acionar o restauro da base de dados "Mestra" Inicie o SQL Server no modo de utilizador único com a opção de arranque "-m AzureWorkloadBackup". O argumento -m é o nome do cliente, apenas este cliente terá permissão para abrir a ligação. Para todas as bases de dados do sistema (modelo, mestra, msdb) pare o serviço SQL Agent antes de acionar o restauro. Feche todas as aplicações que podem tentar roubar uma ligação a qualquer um desses bds.
>

Para restaurar uma base de dados

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL.

2. No dashboard do cofre, selecione **utilização** itens de cópia de segurança para abrir o menu de itens de cópia de segurança.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Na **itens de cópia de segurança** menu, selecione o tipo de gestão de cópia de segurança **SQL na VM do Azure**. 

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    A lista de itens de cópia de segurança ajusta-se para mostrar a lista de bases de dados SQL. 

4. Na lista de bases de dados SQL, selecione a base de dados que pretende restaurar.

    ![Selecione o SQL na VM do Azure da lista](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Ao selecionar a base de dados, é aberto o respetivo menu. Esse menu fornece os detalhes de cópia de segurança para a base de dados, incluindo:

    * o mais antigo e mais recente restaurar pontos,
    * Estado de cópia de segurança do registo nas últimas 24 horas (para bases de dados completo em massa com sessão iniciada recuperação modelo e, se configurado para cópias de segurança do registo transacional)

5. No menu da base de dados selecionada, clique em **restaurar DB** para abrir o menu de restauro.

    ![Selecione restaurar base de dados](./media/backup-azure-sql-database/restore-db-button.png)

    O **restaurar** menu abre-se e, portanto, faz a **restaurar configuração** menu. O **restaurar configuração** menu é a primeira etapa na configuração do restauro. Neste menu, selecione onde pretende restaurar os dados. As opções são:
    * Alternativo de localização - utilize esta opção se pretende restaurar a base de dados para uma localização alternativa ao mesmo tempo retendo a base de dados de origem original.
    * Substitua a BD - restaura os dados para a mesma instância do SQL Server como a origem original. O efeito disso é que substituir a base de dados original.

    > [!Important]
    > Se a base de dados selecionada pertence a um grupo de disponibilidade Always On, o SQL não permite a base de dados sejam substituídas. Neste caso, apenas os **localização alternativa** opção está ativada.
    >

    ![Clique em configurar para abrir o menu de configuração de restauro](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restaurar para uma localização alternativa

Este procedimento descreve a restauração de dados para uma localização alternativa. Se pretender substituir a base de dados durante a restauração, avance para a secção [restaurar e substituir a base de dados](backup-azure-sql-database.md#restore-and-overwrite-the-database). Este procedimento pressupõe que o Cofre de serviços de recuperação aberto e, no menu de restaurar a configuração. Se não estiver, começar com a secção [restaurar uma base de dados do SQL](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Pode restaurar a base de dados para um SQL Server na mesma região do Azure e o servidor de destino tem de ser registado no Cofre de serviços de recuperação. 
>

O **servidor** menu pendente só mostra os SQL servers registados no Cofre de serviços de recuperação. Se não está no servidor ao qual pretende o **servidor** lista, consulte a secção [detetar bases de dados](backup-azure-sql-database.md#discover-sql-server-databases) para localizar o servidor. Durante o processo de base de dados de deteção, quaisquer novos servidores são registados no Cofre de serviços de recuperação.

1. Na **restaurar configuração** menu:

    * Selecione **localização alternativa**,
    * para **servidor**, escolha o SQL server onde pretende restaurar a base de dados.
    * na **instância** menu pendente, escolha uma instância de SQL
    * na **nome de BD restaurada** caixa de diálogo, forneça o nome da base de dados de destino.
    * Se aplicável, selecione **substituir se a BD com o mesmo nome já existe na instância SQL selecionada**.
    * Clique em **OK** para concluir a configurar o destino e mover para escolher um ponto de restauro.

    ![Selecione uma localização alternativa, a instância e o nome, no menu de configuração de restauro](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Na **ponto de restauro selecione** menu, pode escolher um registos (ponto no tempo) ou ponto de restauro completa e diferencial. Se quiser restaurar para um registo de ponto no tempo específico, continue com este passo. Se quiser restaurar um ponto de restauro completo ou diferenciais, avançar diretamente para o passo 3.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    O ponto no tempo só está disponível para backups de log para bases de dados com completo e o modelo de recuperação de registadas em massa. Para restaurar para um ponto no tempo específico:

    1. Selecione **registos (ponto no tempo)** como a opção de restauro.

        ![Escolha o tipo de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Sob **restaurar data/hora**, clique no ícone de calendário para abrir o calendário. Datas em negrito contenham pontos de recuperação e a data atual é realçada. Selecione uma data no calendário com pontos de recuperação. Não é possível selecionar datas com nenhum ponto de recuperação.

        ![Abrir calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis num intervalo contínuo.

    3. Usando o gráfico de linha de tempo ou a caixa de diálogo de tempo, especifique uma hora específica para o ponto de recuperação e clique em **OK** para concluir o passo de ponto de restauro.
    
       ![Abrir calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        O **selecionar ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Partir do **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, diante da **restaurar com NORECOVERY** menu, selecione **ativado**.
        * Se pretender alterar a localização de restauro no servidor de destino, fornecer um novo caminho na **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    5. Sobre o **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso nas tarefas de restauro da base de dados.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)

3. Na **ponto de restauro selecione** menu, escolha um ponto de recuperação. Pode escolher um registos (ponto no tempo) ou completa e diferencial. Se quiser restaurar um log de ponto no tempo, volte ao passo 2. Este passo restaura um ponto de restauro de completo ou diferencial específico. Com esta opção, pode ver pontos de recuperação de todas as completas e diferenciais nos últimos 30 dias. Se quiser ver os pontos de recuperação com mais de 30 dias, clique em **filtro** para abrir o **pontos de restauração de filtro** menu. Se optar por um ponto de recuperação diferencial, cópia de segurança do Azure restaura primeiro o ponto de recuperação completo apropriado e, em seguida, aplica-se o ponto de recuperação diferencial selecionado.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Na **selecionar ponto de restauro** menu, selecione **completa e diferencial**.

       ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        É apresentada a lista de pontos de recuperação disponíveis.

    2. Na lista de pontos de recuperação, selecione um ponto de recuperação e clique em **OK** para concluir o procedimento de ponto de restauro. 

        ![Escolha o ponto de recuperação completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        O **ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

        ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Partir do **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, diante da **restaurar com NORECOVERY** menu, selecione **ativado**. **Restaurar com NORECOVERY** está desativada por predefinição.
        * Se pretender alterar a localização de restauro no servidor de destino, fornecer um novo caminho na **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    4. Sobre o **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso nas tarefas de restauro da base de dados.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Restaurar e substituir a base de dados

Este procedimento orienta-se a restauração de dados e substituir a base de dados. Se pretender restaurar para uma localização alternativa, avance para a secção [restaurar para uma localização alternativa](backup-azure-sql-database.md#restore-to-an-alternate-location). Este procedimento assume que tem o seu Cofre de serviços de recuperação aberto e está no **restaurar configuração** menu (veja a imagem seguinte). Se não estiver, começar com a secção [restaurar uma base de dados do SQL](backup-azure-sql-database.md#restore-a-sql-database).

![Clique em configurar para abrir o menu de configuração de restauro](./media/backup-azure-sql-database/restore-overwrite-db.png)

O **servidor** menu pendente só mostra os SQL servers registados no Cofre de serviços de recuperação. Se não está no servidor ao qual pretende o **servidor** lista, consulte a secção [detetar bases de dados](backup-azure-sql-database.md#discover-sql-server-databases) para localizar o servidor. Durante o processo de base de dados de deteção, quaisquer novos servidores são registados no Cofre de serviços de recuperação.

1. Na **restaurar Configuration** menu, selecione **substituir DB** e clique em **OK** para concluir a configurar o destino. 

   ![Clique em substituir DB](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    O **servidor**, **instância**, e **nome da BD restaurada** caixas de diálogo não são necessárias.

2. Na **ponto de restauro selecione** menu, pode escolher um registos (ponto no tempo) ou ponto de restauro completa e diferencial. Se quiser restaurar um log de ponto no tempo, continue com este passo. Se quiser restaurar um ponto de restauro completa e diferencial, avançar diretamente para o passo 3.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    O ponto no tempo só está disponível para backups de log para bases de dados com completo e o modelo de recuperação de registadas em massa. Escolher um ponto no tempo de restauro para um específico em segundo lugar:

    1. Selecione **registos (ponto no tempo)** como a opção de restauro.

        ![Escolha o tipo de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Sob **restaurar data/hora**, clique no ícone de calendário para abrir o calendário. Datas em negrito contenham pontos de recuperação e a data atual é realçada. Selecione uma data no calendário com pontos de recuperação. Não é possível selecionar datas com nenhum ponto de recuperação.

        ![Abrir calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis.

    3. Usando o gráfico de linha de tempo ou a caixa de diálogo de tempo, especifique uma hora específica para o ponto de recuperação e clique em **OK** para concluir o passo de ponto de restauro.
    
       ![Abrir calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        O **selecionar ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Partir do **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, diante da **restaurar com NORECOVERY** menu, selecione **ativado**.
        * Se pretender alterar a localização de restauro no servidor de destino, fornecer um novo caminho na **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    5. Sobre o **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso nas tarefas de restauro da base de dados.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)

3. Na **ponto de restauro selecione** menu, escolha um ponto de recuperação. Pode escolher um registos (ponto no tempo) ou completa e diferencial. Se quiser restaurar um log de ponto no tempo, volte ao passo 2. Este passo restaura um ponto de restauro de completo ou diferencial específico. Com esta opção, pode ver pontos de recuperação de todas as completas e diferenciais nos últimos 30 dias. Se quiser ver os pontos de recuperação com mais de 30 dias, clique em **filtro** para abrir o **pontos de restauração de filtro** menu. Se optar por um ponto de recuperação diferencial, cópia de segurança do Azure restaura primeiro o ponto de recuperação completo apropriado e, em seguida, aplica-se o ponto de recuperação diferencial selecionado.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Na **selecionar ponto de restauro** menu, selecione **completa e diferencial**.

       ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        É apresentada a lista de pontos de recuperação disponíveis.

    2. Na lista de pontos de recuperação, selecione um ponto de recuperação e clique em **OK** para concluir o procedimento de ponto de restauro. 

        ![Escolha o ponto de recuperação completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        O **ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

        ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Partir do **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, diante da **restaurar com NORECOVERY** menu, selecione **ativado**. **Restaurar com NORECOVERY** está desativada por predefinição.
        * Se pretender alterar a localização de restauro no servidor de destino, fornecer um novo caminho na **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    4. Sobre o **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso nas tarefas de restauro da base de dados.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Gerir as operações de cópia de segurança do Azure para SQL em VMs do Azure

Esta seção fornece informações sobre as várias cópias de segurança do Azure operações de gestão disponíveis para o SQL sobre máquinas virtuais do Azure. Existem as seguintes operações de alto nível:

* Monitorizar Tarefas
* Alertas de cópia de segurança
* Parar a proteção numa base de dados SQL
* Retomar a proteção para uma base de dados SQL
* Acionar uma tarefa de cópia de segurança ad hoc
* Anular o registo de um SQL server

### <a name="monitor-jobs"></a>Monitorizar Tarefas
Cópia de segurança do Azure que está a ser uma solução de classe empresarial fornece notificação para qualquer falhas (consulte a secção de alertas de cópia de segurança abaixo) e alertas de cópia de segurança avançados. Se pretender continuar a monitorizar tarefas específicas pode utilizar qualquer uma das opções seguintes com base nos seus requisitos:

#### <a name="use-azure-portal-for-all-adhoc-operations"></a>Utilizar o portal do Azure para todas as operações do ad hoc
O Azure mostra de cópia de segurança acionada manualmente todas as tarefas ou de ad hoc, no portal de tarefas de cópia de segurança. As tarefas disponíveis no portal inclusão: todos os configurar operações de cópia de segurança, manualmente acionada por operações de cópia de segurança, operações de restauração, registo e detetar operações de base de dados e parar as operações de cópia de segurança. 
![menu de configuração avançada](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Todos os trabalhos de backup agendados incluindo completo, o backup de Log e diferenciais não será apresentado no portal do e pode ser monitorizada com SQL Server Management Studio, conforme descrito abaixo.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Utilizar o SQL Server Management Studio para tarefas de cópia de segurança
O Azure Backup utiliza as APIs nativas do SQL para todas as operações de cópia de segurança. Com as APIs nativas, pode buscar todas as informações de tarefa do [tabela de backupset SQL](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) na base de dados msdb.

O exemplo seguinte é uma consulta para obter todas as tarefas de cópia de segurança para uma base de dados com o nome **DB1**. Personalize a consulta para monitorização mais avançada.
```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  
 
```

### <a name="backup-alerts"></a>Alertas de cópia de segurança

Com os backups de log que ocorrem a cada 15 minutos, a monitorização ocasionalmente as tarefas de cópia de segurança pode ser enfadonho. Cópia de segurança do Azure planeadas para esta situação potencialmente entediante ao fornecer alertas de e-mail acionados por qualquer falha de cópia de segurança. Alertas são consolidados ao nível da base de dados por código de erro. Por exemplo, se uma base de dados tem várias falhas de cópia de segurança, em vez de receber um alerta para cada falha, recebe e-mail para a primeira falha. Pode, em seguida, inicie sessão no portal do Azure para monitorizar falhas subsequentes para essa base de dados. 

Para monitorizar alertas de cópia de segurança:

1. Inicie sessão na sua subscrição do Azure no [portal do Azure](https://portal.azure.com).

2. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL.

3. No menu do cofre dos serviços de recuperação, selecione **alertas e eventos**. 

   ![menu de configuração avançada](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Na **eventos e alertas** menu, selecione **alertas de cópia de segurança** para ver a lista de alertas.

   ![menu de configuração avançada](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Parar a proteção numa base de dados do SQL Server

Se parar a proteção de uma base de dados do SQL Server, cópia de segurança do Azure pede-lhe se pretende manter os pontos de recuperação. Existem duas formas de parar a proteção de base de dados SQL:

* Parar todas as tarefas de cópia de segurança futuras e eliminar todos os pontos de recuperação,
* Parar todas as tarefas de cópia de segurança futuras, mas deixar os pontos de recuperação 

Deixar os pontos de recuperação acarreta um custo, como os pontos de recuperação para SQL executar a instância protegida de SQL preços custo e o armazenamento consumido. Para obter mais informações sobre os preços de cópia de segurança do Azure para SQL, consulte a [página de preços do Azure Backup](https://azure.microsoft.com/pricing/details/backup/). Para parar a proteção da base de dados:

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL.

2. No dashboard do cofre, selecione **utilização** itens de cópia de segurança para abrir o menu de itens de cópia de segurança.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Na **itens de cópia de segurança** menu, selecione o tipo de gestão de cópia de segurança **SQL na VM do Azure**. 

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    A lista de itens de cópia de segurança ajusta-se para mostrar a lista de bases de dados SQL. 

4. Na lista de bases de dados SQL, selecione a base de dados que pretende parar a proteção.

    ![Selecione o SQL na VM do Azure da lista](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Ao selecionar a base de dados, é aberto o respetivo menu. 

5. No menu da base de dados selecionada, clique em **parar cópia de segurança** para parar de proteger a base de dados.

    ![Selecione restaurar base de dados](./media/backup-azure-sql-database/stop-db-button.png)

    O **parar cópia de segurança** é aberto o menu.

6. Na **parar cópia de segurança** menu, optar por reter dados de cópia de segurança ou eliminar dados de cópia de segurança. Opcionalmente, pode indicar um motivo para interromper a proteção e um comentário.

    ![Selecione restaurar base de dados](./media/backup-azure-sql-database/stop-backup-button.png)

7. Clique em **parar cópia de segurança** para parar a proteção na base de dados. 

### <a name="resume-protection-for-a-sql-database"></a>Retomar a proteção para uma base de dados SQL

Se o **reter dados de cópia de segurança** opção tiver sido selecionada quando parar a proteção da base de dados SQL, é possível retomar a proteção. Se os dados de cópia de segurança não foi mantidos, não é possível retomar a proteção. 

1. Para retomar a proteção da base de dados SQL, abra o item de cópia de segurança e clique em **retomar cópia de segurança**.

    ![retomar a proteção de base de dados](./media/backup-azure-sql-database/resume-backup-button.png)

   É aberto o menu de política de cópia de segurança.

2. Partir do **política de cópia de segurança** menu, selecione uma política e clique em **guardar**.

### <a name="trigger-an-adhoc-backup"></a>Acionar uma cópia de segurança ad hoc

Pode acionar uma cópia de segurança ad hoc sempre que quiser. Existem quatro tipos de cópias de segurança ad hoc. Para obter detalhes sobre cada tipo, consulte o artigo [cópias de segurança de tipos de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Cópia de segurança completa
* Copiar apenas cópia de segurança completa
* Cópia de segurança diferencial
* Cópia de segurança do registo

### <a name="unregister-a-sql-server"></a>Anular o registo de um SQL Server

Para anular o registo de um SQL server depois de remover a proteção, mas antes de eliminar o Cofre

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL.

2. Na **Manage** secção do menu do cofre, clique em **infraestrutura de cópia de segurança**.  

   ![retomar a proteção de base de dados](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Na **servidores de gestão** secção, clique em **servidores protegidos**.

   ![retomar a proteção de base de dados](./media/backup-azure-sql-database/protected-servers.png)

    É aberto o menu de servidores protegidos. 

4. Na **servidores protegidos** menu, selecione o servidor que pretende anular o registo. Se pretender eliminar o cofre, tem de anular o registo todos os servidores.

5. No menu de servidores protegidos, o servidor protegido com o botão direito e selecione **eliminar**. 

   ![retomar a proteção de base de dados](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="sql-database-backup-faq"></a>FAQ sobre o backup de banco de dados SQL

A secção seguinte fornece informações adicionais sobre a cópia de segurança do SQL da base de dados.

### <a name="can-i-throttle-the-speed-of-the-sql-backup-policy-so-it-minimizes-impact-on-the-sql-server"></a>Posso limitar a velocidade da política de cópia de segurança de SQL para que ele minimiza o impacto sobre o SQL server

Sim, pode limitar a taxa a que executa a política de cópia de segurança. Para alterar a definição:

1. No SQL Server, na `C:\Program Files\Azure Workload Backup\bin` pasta, abra **TaskThrottlerSettings.json**.

2. Na **TaskThrottlerSettings.json** de ficheiros, alterar **DefaultBackupTasksThreshold** para um valor inferior, por exemplo, 5.

3. Guarde as alterações e feche o ficheiro.

4. No SQL Server, abra o Gestor de tarefas e reinicie o **serviço de coordenador de carga de trabalho de cópia de segurança do Azure**.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Pode executar um backup completo de uma réplica secundária

Não, esta funcionalidade não é suportada.

### <a name="do-successful-backup-jobs-create-alerts"></a>Tarefas de cópia de segurança bem-sucedida criar alertas

Não. Tarefas de cópia de segurança com êxito não geram alertas. Os alertas são enviados apenas para tarefas de cópia de segurança que não obedeçam.

### <a name="are-scheduled-backup-job-details-shown-in-the-jobs-menu"></a>Detalhes da tarefa de cópia de segurança agendada são apresentadas no menu tarefas

Não. O menu de tarefas mostra detalhes da tarefa ad hoc, mas não mostra as tarefas de cópia de segurança agendadas. Os alertas de tarefa falhada, se as tarefas de cópia de segurança agendadas falharem, encontrará todos os detalhes. Se pretender monitorizar todas as agendada e tarefas de cópia de segurança ad hoc, [utilizar o SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="if-i-select-a-sql-server-will-future-databases-automatically-be-added"></a>Se eu selecionar um SQL server serão bases de dados futuros adicionados automaticamente

Não. Quando configurar a proteção para um SQL server, se selecionar a caixa de verificação ao nível do servidor, ele adiciona todas as bases de dados. No entanto, se adicionar bases de dados para o SQL server depois de configurar a proteção, tem de adicionar manualmente as novas bases de dados para protegê-los. As bases de dados não estão incluídos automaticamente a proteção configurada.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Ao alterar o modelo de recuperação como reiniciar proteção

Se alterar o modelo de recuperação, acionar uma cópia de segurança completa e backups de log serão iniciada conforme esperado.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o Azure Backup, veja o exemplo do PowerShell para realizar a cópia de segurança de máquinas virtuais encriptadas.

> [!div class="nextstepaction"]
> [Fazer cópia de segurança de VM encriptada](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
