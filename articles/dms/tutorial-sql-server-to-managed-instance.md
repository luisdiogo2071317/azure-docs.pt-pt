---
title: Utilize o DMS para migrar para a base de dados geridas por instância de SQL do Azure | Microsoft Docs
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
ms.date: 05/07/2018
ms.openlocfilehash: 86af0101d84fe9cd44211a931567a85d7b5166e0
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261615"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>Migrar do SQL Server para o Azure SQL da base de dados geridos instância utilizando DMS
Pode utilizar o serviço de migração de base de dados do Azure para migrar as bases de dados a partir de uma instância do SQL Server no local para um [da base de dados geridas por instância de SQL do Azure](../sql-database/sql-database-managed-instance.md). Para métodos adicionais que podem necessitar de algum esforço manual, consulte o artigo [migração de instância do SQL Server para instância geridos base de dados SQL do Azure](../sql-database/sql-database-managed-instance-migrate.md).

> [!IMPORTANT]
> Projetos de migração do SQL Server para a base de dados geridas por instância de SQL do Azure estão sujeitos a pré-visualização e o [suplementares os termos de utilização para a pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Neste tutorial, migra a **Adventureworks2012** base de dados de uma instância no local do SQL Server para uma instância de geridos das base de dados Azure SQL Server utilizando o serviço de migração de base de dados do Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Crie uma instância do serviço de migração de base de dados do Azure.
> * Crie um projeto de migração utilizando o serviço de migração de base de dados do Azure.
> * Execute a migração.
> * Monitorize a migração.
> * Transferir um relatório de migração.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, precisa de:

- Criar uma VNET para o serviço de migração de base de dados do Azure utilizando o modelo de implementação Azure Resource Manager, que fornece a conectividade de site a site para os seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). [Saiba topologias de rede para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure](https://aka.ms/dmsnetworkformi).
- Certifique-se de que a sua escolha de regras do Azure (VNET) rede segurança grupo de rede Virtual bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configurar o seu [Firewall do Windows para acesso ao motor de base de dados de origem](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Abra a Firewall do Windows para permitir que o serviço de migração de base de dados do Azure para aceder à origem de SQL Server, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias do SQL Server com nome utilizando as portas dinâmicas, poderá pretender ativar o serviço de Browser do SQL Server e permitir o acesso à porta UDP 1434 através das firewalls para que o serviço de migração de base de dados do Azure podem ligar a uma instância nomeada na sua origem servidor.
- Se estiver a utilizar uma aplicação de firewall à frente das bases de dados de origem, poderá ter de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure para aceder a bases de dados de origem para migração, bem como os ficheiros através de porta SMB 445.
- Criar uma instância de gerido da base de dados do Azure SQL, seguindo o detalhe no artigo [criar uma instância de gerido da base de dados do Azure SQL no portal do Azure](https://aka.ms/sqldbmi).
- Certifique-se de que os inícios de sessão utilizados para ligar a origem de SQL Server e instância geridos de destino são membros da função de servidor sysadmin.
- Crie uma partilha de rede que pode utilizar o serviço de migração de base de dados do Azure para criar cópias de segurança da base de dados de origem.
- Certifique-se de que a conta de serviço com a origem de SQL Server, instância tem privilégios de escrita na partilha de rede que criou e de que a conta de computador para o servidor de origem tem acesso de leitura/escrita para a mesma partilha.
- Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede que criou anteriormente. O serviço de migração de base de dados do Azure representa tem as credenciais de utilizador para carregar os ficheiros de cópia de segurança no contentor de armazenamento do Azure para a operação de restauro.
- Criar um contentor de blob e obter o seu respetivo URI de SAS, utilizando os passos no artigo [recursos de gerir o armazenamento de Blobs do Azure com o Explorador de armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container), certifique-se de selecionar todas as permissões (leitura, escrita, eliminar, lista) na janela de política ao criar o URI de SAS. Este detalhe fornece o serviço de migração de base de dados do Azure com acesso ao contentor de conta de armazenamento para carregar os ficheiros de cópia de segurança utilizados para migrar bases de dados da base de dados geridos instância do SQL do Azure

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft.DataMigration

1. Inicie sessão no portal do Azure, selecione **todos os serviços**e, em seguida, selecione **subscrições**.

    ![Mostrar subscrições portais](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.

    ![Mostrar os fornecedores de recursos](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Pesquisa para a migração e, em seguida, à direita do **Microsoft.DataMigration**, selecione **registar**.

    ![Registar o fornecedor de recursos](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Criar uma instância de serviço de migração de base de dados do Azure

1. No portal do Azure, selecione + **crie um recurso**, procure **serviço de migração de base de dados do Azure**e, em seguida, selecione **serviço de migração de base de dados do Azure** da lista pendente lista.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. No **serviço de migração de base de dados do Azure** ecrã, selecione **criar**.

    ![Criar uma instância de serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. No **criar serviço de migração** ecrã, especifique um nome para o serviço, a subscrição e um grupo de recursos novo ou existente.

4. Selecione uma rede virtual (VNET) existente ou criar um.
 
    A VNET fornece o serviço de migração de base de dados do Azure com acesso para a origem do SQL Server e o destino da base de dados geridas por instância de SQL do Azure.

    Para obter mais informações sobre como criar uma VNET no portal do Azure, consulte o artigo [criar uma rede virtual com o portal do Azure](https://aka.ms/DMSVnet).

    Para obter detalhes adicionais, consulte o artigo [topologias para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure de rede](https://aka.ms/dmsnetworkformi).

5. Selecione um escalão de preço.

    Para obter mais informações sobre os custos e escalões de preços, consulte o [página de preços](https://aka.ms/dms-pricing).
   
    ![Criar serviço DMS](media\tutorial-sql-server-to-managed-instance\dms-create-service1.png)

6.  Selecione **criar** ao criar o serviço.

## <a name="create-a-migration-project"></a>Criar um projeto de migração

Depois de criar o serviço, localizá-la no portal do Azure, abra-o e, em seguida, crie um novo projeto de migração.

1. No portal do Azure, selecione **todos os serviços**, procure o serviço de migração de base de dados do Azure e, em seguida, selecione **serviços de migração de base de dados do Azure**.

    ![Localizar todas as instâncias do serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. No **ecrã do serviço de migração de base de dados do Azure**, procure o nome da instância que criou e, em seguida, selecione a instância.
 
3. Selecione + **novo projeto de migração**.

4. No **novo projeto de migração** ecrã, especifique um nome para o projeto no **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**e, em seguida, no **destino tipo de servidor** caixa de texto, selecione **da base de dados geridas por instância de SQL do Azure**.

   ![Criar projeto do DMS](media\tutorial-sql-server-to-managed-instance\dms-create-project1.png)

5. Selecione **criar** para criar o projeto.

## <a name="specify-source-details"></a>Especifique os detalhes de origem

1. No **origem detalhes** ecrã, especifique os detalhes de ligação para a origem de SQL Server.

2. Se não tiver instalado um certificado fidedigno no seu servidor, selecione o **certificado de servidor de confiança** caixa de verificação.

    Quando um certificado fidedigno não estiver instalado, o SQL Server gera um certificado autoassinado quando é iniciada a instância. Este certificado é utilizado para encriptar as credenciais para ligações de cliente.

    > [!CAUTION]
    > Ligações de SSL estão encriptadas com um certificado autoassinado não fornecem segurança forte. Estes são suscetíveis a ataques man-in-the-middle. Não deverá confiar no SSL, utilizar certificados autoassinados num ambiente de produção ou nos servidores que estejam ligados à internet.

   ![Detalhes de origem](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. Selecione **Guardar**.

4. No **selecionar bases de dados de origem** ecrã, selecione o **Adventureworks2012** base de dados para migração.

   ![Selecionar bases de dados de origem](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. Selecione **Guardar**.

## <a name="specify-target-details"></a>Especifique os detalhes de destino

1.  No **detalhes de destino** ecrã, especifique os detalhes de ligação para o destino, o que é pré-aprovisionada Azure instância do SQL da base de dados geridos para os quais o **AdventureWorks2012** base de dados será migradas.

    Se ainda não aprovisionou a instância de gerido da base de dados do Azure SQL, selecione **não** para uma ligação para o ajudar a aprovisionar a instância. Pode ainda pode continuar com a criação do projeto e, quando a instância de gerido da base de dados do Azure SQL estiver pronta, volte a este projeto específico ao executar a migração.   
 
       ![Selecione o destino](media\tutorial-sql-server-to-managed-instance\dms-target-details1.png)

2.  Selecione **Guardar**.

3.  No **resumo de projeto** ecrã, reveja e verifique os detalhes associados ao projeto de migração.
 
    ![Resumo de projeto de migração](media\tutorial-sql-server-to-managed-instance\dms-project-summary1.png)

4.  Selecione **Guardar**.   

## <a name="run-the-migration"></a>Executar a migração

1.  Selecione o projeto guardado recentemente, selecione + **nova atividade**e, em seguida, selecione **executar migração**.

    ![Criar Nova Atividade](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity1.png)

2.  Quando lhe for pedido, introduza as credenciais de origem e os servidores de destino e, em seguida, selecione **guardar**.

3.  No **selecionar bases de dados de origem** ecrã, selecione a base de dados de origem que pretende migrar.

    ![Selecionar bases de dados de origem](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases1.png)

4.  Selecione **guardar**e, em seguida, no **configurar definições de migração** ecrã, forneça os detalhes seguintes:

    | | |
    |--------|---------|
    |**Partilha da localização de rede** | A rede local partilhar que o serviço de migração de base de dados do Azure pode demorar a origem a cópias de segurança da base de dados. A conta de serviço que executa a instância do SQL Server de origem tem de ter privilégios de escrita nesta partilha de rede. Forneça um endereços IP ou FQDN do servidor na partilha de rede, por exemplo, '\\\servername.domainname.com\backupfolder' ou '\\\IP address\backupfolder'.|
    |**Nome de utilizador** | Nome de utilizador windows que o serviço de migração de base de dados do Azure pode representar e carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure para a operação de restauro. |
    |**Palavra-passe** | Palavra-passe para o utilizador. |
    |**Definições de conta de armazenamento** | O URI de SAS que fornece o serviço de migração de base de dados do Azure com acesso ao contentor de conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança e de que é utilizado para migrar as bases de dados à instância gerido da base de dados SQL do Azure. [Saber como obter o URI de SAS do contentor de blob](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Configurar definições de migração](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings1.png)

5.  Selecione **guardar**e, em seguida, no **resumo de migração** no ecrã o **nome da atividade** texto caixa, especifique um nome para a atividade de migração.

6. Expanda o **opção de validação** secção para visualizar o **escolher a opção de validação** ecrã, especifique se pretende validar a base de dados migrado correcção de consulta e, em seguida, selecione **guardar** .  

    ![Resumo de migração](media\tutorial-sql-server-to-managed-instance\dms-migration-summary1.png)

7. Selecione **executar migração**.

    É apresentada a janela de atividade de migração e o estado da atividade é **pendente**.

   ![Atividade de migração pendentes](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-pending.png)

## <a name="monitor-the-migration"></a>Monitorizar a migração

1. No ecrã de atividade de migração, selecione **atualizar** para atualizar a apresentação até verá que o estado da migração é apresentado como **concluído**.
 
   ![Atividade de migração foi concluída](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-finished.png)

2. Depois de concluir a migração, selecione **transferir relatório** para obter um relatório lista os detalhes associados com o processo de migração.
 
3. Certifique-se de que a base de dados de destino no ambiente de base de dados geridas por instância de SQL do Azure de destino.

## <a name="next-steps"></a>Passos Seguintes

- Para um tutorial mostrar como migrar uma base de dados para uma instância geridos utilizando o comando de RESTAURO de T-SQL, consulte [restaurar uma cópia de segurança para uma instância geridos utilizando o comando restore](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Para obter informações sobre a instância geridos, consulte [o que é uma instância geridos](../sql-database/sql-database-managed-instance.md).
- Para obter informações sobre a ligação a aplicações a uma instância geridos, consulte [ligar aplicações](../sql-database/sql-database-managed-instance-connect-app.md).
