---
title: Cópia de segurança de bases de dados do SQL Server para o Azure | Documentos da Microsoft
description: Este tutorial explica como criar cópias de segurança do SQL Server para o Azure. O artigo também explica a recuperação do SQL Server.
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
ms.date: 08/02/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 72d48bd1716e1b62ae92f8317f3f9611ac463453
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211507"
---
# <a name="back-up-sql-server-databases-to-azure"></a>Fazer cópias de segurança de bases de dados do SQL Server para o Azure

Bases de dados do SQL Server são cargas de trabalho críticas que exigem um objetivo de ponto de recuperação (RPO) e a retenção de longa duração. O Azure Backup fornece uma solução de cópia de segurança do SQL Server que requer zero infraestrutura: nenhum servidor de cópia de segurança complexa, nenhum agente de gestão e sem armazenamento de cópia de segurança para gerir. O Azure Backup fornece uma gestão centralizada para as cópias de segurança em todos os servidores que estejam a executar o SQL Server, ou até mesmo diferentes cargas de trabalho.

Neste artigo, vai aprender:

> [!div class="checklist"]
> * Pré-requisitos para criar uma instância do SQL Server no Azure.
> * Como criar e utilizar um cofre dos serviços de recuperação.
> * Como configurar cópias de segurança do SQL Server da base de dados.
> * Como definir uma política de cópia de segurança (ou de retenção) para os pontos de recuperação.
> * Como restaurar a base de dados.

Antes de iniciar os procedimentos neste artigo, deve ter uma instância do SQL Server que está em execução no Azure. [Utilize as máquinas virtuais do SQL Marketplace para criar rapidamente uma instância do SQL Server](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Limitações da pré-visualização públicas

Os seguintes itens são conhecidos limitações da pré-visualização pública:

- A máquina virtual (VM) do SQL requer ligação à internet para acessar os endereços IP públicos do Azure. Para obter detalhes, consulte [estabelecer conectividade de rede](backup-azure-sql-database.md#establish-network-connectivity).
- Protege até 2000 bases de dados SQL num cofre dos serviços de recuperação. Bases de dados SQL adicionais devem ser armazenados num cofre dos serviços de recuperação separado.
- [As cópias de segurança dos grupos de disponibilidade distribuída](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) têm limitações.
- Sempre na ativação pós-falha Cluster instâncias do SQL Server (FCIs) não são suportadas.
- Utilize o portal do Azure para configurar o Azure Backup para proteger bases de dados do SQL Server. O Azure PowerShell, a CLI do Azure e as APIs REST não são atualmente suportadas.

Consulte a [secção de FAQ](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq) para obter mais detalhes sobre o suporte/não suportadas cenários.

## <a name="support-for-azure-geos"></a>Suporte para geos do Azure

Cópia de segurança do Azure é suportada para as seguintes áreas geográficas:

- Sudeste da Austrália (ASE) 
- Sul do Brasil (BRS)
- Canadá Central (CNC)
- Leste do Canadá (CE)
- EUA Central (CUS)
- Ásia Oriental (EA)
- Leste da Austrália (AE) 
- EUA Leste (EUS)
- EUA Leste 2 (EUS2)
- Índia Central (INC) 
- Índia do Sul (INS)
- Leste do Japão (JPE)
- Oeste do Japão (JPW)
- KR do Sul Central (KRC)
- Sul KR do Sul (KRS)
- EUA Centro-Norte (NCUS) 
- Europa do Norte (NE) 
- EUA Centro-Sul (SCUS) 
- Sudeste Asiático (SEA)
- Sul do Reino Unido (UKS) 
- Oeste do Reino Unido (UKW) 
- EUA Centro-Oeste (WCUS)
- Europa Ocidental (WE) 
- EUA Oeste (WUS)
- EUA Oeste 2 (WUS 2) 

## <a name="support-for-operating-systems-and-sql-server-versions"></a>Suporte para sistemas operativos e versões do SQL Server

Esta secção descreve o suporte de cópia de segurança do Azure para sistemas operativos e versões do SQL Server. Máquinas virtuais do Azure Marketplace do SQL e máquinas de virtuais externas (onde do SQL Server é instalado manualmente) são suportadas.

### <a name="supported-operating-systems"></a>Sistemas operativos suportados

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux não é atualmente suportado.

### <a name="supported-sql-server-versions-and-editions"></a>As edições e versões do SQL Server suportadas

- SQL Server 2012 Enterprise, Standard, Web, desenvolvedor, Express
- SQL Server 2014 Enterprise, Standard, Web, desenvolvedor, Express
- SQL Server 2016 Enterprise, Standard, Web, desenvolvedor, Express
- SQL Server 2017 Enterprise, Standard, Web, desenvolvedor, Express

## <a name="prerequisites"></a>Pré-requisitos

Antes de fazer cópias de segurança da base de dados do SQL Server, verifique as seguintes condições:

- Identificar ou [criar um cofre dos serviços de recuperação](backup-azure-sql-database.md#create-a-recovery-services-vault) na mesma região ou região do que a máquina virtual que aloja a instância do SQL Server.
- [Verifique as permissões na máquina virtual](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) que são necessários para fazer backup de bancos de dados SQL.
- Certifique-se de que o [máquina virtual do SQL tem conectividade de rede](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> Pode ter apenas uma solução de cópia de segurança num tempo para fazer cópias de segurança de bases de dados do SQL Server. Desativar todas as outras cópias de segurança SQL antes de utilizar esta funcionalidade; caso contrário, as cópias de segurança irão interferir e falhar. Pode ativar a cópia de segurança do Azure para a VM de IaaS, juntamente com a cópia de segurança SQL sem qualquer conflito.
>

Se estas condições existem no seu ambiente, continuar [configurar a cópia de segurança para bases de dados do SQL Server](backup-azure-sql-database.md#configure-backup-for-sql-server-databases). Se qualquer um dos pré-requisitos não existir, continue a ler.


## <a name="establish-network-connectivity"></a>Estabelecer conectividade de rede

Para todas as operações, a máquina virtual do SQL necessita de conectividade para os endereços IP públicos do Azure. Operações de máquina virtual do SQL (por exemplo, a deteção de base de dados, configurar cópias de segurança, agendar cópias de segurança, restaurar pontos de recuperação e assim por diante) falhar sem conectividade com os endereços IP públicos. Utilize qualquer uma das seguintes opções para fornecer um caminho claro para o tráfego de cópia de segurança:

- Lista de permissões de intervalos de IP do datacenter do Azure: À lista de permissões os intervalos IP do datacenter do Azure, utilize o [página do Centro de Download para obter detalhes sobre os intervalos de IP e as instruções](https://www.microsoft.com/download/details.aspx?id=41653). 
- Implementar um servidor proxy HTTP para encaminhar o tráfego: Quando cria cópias de uma base de dados SQL numa VM, a extensão de cópia de segurança na VM utiliza as APIs de HTTPS para enviar comandos de gestão para a cópia de segurança do Azure e os dados ao armazenamento do Azure. A extensão de cópia de segurança também utiliza o Azure Active Directory (Azure AD) para autenticação. Encaminhe o tráfego de extensão de cópia de segurança para estes três serviços através do proxy HTTP. A extensão 's o único componente que está configurado para acesso à internet pública.

As compensações entre as opções são a capacidade de gerenciamento, um controle granular e custo.

> [!NOTE]
> Etiquetas de serviço para o Azure Backup devem estar disponíveis por disponibilidade geral.
>

| Opção | Vantagens | Desvantagens |
| ------ | ---------- | ------------- |
| Intervalos de IP da lista branca | Sem custos adicionais. <br/> Para obter acesso abrir num NSG, utilize o **Set-AzureNetworkSecurityRule** cmdlet. | Complexo de gerir porque os intervalos IP afetados alteram ao longo do tempo. <br/>Fornece acesso a todo do Azure, não apenas o armazenamento do Azure.|
| Utilizar um proxy de HTTP   | Um controle granular no proxy sobre o armazenamento é permitida a URLs. <br/>Único ponto de acesso à internet para VMs. <br/> Não sujeitas a alterações ao endereço IP do Azure. | Custos adicionais para executar uma VM com o software de proxy. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Definir permissões para o Marketplace SQL VMs não

Para fazer backup de uma máquina virtual, o Azure Backup requer o **AzureBackupWindowsWorkload** extensão seja instalado. Se utilizar as máquinas virtuais do Azure Marketplace, continuar a [bases de dados do SQL Server detetar](backup-azure-sql-database.md#discover-sql-server-databases). Se a máquina virtual que aloja as bases de dados SQL não é criada no Azure Marketplace, conclua o procedimento seguinte para instalar a extensão e definir as permissões adequadas. Para além da **AzureBackupWindowsWorkload** extensão, o Azure Backup requer privilégios de sysadmin do SQL para proteger as bases de dados SQL. Para detetar as bases de dados na máquina virtual, o Azure Backup cria a conta **NT Service\AzureWLBackupPluginSvc**. Para o Azure Backup detetar as bases de dados SQL, o **NT Service\AzureWLBackupPluginSvc** conta tem de ter SQL e SQL permissões de administrador do sistema. O procedimento seguinte explica como fornecer estas permissões.

Para configurar permissões:

1. Na [portal do Azure](https://portal.azure.com), abra o Cofre de serviços de recuperação que utilizar para proteger as bases de dados SQL.

2. Sobre o **cofre dos serviços de recuperação** dashboard, selecione **cópia de segurança**. O **objetivo de cópia de segurança** é aberto o menu.

   ![Selecionar cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

3. Na **objetivo de cópia de segurança** menu, defina **em que a sua carga de trabalho é executado** para o padrão: **Azure**.

4. Expanda a **o que fazer quiser a cópia de segurança** caixa de lista pendente e selecione **do SQL Server na VM do Azure**.

    ![Selecione o SQL Server na VM do Azure para a cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    O **objetivo de cópia de segurança** menu apresenta dois passos: **detetar DBs em VMs** e **configurar a cópia de segurança**. O **detetar DBs em VMs** passo iniciar uma pesquisa de máquinas virtuais do Azure.

    ![Reveja os dois passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Sob **detetar DBs em VMs**, selecione **Iniciar deteção** para procurar as máquinas virtuais não protegidas na subscrição. Pode demorar algum tempo a procurar todas as máquinas virtuais. O tempo de pesquisa depende do número de máquinas virtuais não protegidas na subscrição.

    ![Cópia de segurança está pendente durante a pesquisa do DBs em VMs](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Depois de uma máquina virtual não protegida for detetada, ele aparece na lista. Máquinas virtuais não protegidas são listadas pelo respetivo grupo de recursos e o nome da máquina virtual. É possível que várias máquinas virtuais para ter o mesmo nome. No entanto, as máquinas virtuais com o mesmo nome pertencer aos grupos de recursos diferentes. Se uma máquina virtual esperada não estiver listada, veja se a máquina virtual já protegida para um cofre.

6. Na lista de máquinas virtuais, selecione a VM que tenha o SQL para criar cópias de segurança de base de dados e, em seguida, selecione **detetar bds**. 

    O processo de deteção instala o **AzureBackupWindowsWorkload** extensão na máquina virtual. A extensão permite que o serviço de cópia de segurança do Azure comunicar com a máquina virtual, pelo que pode fazer backup de bancos de dados SQL. Após a instalação da extensão, cópia de segurança do Azure cria a conta de serviço virtual do Windows **NT Service\AzureWLBackupPluginSvc** na máquina virtual. A conta de serviço virtual requer permissão de sysadmin do SQL. Durante o serviço virtual conta processo de instalação, se receber o erro `UserErrorSQLNoSysadminMembership`, consulte [permissões de administrador do sistema SQL corrigir](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

    O **notificações** área mostra o progresso da deteção da base de dados. Pode demorar algum tempo para que a tarefa seja concluída. O tempo de trabalho depende de quantos bancos de dados estão na máquina virtual. Quando forem detetadas as bases de dados selecionadas, é apresentada uma mensagem de êxito.

    ![Mensagem de êxito da implementação](./media/backup-azure-sql-database/notifications-db-discovered.png)

Depois de associar a base de dados com o Cofre dos serviços de recuperação, a próxima etapa é [configurar a tarefa de cópia de segurança](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="fix-sql-sysadmin-permissions"></a>Corrigir permissões de administrador do sistema do SQL

Durante o processo de instalação, se receber o erro `UserErrorSQLNoSysadminMembership`, utilizar uma conta com permissões de administrador do sistema do SQL Server para iniciar sessão para o SQL Server Management Studio (SSMS). A menos que precise permissões especiais, autenticação do Windows deverá funcionar.

1. No SQL Server, abra a pasta de segurança/inícios de sessão.

    ![Abra a pasta de segurança/inícios de sessão para ver as contas](./media/backup-azure-sql-database/security-login-list.png)

2. A pasta de inícios de sessão com o botão direito e selecione **novo início de sessão**. Na **início de sessão - novo** caixa de diálogo, selecione **pesquisa**.

    ![No início de sessão - nova caixa de diálogo, selecione Search](./media/backup-azure-sql-database/new-login-search.png)

3. Conta de serviço do Windows virtual **NT Service\AzureWLBackupPluginSvc** foi criado durante o registo de máquina virtual e a fase de deteção SQL. Introduza o nome da conta, como mostra a **introduza o nome de objeto a selecionar** caixa. Selecione **verificar nomes** para resolver o nome. 

    ![Selecione verificar nomes para resolver o nome do serviço desconhecido](./media/backup-azure-sql-database/check-name.png)

4. Selecione **OK** para fechar a caixa de diálogo.

5. Na **funções de servidor** caixa, certifique-se a **sysadmin** função está selecionada. Selecione **OK** para fechar a caixa de diálogo.

    ![Certifique-se de que a função de servidor sysadmin](./media/backup-azure-sql-database/sysadmin-server-role.png)

    As permissões necessárias agora devem existir.

6. Embora a corrigir o erro de permissões, terá de associar a base de dados com o Cofre dos serviços de recuperação. No portal do Azure, no **servidores protegidos** lista, com o servidor que está num erro de estado e selecione o botão direito **Redeteção de DBs**.

    ![Verifique se que o servidor tem as permissões adequadas](./media/backup-azure-sql-database/check-erroneous-server.png)

    O **notificações** área mostra o progresso da deteção da base de dados. Pode demorar algum tempo para que a tarefa seja concluída. O tempo de trabalho depende de quantos bancos de dados estão na máquina virtual. Quando são encontrados as bases de dados selecionadas, é apresentada uma mensagem de êxito.

    ![Mensagem de êxito da implementação](./media/backup-azure-sql-database/notifications-db-discovered.png)

Depois de associar a base de dados com o Cofre dos serviços de recuperação, a próxima etapa é [configurar a tarefa de cópia de segurança](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detetar as bases de dados do SQL Server

Cópia de segurança do Azure Deteta todas as bases de dados numa instância do SQL Server. Pode proteger as bases de dados, de acordo com os requisitos de cópia de segurança. Utilize o procedimento seguinte para identificar a máquina virtual que aloja as bases de dados SQL. Depois de identificar a máquina virtual, o Azure Backup instala uma extensão simples para detetar as bases de dados do SQL Server.

1. Inicie sessão na sua subscrição no [portal do Azure](https://portal.azure.com/).

2. No menu da esquerda, selecione **todos os serviços**.

    ![Selecione todos os serviços](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Na **todos os serviços** caixa de diálogo, introduza **dos serviços de recuperação**. À medida que escreve, sua entrada filtra a lista de recursos. Selecione **cofres dos serviços de recuperação** na lista.

    ![Introduza e selecione os cofres dos serviços de recuperação](./media/backup-azure-sql-database/all-services.png) <br/>

    É apresentada a lista de cofres de serviços de recuperação na subscrição. 

4. Na lista de cofres dos serviços de recuperação, selecione o cofre para utilizar para proteger as bases de dados SQL.

5. Sobre o **cofre dos serviços de recuperação** dashboard, selecione **cópia de segurança**. O **objetivo de cópia de segurança** é aberto o menu.

   ![Selecionar cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

6. Na **objetivo de cópia de segurança** menu, defina **em que a sua carga de trabalho é executado** para o padrão: **Azure**.

7. Expanda a **o que pretende criar cópias de segurança** caixa de lista pendente e selecione **do SQL Server na VM do Azure**.

    ![Selecione o SQL Server na VM do Azure para a cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    O **objetivo de cópia de segurança** menu apresenta dois passos: **detetar DBs em VMs** e **configurar a cópia de segurança**.
    
    ![Reveja os dois passos de objetivo de cópia de segurança](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Sob **detetar DBs em VMs**, selecione **Iniciar deteção** para procurar as máquinas virtuais não protegidas na subscrição. Pode demorar algum tempo a procurar em todas as máquinas virtuais. O tempo de pesquisa depende do número de máquinas virtuais não protegidas na subscrição.

    ![Cópia de segurança está pendente durante a pesquisa do DBs em VMs](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Depois de uma máquina virtual não protegida for detetada, ele aparece na lista. Várias máquinas virtuais podem ter o mesmo nome. No entanto, as máquinas virtuais com o mesmo nome pertencer aos grupos de recursos diferentes. Máquinas virtuais não protegidas são listadas pelo respetivo grupo de recursos e o nome da máquina virtual. Se uma máquina virtual esperada não estiver listada, veja se essa máquina virtual já protegida para um cofre.

9. Na lista de máquinas virtuais, selecione a VM que tenha o SQL para criar cópias de segurança de base de dados e, em seguida, selecione **detetar bds**.

    Cópia de segurança do Azure Deteta todas as bases de dados do SQL na máquina virtual. Para obter informações sobre o que acontece durante a fase de deteção de base de dados, consulte [operações de fundo](backup-azure-sql-database.md#background-operations). Depois das bases de dados do SQL são detetados, está pronto para [configurar a tarefa de cópia de segurança](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="background-operations"></a>Operações em segundo plano

Quando utiliza a **detetar bds** ferramenta, cópia de segurança do Azure executa as seguintes operações em segundo plano:

- Registe a máquina virtual com o Cofre de serviços de recuperação para cópia de segurança da carga de trabalho. Todas as bases de dados na máquina virtual registada podem ser uma cópia de segurança a este Cofre de serviços de recuperação apenas. 

- Instalar o **AzureBackupWindowsWorkload** extensão na máquina virtual. Cópia de segurança de um SQL base de dados é uma solução sem agente. A extensão é instalada na máquina virtual e nenhum agente instalado na base de dados SQL.

- Criar a conta de serviço **NT Service\AzureWLBackupPluginSvc** na máquina virtual. Todas as operações de cópia de segurança e restauro, utilize a conta de serviço. **NT Service\AzureWLBackupPluginSvc** necessita de permissões de administrador do sistema do SQL. Todas as máquinas virtuais do SQL Marketplace são fornecidos com o **SqlIaaSExtension** instalado. O **AzureBackupWindowsWorkload** extensão utiliza o **SQLIaaSExtension** para obter automaticamente as permissões necessárias. Se a sua máquina virtual não tiver o **SqlIaaSExtension** instalado, a operação de detetar DB falhar com a mensagem de erro `UserErrorSQLNoSysAdminMembership`. Para adicionar a permissão de administrador do sistema para cópia de segurança, siga as instruções em [configurar permissões de cópia de segurança do Azure para o Marketplace SQL VMs não](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

    ![Selecione a VM e a base de dados](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-databases"></a>Configurar cópia de segurança para bases de dados do SQL Server

Cópia de segurança do Azure fornece serviços de gestão para proteger as bases de dados do SQL Server e gerir tarefas de cópia de segurança. As funções de monitorização e gestão dependem do seu Cofre dos serviços de recuperação. 

> [!NOTE]
> Pode ter apenas uma solução de cópia de segurança num tempo para fazer cópias de segurança de bases de dados do SQL Server. Desativar todas as outras cópias de segurança SQL antes de utilizar esta funcionalidade; caso contrário, as cópias de segurança irão interferir e falhar. Pode ativar a cópia de segurança do Azure para a VM de IaaS, juntamente com a cópia de segurança SQL sem qualquer conflito.
>

Para configurar a proteção para uma base de dados SQL:

1. Abra o Cofre de serviços de recuperação que está registado com a máquina virtual do SQL.

2. Sobre o **cofre dos serviços de recuperação** dashboard, selecione **cópia de segurança**. O **objetivo de cópia de segurança** é aberto o menu.

   ![Selecionar cópia de segurança para abrir o menu de objetivo de cópia de segurança](./media/backup-azure-sql-database/open-backup-menu.png)

3. Na **objetivo de cópia de segurança** menu, defina **em que a sua carga de trabalho é executado** para o padrão: **Azure**.

4. Expanda a **o que pretende criar cópias de segurança** caixa de lista pendente e selecione **do SQL Server na VM do Azure**.

    ![Selecione o SQL Server na VM do Azure para a cópia de segurança](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    O **objetivo de cópia de segurança** menu apresenta dois passos: **detetar DBs em VMs** e **configurar a cópia de segurança**.
    
    Se tiver concluído os passos neste artigo em ordem, descobertos as máquinas virtuais desprotegidas e este Cofre está registado com uma máquina virtual. Agora, está pronto para configurar a proteção para as bases de dados SQL.
    
5. Sobre o **objetivo de cópia de segurança** menu, selecione **configurar a cópia de segurança**.

    ![Selecione Configurar cópia de segurança](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    O serviço de cópia de segurança do Azure apresenta todas as instâncias do SQL Server com bases de dados autónomas e grupos de disponibilidade Always On do SQL Server. Para ver as bases de dados autónomo na instância do SQL Server, selecione a divisa para a esquerda do nome de instância. As imagens seguintes mostram exemplos de uma instância autónoma e um grupo de disponibilidade Always On.

    > [!NOTE]
    > Para um SQL Server Always On grupo de disponibilidade, é honrada a preferência de cópia de segurança de SQL. No entanto, devido a uma limitação de plataforma do SQL, cópias de segurança completas e diferenciais têm de ocorrer a partir do nó principal. Registo de volta até acontece, de acordo com sua preferência de cópia de segurança. Devido a essa limitação, o nó principal sempre deve ser registrado para grupos de disponibilidade.
    >

    ![Lista de bases de dados na instância do SQL](./media/backup-azure-sql-database/discovered-databases.png)

    Selecione a divisa para a esquerda do grupo de disponibilidade Always On para ver a lista de bases de dados.

    ![Lista de bases de dados no grupo de disponibilidade Always On](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Na lista de bases de dados, selecione todas as bases de dados para proteger e, em seguida, selecione **OK**.

    ![Selecione várias bases de dados para proteger](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Selecione até 50 bases de dados cada vez. Para proteger mais de 50 bases de dados, fazer várias fases. Depois de proteger as 50 primeiras bases de dados, repita este passo para proteger o próximo conjunto de bases de dados.

    > [!Note] 
    > Para otimizar cargas de cópia de segurança, cópia de segurança do Azure divide grandes trabalhos de cópia de segurança em vários lotes. O número máximo de bases de dados numa tarefa de cópia de segurança é 50.
    >
    >

7. Para criar ou escolha uma política de cópia de segurança, sobre o **cópia de segurança** menu, selecione **política de cópia de segurança**. O **política de cópia de segurança** é aberto o menu.

    ![Selecione a política de cópia de segurança](./media/backup-azure-sql-database/select-backup-policy.png)

8. Na **escolher política de cópia de segurança** caixa de lista pendente, escolha uma política de cópia de segurança e, em seguida, selecione **OK**. Para obter informações sobre como criar uma política de cópia de segurança, consulte [definir uma política de cópia de segurança](backup-azure-sql-database.md#define-a-backup-policy).

   > [!NOTE]
   > Durante a pré-visualização, não é possível editar as políticas de cópia de segurança. Se pretender uma política diferente do que está disponível na lista, tem de criar essa política. Para informações sobre como criar uma nova política de cópia de segurança, consulte a secção [definir uma política de cópia de segurança](backup-azure-sql-database.md#define-a-backup-policy).

    ![Escolha uma política de cópia de segurança da lista](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Sobre o **política de cópia de segurança** menu, no **escolher política de cópia de segurança** caixa de lista pendente, pode: 
    - Selecione a política predefinida: **HourlyLogBackup**.
    - Escolha uma política de cópia de segurança existente criada anteriormente para SQL.
    - [Definir uma nova política](backup-azure-sql-database.md#define-a-backup-policy) baseada no período de retenção e RPO. 

    > [!Note]
    > O Azure Backup suporta a retenção de longo prazo com base no esquema de cópia de segurança avô-pai-filho. O esquema de otimiza o consumo de armazenamento de back-end enquanto precisa de conformidade da reunião.
    >

9. Depois de escolher uma política de cópia de segurança, sobre o **menu de cópia de segurança**, selecione **ativar cópia de segurança**.

    ![Ativar a política de cópia de segurança escolhida](./media/backup-azure-sql-database/enable-backup-button.png)

    Controlar o progresso da configuração no **notificações** área do portal.

    ![Área de notificação](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Definir uma política de cópia de segurança

Uma política de cópia de segurança define uma matriz de quando os backups são feitos e o período de tempo são retidos. Utilize a cópia de segurança do Azure para agendar a três tipos de cópia de segurança de bases de dados SQL:

* Cópia de segurança completa: uma cópia de segurança completa da base de dados faz backup de todo o banco de dados. Uma cópia de segurança completa contém todos os dados numa base de dados específica ou um conjunto de grupos de ficheiros ou ficheiros e suficiente registos para recuperar esses dados. No máximo, pode acionar uma cópia de segurança completa por dia. Pode optar por realizar um backup completo num intervalo diário ou semanal. 
* Cópia de segurança diferencial: um backup diferencial baseia-se a cópia de segurança de dados completa mais recente, anterior. Uma cópia de segurança diferencial captura apenas os dados que são alterados desde a cópia de segurança completa. No máximo, pode acionar uma cópia de segurança diferencial por dia. Não é possível configurar uma cópia de segurança completa e uma cópia de segurança diferencial no mesmo dia.
* Cópia de segurança de registo de transações: restauro de ponto no tempo até um segundo específico permite a uma cópia de segurança do registo. No máximo, pode configurar cópias de segurança do registo transacional a cada 15 minutos.

A política criada no cofre dos serviços de recuperação de nível. Vários cofres podem utilizar a mesma política de cópia de segurança, mas tem de aplicar a política de cópia de segurança para cada cofre. Quando cria uma política de cópia de segurança, a cópia de segurança completa diária é a predefinição. Pode adicionar um backup diferencial, mas apenas se configurar cópias de segurança completas para ocorrer semanalmente. O procedimento seguinte explica como criar uma política de cópia de segurança para uma instância do SQL Server numa máquina virtual do Azure. 

> [!NOTE]
> Em pré-visualização, não é possível editar uma política de cópia de segurança. Em vez disso, tem de criar uma nova política com os detalhes pretendidos.  
 
Para criar uma política de cópia de segurança:

1. No cofre dos serviços de recuperação que protege a base de dados SQL, clique em **políticas de cópia de segurança**e, em seguida, clique em **Add**. 

   ![Abra a criar nova política de cópia de segurança caixa de diálogo](./media/backup-azure-sql-database/new-policy-workflow.png)

   O **adicionar** menu é apresentada.

2. Na **Add** menu, clique em **do SQL Server na VM do Azure**.

   ![Escolha um tipo de política para a nova política de cópia de segurança](./media/backup-azure-sql-database/policy-type-details.png)

   Selecionar o SQL Server na VM do Azure define o tipo de política e abre o menu de política de cópia de segurança. O **política de cópia de segurança** menu mostra os campos que são necessários para uma nova política de cópia de segurança do SQL Server.

3. Na **nome da política**, introduza um nome para a nova política.

4. Um backup completo é obrigatório; Não é possível desativar a **cópia de segurança completa** opção. Clique em **cópia de segurança completa** para ver e editar a política. Mesmo que não altere a política de cópia de segurança, deve ver os detalhes da política.

    ![novos campos de política de cópia de segurança](./media/backup-azure-sql-database/full-backup-policy.png)

    Na **política de cópia de segurança completa** menu, para **frequência de cópia de segurança**, escolha **diária** ou **semanal**. Para **diária**, selecione a hora e fuso horário quando começa a tarefa de cópia de segurança. Não é possível criar cópias de segurança diferenciais para cópias de segurança completas diárias.

   ![definição de intervalo diário](./media/backup-azure-sql-database/daily-interval.png)

    Para **semanal**, selecione o dia da semana, hora e fuso horário quando começa a tarefa de cópia de segurança.

   ![definição de intervalo semanal](./media/backup-azure-sql-database/weekly-interval.png)

5. Por predefinição, todos os **período de retenção** opções estão selecionadas: diárias, semanais, mensais e anuais. Anular seleção de limites de intervalo de retenção indesejados. Defina os intervalos de usar. Na **política de cópia de segurança completa** menu, selecione **OK** para aceitar as configurações.

   ![Definições de intervalo de intervalo de retenção](./media/backup-azure-sql-database/retention-range-interval.png)

    Pontos de recuperação são marcados para retenção com base no seu período de retenção. Por exemplo, se selecionar uma cópia de segurança completa diária, apenas uma cópia de segurança completa é acionada por dia. A cópia de segurança para um dia específico é marcado e mantido com base no período de retenção semanais e a definição de retenção semanal. Mensal e anual períodos de retenção se comportam de maneira semelhante.

6. Para adicionar uma política de cópia de segurança diferencial, selecione **cópia de segurança diferencial**. O **política de cópia de segurança diferencial** é aberto o menu. 

   ![Abra o menu de política de cópia de segurança diferencial](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Sobre o **política de cópia de segurança diferencial** menu, selecione **ativar** para abrir os controles de frequência e retenção. No máximo, pode acionar uma cópia de segurança diferencial por dia.
    
    > [!Important] 
    > As cópias de segurança diferenciais podem ser mantidas para um máximo de 180 dias. Se precisar de retenção mais longa, tem de utilizar cópias de segurança completas.
    >

   ![Editar a política de cópia de segurança diferencial](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Selecione **OK** para guardar a política e retornar o Main **política de cópia de segurança** menu.

7. Para adicionar uma política de cópia de segurança do registo transacional, selecione **cópia de segurança do registo**. O **registo de cópia de segurança** é aberto o menu.

    Na **registo de cópia de segurança** menu, selecione **ativar**e, em seguida, defina os controlos de frequência e retenção. Backups de log podem ocorrer com a frequência de 15 minutos e podem ser mantidos até 35 dias. Selecione **OK** para guardar a política e retornar o Main **política de cópia de segurança** menu.

   ![Editar a política de cópia de segurança do registo](./media/backup-azure-sql-database/log-backup-policy-editor.png)

8. Sobre o **política de cópia de segurança** menu, escolha se pretende ativar **compressão de cópia de segurança de SQL**. Compressão está desativada por predefinição.

    No back-end, o Azure Backup utiliza compressão de cópias de segurança nativa SQL.

9. Depois de concluir as edições para a política de cópia de segurança, selecione **OK**. 

   ![Aceite a nova política de cópia de segurança](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Restaurar uma base de dados SQL
Cópia de segurança do Azure fornece funcionalidades para restaurar bases de dados individuais para uma data específica ou a hora (para o segundo) com os backups de log de transação. Cópia de segurança do Azure automaticamente determina o diferencial completa adequado e a cadeia de cópias de segurança de registo que são necessários para restaurar os dados com base nos seus tempos de restauro.

Também pode selecionar um backup completo ou diferencial específico para restaurar para um ponto de recuperação específico, em vez de uma hora específica.

### <a name="pre-requisite-before-triggering-a-restore"></a>Pré-requisitos antes de acionar um restauro

1. Pode restaurar a base de dados para uma instância do SQL Server na mesma região do Azure. O servidor de destino tem de estar registado no mesmo cofre dos serviços de recuperação como a origem.  
2. Para restaurar uma base de dados encriptado do TDE para outro SQL Server, primeiro restaure o certificado para o servidor de destino ao seguir os passos documentados [aqui](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
3. Antes de acionar um restauro de base de dados "Mestra", iniciar a instância do SQL Server no modo de utilizador único com a opção de arranque `-m AzureWorkloadBackup`. O argumento para o `-m` opção é o nome do cliente. Apenas este cliente tem permissão para abrir a ligação. Para todas as bases de dados do sistema (modelo, mestra, msdb), pare o serviço SQL Agent antes de acionar o restauro. Feche todas as aplicações que podem tentar roubar uma ligação a qualquer uma destas bases de dados.

### <a name="steps-to-restore-a-database"></a>Passos para restaurar uma base de dados:

1. Abra o Cofre de serviços de recuperação que está registado com a máquina virtual do SQL.

2. Sobre o **cofre dos serviços de recuperação** dashboard, em **utilização**, selecione **itens de cópia de segurança** para abrir o **itens de cópia de segurança** menu.

    ![Abra o menu de itens de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Sobre o **itens de cópia de segurança** menu, em **tipo de gestão de cópia de segurança**, selecione **SQL na VM do Azure**. 

    ![Selecione o SQL numa VM do Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    O **itens de cópia de segurança** menu mostra a lista de bases de dados SQL. 

4. Na lista de bases de dados SQL, selecione a base de dados para restaurar.

    ![Selecione a base de dados para restaurar](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Ao selecionar a base de dados, é aberto o respetivo menu. O menu fornece os detalhes de cópia de segurança da base de dados, incluindo:

    * Pontos de restauro mais antigo e mais recentes.
    * Registo de estado de cópia de segurança nas últimas 24 horas (para bases de dados no modelo de recuperação completa e registadas em massa, se configurado para cópias de segurança do registo transacional).

5. No menu da base de dados selecionada, selecione **restaurar DB**. O **restaurar** é aberto o menu.

    ![Selecione restaurar DB](./media/backup-azure-sql-database/restore-db-button.png)

    Quando o **restaurar** é aberto o menu, o **restaurar configuração** também é aberto o menu. O **restaurar configuração** menu é o primeiro passo para configurar o restauro. Use esse menu para selecione onde pretende restaurar os dados. As opções são:
    - **Alternativo de localização**: restaurar a base de dados para uma localização alternativa e manter a base de dados de origem original.
    - **Substituir a BD**: restaurar os dados para a mesma instância do SQL Server como a origem original. O efeito desta opção é substituir a base de dados original.

    > [!Important]
    > Se a base de dados selecionada pertence a um grupo de disponibilidade Always On, o SQL Server não permite que a base de dados sejam substituídas. Neste caso, apenas os **localização alternativa** opção está ativada.
    >

    ![Restaurar o menu de configuração](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restaurar para uma localização alternativa

Este procedimento explica como restaurar dados para uma localização alternativa. Para substituir a base de dados durante a restauração, avance para [restaurar e substituir a base de dados](backup-azure-sql-database.md#restore-and-overwrite-the-database). Nesta fase, o seu Cofre dos serviços de recuperação está aberta e o **restaurar configuração** menu está visível. Se não estiver nesta fase, comece por [restaurar uma base de dados do SQL](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Pode restaurar a base de dados para uma instância do SQL Server na mesma região do Azure. O servidor de destino tem de ser registados no Cofre de serviços de recuperação. 
>

Sobre o **restaurar configuração** menu, o **servidor** caixa de lista pendente mostra apenas as instâncias do SQL Server que são registadas no Cofre de serviços de recuperação. Se o servidor que pretende não estiver na lista, consulte [bases de dados do SQL Server detetar](backup-azure-sql-database.md#discover-sql-server-databases) para localizar o servidor. Durante o processo de deteção, os novos servidores são registados no Cofre de serviços de recuperação.

1. Na **restaurar configuração** menu:

    * Sob **restaurar onde**, selecione **localização alternativa**.
    * Abra o **servidor** na lista pendente caixa e selecione a instância do SQL Server para restaurar a base de dados.
    * Abra o **instância** na lista pendente caixa e selecione uma instância do SQL Server.
    * Na **nome de BD restaurada** , introduza o nome da base de dados de destino.
    * Conforme aplicável, selecione **substituir se a BD com o mesmo nome já existe na instância SQL selecionada**.
    * Selecione **OK** para concluir a configuração do destino e continue para escolher um ponto de restauro.

    ![Fornecer valores para o menu de restaurar a configuração](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Sobre o **selecionar ponto de restauro** menu, escolha **registos (ponto no tempo)** ou **completa e diferencial** como o ponto de restauro. Para restaurar para um registo de ponto no tempo específico, continue com este passo. Para restaurar um ponto de restauro completas e diferenciais, avance para o passo 3.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    O restauro de ponto no tempo só está disponível para backups de log para bases de dados com um modelo de recuperação completa e registadas em massa. Para restaurar para um ponto específico no tempo:

    1. Selecione **registos (ponto no tempo)** como o tipo de restauro.

        ![Escolha o tipo de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Sob **restaurar data/hora**, selecione o calendário mini para abrir o **calendário**. Sobre o **calendário**, as datas a negrito tem pontos de recuperação e a data atual é realçada. Selecione uma data com pontos de recuperação. Não não possível selecionar datas sem pontos de recuperação.

        ![Abrir o calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis num intervalo contínuo.

    3. Utilize o gráfico de linha de tempo ou o **tempo** caixa de diálogo para especificar uma hora específica para o ponto de recuperação. Selecione **OK** para concluir o passo de ponto de restauro.
    
       ![Abrir o calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        O **selecionar ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Sobre o **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, defina **restaurar com NORECOVERY** ao **ativado**.
        * Para alterar a localização de restauro no servidor de destino, introduza um novo caminho na **destino** coluna.
        * Selecione **OK** para aprovar as definições. Fechar o **configuração avançada** menu.

    5. Sobre o **restaurar** menu, selecione **restaurar** para iniciar a tarefa de restauro. Acompanhar o progresso da restauração no **notificações** área ou selecione **restaurar trabalhos** no menu da base de dados.

       ![Restaurar o progresso da tarefa](./media/backup-azure-sql-database/restore-job-notification.png)

3. Sobre o **selecionar ponto de restauro** menu, escolha **registos (ponto no tempo)** ou **completa e diferencial** como o ponto de restauro. Para restaurar um log de ponto no tempo, volte ao passo 2. Este passo restaura um ponto de restauro de completo ou diferencial específico. Pode ver todos os pontos de recuperação completas e diferenciais nos últimos 30 dias. Para ver os pontos de recuperação com mais de 30 dias, selecione **filtro** para abrir o **pontos de restauração de filtro** menu. Para um ponto de recuperação diferencial, cópia de segurança do Azure restaura primeiro o ponto de recuperação completo apropriado e, em seguida, aplica-se o ponto de recuperação diferencial selecionado.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Na **selecionar ponto de restauro** menu, selecione **completa e diferencial**.

       ![Selecione completas e diferenciais](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        O menu mostra a lista de pontos de recuperação disponíveis.

    2. Selecione um ponto de recuperação da lista e selecione **OK** para concluir o procedimento de ponto de restauro. 

        ![Escolha um ponto de recuperação completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        O **ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

        ![Menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Sobre o **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, defina **restaurar com NORECOVERY** ao **ativado**. **Restaurar com NORECOVERY** está desativada por predefinição.
        * Para alterar a localização de restauro no servidor de destino, introduza um novo caminho na **destino** coluna.
        * Selecione **OK** para aprovar as definições. Fechar o **configuração avançada** menu.

    4. Sobre o **restaurar** menu, selecione **restaurar** para iniciar a tarefa de restauro. Acompanhar o progresso da restauração no **notificações** área ou selecione **restaurar trabalhos** no menu da base de dados.

       ![Restaurar o progresso da tarefa](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Restaurar e substituir a base de dados

Este procedimento explica como restaurar dados e a substituir uma base de dados. Para restaurar para uma localização alternativa, avance para [restaurar para uma localização alternativa](backup-azure-sql-database.md#restore-to-an-alternate-location). Nesta fase, o seu Cofre dos serviços de recuperação está aberta e o **restaurar configuração** menu é visível (veja a imagem seguinte). Se não estiver nesta fase, comece por [restaurar uma base de dados do SQL](backup-azure-sql-database.md#restore-a-sql-database).

![Restaurar o menu de configuração](./media/backup-azure-sql-database/restore-overwrite-db.png)

Sobre o **restaurar configuração** menu, o **servidor** caixa de lista pendente mostra apenas as instâncias do SQL Server que são registadas no Cofre de serviços de recuperação. Se o servidor que pretende não estiver na lista, consulte [bases de dados do SQL Server detetar](backup-azure-sql-database.md#discover-sql-server-databases) para localizar o servidor. Durante o processo de deteção, os novos servidores são registados no Cofre de serviços de recuperação.

1. Na **restaurar configuração** menu, selecione **substituir DB**e, em seguida, selecione **OK** para concluir a configuração de destino. 

   ![Selecione substituir a BD](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    O **servidor**, **instância**, e **nome da BD restaurada** as definições não são necessárias.

2. Sobre o **selecionar ponto de restauro** menu, escolha **registos (ponto no tempo)** ou **completa e diferencial** como o ponto de restauro. Para restaurar para um registo de ponto no tempo específico, continue com este passo. Para restaurar um ponto de restauro completas e diferenciais, avance para o passo 3.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    O restauro de ponto no tempo só está disponível para backups de log para bases de dados com um modelo de recuperação completa e registadas em massa. Para restaurar para um segundo específico:

    1. Selecione **registos (ponto no tempo)** como o ponto de restauro.

        ![Escolha o tipo de ponto de restauro](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Sob **restaurar data/hora**, selecione o calendário mini para abrir o **calendário**. Sobre o **calendário**, as datas a negrito tem pontos de recuperação e a data atual é realçada. Selecione uma data com pontos de recuperação. Não não possível selecionar datas sem pontos de recuperação.

        ![Abrir o calendário](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Depois de selecionar uma data, o gráfico de linha cronológica mostra os pontos de recuperação disponíveis.

    3. Utilize o gráfico de linha de tempo ou o **tempo** caixa de diálogo para especificar uma hora específica para o ponto de recuperação. Selecione **OK** para concluir o passo de ponto de restauro.
    
       ![Abrir o calendário](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        O **selecionar ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

       ![menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Sobre o **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, defina **restaurar com NORECOVERY** ao **ativado**.
        * Para alterar a localização de restauro no servidor de destino, introduza um novo caminho na **destino** coluna.
        * Selecione **OK** para aprovar as definições. Fechar o **configuração avançada** menu.

    5. Sobre o **restaurar** menu, selecione **restaurar** para iniciar a tarefa de restauro. Acompanhar o progresso da restauração no **notificações** área ou selecione **restaurar trabalhos** no menu da base de dados.

       ![Restaurar o progresso da tarefa](./media/backup-azure-sql-database/restore-job-notification.png)

3. Sobre o **selecionar ponto de restauro** menu, escolha **registos (ponto no tempo)** ou **completa e diferencial** como o ponto de restauro. Para restaurar um log de ponto no tempo, volte ao passo 2. Este passo restaura um ponto de restauro de completo ou diferencial específico. Pode ver todos os pontos de recuperação completas e diferenciais nos últimos 30 dias. Para ver os pontos de recuperação com mais de 30 dias, selecione **filtro** para abrir o **pontos de restauração de filtro** menu. Para um ponto de recuperação diferencial, cópia de segurança do Azure restaura primeiro o ponto de recuperação completo apropriado e, em seguida, aplica-se o ponto de recuperação diferencial selecionado.

    ![Selecione o menu de ponto de restauro](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Na **selecionar ponto de restauro** menu, selecione **completa e diferencial**.

       ![Selecione completas e diferenciais](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        O menu mostra a lista de pontos de recuperação disponíveis.

    2. Selecione um ponto de recuperação da lista e selecione **OK** para concluir o procedimento de ponto de restauro. 

        ![Escolha um ponto de recuperação completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        O **ponto de restauro** fecha o menu e o **configuração avançada** é aberto o menu.

        ![Menu de configuração avançada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Sobre o **configuração avançada** menu:

        * Para manter a base de dados não operacional após o restauro, defina **restaurar com NORECOVERY** ao **ativado**. **Restaurar com NORECOVERY** está desativada por predefinição.
        * Para alterar a localização de restauro no servidor de destino, introduza um novo caminho na **destino** coluna.
        * Selecione **OK** para aprovar as definições. Fechar o **configuração avançada** menu.

    4. Sobre o **restaurar** menu, selecione **restaurar** para iniciar a tarefa de restauro. Acompanhar o progresso da restauração no **notificações** área ou ao selecionar **restaurar trabalhos** no menu da base de dados.

       ![Restaurar o progresso da tarefa](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Gerir as operações de cópia de segurança do Azure para SQL em VMs do Azure

Esta seção fornece informações sobre as várias operações de gestão de cópia de segurança do Azure que estão disponíveis para o SQL em máquinas virtuais do Azure. Existem as seguintes operações de alto nível:

* Monitorizar trabalhos
* Alertas de cópia de segurança
* Parar a proteção numa base de dados SQL
* Retomar a proteção para uma base de dados SQL
* Acionar uma tarefa de cópia de segurança ad hoc
* Anular o registo de um servidor que está a executar o SQL Server

### <a name="monitor-backup-jobs"></a>Monitorizar tarefas de cópia de segurança
O Azure Backup é uma solução de classe empresarial que fornece alertas de cópias de segurança avançadas e notificações para eventuais falhas. (Consulte [ver alertas de cópia de segurança](backup-azure-sql-database.md#view-backup-alerts).) Para monitorizar trabalhos específicos, utilize qualquer uma das seguintes opções, de acordo com os seus requisitos.

#### <a name="use-the-azure-portal-for-adhoc-operations"></a>Utilizar o portal do Azure para operações de ad hoc
Cópia de segurança do Azure mostra tudo manualmente acionada ou ad hoc, as tarefas no **tarefas de cópia de segurança** portal. As tarefas que estão disponíveis no **tarefas de cópia de segurança** portal incluem:
- Configurar todas as operações de cópia de segurança.
- Acionado manualmente as operações de cópia de segurança.
- Operações de restauro.
- Registo e descubra as operações de base de dados.
- Pare as operações de cópia de segurança. 

![Portal de cópias de segurança](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Todas as tarefas de cópia de segurança agendadas, incluindo completas, diferenciais e cópia de segurança do registo, não são apresentadas no **tarefas de cópia de segurança** portal. Utilize SQL Server Management Studio para monitorizar tarefas de cópia de segurança agendadas, conforme descrito na secção seguinte.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Utilizar o SQL Server Management Studio para tarefas de cópia de segurança
O Azure Backup utiliza as APIs nativas do SQL para todas as operações de cópia de segurança. Utilizar as APIs nativas para obter todas as informações de tarefa do [tabela de backupset SQL](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) na base de dados msdb.

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

### <a name="view-backup-alerts"></a>Ver alertas de cópia de segurança

Uma vez que ocorrerem de backups de log a cada 15 minutos, ocasionalmente, monitorizar tarefas de cópia de segurança pode ser enfadonho. O Azure Backup fornece ajuda nessa situação. Alertas por e-mail são acionados para todas as falhas de cópia de segurança. Alertas são consolidados ao nível da base de dados por código de erro. Um alerta de e-mail é enviado apenas para a primeira falha de cópia de segurança para uma base de dados. Inicie sessão no portal do Azure para monitorizar todas as falhas de uma base de dados. 

Para monitorizar alertas de cópia de segurança:

1. Inicie sessão na sua subscrição do Azure no [portal do Azure](https://portal.azure.com).

2. Abra o Cofre de serviços de recuperação que está registado com a máquina virtual do SQL.

3. Sobre o **cofre dos serviços de recuperação** dashboard, selecione **alertas e eventos**. 

   ![Selecione os alertas e eventos](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Sobre o **eventos e alertas** menu, selecione **alertas de cópia de segurança** para ver a lista de alertas.

   ![Selecione os alertas de cópia de segurança](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-for-a-sql-server-database"></a>Parar a proteção de uma base de dados do SQL Server

Quando parar a proteção para uma base de dados do SQL Server, o Azure Backup pedidos se pretende manter os pontos de recuperação. Existem duas formas de parar a proteção de uma base de dados SQL:

* Parar todas as tarefas de cópia de segurança futuras e eliminar todos os pontos de recuperação.
* Parar todas as tarefas de cópia de segurança futuras, mas deixar os pontos de recuperação.

Se optar por parar cópia de segurança mantendo os dados, pontos de recuperação serão limpo de acordo com a política de cópia de segurança. Será cobrado a instância protegida de SQL preços custo e o armazenamento consumido até que todos os pontos de recuperação são limpos. Para obter mais informações sobre os preços de cópia de segurança do Azure para SQL, consulte a [página de preços do Azure Backup](https://azure.microsoft.com/pricing/details/backup/). 

Para parar a proteção para uma base de dados:

1. Abra o Cofre de serviços de recuperação que está registado com a máquina virtual do SQL.

2. Sobre o **cofre dos serviços de recuperação** dashboard, em **utilização**, selecione **itens de cópia de segurança** para abrir o **itens de cópia de segurança** menu.

    ![Abra o menu de itens de cópia de segurança](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Sobre o **itens de cópia de segurança** menu, em **tipo de gestão de cópia de segurança**, selecione **SQL na VM do Azure**. 

    ![Selecione o SQL numa VM do Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    O **itens de cópia de segurança** menu mostra a lista de bases de dados SQL. 

4. Na lista de bases de dados SQL, selecione a base de dados para parar a proteção.

    ![Selecione a base de dados para parar a proteção](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Ao selecionar a base de dados, é aberto o respetivo menu.

5. No menu da base de dados selecionada, selecione **parar cópia de segurança**. 

    ![Selecione parar cópia de segurança](./media/backup-azure-sql-database/stop-db-button.png)

    O **parar cópia de segurança** é aberto o menu.

6. Sobre o **parar cópia de segurança** menu, optar por **reter dados de cópia de segurança** ou **eliminar dados de cópia de segurança**. Como opção, forneça um motivo para parar a proteção e um comentário.

    ![Parar o menu de cópia de segurança](./media/backup-azure-sql-database/stop-backup-button.png)

7. Selecione **parar cópia de segurança** para parar a proteção na base de dados. 

### <a name="resume-protection-for-a-sql-database"></a>Retomar a proteção para uma base de dados SQL

Se o **reter dados de cópia de segurança** opção tiver sido selecionada quando a proteção para a base de dados do SQL foi parada, pode retomar a proteção. Se os dados de cópia de segurança não era retidos, não é possível retomar a proteção. 

1. Para retomar a proteção da base de dados SQL, abra o item de cópia de segurança e selecione **retomar cópia de segurança**.

    ![Selecione a retomar a cópia de segurança para retomar a proteção de base de dados](./media/backup-azure-sql-database/resume-backup-button.png)

   O **política de cópia de segurança** é aberto o menu.

2. Sobre o **política de cópia de segurança** menu, selecione uma política e, em seguida, selecione **guardar**.

### <a name="trigger-an-adhoc-backup"></a>Acionar uma cópia de segurança ad hoc

Acionar as cópias de segurança ad hoc, conforme necessário. Existem quatro tipos de cópias de segurança ad hoc:

* Cópia de segurança completa
* Cópia de segurança completa apenas de cópia
* Cópia de segurança diferencial
* Cópia de segurança do registo

Para obter detalhes sobre cada tipo, consulte [cópias de segurança de tipos de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

### <a name="unregister-a-sql-server-instance"></a>Anular o registo de uma instância do SQL Server

Depois de remover a proteção, mas antes de eliminar o cofre, anular o registo uma instância do SQL Server:

1. Abra o Cofre de serviços de recuperação que está registado com a máquina virtual do SQL.

2. Sobre o **cofre dos serviços de recuperação** dashboard, em **gerir**, selecione **infraestrutura de cópia de segurança**.  

   ![Selecione a infraestrutura de cópia de segurança](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Sob **servidores de gestão**, selecione **servidores protegidos**.

   ![Selecione os servidores protegidos](./media/backup-azure-sql-database/protected-servers.png)

    O **servidores protegidos** é aberto o menu. 

4. Sobre o **servidores protegidos** menu, selecione o servidor ao anular o registo. Para eliminar o cofre, tem de anular o registo de todos os servidores.

5. Sobre o **servidores protegidos** menu, o servidor protegido com o botão direito e selecione **eliminar**. 

   ![Selecione Delete](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="faq"></a>FAQ

A secção seguinte fornece informações adicionais sobre a cópia de segurança do SQL da base de dados.

### <a name="can-i-throttle-the-speed-of-the-sql-server-backup-policy"></a>Pode limitar a velocidade da política de cópia de segurança do SQL Server?

Sim. Pode limitar a taxa a que a política de cópia de segurança é executada para minimizar o impacto numa instância do SQL Server.

Para alterar a definição:

1. Na instância do SQL Server, na pasta C:\Program c:\programas\azure carga de trabalho Backup\bin, abra a **TaskThrottlerSettings.json** ficheiro.

2. No ficheiro TaskThrottlerSettings.json, alterar os **DefaultBackupTasksThreshold** na definição de um valor inferior (por exemplo, 5).

3. Guarde as alterações. Feche o ficheiro.

4. Na instância do SQL Server, abra **Gerenciador de tarefas**. Reinicie o **serviço de coordenador da carga de trabalho de cópia de segurança do Azure**.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Pode executar um backup completo de uma réplica secundária?

Não. Esta funcionalidade não é suportada.

### <a name="do-successful-backup-jobs-create-alerts"></a>Tarefas de cópia de segurança bem-sucedida criar alertas?

Não. Tarefas de cópia de segurança com êxito não geram alertas. Os alertas são enviados apenas para tarefas de cópia de segurança que não obedeçam.

### <a name="can-i-see-details-for-scheduled-backup-jobs-in-the-jobs-menu"></a>Pode ver detalhes para tarefas de cópia de segurança agendadas no menu tarefas?

Não. O **tarefas de cópia de segurança** menu mostra detalhes da tarefa ad hoc, mas não agendadas tarefas de cópia de segurança. Se as tarefas de cópia de segurança agendadas falharem, os detalhes estão disponíveis nos alertas de tarefa falhada. Para monitorizar todos os agendados e tarefas de cópia de segurança ad hoc, usar [SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="when-i-select-a-sql-server-instance-are-future-databases-automatically-added"></a>Quando eu seleciono uma instância do SQL Server são bases de dados futuras adicionadas automaticamente?

Não. Ao configurar a proteção para uma instância do SQL Server, se selecionar a opção de nível de servidor, são adicionados a todas as bases de dados. Se adicionar bases de dados para uma instância do SQL Server depois de configurar a proteção, tem de adicionar manualmente as novas bases de dados para protegê-los. As bases de dados não são incluídos automaticamente na proteção configurada.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Alterar o modelo de recuperação, como reiniciar proteção?

Acione uma cópia de segurança completa. Backups de log começarem conforme esperado.

### <a name="can-i-protect-sql-always-on-availability-groups-where-the-primary-replica-is-on-premises"></a>Pode proteger SQL grupos de Disponibilidade AlwaysOn em que a réplica primária for no local

Não. O Azure Backup protege os SQL Servers em execução no Azure. Se um grupo de disponibilidade (AG) é distribuído por entre as máquinas do Azure e no local, o AG pode ser protegido apenas se a réplica primária está em execução no Azure. Além disso, o Azure Backup protege apenas os nós em execução na mesma região do Azure que o Cofre dos serviços de recuperação.

### <a name="can-i-protect-sql-always-on-availability-groups-which-are-spread-across-azure-regions"></a>Pode proteger sempre grupos de disponibilidade SQL que são distribuídas em várias regiões do Azure
Cofre de serviços de recuperação de cópia de segurança do Azure pode detetar e proteger todos os nós que estão na mesma região que o Cofre dos serviços de recuperação. Se tiver um SQL sempre no grupo de disponibilidade em diversas regiões do Azure, terá de configurar a cópia de segurança da região que tem o nó principal. Cópia de segurança do Azure será capaz de detetar e proteger todas as bases de dados no grupo de disponibilidade de acordo com a preferência de cópia de segurança. Se a preferência de cópia de segurança não for cumprida, as cópias de segurança irão falhar e receberá o alerta de falha.


## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o Azure Backup, veja o exemplo do Azure PowerShell para efetuar cópias de segurança de máquinas virtuais encriptadas.

> [!div class="nextstepaction"]
> [Fazer uma cópia de segurança de uma VM encriptada](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
