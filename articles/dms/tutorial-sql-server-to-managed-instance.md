---
title: Utilizar o serviço de migração de base de dados do Azure para migrar o SQL Server para instância geridos base de dados SQL do Azure | Microsoft Docs
description: Saiba como migrar a partir do SQL Server no local para a base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 03/29/2018
ms.openlocfilehash: 8abf3bae3a2274ed5514a5c621675b4c9ec27ae2
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance"></a>Migrar do SQL Server para instância gerida de base de dados SQL do Azure
Pode utilizar o serviço de migração de base de dados do Azure para migrar as bases de dados de uma instância do SQL Server no local para a SQL Database do Azure. Neste tutorial, migra a **Adventureworks2012** base de dados de uma instância no local do SQL Server para uma base de dados do SQL do Azure utilizando o serviço de migração de base de dados do Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Crie uma instância do serviço de migração de base de dados do Azure.
> * Crie um projeto de migração utilizando o serviço de migração de base de dados do Azure.
> * Execute a migração.
> * Monitorize a migração.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, precisa de:

- Criar uma VNET para o serviço de migração de base de dados do Azure utilizando o modelo de implementação Azure Resource Manager, que fornece a conectividade de site a site para os seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Saiba topologias de rede para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure](https://aka.ms/dmsnetworkformi).
- Certifique-se de que a sua escolha de regras do Azure (VNET) rede segurança grupo de rede Virtual bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Configurar o seu [Firewall do Windows para acesso ao motor de base de dados de origem](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Abra a Firewall do Windows para permitir que o serviço de migração de base de dados do Azure para aceder à origem de SQL Server, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias do SQL Server com nome utilizando as portas dinâmicas, poderá pretender ativar o serviço de Browser do SQL Server e permitir o acesso à porta UDP 1434 através das firewalls para que o serviço de migração de base de dados do Azure podem ligar a uma instância nomeada na sua origem servidor.
- Se estiver a utilizar uma aplicação de firewall à frente das bases de dados de origem, poderá ter de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure para aceder a bases de dados de origem para migração, bem como os ficheiros através de porta SMB 445.
- Criar uma instância de base de dados geridas por instância de SQL do Azure, seguindo o detalhe no artigo [criar uma instância de gerido da base de dados do Azure SQL no portal do Azure](https://aka.ms/sqldbmi).
- Certifique-se de que os inícios de sessão utilizados para ligar a origem de SQL Server e instância geridos de destino são membros da função de servidor sysadmin.
- Crie uma partilha de rede que pode utilizar o serviço de migração de base de dados do Azure para criar cópias de segurança da base de dados de origem.
- Certifique-se de que a conta de serviço que executa a instância do SQL Server de origem tem privilégios de escrita na partilha de rede que criou.
- Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede que criou acima. O serviço de migração de base de dados do Azure representa tem as credenciais de utilizador para carregar os ficheiros de cópia de segurança no contentor de armazenamento do Azure para a operação de restauro.
- Criar um contentor de blob e obter o seu respetivo URI de SAS, utilizando os passos no artigo [recursos de gerir o armazenamento de Blobs do Azure com o Explorador de armazenamento (pré-visualização)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), certifique-se de selecionar todas as permissões (leitura, escrita, eliminar, lista) na janela de política ao criar o URI de SAS. Isto fornece o serviço de migração de base de dados do Azure com acesso ao contentor de conta de armazenamento para carregar os ficheiros de cópia de segurança utilizados para migrar bases de dados da base de dados geridos instância do SQL do Azure

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft.DataMigration

1.  Inicie sessão no portal do Azure, selecione **todos os serviços**e, em seguida, selecione **subscrições**.
![Mostrar subscrições portais](media\tutorial-sql-server-to-managed-instance\portal-select-subscription.png)

1.  Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.
![Mostrar os fornecedores de recursos](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

1.  Pesquisa para a migração e, em seguida, à direita do **Microsoft.DataMigration**, selecione **registar**.
![registar o fornecedor de recursos](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Criar uma instância

1.  No portal do Azure, selecione **+ criar um recurso**, procure **serviço de migração de base de dados do Azure**e, em seguida, selecione **serviço de migração de base de dados do Azure** da lista pendente lista.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

1.  No **serviço de migração de base de dados do Azure (pré-visualização)** ecrã, selecione **criar**.

    ![Criar uma instância de serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-managed-instance\dms-create.png)

1.  No **serviço de migração de base de dados** ecrã, especifique um nome para o serviço, a subscrição, grupo de recursos, uma rede virtual e o escalão de preço.

    Para obter mais informações sobre os custos e escalões de preços, consulte o [página de preços](https://aka.ms/dms-pricing). *O serviço de migração de base de dados do Azure está atualmente em pré-visualização e não será cobrada.*

    **Rede:** selecione um existente ou crie uma nova VNET, que fornece o serviço de migração de base de dados do Azure com acesso à origem de SQL Server e o destino da base de dados geridas por instância de SQL do Azure. [Saiba topologias de rede para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure](https://aka.ms/dmsnetworkformi).

    Para obter mais informações sobre como criar a VNET no portal do Azure, consulte [criar uma rede virtual com várias sub-redes no portal do Azure](https://aka.ms/DMSVnet).

    ![Criar serviço DMS](media\tutorial-sql-server-to-managed-instance\dms-create-service.png)

1.  Selecione **criar** ao criar o serviço.


## <a name="create-a-migration-project"></a>Criar um projeto de migração

Depois de criar o serviço, localizá-la no portal do Azure e abri-lo.

1.  Selecione **+ novo projeto de migração**.

1.  No **novo projeto de migração** ecrã, especifique um nome para o projeto no **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**e, em seguida, no **destino tipo de servidor** caixa de texto, selecione **da base de dados geridas por instância de SQL do Azure**.

    ![Criar projeto do DMS](media\tutorial-sql-server-to-managed-instance\dms-create-project.png)

1.  Selecione **criar** para criar o projeto.

## <a name="specify-source-details"></a>Especifique os detalhes de origem

1.  No **origem detalhes** ecrã, especifique os detalhes de ligação para a origem de SQL Server.

    ![Detalhes de origem](media\tutorial-sql-server-to-managed-instance\dms-source-details.png)

1.  Selecione **guardar**e, em seguida, selecione o **Adventureworks2012** base de dados para migração.

    ![Selecionar bases de dados de origem](media\tutorial-sql-server-to-managed-instance\dms-source-database.png)

## <a name="specify-target-details"></a>Especifique os detalhes de destino

1.  Selecione **guardar**e, em seguida, no **detalhes de destino** ecrã, especifique os detalhes de ligação para o destino, o que é pré-aprovisionada Azure instância do SQL da base de dados geridos para os quais o  **AdventureWorks2012** será possível migrar a base de dados.

    ![Selecione o destino](media\tutorial-sql-server-to-managed-instance\dms-target-details.png)

1.  Selecione **Guardar**.

1.  No **resumo de projeto** ecrã, reveja e verifique os detalhes associados ao projeto de migração.

## <a name="run-the-migration"></a>Executar a migração

1.  Selecione o projeto guardado recentemente, selecione **+ nova atividade**e, em seguida, selecione **executar migração**.

    ![Criar nova atividade](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity.png)

1.  Quando lhe for pedido, introduza as credenciais de origem e os servidores de destino e, em seguida, selecione **guardar**.

1.  No **mapa para bases de dados de destino** ecrã, selecione as bases de dados de origem que pretende migrar.

    ![Selecionar bases de dados de origem](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases.png)

1.  Selecione **guardar**, no **configurar definições de migração** ecrã, forneça os detalhes seguintes:

    | | |
    |--------|---------|
    |**Localização de cópia de segurança do servidor** | A rede local partilhar que o serviço de migração de base de dados do Azure pode demorar a origem a cópias de segurança da base de dados. A conta de serviço que executa a instância do SQL Server de origem tem de ter privilégios de escrita nesta partilha de rede. |
    |**Nome de utilizador** | Nome de utilizador windows que o serviço de migração de base de dados do Azure pode representar e carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure para a operação de restauro. |
    |**Palavra-passe** | Palavra-passe para o utilizador acima. |
    |**URI de SAS do armazenamento** | URI de SAS que fornece o serviço de migração de base de dados do Azure com acesso ao contentor de conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança e de que é utilizado para migrar as bases de dados à instância gerido da base de dados SQL do Azure. [Saber como obter o URI de SAS do contentor de blob](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Configurar definições de migração](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings.png)

1.  Selecione **guardar**, no ecrã de migração resumo, no **nome da atividade** texto caixa, especifique um nome para a atividade de migração.

    ![Resumo de migração](media\tutorial-sql-server-to-managed-instance\dms-migration-summary.png)


## <a name="monitor-the-migration"></a>Monitorizar a migração

1.  Selecione a atividade de migração para rever o estado da atividade.

1.  Depois de concluída a migração, certifique-se as bases de dados de destino no destino da base de dados geridas por instância de SQL do Azure.

    ![Monitorizar a migração](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration.png)

