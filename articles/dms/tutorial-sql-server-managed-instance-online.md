---
title: Utilizar o Azure Database Migration Service para realizar uma migração online do SQL Server para a Instância Gerida da Base de Dados Azure SQL | Microsoft Docs
description: Aprenda a realizar uma migração do SQL Server no local para a Instância Gerida da Base de Dados Azure SQL com o Azure Database Migration Service.
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
ms.openlocfilehash: 56bd3ab61045ff99601ed50cd27332b370204795
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068031"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-online-using-dms"></a>Migrar o SQL Server para a Instância Gerida da Base de Dados Azure SQL online com o DMS
Pode utilizar o Azure Database Migration Service para migrar as bases de dados de uma instância do SQL Server no local para uma [Instância Gerida da Base de Dados Azure SQL](../sql-database/sql-database-managed-instance.md), com um período de indisponibilidade mínimo. Para métodos adicionais que podem exigir algum trabalho manual, veja o artigo [SQL Server instance migration to Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-migrate.md) (Migração de instância do SQL Server para a Instância Gerida da Base de Dados SQL do Azure).

>[!IMPORTANT]
>Os projetos de migração online do SQL Server para a Instância Gerida da Base de Dados Azure SQL estão em pré-visualização e estão sujeitos aos [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Neste tutorial, vai utilizar o Azure Database Migration Service para migrar a base de dados **Adventureworks2012** de uma instância no local do SQL Server para uma Instância Gerida da Base de Dados Azure SQL, com um período de indisponibilidade mínimo.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Criar uma instância do Azure Database Migration Service.
> * Crie um projeto de migração e inicie a migração online com o Azure Database Migration Service.
> * Monitorizar a migração.
> * Transfira a migração quando estiver pronto.

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
- Forneça uma partilha de rede SMB que contenha os ficheiros completos de cópia de segurança de base de dados e os ficheiros subsequentes de cópia de segurança de registo de transações, que o Azure Database Migration Service pode utilizar para a migração da base de dados.
- Confirme que a conta de serviço em execução na instância do SQL Server de origem tem privilégios de escrita na partilha de rede que criou e que a conta do computador do servidor de origem tem acesso de leitura/escrita à mesma partilha.
- Tome nota de um utilizador do Windows (e da palavra-passe) que tenha privilégio de controlo total na partilha de rede que criou anteriormente. O Azure Database Migration Service representa a credencial do utilizador para carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure, para a operação de restauro.
- Crie um ID de Aplicação do Azure Active Directory que vai gerar a chave de ID da Aplicação que o Serviço DMS pode utilizar para ligar à MI de destino e ao Contentor de Armazenamento do Azure. Para obter mais informações, veja o artigo [Utilizar o portal para criar uma aplicação e um principal de serviço do Azure Active Directory que possam aceder aos recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).
- Crie ou anote o **escalão de Desempenho Standard**, a Conta de Armazenamento do Azure, que permite ao serviço DMS carregar os ficheiros de cópia de segurança para a base de dados e utilizá-la para a migração de bases de dados.  Certifique-se de que cria a Conta de Armazenamento do Azure na mesma região do serviço DMS criado.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft.DataMigration

1. Inicie sessão no portal do Azure, selecione **Todos os serviços** e **Subscrições**.

    ![Mostrar subscrições no portal](media\tutorial-sql-server-to-managed-instance-online\portal-select-subscriptions.png)        
2. Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.

    ![Mostrar fornecedores de recursos](media\tutorial-sql-server-to-managed-instance-online\portal-select-resource-provider.png)

3. Procure por migração e, à direita de **Microsoft.DataMigration**, selecione **Registar**.

    ![Registar o fornecedor de recursos](media\tutorial-sql-server-to-managed-instance-online\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Criar uma instância do Azure Database Migration Service

1. No portal do Azure, selecione + **Criar um recurso**, procure o **Azure Database Migration Service** e selecione **Azure Database Migration Service**, na lista pendente.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance-online\portal-marketplace.png)

2. No ecrã **Azure Database Migration Service**, selecione **Criar**.

    ![Criar instância do Azure Database Migration Service](media\tutorial-sql-server-to-managed-instance-online\dms-create1.png)

3. No ecrã **Criar Serviço de Migração**, especifique um nome para o serviço, a subscrição e um grupo de recursos novo ou já existente.

4.  Selecione a localização em que pretende criar a instância do DMS.

5. Selecione uma rede virtual (VNET) já existente ou crie uma.
 
    A VNET dá ao Azure Database Migration Service acesso à instância do SQL Server de origem e à Instância Gerida da Base de Dados SQL do Azure de destino.

    Para obter mais informações sobre como criar uma VNET no portal do Azure, veja o artigo [Criar uma rede virtual com o portal do Azure](https://aka.ms/DMSVnet).

    Para obter detalhes adicionais, veja o artigo [Network topologies for Azure SQL DB Managed Instance migrations using the Azure Database Migration Service](https://aka.ms/dmsnetworkformi) (Topologias de rede para migrações da Instância Gerida da DB SQL do Azure com o Azure Database Migration Service).

6. Selecione um SKU do escalão de preço “Crítico para a Empresa (Pré-visualização)”.

    > [!NOTE]
    > As migrações online só são suportadas ao utilizar o escalão “Crítico para a Empresa (Pré-visualização)”. 
   
    Para obter mais informações sobre os custos e os escalões de preços, veja a [página de preços](https://aka.ms/dms-pricing).
   
    ![Criar o serviço DMS](media\tutorial-sql-server-to-managed-instance-online\dms-create-service3.png)

7.  Selecione **Criar** para criar o serviço.

## <a name="create-a-migration-project"></a>Criar um projeto de migração

Após a criação de uma instância do serviço, localize-a no portal do Azure, abra-a e crie um projeto de migração novo.

1. No portal do Azure, selecione **Todos os serviços**, procure Azure Database Migration Service e selecione **Azure Database Migration Services**.

    ![Localizar todas as instâncias do Azure Database Migration Service](media\tutorial-sql-server-to-managed-instance-online\dms-search.png)

2. No ecrã **Azure Database Migration Service**, procure o nome da instância que criou e selecione-a.
 
3. Selecione + **Novo Projeto de Migração**.

4. No ecrã **Novo projeto de migração**, indique um nome para o projeto, na caixa de texto **Tipo de servidor de origem**, selecione **SQL Server**, na caixa de texto **Tipo de servidor de destino**, selecione **Instância Gerida da Base de Dados Azure SQL** e, em **Escolher tipo de atividade**, selecione **Migração de dados online (pré-visualização)**.

   ![Criar o projeto do DMS](media\tutorial-sql-server-to-managed-instance-online\dms-create-project3.png)

5. Selecione **Criar e executar atividade** para criar o projeto.

## <a name="specify-source-details"></a>Especificar os detalhes da origem

1. No ecrã **Detalhes da origem da migração**, especifique os detalhes da ligação do SQL Server de origem.

2. Se não tiver um certificado fidedigno instalado no seu servidor, selecione a caixa de verificação **Confiar no certificado de servidor**.

    Se não tiver um certificado fidedigno instalado, o SQL Server gera um certificado autoassinado quando a instância é iniciada. Este certificado é utilizado para encriptar as credenciais para as ligações de cliente.

    > [!CAUTION]
    > As ligações SSL encriptadas que utilizem um certificado autoassinado não proporcionam segurança forte. São suscetíveis a ataques man-in-the-middle. Não deve confiar em SSL com certificados autoassinados num ambiente de produção ou em servidores que estejam ligados à Internet.

   ![Detalhes da origem](media\tutorial-sql-server-to-managed-instance-online\dms-source-details2.png)

3. Selecione **Guardar**.

## <a name="specify-target-details"></a>Especificar os detalhes do destino

1.  No ecrã **Detalhes do destino da migração**, especifique o **ID da aplicação** e a **Chave** que a instância do DMS pode utilizar para ligar à instância de destino da Instância Gerida da Base de Dados Azure SQL e à Conta de Armazenamento do Azure.

    Para obter mais informações, veja o artigo [Utilizar o portal para criar uma aplicação e um principal de serviço do Azure Active Directory que possam aceder aos recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).
    
2. Selecione a **Subscrição** que contém a instância de destino da Instância Gerida da Base de Dados Azure SQL e, em seguida, selecione a instância de destino.

    Se ainda não tiver aprovisionado a Instância Gerida da Base de Dados Azure SQL, selecione a [ligação](https://aka.ms/SQLDBMI) para ajudá-lo a aprovisionar a instância. Quando a instância da Instância Gerida da Base de Dados Azure SQL estiver pronta, regresse a este projeto específico e execute a migração.

3. Indique o **Utilizador SQL** e a **Palavra-passe** para ligar à instância de destino da Instância Gerida da Base de Dados Azure SQL.

       ![Select Target](media\tutorial-sql-server-to-managed-instance-online\dms-target-details3.png)

2.  Selecione **Guardar**.

## <a name="select-source-databases"></a>Selecionar as bases de dados de origem

1. No ecrã **Selecionar as base de dados de origem**, selecione a base de dados de origem que quer migrar.

    ![Selecionar as bases de dados de origem](media\tutorial-sql-server-to-managed-instance-online\dms-select-source-databases2.png)

2. Selecione **Guardar**.

## <a name="configure-migration-settings"></a>Configurar as definições da migração
 
1. No ecrã **Configurar as definições da migração**, indique os detalhes seguintes:

    | | |
    |--------|---------|
    |**Partilha de localização na Rede SMB** | A partilha de rede SMB local que contém os ficheiros Completos de cópia de segurança de base de dados e os ficheiros de cópia de segurança de registo de transações, que o Azure Database Migration Service pode utilizar para a migração. A conta de serviço que estiver a executar a instância do SQL Server de origem tem de ter privilégios de leitura\escrita nesta partilha de rede. Indique um FQDN ou um endereço IP do servidor na partilha de rede, como, por exemplo, “\\\servername.domainname.com\backupfolder” ou “\\\IP address\backupfolder”.|
    |**Nome de utilizador** | Certifique-se de que o utilizador do Windows tem privilégio de controlo total na partilha de rede que indicou acima. O Azure Database Migration Service vai representar a credencial do utilizador para carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure, para a operação de restauro. |
    |**Palavra-passe** | A palavra-passe do utilizador. |
    |**Subscrição da Conta de Armazenamento do Azure** | Selecione a subscrição que contém a Conta de Armazenamento do Azure. |
    |**Conta de Armazenamento do Azure** | Selecione a Conta de Armazenamento do Azure para a qual o DMS pode carregar os ficheiros de cópia de segurança da partilha de rede SMB e utilizá-los para a migração de base de dados.  Recomendamos que selecione a Conta de Armazenamento na mesma região do serviço DMS para um desempenho de carregamento de ficheiros ideal. |
    
    ![Configurar as Definições da Migração](media\tutorial-sql-server-to-managed-instance-online\dms-configure-migration-settings4.png)

2. Selecione **Guardar**.
 
## <a name="review-the-migration-summary"></a>Rever o resumo da migração

1. No ecrã **Resumo da migração**, na caixa de texto **Nome da atividade**, indique um nome para a atividade de migração.

2. Reveja e verifique os detalhes associados ao projeto de migração.
 
    ![Resumo do projeto de migração](media\tutorial-sql-server-to-managed-instance-online\dms-project-summary3.png)

## <a name="run-and-monitor-the-migration"></a>Executar e monitorizar a migração

1. Selecione **Executar a migração**.

2. No ecrã de atividade da migração, selecione **Atualizar** para atualizar o ecrã.
 
   ![Atividade de migração em curso](media\tutorial-sql-server-to-managed-instance-online\dms-monitor-migration2.png)

    Pode expandir ainda mais as categorias de bases de dados e inícios de sessão para monitorizar o estado da migração dos respetivos objetos de servidor.

   ![Atividade de migração em curso](media\tutorial-sql-server-to-managed-instance-online\dms-monitor-migration-extend2.png)

## <a name="performing-migration-cutover"></a>Efetuar a transferência da migração

Depois de restaurar a cópia de segurança completa da base de dados na instância do destino da Instância Gerida da Base de Dados Azure SQL, a base de dados fica disponível para executar a transferência da migração.

1.  Quando estiver pronto para concluir a migração da base de dados online, selecione **Iniciar Transferência**.

2.  Pare todo o tráfego de entrada para as bases de dados de origem.

3.  Com a [cópia de segurança de registo após erro], disponibilize o ficheiro de cópia de segurança na rede de partilha SMB e, em seguida, aguarde até que a cópia de segurança de registo de transações seja restaurada.

    Nesse momento, verá que as **Alterações pendentes** estão definidas como 0.

4.  Selecione **Confirmar** e, em seguida, selecione **Aplicar**.

    ![Preparar para concluir transferência](media\tutorial-sql-server-to-managed-instance-online\dms-complete-cutover.png)

5.  Quando o estado da migração de base de dados apresentar **Concluído**, ligue as suas aplicações à nova instância de destino da Instância Gerida da Base de Dados Azure SQL.

    ![Transferência concluída](media\tutorial-sql-server-to-managed-instance-online\dms-cutover-complete.png)

## <a name="next-steps"></a>Passos seguintes

- Para um tutorial que lhe mostra como migrar uma base de dados para uma Instância Gerida com o comando T-SQL RESTORE, veja [Restaurar uma cópia de segurança para uma Instância Gerida com o comando “restore”](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Para obter informações sobre a Instância Gerida, veja [What is a Managed Instance](../sql-database/sql-database-managed-instance.md) (O que é uma Instância Gerida?).
- Para obter informações sobre como ligar aplicações a uma Instância Gerida, veja [Connect applications](../sql-database/sql-database-managed-instance-connect-app.md) (Ligar aplicações).
