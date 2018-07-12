---
title: Utilizar o DMS para migrar para a instância gerida da base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como migrar a partir do SQL Server no local para a instância gerida da base de dados SQL do Azure ao utilizar o serviço de migração de base de dados do Azure.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/07/2018
ms.openlocfilehash: be007fe06f7b3354dc88e319a76715d2e94eb3fb
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971493"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>Migrar o SQL Server para o Azure SQL Database Managed Instance com o DMS
Pode utilizar o serviço de migração de base de dados do Azure para migrar as bases de dados a partir de uma instância do SQL Server no local para um [instância gerida da base de dados SQL do Azure](../sql-database/sql-database-managed-instance.md). Para métodos adicionais que podem exigir algum esforço manual, consulte o artigo [migração de instância do SQL Server para instância gerida da base de dados SQL do Azure](../sql-database/sql-database-managed-instance-migrate.md).

> [!IMPORTANT]
> Projetos de migração do SQL Server para instância gerida da base de dados SQL do Azure estão em pré-visualização e sujeito a [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Neste tutorial, migra os **Adventureworks2012** base de dados de uma instância no local do SQL Server para um Azure SQL Database Managed Instance ao utilizar o serviço de migração de base de dados do Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Crie uma instância do serviço de migração de base de dados do Azure.
> * Crie um projeto de migração com o serviço de migração de base de dados do Azure.
> * Execute a migração.
> * Monitorize a migração.
> * Transferir um relatório de migração.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, terá de:

- Criar uma VNET para o serviço de migração de base de dados do Azure com o modelo de implementação Azure Resource Manager, que garante uma conectividade site a site aos seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Saiba as topologias de rede para migrações de instância gerida BD SQL do Azure com o Azure Database Migration Service](https://aka.ms/dmsnetworkformi).
- Certifique-se de que seu executar de regras do Azure (VNET) rede segurança grupo de rede Virtual não bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para obter mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configurar o seu [Firewall do Windows para acesso ao motor de base de dados de origem](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Abra a Firewall do Windows para permitir que o serviço de migração de base de dados do Azure aceder à origem de SQL Server, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias nomeadas de SQL Server com as portas dinâmicas, pode querer ativar o serviço de Browser do SQL e permitir o acesso à porta UDP 1434 através de seus firewalls para que o serviço de migração de base de dados do Azure pode ligar a uma instância nomeada na sua origem servidor.
- Se estiver a utilizar uma aplicação de firewall na frente de seus bancos de dados de origem, terá de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure acessar os bancos de dados de origem para migração, bem como os arquivos por meio da porta SMB 445.
- Criar uma instância de gerida de base de dados do Azure SQL, seguindo o detalhe no artigo [criar uma instância de gerida de base de dados do Azure SQL no portal do Azure](https://aka.ms/sqldbmi).
- Certifique-se de que os inícios de sessão utilizados para ligar a origem do SQL Server e instância gerida de destino são membros da função de servidor sysadmin.
- Crie uma partilha de rede que o serviço de migração de base de dados do Azure podem utilizar para criar cópias de segurança da base de dados de origem.
- Certifique-se de que a conta de serviço que executa a origem do SQL Server, instância tem privilégios de escrita na partilha de rede que criou e que a conta de computador para o servidor de origem tem acesso de leitura/escrita para a mesma partilha.
- Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede que criou anteriormente. O Azure Database Migration Service representa a credencial do usuário para carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure para a operação de restauro.
- Criar um contentor de BLOBs e recuperar o seu URI de SAS, utilizando os passos no artigo [recursos de gerir o armazenamento de Blobs do Azure com o Explorador de armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), certifique-se de que seleciona todas as permissões (leitura, escrita, eliminação, lista) na janela de política ao mesmo tempo a criar o URI de SAS. Esse detalhe fornece o serviço de migração de base de dados do Azure com o acesso ao seu contentor da conta de armazenamento para carregar os ficheiros de cópia de segurança utilizados para a migração de bases de dados para a instância gerida da base de dados SQL do Azure

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft. datamigration

1. Inicie sessão no portal do Azure, selecione **todos os serviços**e, em seguida, selecione **subscrições**.

    ![Mostrar as assinaturas do portais](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.

    ![Mostrar os fornecedores de recursos](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Pesquisa para a migração e, em seguida, à direita da **Microsoft. datamigration**, selecione **registar**.

    ![Registar o fornecedor de recursos](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Criar uma instância de serviço de migração de base de dados do Azure

1. No portal do Azure, selecione + **criar um recurso**, procure **Azure Database Migration Service**e, em seguida, selecione **Azure Database Migration Service** na lista suspensa lista.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. Sobre o **Azure Database Migration Service** ecrã, selecione **criar**.

    ![Criar instância de serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. Sobre o **criar serviço de migração** ecrã, especifique um nome para o serviço, a subscrição e um grupo de recursos novo ou existente.

4. Selecione uma rede virtual (VNET) existente ou criar um.
 
    A VNET fornece o serviço de migração de base de dados do Azure com o acesso a origem do SQL Server e a instância gerida da base de dados SQL do Azure de destino.

    Para obter mais informações sobre como criar uma VNET no portal do Azure, consulte o artigo [criar uma rede virtual com o portal do Azure](https://aka.ms/DMSVnet).

    Para obter detalhes adicionais, consulte o artigo [topologias para migrações de instância gerida BD SQL do Azure com o serviço de migração de base de dados do Azure de rede](https://aka.ms/dmsnetworkformi).

5. Selecione um escalão de preço.

    Para obter mais informações sobre os custos e os escalões de preços, consulte a [página de preços](https://aka.ms/dms-pricing).
   
    ![Criar o serviço DMS](media\tutorial-sql-server-to-managed-instance\dms-create-service1.png)

6.  Selecione **criar** para criar o serviço.

## <a name="create-a-migration-project"></a>Criar um projeto de migração

Depois do serviço é criado, localizá-la no portal do Azure, abra-o e, em seguida, crie um novo projeto de migração.

1. No portal do Azure, selecione **todos os serviços**, procure o serviço de migração de base de dados do Azure e, em seguida, selecione **serviços de migração de base de dados do Azure**.

    ![Localize todas as instâncias do serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Na **ecrã Azure Database Migration Service**, procure o nome da instância que criou e, em seguida, selecione a instância.
 
3. Selecione + **novo projeto de migração**.

4. Na **novo projeto de migração** ecrã, especifique um nome para o projeto, no **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**e, em seguida, no **destino tipo de servidor** caixa de texto, selecione **instância gerida da base de dados SQL do Azure**.

   ![Criar projeto do DMS](media\tutorial-sql-server-to-managed-instance\dms-create-project1.png)

5. Selecione **criar** para criar o projeto.

## <a name="specify-source-details"></a>Especifique os detalhes de origem

1. Sobre o **detalhes de origem** ecrã, especifique os detalhes de ligação para a origem do SQL Server.

2. Se não tiver instalado um certificado fidedigno no seu servidor, selecione o **certificado de servidor fidedigno** caixa de verificação.

    Quando um certificado fidedigno não estiver instalado, o SQL Server gera um certificado autoassinado quando a instância é iniciada. Este certificado é utilizado para encriptar as credenciais para ligações de cliente.

    > [!CAUTION]
    > Ligações de SSL, que são encriptadas com um certificado autoassinado não fornece uma segurança forte. Eles são suscetíveis a ataques man-in-the-middle. Não deve confiar no SSL usando certificados auto-assinados num ambiente de produção ou nos servidores que estejam ligados à internet.

   ![Detalhes da origem](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. Selecione **Guardar**.

4. Sobre o **selecionar bases de dados de origem** ecrã, selecione o **Adventureworks2012** base de dados para migração.

   ![Selecionar bases de dados de origem](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. Selecione **Guardar**.

## <a name="specify-target-details"></a>Especifique os detalhes de destino

1.  Sobre o **detalhes de destino** ecrã, especifique os detalhes de ligação para o destino, o que é o pré-aprovisionado SQL da base de dados instância gerida do Azure para o qual o **AdventureWorks2012** base de dados está a ser migradas.

    Se ainda não tem o Azure SQL Database Managed Instance, selecione **não** para uma ligação para o ajudar a aprovisionar a instância. Pode mesmo assim, continuar com a criação do projeto e, em seguida, quando o Azure SQL Database Managed Instance estiver pronto, regresse a este projeto específico para executar a migração.   
 
       ![Selecionar destino](media\tutorial-sql-server-to-managed-instance\dms-target-details1.png)

2.  Selecione **Guardar**.

3.  Sobre o **resumo do projeto** ecrã, reveja e verifique os detalhes associados com o projeto de migração.
 
    ![Resumo do projeto de migração](media\tutorial-sql-server-to-managed-instance\dms-project-summary1.png)

4.  Selecione **Guardar**.   

## <a name="run-the-migration"></a>Executar a migração

1.  Selecione o projeto recentemente guardado, selecione + **nova atividade**e, em seguida, selecione **executar migração**.

    ![Criar Nova Atividade](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity1.png)

2.  Quando lhe for pedido, introduza as credenciais de origem e os servidores de destino e, em seguida, selecione **guardar**.

3.  Sobre o **selecionar bases de dados de origem** ecrã, selecione as bases de dados de origem que pretende migrar.

    ![Selecionar bases de dados de origem](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases2.png)

4.  Selecione **salvar**e, em seguida, no **selecione inícios de sessão** ecrã, selecione os inícios de sessão que pretende migrar.

    A versão atual só suporta a migração inícios de sessão do SQL.

    ![Selecione os inícios de sessão](media\tutorial-sql-server-to-managed-instance\dms-select-logins.png)

5. Selecione **salvar**e, em seguida, no **configurar definições de migração** ecrã, fornecer os seguintes detalhes:

    | | |
    |--------|---------|
    |**Partilha da localização de rede** | Rede local partilhar que o Azure Database Migration Service pode levar a origem a cópias de segurança da base de dados. A conta de serviço que executa a instância do SQL Server de origem tem de ter privilégios de escrita nesta partilha de rede. Fornecer um endereços IP ou FQDN do servidor no compartilhamento de rede, por exemplo, "\\\servername.domainname.com\backupfolder' ou '\\\IP address\backupfolder'.|
    |**Nome de utilizador** | Nome de utilizador windows que o serviço de migração de base de dados do Azure pode representar e carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure para a operação de restauro. |
    |**Palavra-passe** | Palavra-passe do utilizador. |
    |**Definições de conta de armazenamento** | O URI de SAS que fornece o serviço de migração de base de dados do Azure com o acesso ao seu contentor da conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança e de que é utilizado para a migração de bases de dados para a instância gerida da base de dados SQL do Azure. [Saiba como obter o URI de SAS para o contentor de BLOBs](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Configurar definições de migração](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings2.png)

5.  Selecione **salvar**e, em seguida, no **resumo da migração** , além de ecrã a **nome da atividade** texto caixa, especifique um nome para a atividade de migração.

    ![Resumo da migração](media\tutorial-sql-server-to-managed-instance\dms-migration-summary2.png)

6. Expanda a **opção de validação** secção para visualizar o **escolher a opção de validação** ecrã, especifique se pretende validar a base de dados migrada para a correção de consultas e, em seguida, selecione **guardar** .  

7. Selecione **executar migração**.

    Aparece a janela de atividade de migração e o estado da atividade é **pendente**.

## <a name="monitor-the-migration"></a>Monitorizar a migração

1. No ecrã de atividade de migração, selecione **atualizar** para atualizar a apresentação.
 
   ![Atividade de migração em curso](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-in-progress.png)

2. Pode expandir ainda mais as categorias de bases de dados e inícios de sessão para monitorizar o estado de migração de objetos do respetivo servidor.

   ![Atividade de migração em curso](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-monitor.png)

3. Depois de concluir a migração, selecione **transferir relatório** para obter um relatório que lista os detalhes associados com o processo de migração.
 
4. Certifique-se de que a base de dados de destino no ambiente de instância gerida da base de dados SQL do Azure de destino.

## <a name="next-steps"></a>Passos Seguintes

- Para obter um tutorial que mostra como migrar uma base de dados para uma instância gerida com o comando T-SQL RESTORE, consulte [restaurar uma cópia de segurança para uma instância gerida com o comando restore](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Para obter informações sobre a instância gerida, veja [o que é uma instância gerida](../sql-database/sql-database-managed-instance.md).
- Para obter informações sobre como ligar aplicações para uma instância gerida, veja [ligar aplicações](../sql-database/sql-database-managed-instance-connect-app.md).
