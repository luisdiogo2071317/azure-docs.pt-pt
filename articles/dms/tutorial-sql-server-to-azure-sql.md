---
title: Utilizar o serviço de migração de base de dados do Azure para migrar o SQL Server a SQL Database do Azure | Microsoft Docs
description: Saiba como migrar a partir do SQL Server no local para a SQL Database do Azure utilizando o serviço de migração de base de dados do Azure.
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
ms.openlocfilehash: c110011f3b4c3c677354bc8423c8cd86cca6ac90
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-using-dms"></a>Migrar o servidor de SQL para a base de dados do SQL do Azure utilizando o DMS
Pode utilizar o serviço de migração de base de dados do Azure para migrar as bases de dados a partir de uma instância do SQL Server no local para [SQL Database do Azure](https://docs.microsoft.com/en-us/azure/sql-database/). Neste tutorial, migra a **Adventureworks2012** base de dados restaurada para uma instância no local do SQL Server 2016 (ou posterior) para uma base de dados do SQL do Azure utilizando o serviço de migração de base de dados do Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Avalie a sua base de dados no local utilizando o Assistente de migração de dados.
> * Migre o esquema de exemplo, utilizando o Assistente de migração de dados.
> * Crie uma instância do serviço de migração de base de dados do Azure.
> * Crie um projeto de migração utilizando o serviço de migração de base de dados do Azure.
> * Execute a migração.
> * Monitorize a migração.
> * Transferir um relatório de migração.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, precisa de:

- Transfira e instale [do SQL Server 2016 ou posterior](https://www.microsoft.com/sql-server/sql-server-downloads) (qualquer edição).
- Ative o protocolo TCP/IP, que está desativado por predefinição durante a instalação do SQL Server Express, ao seguir as instruções no artigo [ativar ou desativar um protocolo de rede do servidor](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Criar uma instância de instância de SQL Database do Azure, o que fazer, seguindo o detalhe no artigo [criar uma base de dados SQL do Azure no portal do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Transfira e instale o [Assistente de migração de dados](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 ou posterior.
- Criar uma VNET para o serviço de migração de base de dados do Azure utilizando o modelo de implementação Azure Resource Manager, que fornece a conectividade de site a site para os seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Certifique-se de que a sua escolha de regras do Azure (VNET) rede segurança grupo de rede Virtual bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Configurar o seu [Firewall do Windows para acesso ao motor de base de dados](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Abra a firewall do Windows para permitir que o serviço de migração de base de dados do Azure para aceder à origem de SQL Server, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias do SQL Server com nome utilizando as portas dinâmicas, poderá pretender ativar o serviço de Browser do SQL Server e permitir o acesso à porta UDP 1434 através das firewalls para que o serviço de migração de base de dados do Azure podem ligar a uma instância nomeada na sua origem servidor.
- Quando utilizar uma aplicação de firewall à frente da sua bases de dados de origem, poderá ter de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure para aceder a bases de dados de origem para migração.
- Criar um nível de servidor [regra de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) para o servidor da SQL Database do Azure permitir o acesso de serviço de migração de base de dados do Azure para as bases de dados de destino. Forneça o intervalo de sub-rede da VNET utilizado para o serviço de migração de base de dados do Azure.
- Certifique-se de que as credenciais utilizadas para ligar à instância do SQL Server de origem têm [controlo servidor](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) permissões.
- Certifique-se de que as credenciais utilizadas para ligar à instância de SQL Database do Azure de destino tem a permissão controlar base de dados nas bases de dados de SQL do Azure de destino.

## <a name="assess-your-on-premises-database"></a>Avaliar a sua base de dados no local
Antes de poder migrar dados a partir de uma instância do SQL Server no local para a SQL Database do Azure, terá de avaliar a base de dados do SQL Server para quaisquer problemas de bloqueio que possam impedir a migração. Utilizar o Assistente de migração de dados v3.3 ou posterior, siga os passos descritos no artigo [efetuar uma avaliação de migração do SQL Server](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) para concluir no local da base de dados avaliação. Segue um resumo dos passos necessários:
1.  No Assistente de migração de dados, selecione o ícone de novo (+) e, em seguida, selecione o **avaliação** o tipo de projeto.
2.  Especifique um nome de projeto, no **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**, no **tipo de servidor de destino** caixa de texto, selecione **SQL Database do Azure**e, em seguida, selecione **criar** para criar o projeto.

    Quando estiver a avaliar a origem do SQL Server da base de dados a migrar para a SQL Database do Azure, pode escolher um ou ambos dos seguintes tipos de relatório de avaliação:
    - Verificar a compatibilidade da base de dados
    - Paridade da funcionalidade de verificação

    Ambos os tipos de relatório estão selecionados por predefinição.

4.  No Assistente de migração de dados, no **opções** ecrã, selecione **seguinte**.
5.  No **selecione origens** no ecrã o **ligar a um servidor** , forneça os detalhes de ligação ao SQL Server e, em seguida, selecione **Connect**.
6.  No **adicionar origens** caixa de diálogo, selecione **AdventureWorks2012**, selecione **adicionar**e, em seguida, selecione **iniciar avaliação**.

    Quando estiver concluída a avaliação, apresentam os resultados conforme mostrado no seguinte gráfico:

    ![Avaliar a migração de dados](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Para a base de dados SQL do Azure, as avaliações de identificam problemas de paridade de funcionalidades e problemas de bloqueio de migração.

    - O **paridade de funcionalidades do SQL Server** categoria fornece um conjunto abrangente de recomendações, abordagens alternativas disponíveis no Azure, e mitigar os passos para o ajudar a planear o esforço do utilizador para os projetos de migração.
    - O **problemas de compatibilidade** categoria identifica parcialmente suportada ou funcionalidades não suportadas que refletem a problemas de compatibilidade, que poderão bloquear Migrar bases de SQL Server no local dados para a SQL Database do Azure. Recomendações também são fornecidas para o ajudar a resolver esses problemas.

7.  Reveja os resultados da avaliação para problemas de bloqueio de migração e problemas de paridade da funcionalidade selecionando as opções específicas.

## <a name="migrate-the-sample-schema"></a>Migrar o esquema de exemplo
Depois de estiver familiarizado com a avaliação e estiver satisfeito que a base de dados selecionada é uma candidata viável para migração para a SQL Database do Azure, utilize o Assistente de migração de dados para migrar o esquema para a SQL Database do Azure.

> [!NOTE]
> Antes de criar um projeto de migração no Assistente de migração de dados, lembre-se de que já aprovisionou uma base de dados SQL do Azure tal como mencionado nos pré-requisitos. Para efeitos deste tutorial, o nome da base de dados do SQL do Azure é prestado **AdventureWorksAzure**, mas pode fornecer qualquer nome que desejar.

Para migrar o **AdventureWorks2012** esquema para a base de dados SQL do Azure, execute os seguintes passos:

1.  No Assistente de migração de dados, selecione o ícone de novo (+) e, em **o tipo de projeto**, selecione **migração**.
3.  Especifique um nome de projeto, no **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**e, em seguida, no **tipo de servidor de destino** caixa de texto, selecione **SQL do Azure Base de dados**.
4.  Em **migração âmbito**, selecione **apenas o esquema**.

    Depois de efetuar os passos anteriores, a interface de Assistente de migração de dados deve aparecer conforme mostrado no seguinte gráfico:
    
    ![Criar projeto através do Assistente de migração de dados](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

5.  Selecione **criar** para criar o projeto.
6.  No Assistente de migração de dados, especifique os detalhes de ligação de origem para o SQL Server, selecione **Connect**e, em seguida, selecione o **AdventureWorks2012** base de dados.

    ![Detalhes de ligação de origem através do Assistente de migração de dados](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)

7.  Selecione **seguinte**, em **ligar ao servidor de destino**, especifique os detalhes de ligação de destino para a base de dados SQL do Azure, selecione **Connect**e, em seguida, selecione o **AdventureWorksAzure** base de dados previamente tinha sido aprovisionadas na base de dados SQL do Azure.

    ![Detalhes de ligação de destino através do Assistente de migração de dados](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)

8.  Selecione **seguinte** para avançar para o **selecionar objetos** ecrã, onde pode especificar os objetos de esquema no **AdventureWorks2012** base de dados que têm de ser implementada no Azure Base de dados do SQL Server.

    Por predefinição, são selecionados todos os objetos.

    ![Gerar Scripts SQL](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)

9.  Selecione **script SQL gerar** para criar os scripts de SQL e, em seguida, reveja os scripts de eventuais erros.

    ![Script de esquema](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)

10. Selecione **implementar esquema** para implementar o esquema SQL Database do Azure e, em seguida, depois de implementar o esquema, verifique o servidor de destino para qualquer anomalias.

    ![Implementar o esquema](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft.DataMigration
1. Inicie sessão no portal do Azure, selecione **todos os serviços**e, em seguida, selecione **subscrições**.
 
   ![Mostrar subscrições portais](media\tutorial-sql-server-to-azure-sql\portal-select-subscription1.png)
       
2. Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.
 
    ![Mostrar os fornecedores de recursos](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)
    
3.  Pesquisa para a migração e, em seguida, à direita do **Microsoft.DataMigration**, selecione **registar**.
 
    ![Registar o fornecedor de recursos](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Criar uma instância
1.  No portal do Azure, selecione + **crie um recurso**, procure o serviço de migração de base de dados do Azure e, em seguida, selecione **serviço de migração de base de dados do Azure** na lista pendente.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)

2.  No **serviço de migração de base de dados do Azure** ecrã, selecione **criar**.
 
    ![Criar uma instância de serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-azure-sql\dms-create1.png)
  
3.  No **criar serviço de migração** ecrã, especifique um nome para o serviço, a subscrição e um grupo de recursos novo ou existente.

4. Selecione uma rede virtual (VNET) existente ou crie um novo.

    A VNET fornece o serviço de migração de base de dados do Azure com acesso à origem de SQL Server e a instância de SQL Database do Azure de destino.

    Para obter mais informações sobre como criar uma VNET no portal do Azure, consulte o artigo [criar uma rede virtual com o portal do Azure](https://aka.ms/DMSVnet).

5. Selecione um escalão de preço.

    Para obter mais informações sobre os custos e escalões de preços, consulte o [página de preços](https://aka.ms/dms-pricing).

    Se precisar de ajuda na escolha o escalão de serviço de migração de base de dados do Azure à direita, consulte as recomendações de publicação [aqui](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Configurar as definições de instância de serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-azure-sql\dms-settings1.png)

6.  Selecione **criar** ao criar o serviço.

## <a name="create-a-migration-project"></a>Criar um projeto de migração
Depois de criar o serviço, localizá-la no portal do Azure, abra-o e, em seguida, crie um novo projeto de migração.

1. No portal do Azure, selecione **todos os serviços**, procure o serviço de migração de base de dados do Azure e, em seguida, selecione **serviços de migração de base de dados do Azure**.
 
      ![Localizar todas as instâncias do serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. No **serviços de migração de base de dados do Azure** ecrã, procure o nome do serviço de migração de base de dados do Azure a instância que criou e, em seguida, selecione a instância.
 
     ![Localizar a instância do serviço de migração de base de dados do Azure](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Selecione + **novo projeto de migração**.
4. No **novo projeto de migração** ecrã, especifique um nome para o projeto no **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**e, em seguida, no **destino tipo de servidor** caixa de texto, selecione **SQL Database do Azure**.

    ![Criar projeto do serviço de migração de base de dados](media\tutorial-sql-server-to-azure-sql\dms-create-project1.png)

5.  Selecione **criar** para criar o projeto.

## <a name="specify-source-details"></a>Especifique os detalhes de origem
1. No **origem detalhes** ecrã, especifique os detalhes de ligação para o nome de instância do SQL Server de origem.
 
    Certifique-se de que utiliza um completamente qualificado domínio nome (FQDN) para o nome de instância do SQL Server de origem. Também pode utilizar o endereço IP para situações em que DNS resolução de nomes não é possível.

2. Se não tiver instalado um certificado fidedigno no seu servidor de origem, selecione o **certificado de servidor de confiança** caixa de verificação.

    Quando um certificado fidedigno não estiver instalado, o SQL Server gera um certificado autoassinado quando é iniciada a instância. Este certificado é utilizado para encriptar as credenciais para ligações de cliente.

    > [!CAUTION]
    > Ligações de SSL encyopted utilizando um certificado autoassinado não fornecem segurança forte. Estes são suscetíveis a ataques man-in-the-middle. Não deverá confiar no SSL, utilizar certificados autoassinados num ambiente de produção ou nos servidores que estejam ligados à internet.

   ![Detalhes de origem](media\tutorial-sql-server-to-azure-sql\dms-source-details1.png)
  
2. Selecione **guardar**e, em seguida, selecione o **AdventureWorks2012** base de dados para migração.

    ![Selecione a base de dados de origem](media\tutorial-sql-server-to-azure-sql\dms-select-source-db1.png)

## <a name="specify-target-details"></a>Especifique os detalhes de destino
1. Selecione **guardar**e, em seguida, no **detalhes de destino** ecrã, especifique os detalhes de ligação para o destino SQL Database do Azure, que é pré-aprovisionada SQL Database do Azure para o qual o **AdventureWorks2012** esquema foi implementado utilizando o Assistente de migração de dados.

    ![Selecione o destino](media\tutorial-sql-server-to-azure-sql\dms-select-target1.png)

2. Selecione **guardar** para guardar o projeto.

3. No **resumo de projeto** ecrã, reveja e verifique os detalhes associados ao projeto de migração.

    ![O DMS resumo](media\tutorial-sql-server-to-azure-sql\dms-summary1.png)

4. Selecione **Guardar**.

## <a name="run-the-migration"></a>Executar a migração
1.  Selecione o projeto guardado recentemente, selecione + **nova atividade**e, em seguida, selecione **executar migração**.

    ![Nova atividade](media\tutorial-sql-server-to-azure-sql\dms-new-activity1.png)

2.  Quando lhe for pedido, introduza as credenciais para a origem e os servidores de destino e, em seguida, selecione **guardar**.

3.  No **mapa para bases de dados de destino** ecrã, mapear a origem e a base de dados de destino para migração.

    Se a base de dados de destino contém o mesmo nome de base de dados que a base de dados de origem, o Azure DMS seleciona a base de dados de destino por predefinição.

    ![Mapear para bases de dados de destino](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity1.png)

4. Selecione **guardar**, no **selecionar tabelas** ecrã, expanda a tabela que lista e, em seguida, reveja a lista de campos afetados.

    Tenha em atenção que o automático do serviço de migração de base de dados do Azure seleciona todas as tabelas de origem vazia que existem na instância de SQL Database do Azure de destino. Se pretender migrar novamente as tabelas que já incluem dados, terá de selecionar as tabelas neste painel explicitamente.

    ![Selecionar tabelas](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity1.png)

5.  Selecione **guardar**, no **resumo de migração** no ecrã o **nome da atividade** texto caixa, especifique um nome para a atividade de migração.

6. Expanda o **opção de validação** secção para visualizar o **escolher a opção de validação** ecrã, específico se pretende validar as bases de dados migrados para a comparação de esquema, a consistência dos dados e a consulta estão correctos.
    
    ![Escolha a opção de validação](media\tutorial-sql-server-to-azure-sql\dms-configuration1.png)

6.  Selecione **guardar**, reveja o resumo para se certificar de que os detalhes de origem e de destino corresponde ao que é especificado.

    ![Resumo de migração](media\tutorial-sql-server-to-azure-sql\dms-run-migration1.png)

7.  Selecione **executar migração**.

    É apresentada a janela de atividade de migração e o **estado** da atividade é **pendente**.

    ![Estado da Atividade](media\tutorial-sql-server-to-azure-sql\dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Monitorizar a migração
1. No ecrã de atividade de migração, selecione **atualizar** para atualizar a apresentação até verá que o **estado** das migrações mostra como **concluído**.

    ![Estado da atividade foi concluído](media\tutorial-sql-server-to-azure-sql\dms-completed-activity1.png)

2. Depois de concluir a migração, selecione **transferir relatório** para obter um relatório lista os detalhes associados com o processo de migração.

3. Certifique-se as bases de dados de destino na base de dados de SQL do Azure de destino.

### <a name="additional-resources"></a>Recursos adicionais

 * [Migração de SQL com o serviço de migração de dados do Azure (DMS)](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) laboratório prático.