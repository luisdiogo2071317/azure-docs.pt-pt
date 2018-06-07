---
title: Cópia de segurança da base de dados do SQL Server para o Azure | Microsoft Docs
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
ms.date: 6/1/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: f48cbdb41f8ad7a3bad4546fa5cb77cf66780bed
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808507"
---
# <a name="back-up-sql-server-database-in-azure"></a>Criar cópias de segurança da base de dados do SQL Server no Azure

Bases de dados do SQL Server são cargas de trabalho críticas exigir baixa objetivo de ponto de recuperação (RPO) e a retenção de longo prazo. Cópia de segurança do Azure fornece uma solução de Serverbackup de SQL que necessita de zero infraestrutura, o que significa que nenhum servidor de cópia de segurança complexa, nenhum agente de gestão ou armazenamento de cópia de segurança para gerir. Cópia de segurança do Azure fornece uma gestão centralizada para as cópias de segurança em todos os servidores SQL, ou mesmo diferentes cargas de trabalho.

 Neste artigo que aprender:

> [!div class="checklist"]
> * Pré-requisitos para fazer cópias de segurança do SQL Server para o Azure
> * Criar e utilizar um cofre dos serviços de recuperação
> * Configurar cópias de segurança do SQL Server da base de dados
> * Definir uma política de cópia de segurança (ou retenção) para os pontos de recuperação
> * Como restaurar a base de dados

Antes de iniciar os procedimentos neste artigo, deverá ter um SQL Server em execução no Azure. [Pode utilizar máquinas virtuais do SQL marketplace para criar rapidamente um servidor de SQL](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Limitações de pré-visualização públicas

Os itens seguintes são as limitações conhecidas para a pré-visualização pública.

- A máquina virtual do SQL Server necessita de conectividade de internet para aceder aos endereços IP públicos do Azure. Para obter mais detalhes, consulte a secção [estabelecer conectividade de rede](backup-azure-sql-database.md#establish-network-connectivity).
- Pode proteger até 2000 bases de dados do SQL num cofre dos serviços de recuperação. Bases de dados adicionais do SQL devem ser armazenados num cofre dos serviços de recuperação separado.
- [Cópia de segurança de grupos de disponibilidade distribuída tem limitações](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- Instâncias de Cluster de ativação pós-falha (FCI) do SQL não são suportadas.
- Utilize o portal do Azure para configurar a cópia de segurança do Azure para proteger as bases de dados do SQL Server. Suporte para o Azure PowerShell, CLI e REST APIs não está atualmente disponível.

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

## <a name="supported-operating-systems-and-versions-of-sql-server"></a>Sistemas operativos suportados e versões do SQL server

Os seguintes sistemas operativos suportados e versões do SQL Server aplicam-se no SQL marketplace máquinas virtuais do Azure e tipo não mercado máquinas virtuais (onde do SQL Server é instalado manualmente).

### <a name="supported-operating-systems"></a>Sistemas operativos suportados

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux não é atualmente suportada.

### <a name="supported-versionseditions-of-sql-server"></a>Versões/edições suportadas do SQL Server

- SQL Server 2012 Enterprise, Standard, Web, programador, Express
- SQL Server 2014 Enterprise, Standard, Web, programador, Express
- SQL Server 2016 Enterprise, Standard, Web, programador, Express
- SQL 2017 Enterprise, Standard, Web, programador, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>Pré-requisitos para utilizar o Backup do Azure para proteger o SQL Server 

Antes de fazer uma cópia de segurança da base de dados do SQL Server, verifique as seguintes condições. :

- Identificar ou [criar um cofre dos serviços de recuperação](backup-azure-sql-database.md#create-a-recovery-services-vault) na mesma região ou região, que a máquina virtual alojar o SQL Server.
- [Verifique as permissões na máquina virtual](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) necessários para efetuar cópias de segurança de bases de dados SQL.
- [Máquina virtual do SQL tem conectividade de rede](backup-azure-sql-database.md#establish-network-connectivity).

Se estas condições existem no seu ambiente, avance para a secção [configurar o seu Cofre para proteger uma base de dados do SQL Server](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Se algum dos pré-requisitos não existirem, não continue a ler esta secção.


## <a name="establish-network-connectivity"></a>Estabelecer conectividade de rede

Para todas as operações, a máquina virtual do SQL Server necessita de conectividade para os endereços IP públicos do Azure. Efetuar operações de máquina virtual do SQL Server (por exemplo, a deteção de base de dados, configurar cópia de segurança, cópias de segurança agendadas, restaurar pontos de recuperação e assim sucessivamente) sem conectividade com os endereços IP públicos. Utilize qualquer uma das seguintes opções para fornecer um caminho de simples para o tráfego de cópia de segurança:

- Lista branca de IP do datacenter do Azure intervalos - lista branca do Azure intervalos de IP do datacenter, utilize o [center página para obter detalhes sobre os intervalos de IP e instruções de transferência](https://www.microsoft.com/download/details.aspx?id=41653). 
- Implementar um servidor de proxy HTTP para o encaminhamento de tráfego - quando estiver a cópia de segurança de uma base de dados do SQL Server numa VM, a extensão de cópia de segurança da VM utiliza a APIs de HTTPS para enviar os comandos de gestão para cópia de segurança do Azure e os dados ao Storage do Azure. A extensão de cópia de segurança também utiliza o Azure Active Directory (AAD) para autenticação. Encaminhar o tráfego de extensão de cópia de segurança para estes três serviços através do proxy HTTP, porque é o único componente configurado para acesso à internet pública.

Fala entre as opções é: capacidade de gestão, o controlo granular e o custo.

> [!NOTE]
>Etiquetas de serviço de cópia de segurança do Azure devem estar disponíveis através da disponibilidade geral.
>

| Opção | Vantagens | Desvantagens |
| ------ | ---------- | ------------- |
| Intervalos de IP da lista branca | Sem custos adicionais. <br/> Para abrir o acesso num NSG, utilize **conjunto AzureNetworkSecurityRule** cmdlet. | Complexo de gerir o afetado como alteram os intervalos de IP ao longo do tempo. <br/>Fornece acesso a totalidade do Azure, não apenas armazenamento.|
| Utilizar um proxy HTTP   | Controlo granular no proxy sobre o armazenamento URLs é permitida. <br/>Único ponto de acesso à internet às VMs. <br/> Não sujeitos a alterações do endereço IP do Azure. | Custos adicionais para executar uma VM com o software de proxy. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Definir as permissões para as VMs de SQL Server não marketplace

Para fazer uma cópia de segurança de uma máquina virtual, a cópia de segurança do Azure requer o **AzureBackupWindowsWorkload** extensão ser instalada. Se estiver a utilizar máquinas virtuais do Azure marketplace, avançar diretamente para [detetar bases de dados](backup-azure-sql-database.md#discover-sql-server-databases). Se a máquina virtual que aloja as bases de dados do SQL Server não foi criada no Azure Marketplace, conclua a secção seguinte para instalar a extensão e definir as permissões apropriadas. Para além de **AzureBackupWindowsWorkload** extensão, a cópia de segurança do Azure requer privilégios de administrador do sistema SQL ao proteger bases de dados do SQL Server. Ao detetar as bases de dados na máquina virtual, a cópia de segurança do Azure cria uma conta, NT Service\AzureWLBackupPluginSvc. Para cópia de segurança do Azure detetar as bases de dados do SQL Server, a conta NT Service\AzureWLBackupPluginSvc tem início de sessão SQL e permissões de administrador do sistema do SQL Server. O procedimento seguinte explica como fornecer estas permissões.

Para configurar permissões:

1. No [portal do Azure](https://portal.azure.com), abra o Cofre de serviços de recuperação que está a utilizar para proteger as bases de dados SQL.
2. No menu do dashboard do cofre, clique em **+ cópia de segurança** para abrir o **objetivo de cópia de segurança** menu.

   ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

3. No **objetivo de cópia de segurança** menu, no **onde está a carga de trabalho em execução?** menu, deixe **Azure** como predefinição.

4. No **que itens pretende cópia de segurança** menu, expanda o menu pendente e selecione **SQL Server numa VM do Azure**.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    O **objetivo de cópia de segurança** menu apresenta dois novos passos: **detetar DBs em VMs** e **Configurar cópia de segurança**. **Detetar DBs em VMs** inicia uma pesquisa de máquinas virtuais do Azure.

    ![Mostra os novos passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Clique em **Iniciar deteção** para procurar as máquinas virtuais desprotegidas na subscrição. Dependendo do número de máquinas virtuais desprotegidas na subscrição, pode demorar um pouco para percorrer todas as máquinas virtuais.

    ![Cópia de segurança pendente](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Quando uma máquina virtual não protegida for detetada, aparecerá na lista. Máquinas virtuais desprotegidas são listadas pelo respetivo grupo de recursos e nome de máquina virtual. É possível que várias máquinas virtuais têm o mesmo nome. No entanto, as máquinas virtuais com o mesmo nome pertencem a grupos de recursos diferente. Se uma máquina virtual esperada não aparecer na lista, consulte o artigo se a máquina virtual já está protegida para um cofre.

6. Na lista de máquinas virtuais, selecione a VM que contém a base de dados do SQL Server que pretende criar cópias de segurança e clique em **detetar DBs**. 

    O processo de deteção instala o **AzureBackupWindowsWorkload** extensão na máquina virtual. A extensão permite que o serviço de cópia de segurança do Azure comunicar com a máquina virtual, de modo que pode criar cópias de segurança de bases de dados do SQL Server. Uma vez instala a extensão, cópia de segurança do Azure cria a conta de serviço virtuais do Windows, **NT Service\AzureWLBackupPluginSvc**, na máquina virtual. A conta de serviço virtual requer permissões de administrador do sistema do SQL Server. Durante o serviço virtual conta processo de instalação, se vir o erro, **UserErrorSQLNoSysadminMembership**, consulte a secção [permissões de administrador do sistema SQL corrigir](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    A área de notificações mostra o progresso da deteção de base de dados. Dependendo de como muitas bases de dados se encontram na máquina virtual, pode demorar um pouco a conclusão da tarefa. Quando as bases de dados selecionadas tenham sido detetados, é apresentada uma mensagem de êxito.

    ![mensagem de notificação de implementação efetuada com êxito](./media/backup-azure-sql-database/notifications-db-discovered.png)

Depois de o associar a base de dados com o Cofre dos serviços de recuperação, o passo seguinte é [configurar a cópia de segurança](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>Corrigir permissões sysadmin do SQL Server

Durante o processo de instalação, se vir o erro, **UserErrorSQLNoSysadminMembership**, início de sessão no SQL Server Management Studio (SSMS) com uma conta que tenha a permissão de administrador do sistema SQL. Salvo se forem necessários permissões especiais, deverá conseguir utilizar a autenticação do Windows para reconhecer a conta.

1. No SQL Server, abra o **inícios de sessão/segurança** pasta.

    ![Abra as pastas do SQL Server e de segurança e de início de sessão para ver as contas](./media/backup-azure-sql-database/security-login-list.png)

2. Na pasta de inícios de sessão, clique com o botão direito e selecione **novo início de sessão**e no início de sessão - novo caixa de diálogo, clique em **pesquisa**

    ![Abra pesquisa no início de sessão - novo caixa de diálogo](./media/backup-azure-sql-database/new-login-search.png)

3. Dado que a conta de serviço virtuais do Windows, **NT Service\AzureWLBackupPluginSvc** já foi criado durante o registo de máquina virtual e uma fase de deteção do SQL Server, introduza o nome da conta como é apresentado no  **Introduza o nome de objeto a selecionar** caixa de diálogo. Clique em **verificar nomes** para resolver o nome. 

    ![Clique no botão de verificação de nomes para resolver o nome de serviço desconhecido](./media/backup-azure-sql-database/check-name.png)

4. Clique em **OK** para fechar a caixa de diálogo Selecionar utilizador ou grupo.

5. No **funções de servidor** caixa de diálogo, certifique-se de que o **sysadmin** função está selecionada. Em seguida, clique em **OK** fechar **início de sessão - novo**.

    ![Certifique-se que a função de servidor sysadmin é seleccionada](./media/backup-azure-sql-database/sysadmin-server-role.png)

    As permissões necessárias agora devem existir.

6. Apesar de corrigir o erro de permissões, ainda precisa de associar a base de dados com o Cofre de serviços de recuperação. No portal do Azure **servidores protegidos** lista, clique com o botão direito do rato no servidor no registo de erros e selecione **detetar DBs**.

    ![Certifique-se de que o servidor tem as permissões adequadas](./media/backup-azure-sql-database/check-erroneous-server.png)

    A área de notificações mostra o progresso da deteção de base de dados. Dependendo de como muitas bases de dados se encontram na máquina virtual, pode demorar um pouco a conclusão da tarefa. Quando as bases de dados selecionadas foram encontrados, é apresentada uma mensagem de êxito, na área de notificações.

    ![mensagem de notificação de implementação efetuada com êxito](./media/backup-azure-sql-database/notifications-db-discovered.png)

Depois de o associar a base de dados com o Cofre dos serviços de recuperação, o passo seguinte é [configurar a cópia de segurança](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detetar bases de dados do SQL Server

Cópia de segurança do Azure pode detetar todas as bases de dados numa instância do SQL Server, pelo que pode protegê-los pelos requisitos de cópia de segurança. Utilize o procedimento seguinte para identificar a máquina virtual que aloja as bases de dados do SQL Server. Depois de identificar a máquina virtual, a cópia de segurança do Azure instala uma extensão de simples para detetar as bases de dados do SQL server.

1. Inicie sessão na sua subscrição no [portal do Azure](https://portal.azure.com/).
2. No menu da esquerda, selecione **todos os serviços**.

    ![Escolha a opção de todos os serviços no menu principal](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Na caixa de todos os serviços diálogo, escreva *dos serviços de recuperação*. À medida que começa a escrever, a sua entrada filtra a lista de recursos. Depois de o vir, selecione **cofres dos serviços de recuperação**.

    ![Tipo dos serviços de recuperação na caixa de diálogo todos os serviços](./media/backup-azure-sql-database/all-services.png) <br/>

    É apresentada a lista de cofres dos serviços de recuperação na subscrição. 

4. Na lista de cofre dos serviços de recuperação, selecione o cofre que pretende utilizar para proteger as bases de dados do SQL Server.

5. No menu do dashboard do cofre, clique em **+ cópia de segurança** para abrir o **objetivo de cópia de segurança** menu.

   ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

6. No **objetivo de cópia de segurança** menu, no **onde está a carga de trabalho em execução?** menu, deixe **Azure** como predefinição.

7. No **que itens pretende cópia de segurança** menu, expanda o menu pendente e selecione **SQL Server numa VM do Azure**.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Uma vez selecionado, o **objetivo de cópia de segurança** menu apresenta dois passos: detetar DBs em VMs e configurar a cópia de segurança. 

    ![Mostra os novos passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Clique em **Iniciar deteção** para procurar as máquinas virtuais desprotegidas na subscrição. Dependendo do número de máquinas virtuais desprotegidas na subscrição, pode demorar um pouco para percorrer todas as máquinas virtuais.

    ![Cópia de segurança pendente](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Quando uma máquina virtual não protegida for detetada, aparecerá na lista. Várias máquinas virtuais podem ter o mesmo nome. No entanto, várias máquinas virtuais com o mesmo nome pertencem a grupos de recursos diferente. As máquinas virtuais desprotegidas são listadas pelo respetivo grupo de recursos e nome de máquina virtual. Se uma máquina virtual esperada não estiver listada, consulte o artigo se que a máquina virtual já está protegida para um cofre.

9. Na lista de máquinas virtuais, selecione a caixa de verificação da máquina virtual que contém as bases de dados do SQL Server que pretende proteger e clique em **detetar DBs**.

    Cópia de segurança do Azure Deteta todas as bases de dados do SQL Server na máquina virtual. Para obter informações sobre o que acontece durante a fase de deteção de base de dados, consulte a secção seguinte, [operações de back-end ao detetar as bases de dados SQL](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). Após a deteção de bases de dados SQL, estará pronto para [configurar a tarefa de cópia de segurança](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>Operações de back-end ao detetar as bases de dados SQL

Quando utiliza o **detetar DBs** ferramenta de cópia de segurança do Azure executa as seguintes operações em segundo plano:

- regista a máquina virtual com o Cofre dos serviços de recuperação para cópia de segurança da carga de trabalho. Todas as bases de dados na máquina virtual registada só podem ser feitas neste cofre dos serviços de recuperação. 

- instala o **AzureBackupWindowsWorkload** extensão na máquina virtual. Cópia de segurança de uma base de dados do SQL Server é uma solução sem agente, ou seja, com a extensão instalada na máquina virtual, está instalado nenhum agente na base de dados do SQL Server.

- cria a conta de serviço, **NT Service\AzureWLBackupPluginSvc**, na máquina virtual. Todas as operações de cópia de segurança e restauro utilizam a conta de serviço. **NT Server\AzureWLBackupPluginSvc** necessita de permissões de administrador do sistema do SQL Server. Todas as máquinas virtuais do SQL Server Marketplace vêm com SqlIaaSExtension instalado e AzureBackupWindowsWorkload utiliza SQLIaaSExtension automaticamente obter as permissões necessárias. Se a máquina virtual não tem SqlIaaSExtension instalado, a operação de detetar DB falhar e receberá a mensagem de erro, **UserErrorSQLNoSysAdminMembership**. Para adicionar a permissão de administrador do sistema para cópia de segurança, siga as instruções em [configurar permissões de cópia de segurança do Azure para as VMs do SQL Server de tipo não mercado](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![Selecione a vm e a base de dados](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>Configurar cópia de segurança da base de dados do SQL Server

Cópia de segurança do Azure fornece serviços de gestão para proteger as bases de dados do SQL Server e gerir tarefas de cópia de segurança. As capacidades de monitorização e gestão dependem do seu Cofre de serviços de recuperação. Para configurar a proteção para a base de dados do SQL Server:

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL Server.

2. No menu do dashboard do cofre, clique em **+ cópia de segurança** para abrir o **objetivo de cópia de segurança** menu.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

3. No **objetivo de cópia de segurança** menu, no **onde está a carga de trabalho em execução?** menu, deixe **Azure** como predefinição.

4. No **que itens pretende cópia de segurança** menu, expanda o menu pendente e selecione **SQL Server numa VM do Azure**.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Uma vez selecionado, o **objetivo de cópia de segurança** menu apresenta dois passos: detetar DBs em VMs e configurar a cópia de segurança. Se já leu este artigo por ordem, já que já descobriu as máquinas virtuais desprotegidas e este Cofre está registado com uma máquina virtual. Agora, está pronto para configurar a proteção para as bases de dados do SQL Server.

5. No menu do objetivo de cópia de segurança, clique em **Configurar cópia de segurança**.

    ![Mostra os novos passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    O serviço de cópia de segurança do Azure apresenta todas as instâncias SQL com bases de dados autónomo, bem como grupos de disponibilidade SQL AlwaysOn. Para ver as bases de dados autónomo na instância do SQL Server, clique na divisa junto ao nome da instância para ver as bases de dados. As imagens seguintes mostram os exemplos de uma instância autónoma e um grupo de disponibilidade Always On.

    > [!NOTE]
    > Cópias de segurança completas e diferenciais ser efetuadas a partir do nó principal, como plataforma SQL tem essa limitação. Cópia de segurança do registo pode acontecer com base na sua preferência de cópia de segurança. Devido a esta limitação, o nó principal tem de estar registado.
    >

    ![Lista de bases de dados na instância do SQL Server](./media/backup-azure-sql-database/discovered-databases.png)

    Clique na divisa junto a grupos de Disponibilidade AlwaysOn para ver a lista de bases de dados.

    ![Lista de bases de dados num grupo de Disponibilidade AlwaysOn](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Na lista de bases de dados, Selecionar tudo o que pretende proteger e clique em **OK**.

    ![Selecione várias bases de dados para os proteger](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Pode selecionar até 50 bases de dados em simultâneo. Se pretender proteger mais de 50 bases de dados, certifique-transmite vários. Depois de proteger as bases de dados primeiro 50, repita este passo para proteger o seguinte conjunto de bases de dados.
    > [!Note] 
    > Para otimizar cargas de cópia de segurança, cópias de segurança do Azure de quebras de tarefas de cópia de segurança grandes em lotes de vários. O número máximo de uma tarefa de cópia de segurança de bases de dados é 50.
    >
    >

7. Para criar ou escolher uma política de cópia de segurança, o **cópia de segurança** menu, selecione **política de cópia de segurança**, para abrir o menu.

    ![Selecione a opção da política de cópia de segurança](./media/backup-azure-sql-database/select-backup-policy.png)

8. Do **escolher a política de cópia de segurança** menu pendente, escolha uma política de cópia de segurança e clique em **OK**. Para obter informações sobre como criar a sua própria política de cópia de segurança, consulte a secção [definir uma política de cópia de segurança](backup-azure-sql-database.md#define-a-backup-policy).

    ![Escolha uma política de cópia de segurança a partir do menu pendente](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    No menu de política de cópia de segurança, do **escolher a política de cópia de segurança** menu pendente, pode escolher: 
    - a política de HourlyLogBackup predefinida, 
    - uma política de cópia de segurança existente que criou anteriormente para o SQL,
    - para [definir uma nova política](backup-azure-sql-database.md#define-a-backup-policy) com base no seu objetivo de ponto de recuperação (RPO) e o período de retenção. 

    > [!Note]
    > Cópia de segurança do Azure suporta a retenção de longo prazo com base no esquema de cópia de segurança avô-pai-filho para otimizar o consumo de armazenamento de back-end ao satisfazer as necessidades de conformidade.
    >

9. Assim que tiver optado por uma política de cópia de segurança, no **menu de cópia de segurança** clique **ativar cópia de segurança**.

    ![Ativar a política de cópia de segurança que escolheu](./media/backup-azure-sql-database/enable-backup-button.png)

    Pode controlar o progresso da configuração na área de notificações do portal.

    ![área de notificação de vista](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Definir uma política de cópia de segurança

Uma política de cópia de segurança define uma matriz de quando as cópias de segurança são executadas e quanto tempo as cópias de segurança são mantidas. Pode utilizar o Backup do Azure para agendar três tipos de cópia de segurança de bases de dados SQL:

* Cópia de segurança completa - uma cópia de segurança completa da base de dados cria cópias de segurança da base de dados completa. Cópia de segurança completa contém todos os dados numa base de dados específica ou conjunto de grupos de ficheiros ou de ficheiros e suficiente registo para recuperar dados. No máximo, pode acionar uma cópia de segurança completa por dia. Pode optar por colocar um completo cópia de segurança num intervalo diário ou semanal. 
* Cópia de segurança diferencial - uma cópia de segurança diferencial baseia-se a cópia de segurança de dados completa anterior mais recente. Uma cópia de segurança diferencial captura apenas os dados que tem sido alterado desde a cópia de segurança completa. No máximo, pode acionar uma cópia de segurança diferencial por dia. Não é possível configurar uma cópia de segurança completa e uma cópia de segurança diferencial no mesmo dia.
* Cópia de segurança de registo transações - uma cópia de segurança de registo permite que o restauro de ponto no tempo até específico segundo. No máximo, pode configurar cópias de segurança do registo transacional a cada 15 minutos.

Política é criada no Cofre de serviços de recuperação nível. Se tiver vários cofres, os cofres podem utilizar a mesma política de cópia de segurança, mas tem de aplicar a política de cópia de segurança para cada cofre. Ao criar uma política de cópia de segurança, diariamente, cópia de segurança completa é a predefinição. Pode adicionar uma cópia de segurança diferencial, mas apenas se mudar de cópias de segurança completas ocorra semanal. O procedimento seguinte explica como criar uma política de cópia de segurança para um SQL server numa máquina virtual do Azure.

Para criar uma política de cópia de segurança

1. No menu de política de cópia de segurança, do **escolher a política de cópia de segurança** menu pendente, selecione **criar novo**.

   ![Criar nova política de cópia de segurança](./media/backup-azure-sql-database/create-new-backup-policy.png)

    Muda o menu de política de cópia de segurança para fornecer os campos necessários para qualquer política de cópia de segurança de servidor SQL.

   ![novos campos de política de cópia de segurança](./media/backup-azure-sql-database/blank-new-policy.png)

2. No **nome da política**, forneça um nome. 

3. Cópia de segurança completa é obrigatória. Pode aceitar os valores predefinidos para a cópia de segurança completa, ou pode clicar **cópia de segurança completa** para editar a política.

    ![novos campos de política de cópia de segurança](./media/backup-azure-sql-database/full-backup-policy.png)

    Na política de cópia de segurança completa, escolha diária ou semanalmente para a frequência. Se escolher diariamente, escolha a hora e fuso horário, quando a tarefa de cópia de segurança começa. Se escolher cópias de segurança completas diárias, não é possível criar cópias de segurança diferencial.

   ![definição de intervalo diário](./media/backup-azure-sql-database/daily-interval.png)

    Se escolher semanal, escolha o dia da semana, hora e fuso horário quando começa a tarefa de cópia de segurança.

   ![definição de intervalo semanal](./media/backup-azure-sql-database/weekly-interval.png)

4. Por predefinição, estão selecionadas todas as opções de período de retenção (diárias, semanais, mensais e anuais). Desmarque qualquer limite do intervalo de retenção que não pretende e definir os intervalos para utilizar. No menu de política de cópia de segurança completa, clique em **OK** para aceitar as definições.

   ![definição do intervalo de intervalo de retenção](./media/backup-azure-sql-database/retention-range-interval.png)

    Pontos de recuperação são etiquetados para a retenção, com base no respetivo período de retenção. Por exemplo, se selecionar um diariamente, cópia de segurança completa de cópia de segurança completa apenas um é acionada por dia. Dependendo de retenção semana, cópia de segurança o dia específico é marcada e mantida com base no período de retenção semanais. O período de retenção mensal e anual comporta-se da mesma forma.

5. Para adicionar uma política de cópia de segurança diferencial, clique em **cópia de segurança diferencial** para abrir o menu. 

   ![abrir a política diferencial](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    No menu de política de cópia de segurança diferencial, selecione **ativar** para abrir os controlos de frequência e de retenção. Pode acionar, no máximo, uma cópia de segurança diferencial por dia.
    > [!Important] 
    > No máximo, as cópias de segurança diferenciais podem ser mantidas durante 180 dias. Se precisar de retenção mais longa, tem de utilizar cópias de segurança completas, não é possível utilizar cópias de segurança diferenciais.
    >

   ![Editar política diferencial](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Clique em **OK** para guardar a política e regressar ao menu principal de política de cópia de segurança.

6. Para adicionar uma política de cópia de segurança do registo transacional, clique em **cópia de segurança do registo** para abrir o menu. No menu de cópia de segurança do registo, selecione **ativar**e defina os controlos de frequência e de retenção. Cópias de segurança de registo podem ocorrer com frequência de 15 minutos e podem ser mantidas até 35 dias. Clique em **OK** para guardar a política e regressar ao menu principal de política de cópia de segurança.

   ![Editar política de cópia de segurança do registo](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. Escolha se pretende ativar a compressão de cópia de segurança do SQL Server. A compressão está desativada por predefinição.

    No back-end, cópia de segurança do Azure utiliza a compressão cópia de segurança nativa do SQL Server.

8. Quando todas as edições efetuadas para a política de cópia de segurança, clique em **OK**. 

   ![período de retenção diferencial](./media/backup-azure-sql-database/differential-backup-policy.png)

## <a name="restore-a-sql-database"></a>Restaurar uma base de dados do SQL Server

Cópia de segurança do Azure fornece funcionalidades para restaurar as bases de dados individuais para uma data específica ou a hora, até um específico segundo, utilizando cópias de segurança de registo de transações. Com base em tempos de restauro fornecer, cópia de segurança do Azure determina automaticamente apropriados completo, valor diferencial e a cadeia de cópias de segurança do registo necessárias para restaurar os dados.

Em alternativa, pode selecionar uma específica completo valor diferencial cópia de segurança ou restaurar um ponto de recuperação específico em vez de um tempo específico.

Para restaurar uma base de dados

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL Server.

2. No dashboard do cofre, selecione **utilização** itens de cópia de segurança para abrir o menu de itens de cópia de segurança.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. No **itens de cópia de segurança** menu, selecione o tipo de cópia de segurança de gestão, **SQL na VM do Azure**. 

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    A lista de itens de cópia de segurança ajusta para mostrar a lista de bases de dados SQL. 

4. Na lista de bases de dados do SQL Server, selecione a base de dados que pretende restaurar.

    ![Selecione o SQL Server na VM do Azure a partir da lista](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Quando seleciona a base de dados, é aberto o menu. Este menu fornece os detalhes de cópia de segurança para a base de dados, incluindo:

    * o mais antigo e a mais recente restaurar pontos,
    * registo de estado de cópia de segurança para as últimas 24 horas (para bases de dados completo em massa recuperação iniciada modelo e, se configurado para cópias de segurança do registo transacional)

5. No menu de base de dados selecionada, clique em **restaurar a base de dados** para abrir o menu de restauro.

    ![Selecione restaurar base de dados](./media/backup-azure-sql-database/restore-db-button.png)

    O **restaurar** é aberto o menu e não assim o **restauro da configuração** menu. O **restauro da configuração** menu é o primeiro passo na configuração do restauro. Este menu, selecione onde pretende restaurar os dados. As opções são:
    * Alternate localização - utilize esta opção se pretende restaurar a base de dados para uma localização alternativa mantendo a base de dados de origem original.
    * Substitua a base de dados - restaura os dados para a mesma instância do SQL Server como origem original. O efeito desta situação é que substituir a base de dados original.

    > [!Important]
    > Se a base de dados selecionada pertence a um grupo de disponibilidade Always On, SQL Server não permite a base de dados ser substituído. Neste caso, apenas o **localização alternativa** opção está ativada.
    >

    ![Clique em configurar para abrir o menu de configuração de restauro](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restaurar para uma localização alternativa

Este procedimento explica restaurar os dados para uma localização alternativa. Se pretender substituir a base de dados ao restaurar, avance para a secção [restaurar e substituir a base de dados](backup-azure-sql-database.md#restore-and-overwrite-the-database). Este procedimento assume que tem o seu Cofre de serviços de recuperação aberto e está no menu de restauro da configuração. Se não, comece pela secção, [restaurar uma base de dados do SQL Server](backup-azure-sql-database.md#restore-a-sql-database).

O **servidor** menu pendente mostra apenas os SQL servers registados no Cofre de serviços de recuperação. Se o servidor que pretende não se encontra no **servidor** lista, consulte a secção [detetar bases de dados](backup-azure-sql-database.md#discover-sql-server-databases) para localizar o servidor. Durante o processo de base de dados de deteção, todos os novos servidores registados no Cofre de serviços de recuperação.

1. No **restauro da configuração** menu:

    * Selecione **localização alternativa**,
    * para **servidor**, escolha o SQL server onde pretende restaurar a base de dados.
    * no **instância** menu pendente, escolha uma instância do SQL Server
    * no **nome de base de dados restaurada** caixa de diálogo, forneça o nome da base de dados de destino.
    * Se aplicável, selecione **substituir se a base de dados com o mesmo nome já existe na instância selecionada do SQL Server**.
    * Clique em **OK** para concluir a configuração de destino e mover para escolher um ponto de restauro.

    ![Selecionar Localização alternativa, a instância e o nome no menu de configuração de restauro](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. No **selecionar ponto de restauro** menu, pode escolher um inicia (ponto no tempo) ou ponto de restauro completos do & valor diferencial. Se pretender restaurar para um registo de ponto no tempo específico, continue com este passo. Se pretender restaurar um ponto de restauro completo ou valor diferencial, avançar diretamente para o passo 3.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    O ponto de restauro de tempo só está disponível para cópias de segurança de registo para bases de dados com completo & modelo de recuperação de registadas em massa. Para restaurar para um ponto no tempo específico:

    1. Selecione **registos (ponto no tempo)** como a opção de restauro.

        ![Escolha o tipo de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Em **restaurar a data/hora**, clique no ícone de calendário para abrir o calendário. Datas a negrito contenham pontos de recuperação e a data atual é realçada. Selecione uma data no calendário com pontos de recuperação. Não é possível selecionar datas com sem pontos de recuperação.

        ![Abra o calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis num intervalo contínuo.

    3. Utilizar o gráfico de linha cronológica, ou a caixa de diálogo de tempo, especifique um tempo específico para o ponto de recuperação e clique em **OK** para concluir o passo de ponto de restauro.
    
       ![Abra o calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        O **selecionar ponto de restauro** menu fecha-se e o **configuração avançada** é aberto o menu.

       ![menu Configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Do **configuração avançada** menu:

        * Para manter a base de dados inoperacional após o restauro, no **restaurar sem recuperação** menu, selecione **ativado**.
        * Se pretender alterar a localização do restauro no servidor de destino, forneça um caminho novo no **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    5. No **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso das tarefas de restauro da base de dados.

       ![menu Configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)

3. No **selecionar ponto de restauro** menu, escolha um ponto de recuperação. Pode escolher um inicia (ponto no tempo) ou completos do & valor diferencial. Se pretender restaurar um registo de ponto no tempo, volte ao passo 2. Este passo restaura um ponto de restauro completa ou diferencial específico. Com esta opção, pode ver pontos de recuperação de todas as completo e valor diferencial para os últimos 30 dias. Se pretender ver pontos de recuperação com mais de 30 dias, clique em **filtro** para abrir o **pontos de restauro de filtro** menu. Se escolher um ponto de recuperação diferencial, cópia de segurança do Azure restaura primeiro o ponto de recuperação completa adequadas e, em seguida, aplica-se o ponto de recuperação diferencial selecionado.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. No **selecionar ponto de restauro** menu, selecione **completos do & valor diferencial**.

       ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        É apresentada a lista de pontos de recuperação disponíveis.

    2. Na lista de pontos de recuperação, selecione um ponto de recuperação e clique em **OK** para concluir o procedimento de ponto de restauro. 

        ![Escolher o ponto de recuperação completa](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        O **ponto de restauro** menu fecha-se e o **configuração avançada** é aberto o menu.

        ![menu Configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Do **configuração avançada** menu:

        * Para manter a base de dados inoperacional após o restauro, no **restaurar sem recuperação** menu, selecione **ativado**. **Restaurar sem recuperação** está desativada por predefinição.
        * Se pretender alterar a localização do restauro no servidor de destino, forneça um caminho novo no **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    4. No **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso das tarefas de restauro da base de dados.

       ![menu Configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Restaurar e substituir a base de dados

Este procedimento explica restaurar os dados e a substituir a base de dados. Se pretende restaurar para uma localização alternativa, avance para a secção [restaurar para uma localização alternativa](backup-azure-sql-database.md#restore-to-an-alternate-location). Este procedimento assume que tem o seu Cofre de serviços de recuperação aberto e tenham de **restauro da configuração** menu (ver a imagem seguinte). Se não, comece pela secção, [restaurar uma base de dados do SQL Server](backup-azure-sql-database.md#restore-a-sql-database).

![Clique em configurar para abrir o menu de configuração de restauro](./media/backup-azure-sql-database/restore-overwrite-db.png)

O **servidor** menu pendente mostra apenas os SQL servers registados no Cofre de serviços de recuperação. Se o servidor que pretende não se encontra no **servidor** lista, consulte a secção [detetar bases de dados](backup-azure-sql-database.md#discover-sql-server-databases) para localizar o servidor. Durante o processo de base de dados de deteção, todos os novos servidores registados no Cofre de serviços de recuperação.

1. No **restauro da configuração** menu, selecione **substituir DB** e clique em **OK** para concluir a configuração de destino. 

   ![Clique em substituir a base de dados](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    O **servidor**, **instância**, e **nome de base de dados restaurada** caixas de diálogo não são necessárias.

2. No **selecionar ponto de restauro** menu, pode escolher um inicia (ponto no tempo) ou ponto de restauro completos do & valor diferencial. Se pretender restaurar um registo de ponto no tempo, continue com este passo. Se pretender restaurar um ponto de restauro completos do & valor diferencial, avançar diretamente para o passo 3.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    O ponto de restauro de tempo só está disponível para cópias de segurança de registo para bases de dados com completo & modelo de recuperação de registadas em massa. Para escolher um ponto no tempo de restauro para um específico segundo:

    1. Selecione **registos (ponto no tempo)** como a opção de restauro.

        ![Escolha o tipo de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Em **restaurar a data/hora**, clique no ícone de calendário para abrir o calendário. Datas a negrito contenham pontos de recuperação e a data atual é realçada. Selecione uma data no calendário com pontos de recuperação. Não é possível selecionar datas com sem pontos de recuperação.

        ![Abra o calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis.

    3. Utilizar o gráfico de linha cronológica, ou a caixa de diálogo de tempo, especifique um tempo específico para o ponto de recuperação e clique em **OK** para concluir o passo de ponto de restauro.
    
       ![Abra o calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        O **selecionar ponto de restauro** menu fecha-se e o **configuração avançada** é aberto o menu.

       ![menu Configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Do **configuração avançada** menu:

        * Para manter a base de dados inoperacional após o restauro, no **restaurar sem recuperação** menu, selecione **ativado**.
        * Se pretender alterar a localização do restauro no servidor de destino, forneça um caminho novo no **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    5. No **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso das tarefas de restauro da base de dados.

       ![menu Configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)

3. No **selecionar ponto de restauro** menu, escolha um ponto de recuperação. Pode escolher um inicia (ponto no tempo) ou completos do & valor diferencial. Se pretender restaurar um registo de ponto no tempo, volte ao passo 2. Este passo restaura um ponto de restauro completa ou diferencial específico. Com esta opção, pode ver pontos de recuperação de todas as completo e valor diferencial para os últimos 30 dias. Se pretender ver pontos de recuperação com mais de 30 dias, clique em **filtro** para abrir o **pontos de restauro de filtro** menu. Se escolher um ponto de recuperação diferencial, cópia de segurança do Azure restaura primeiro o ponto de recuperação completa adequadas e, em seguida, aplica-se o ponto de recuperação diferencial selecionado.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. No **selecionar ponto de restauro** menu, selecione **completos do & valor diferencial**.

       ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        É apresentada a lista de pontos de recuperação disponíveis.

    2. Na lista de pontos de recuperação, selecione um ponto de recuperação e clique em **OK** para concluir o procedimento de ponto de restauro. 

        ![Escolher o ponto de recuperação completa](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        O **ponto de restauro** menu fecha-se e o **configuração avançada** é aberto o menu.

        ![menu Configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Do **configuração avançada** menu:

        * Para manter a base de dados inoperacional após o restauro, no **restaurar sem recuperação** menu, selecione **ativado**. **Restaurar sem recuperação** está desativada por predefinição.
        * Se pretender alterar a localização do restauro no servidor de destino, forneça um caminho novo no **destino** coluna.
        * Clique em **OK** para aprovar as definições e fechar **configuração avançada**.

    4. No **restaurar** menu, clique em **restaurar** para iniciar a tarefa de restauro. Na área de notificações, pode controlar o progresso. Também pode controlar o progresso das tarefas de restauro da base de dados.

       ![menu Configuração avançada](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Gerir as operações de cópia de segurança do Azure para o SQL Server em VMs do Azure

Esta secção fornece informações sobre as várias cópia de segurança do Azure operações de gestão disponíveis para o SQL Server em virtual machines do Azure. Existem as seguintes operações de alto nível:

* Monitorizar Tarefas
* Alertas de cópias de segurança
* Interrompa a proteção uma base de dados do SQL Server
* Retomar a proteção para uma base de dados do SQL Server
* Acionar uma tarefa de cópia de segurança ad hoc
* Anular o registo do SQL server

### <a name="monitor-jobs"></a>Monitorizar Tarefas

Cópia de segurança do Azure utiliza as APIs nativas da SQL Server para todas as operações de cópia de segurança. Utilizar as APIs nativas, pode obter todas as informações de tarefa do [tabela do SQL backupset](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) na base de dados msdb. Além disso, a cópia de segurança do Azure mostra todos os manualmente accionada ou adhoc, as tarefas no portal de tarefas de cópia de segurança. As tarefas disponíveis na inclusão de portal: todos os configurar operações de cópia de segurança, as operações de restauro, registo e operações de base de dados de detetar e parar as operações de cópia de segurança. Todas as tarefas agendadas também podem ser monitorizadas com a análise de registos do OMS. Através da análise do registo remove clutter de tarefas e proporciona flexibilidade granular para monitorização ou filtrar tarefas específicas.

![menu Configuração avançada](./media/backup-azure-sql-database/jobs-list.png)

### <a name="backup-alerts"></a>Alertas de cópias de segurança

Com cópias de segurança de registo a cada 15 minutos a ocorrer, a monitorização ocasionalmente as tarefas de cópia de segurança pode ser tedious. Cópia de segurança do Azure planeada para esta situação potencialmente tedious fornecendo alertas por e-mail acionadas por qualquer falha de cópia de segurança. Alertas são consolidados ao nível da base de dados por código de erro. Por exemplo, se uma base de dados tem várias falhas de cópia de segurança, em vez de receber um alerta para cada falha, receberá correio eletrónico para a primeira falha. Pode, em seguida, inicie sessão no portal do Azure para monitorizar falhas subsequentes para essa base de dados. 

Monitorizar alertas cópias de segurança:

1. Inicie sessão na sua subscrição do Azure no [portal do Azure](https://portal.azure.com).

2. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL Server.

3. No menu do cofre dos serviços de recuperação, selecione **eventos e alertas**. 

   ![menu Configuração avançada](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. No **eventos e alertas** menu, selecione **alertas de cópia de segurança** para ver a lista de alertas.

   ![menu Configuração avançada](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Interrompa a proteção uma base de dados do SQL Server

Se deixa de proteger uma base de dados do SQL Server, o Backup do Azure pede-lhe se pretende manter os pontos de recuperação. Existem duas formas de parar a proteção de base de dados SQL:

* Todas as tarefas de cópia de segurança futuras de parar e eliminar todos os pontos de recuperação
* Parar todas as tarefas de cópia de segurança futuras, mas deixe os pontos de recuperação 

Abandonar o fileparser os pontos de recuperação acarreta um custo conforme os pontos de recuperação para o SQL transportem protegida a instância do SQL preços encargos e o armazenamento consumido. Para obter mais informações sobre os preços da cópia de segurança do Azure para o SQL Server, consulte o [cópia de segurança do Azure página de preços](https://azure.microsoft.com/pricing/details/backup/). Para parar a proteção para a base de dados:

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL Server.

2. No dashboard do cofre, selecione **utilização** itens de cópia de segurança para abrir o menu de itens de cópia de segurança.

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. No **itens de cópia de segurança** menu, selecione o tipo de cópia de segurança de gestão, **SQL na VM do Azure**. 

    ![Clique em + cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    A lista de itens de cópia de segurança ajusta para mostrar a lista de bases de dados SQL. 

4. Na lista de bases de dados do SQL Server, selecione a base de dados que pretende parar a proteção.

    ![Selecione o SQL Server na VM do Azure a partir da lista](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Quando seleciona a base de dados, é aberto o menu. 

5. No menu de base de dados selecionada, clique em **cópia de segurança de paragem** para parar de proteger a base de dados.

    ![Selecione restaurar base de dados](./media/backup-azure-sql-database/stop-db-button.png)

    O **parar a cópia de segurança** é aberto o menu.

6. No **parar a cópia de segurança** menu, optar por manter dados de cópia de segurança ou eliminar dados de cópia de segurança. Opcionalmente, pode indicar um motivo para parar a proteção e um comentário.

    ![Selecione restaurar base de dados](./media/backup-azure-sql-database/stop-backup-button.png)

7. Clique em **cópia de segurança de paragem** interromper a proteção na base de dados. 

### <a name="resume-protection-for-a-sql-database"></a>Retomar a proteção para uma base de dados do SQL Server

Se o **reter dados de cópia de segurança** opção tiver sido selecionada quando parar a proteção para a base de dados do SQL Server, é possível retomar a proteção. Se os dados de cópia de segurança não foi mantidos, não é possível retomar a proteção. 

1. Para retomar a proteção para a base de dados do SQL Server, abra o item de cópia de segurança e clique em **retomar a cópia de segurança**.

    ![retomar a proteção de base de dados](./media/backup-azure-sql-database/resume-backup-button.png)

   Abre o menu de política de cópia de segurança.

2. Do **política de cópia de segurança** menu, selecione uma política e clique em **guardar**.

### <a name="trigger-an-adhoc-backup"></a>Acione uma cópia de segurança ad hoc

Pode acionar uma cópia de segurança ad hoc sempre que pretender. Existem quatro tipos de cópias de segurança ad hoc. Para obter detalhes sobre cada tipo, consulte o artigo [cópias de segurança de tipos de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Cópia de segurança completa
* Copiar a cópia de segurança completa apenas
* Cópia de segurança diferencial
* Cópia de segurança do registo

### <a name="unregister-a-sql-server"></a>Anular o registo do SQL Server

Para anular o registo do SQL server após a remoção da proteção, mas antes de eliminar o Cofre

1. Abra o Cofre dos serviços de recuperação registado com a máquina virtual do SQL Server.

2. No **gerir** secção do menu do cofre, clique em **infraestrutura de cópia de segurança**.  

   ![retomar a proteção de base de dados](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. No **servidores de gestão** secção, clique em **servidores protegidos**.

   ![retomar a proteção de base de dados](./media/backup-azure-sql-database/protected-servers.png)

    Abre o menu de servidores protegidos. 

4. No **servidores protegidos** menu, selecione o servidor que pretende anular o registo. Se pretender eliminar o cofre, tem de anular o registo todos os servidores.

5. No menu de servidores protegido, o servidor protegido com o botão direito e selecione **eliminar**. 

   ![retomar a proteção de base de dados](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o Azure Backup, veja o exemplo do PowerShell para realizar a cópia de segurança de máquinas virtuais encriptadas.

> [!div class="nextstepaction"]
> [Fazer cópia de segurança de VM encriptada](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
