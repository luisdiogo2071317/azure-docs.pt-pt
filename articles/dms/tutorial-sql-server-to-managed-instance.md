---
title: 'Tutorial: Utilizar o DMS para migrar para a Instância Gerida da Base de Dados SQL do Azure | Microsoft Docs'
description: Saiba como migrar do SQL Server no local para a Instância Gerida da Base de Dados SQL do Azure com o Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: e1cce6231fbb31dac6526a01ec402533b3861a21
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956505"
---
# <a name="tutorial-migrate-sql-server-to-azure-sql-database-managed-instance-offline-using-dms"></a>Tutorial: Migrar o SQL Server para a Instância Gerida da Base de Dados SQL do Azure offline com o DMS
Pode utilizar o Azure Database Migration Service para migrar as bases de dados de uma instância do SQL Server no local para uma [Instância Gerida da Base de Dados SQL do Azure](../sql-database/sql-database-managed-instance.md). Para métodos adicionais que podem exigir algum trabalho manual, veja o artigo [SQL Server instance migration to Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-migrate.md) (Migração de instância do SQL Server para a Instância Gerida da Base de Dados SQL do Azure).

Neste tutorial, vai utilizar o Azure Database Migration Service para migrar a base de dados **Adventureworks2012** de uma instância no local do SQL Server para uma Instância Gerida da Base de Dado SQL do Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Criar uma instância do Azure Database Migration Service.
> * Utilizar o Azure Database Migration Service para criar um projeto de migração.
> * Executar a migração.
> * Monitorizar a migração.
> * Transferir um relatório da migração.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Este artigo descreve uma migração offline do SQL Server para a Instância Gerida da Base de Dados SQL do Azure. Para uma migração online, veja [Migrar o SQL Server para a Instância Gerida da Base de Dados Azure SQL online com o DMS](tutorial-sql-server-managed-instance-online.md).

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, precisa de:

- Utilizar o modelo de implementação Azure Resource Manager para criar uma VNET para o Azure Database Migration Service, que proporciona conectividade site a site aos seus servidores de origens no local mediante a utilização do [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou de [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Veja [Learn network topologies for Azure SQL DB Managed Instance migrations using the Azure Database Migration Service](https://aka.ms/dmsnetworkformi) (Conhecer as topologias de rede para migrações de Instância Gerida da BD SQL com o Azure Database Migration Service).
- Confirmar que as regras de Grupos de Segurança de Rede da Rede Virtual do Azure (VNET) não bloqueia as portas de comunicação 443, 53, 9354, 445 e 12000. Para obter mais detalhes sobre a filtragem de tráfego dos NSGs das VNETs do Azure, veja o artigo [Filter network traffic with network security groups](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Filtrar tráfego de rede com grupos de segurança de rede).
- Configurar a sua Firewall do Windows para acesso ao motor de bases de dados. Veja [Windows Firewall for source database engine access](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Abrir a Firewall do Windows para permitir ao Azure Database Migration Service aceder ao SQL Server de origem, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias nomeadas do SQL Server em portas dinâmicas, poderá ser útil ativar o SQL Browser Service e permitir o acesso à porta UDP 1434 através das suas firewalls, de modo a que o Azure Database Migration Service se possa ligar a uma instância nomeada no servidor de origem.
- Se estiver a utilizar uma aplicação de firewall à frente da base ou bases de dados, poderá ter de adicionar regras de firewall para permitir que o Azure Database Migration Service aceda à base ou bases de dados de origem para migração, bem como ficheiros através da porta SMB 445.
- Criar uma Instância Gerida da Base de Dados SQL do Azure; para tal, siga o detalhe descrito no artigo [Criar uma Instância Gerida da Base de Dados SQL do Azure no portal do Azure](https://aka.ms/sqldbmi).
- Confirme que os inícios de sessão utilizados para ligar o SQL Server de origem e a Instância Gerida de destino são membros da função de servidor sysadmin.
- Crie uma partilha de rede que o Azure Database Migration Service possa utilizar para criar uma cópia de segurança da base de dados de origem.
- Confirme que a conta de serviço em execução na instância do SQL Server de origem tem privilégios de escrita na partilha de rede que criou e que a conta do computador do servidor de origem tem acesso de leitura/escrita à mesma partilha.
- Tome nota de um utilizador do Windows (e da palavra-passe) que tenha privilégio de controlo total na partilha de rede que criou anteriormente. O Azure Database Migration Service representa a credencial do utilizador para carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure, para a operação de restauro.
- Crie um contentor de blobs e obtenha o URI da SAS com os passos no artigo [Manage Azure Blob Storage resources with Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) (Gerir recursos do Armazenamento de Blobs do Azure com o Explorador de Armazenamento). Confirme que seleciona todas as permissões (Leitura, Escrita, Eliminação, Listagem) na janela da política quando criar o URI da SAS. Este detalhe dá ao Azure Database Migration Service acesso ao contentor da conta de armazenamento para carregar os ficheiros de cópia de segurança utilizados para migrar as bases de dados para a Instância Gerida da Base de Dados SQL do Azure.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft.DataMigration

1. Inicie sessão no portal do Azure, selecione **Todos os serviços** e **Subscrições**.

    ![Mostrar subscrições no portal](media/tutorial-sql-server-to-managed-instance/portal-select-subscriptions.png)        

2. Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.

    ![Mostrar fornecedores de recursos](media/tutorial-sql-server-to-managed-instance/portal-select-resource-provider.png)

3. Procure por migração e, à direita de **Microsoft.DataMigration**, selecione **Registar**.

    ![Registar o fornecedor de recursos](media/tutorial-sql-server-to-managed-instance/portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Criar uma instância do Azure Database Migration Service

1. No portal do Azure, selecione + **Criar um recurso**, procure o **Azure Database Migration Service** e selecione **Azure Database Migration Service**, na lista pendente.

     ![Azure Marketplace](media/tutorial-sql-server-to-managed-instance/portal-marketplace.png)

2. No ecrã **Azure Database Migration Service**, selecione **Criar**.

    ![Criar instância do Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance/dms-create1.png)

3. No ecrã **Criar Serviço de Migração**, especifique um nome para o serviço, a subscrição e um grupo de recursos novo ou já existente.

4.  Selecione a localização em que pretende criar a instância do DMS.

5. Selecione uma rede virtual (VNET) já existente ou crie uma.
 
    A VNET dá ao Azure Database Migration Service acesso à instância do SQL Server de origem e à Instância Gerida da Base de Dados SQL do Azure de destino.

    Para obter mais informações sobre como criar uma VNET no portal do Azure, veja o artigo [Criar uma rede virtual com o portal do Azure](https://aka.ms/DMSVnet).

    Para obter detalhes adicionais, veja o artigo [Network topologies for Azure SQL DB Managed Instance migrations using the Azure Database Migration Service](https://aka.ms/dmsnetworkformi) (Topologias de rede para migrações da Instância Gerida da DB SQL do Azure com o Azure Database Migration Service).

6. Selecione um escalão de preço.

    Para obter mais informações sobre os custos e os escalões de preços, veja a [página de preços](https://aka.ms/dms-pricing).
   
    ![Criar o serviço DMS](media/tutorial-sql-server-to-managed-instance/dms-create-service2.png)

7.  Selecione **Criar** para criar o serviço.

## <a name="create-a-migration-project"></a>Criar um projeto de migração

Após a criação de uma instância do serviço, localize-a no portal do Azure, abra-a e crie um projeto de migração novo.

1. No portal do Azure, selecione **Todos os serviços**, procure Azure Database Migration Service e selecione **Azure Database Migration Services**.

    ![Localizar todas as instâncias do Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance/dms-search.png)

2. No ecrã **Azure Database Migration Service**, procure o nome da instância que criou e selecione-a.
 
3. Selecione + **Novo Projeto de Migração**.

4. No ecrã **Novo projeto de migração**, indique um nome para o projeto, na caixa de texto **Tipo de servidor de origem**, selecione **SQL Server**, na caixa de texto **Tipo de servidor de destino**, selecione **Instância Gerida da Base de Dados SQL do Azure** e, em **Escolher tipo de atividade**, selecione **Migração de dados offline**.

   ![Criar o projeto do DMS](media/tutorial-sql-server-to-managed-instance/dms-create-project2.png)

5. Selecione **Create** (Criar) para criar o projeto.

## <a name="specify-source-details"></a>Especificar os detalhes da origem

1. No ecrã **Detalhes da origem da migração**, especifique os detalhes da ligação do SQL Server de origem.

2. Se não tiver um certificado fidedigno instalado no seu servidor, selecione a caixa de verificação **Confiar no certificado de servidor**.

    Se não tiver um certificado fidedigno instalado, o SQL Server gera um certificado autoassinado quando a instância é iniciada. Este certificado é utilizado para encriptar as credenciais para as ligações de cliente.

    > [!CAUTION]
    > As ligações SSL encriptadas que utilizem um certificado autoassinado não proporcionam segurança forte. São suscetíveis a ataques man-in-the-middle. Não deve confiar em SSL com certificados autoassinados num ambiente de produção ou em servidores que estejam ligados à Internet.

   ![Detalhes da origem](media/tutorial-sql-server-to-managed-instance/dms-source-details1.png)

3. Selecione **Guardar**.

4. No ecrã **Selecionar bases de dados de origem**, selecione a base de dados **Adventureworks2012** para migração.

   ![Selecionar as bases de dados de origem](media/tutorial-sql-server-to-managed-instance/dms-source-database1.png)

5. Selecione **Guardar**.

## <a name="specify-target-details"></a>Especificar os detalhes do destino

1.  No ecrã **Detalhes do destino da migração**, especifique os detalhes da ligação do destino, que é a Instância Gerida da Base de Dados SQL do Azure pré-aprovisionada para a qual está a migrar a base de dados **AdventureWorks2012**.

    Se ainda não tiver aprovisionado a Instância Gerida da Base de Dados SQL do Azure, selecione **Não** para obter uma ligação que o ajude a aprovisionar a instância. Pode, mesmo assim, avançar com a criação do projeto e, depois, quando a Instância Gerida da Base de Dados SQL do Azure estiver pronta, regresse a este projeto específico e execute a migração.   
 
       ![Selecionar o Destino](media/tutorial-sql-server-to-managed-instance/dms-target-details2.png)

2.  Selecione **Guardar**.

## <a name="select-source-databases"></a>Selecionar as bases de dados de origem

1. No ecrã **Selecionar as base de dados de origem**, selecione a base de dados de origem que quer migrar.

    ![Selecionar as bases de dados de origem](media/tutorial-sql-server-to-managed-instance/select-source-databases.png)

2. Selecione **Guardar**.

## <a name="select-logins"></a>Selecionar os inícios de sessão
 
1. No ecrã **Selecionar os inícios de sessão**, selecione os inícios de sessão que quer migrar.

    >[!NOTE]
    >Esta versão só suporta a migração de inícios de sessão do SQL.

    ![Selecionar os inícios de sessão](media/tutorial-sql-server-to-managed-instance/select-logins.png)

2. Selecione **Guardar**.
 
## <a name="configure-migration-settings"></a>Configurar as definições da migração
 
1. No ecrã **Configurar as definições da migração**, indique os detalhes seguintes:

    | | |
    |--------|---------|
    |**Escolher a opção de cópia de segurança de origem** | Escolha a opção **Vou disponibilizar os ficheiros de cópia de segurança mais recentes** se já tiver ficheiros de cópia de segurança completa disponíveis para o DMS utilizar para migrar a base de dados. Escolha a opção **O Azure Database Migration Service vai criar os ficheiros de cópia de segurança** se quiser que o DMS utilize a cópia de segurança completa da base de dados de origem e a utilize para a migração. |
    |**Partilha de localização na rede** | A partilha de rede SMB local que o Azure Database Migration Service pode utilizar para criar cópias de segurança da base de dados de origem. A conta de serviço que estiver a executar a instância do SQL Server de origem tem de ter privilégios de escrita nesta partilha de rede. Indique um FQDN ou um endereço IP do servidor na partilha de rede, como, por exemplo, “\\\servername.domainname.com\backupfolder” ou “\\\IP address\backupfolder”.|
    |**Nome de utilizador** | Certifique-se de que o utilizador do Windows tem privilégio de controlo total na partilha de rede que indicou acima. O Azure Database Migration Service vai representar a credencial do utilizador para carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure, para a operação de restauro. Se estiverem selecionadas bases de dados ativadas para TDE para migração, o utilizador do Windows acima tem de ser a conta de administrador incorporada e o [Controlo de Conta de Utilizador](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/user-account-control-overview) tem de ser desativado, para que o Azure Database Migration Service carregue e elimine os ficheiros de certificado. |
    |**Palavra-passe** | A palavra-passe do utilizador. |
    |**Definições da conta de armazenamento** | O URI da SAS que dá ao Azure Database Migration Service acesso ao seu contentor da conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança e que é utilizado para migrar as bases de dados para a Instância Gerida da Base de Dados SQL do Azure. Veja [Learn how to get the SAS URI for blob container](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) (Saiba como obter o URI da SAS para o contentor de blobs).|
    |**Definições de TDE** | Se estiver a migrar as bases de dados de origem com a Encriptação de Dados Transparente (TDE) ativada, terá de ter privilégios de escrita na Instância Gerida da Base de Dados SQL do Azure de destino.  Selecione a subscrição na qual a Instância Gerida da BD SQL do Azure foi aprovisionada, no menu pendente.  Selecione a **Instância Gerida da Base de Dados SQL do Azure** de destino no menu pendente. |
    
    ![Configurar as Definições da Migração](media/tutorial-sql-server-to-managed-instance/dms-configure-migration-settings3.png)

2. Selecione **Guardar**.
 
## <a name="review-the-migration-summary"></a>Rever o resumo da migração

1. No ecrã **Resumo da migração**, na caixa de texto **Nome da atividade**, indique um nome para a atividade de migração.

2. Expanda a secção **Opção de validação** para mostrar o ecrã **Escolher opção de validação**, especifique se a base de dados deve ser verificada quanto à exatidão das consultas e selecione **Guardar**.

3. Reveja e verifique os detalhes associados ao projeto de migração.
 
    ![Resumo do projeto de migração](media/tutorial-sql-server-to-managed-instance/dms-project-summary2.png)

4.  Selecione **Guardar**.   

## <a name="run-the-migration"></a>Executar a migração

- Selecione **Executar a migração**.

  É apresentada a janela de atividade da migração e o estado da atividade é **Pendente**.

## <a name="monitor-the-migration"></a>Monitorizar a migração

1. No ecrã de atividade da migração, selecione **Atualizar**, para atualizar o ecrã.
 
   ![Atividade de migração em curso](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration1.png)

    Pode expandir ainda mais as categorias de bases de dados e inícios de sessão para monitorizar o estado da migração dos respetivos objetos de servidor.

   ![Atividade de migração em curso](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration-extend.png)

2. Após a conclusão da migração, selecione **Transferir relatório** para obter um relatório que liste os detalhes associados ao processo de migração.
 
3. Teste a base de dados de destino no ambiente da Instância Gerida da Base de Dados SQL do Azure de destino.

## <a name="next-steps"></a>Passos Seguintes

- Para um tutorial que lhe mostra como migrar uma base de dados para uma Instância Gerida com o comando T-SQL RESTORE, veja [Restaurar uma cópia de segurança para uma Instância Gerida com o comando “restore”](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Para obter informações sobre a Instância Gerida, veja [What is a Managed Instance](../sql-database/sql-database-managed-instance.md) (O que é uma Instância Gerida?).
- Para obter informações sobre como ligar aplicações a uma Instância Gerida, veja [Connect applications](../sql-database/sql-database-managed-instance-connect-app.md) (Ligar aplicações).