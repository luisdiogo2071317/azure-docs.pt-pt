---
title: "Aprovisionar um runtime de integração do Azure SSIS através do portal | Microsoft Docs"
description: "Este artigo explica como criar um runtime de integração de SSIS do Azure utilizando o portal do Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: spelluru
ms.openlocfilehash: b6a795f8a26340f24f9e09aea371ba90afe50101
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="provision-an-azure-ssis-integration-runtime-by-using-the-data-factory-ui"></a>Aprovisionar um runtime de integração do Azure SSIS através do IU do Data Factory
Este tutorial disponibiliza os passos para utilizar o portal do Azure para aprovisionar um integration runtime (IR) Azure-SSIS no Azure Data Factory. Em seguida, pode utilizar o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar pacotes de SQL Server Integration Services (SSIS) neste runtime no Azure. Para obter informações concetuais sobre o IR Azure-SSIS, veja [Azure-SSIS integration runtime overview (Descrição geral do runtime de integração Azure-SSIS)](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Neste tutorial, vai executar os seguintes passos:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Create and start Azure-SSIS integration runtime (Criar e iniciar um runtime de integração Azure-SSIS)

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar. 
- **Servidor da Base de Dados SQL do Azure**. Se ainda não tiver um servidor de base de dados, crie um no portal do Azure antes de começar. O Azure Data Factory cria a base de dados do Catálogo de SSIS (SSISDB) neste servidor de base de dados. Recomendamos que crie o servidor de base de dados na mesma região do Azure que o integration runtime. Esta configuração permite que o integration runtime escreva registos de execução no SSISDB sem cruzamento entre regiões do Azure. 
    - Confirme que a definição “**Permitir acesso aos serviços do Azure**” está ativada para o seu servidor da base de dados. Para obter mais informações, veja [Proteger a base de dados SQL do Azure](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Para ativar esta definição com o PowerShell, veja [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Adicione o endereço IP do computador cliente ou um intervalo de endereços IP que inclua o endereço IP do computador cliente à lista de endereços IP do cliente nas definições de firewall para o servidor de base de dados. Para obter mais informações, veja [Regras de firewall ao nível do servidor da Base de Dados SQL e ao nível da base de dados](../sql-database/sql-database-firewall-configure.md). 
 
## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).    
2. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Na página **Nova fábrica de dados**, introduza **MyAzureSsisDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se receber o seguinte erro, altere o nome da fábrica de dados (por exemplo, oseunomeMyAzureSsisDataFactory) e tente criá-la novamente. Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
      Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Só são mostradas na lista as localizações que são suportadas para a criação de fábricas de dados.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Clique em **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Data Factory num separador à parte. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionar um runtime de integração de SSIS do Azure

1. Na página de introdução, clique no mosaico **Configurar o Integration Runtime do SSIS**. 

   ![Configurar o mosaico do Integration Runtime do SSIS](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Na página **Definições Gerais** da **Configuração do Integration Runtime**, efetue os seguintes passos: 

   ![Definições gerais](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Especifique um **nome** para o runtime de integração.
    2. Selecione a **localização** para o runtime de integração. Apenas são apresentadas as localizações suportadas.
    3. Selecione o **tamanho do nó** que está a ser configurado com o runtime do SSIS.
    4. Especifique o **número de nós** no cluster.
    5. Clique em **Seguinte**. 
1. Nas **Definições de SQL**, efetue os seguintes passos: 

    ![Definições de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Especifique a **subscrição** do Azure que tem o servidor de SQL. 
    2. Selecione o servidor SQL do Azure para o **Ponto Final do Servidor de Base de Dados do Catálogo**.
    3. Introduza o nome de utilizador **administrador**.
    4. Introduza a **palavra-passe** do administrador.  
    5. Selecione a **camada de serviço** para a base de dados SSISDB. O valor predefinido é Básico.
    6. Clique em **Seguinte**. 
1.  Na página **Definições Avançadas**, selecione um valor para o **Máximo de Execuções Paralelas por Nó**.   

    ![Definições avançadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Este passo é **opcional**. Se tiver uma rede virtual clássica (VNet) que pretende que o tempo de execução de integração para aderir, selecione a opção **Selecionar uma VNet para o runtime de integração do Azure SSIS para associar e permitir que os serviços do Azure configurar permissões/definições da VNet** e efetue os seguintes passos: 

    ![Definições avançadas com o VNet](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Especifique a **subscrição** que tenha a VNet clássica. 
    2. Selecione a **VNet**. <br/>
    4. Selecione a **Subnet**.<br/> 
1. Clique em **Concluir** para iniciar a criação do runtime de integração de SSIS do Azure. 

    > [!IMPORTANT]
    > - Este processo demora cerca de 20 minutos a concluir
    > - O serviço Data Factory liga à sua Base de Dados SQL do Azure ou para preparar a base de dados do Catálogo do SSIS (SSISDB). O script também configura permissões e definições para a sua VNet, caso sejam especificadas, e associa a nova instância do integration runtime do Azure-SSIS à VNet.
7. Na janela **Ligações**, mude para **Runtimes de Integração** se for necessário. Clique em **Atualizar** para atualizar o estado. 

    ![Estado da criação](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Utilize as ligações na coluna **Ações** para monitorizar, iniciar/parar, editar ou eliminar o runtime de integração. Utilize a última ligação para ver o código JSON para o runtime de integração. Os botões de editar e eliminar estão ativados, apenas quando a resposta a incidentes está parada. 

    ![Azure SSIS IR - ações](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Clique na ligação **Monitorização** em **Ações**.  

    ![Azure SSIS IR - detalhes](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Se tiver ocorrido um **erro** associado a resposta a incidentes SSIS do Azure, veja o número de erros nesta página e na ligação para ver detalhes sobre o erro. Por exemplo, se o catálogo de SSIS já existe no servidor de base de dados, verá um erro que indica a existência de base de dados SSISDB.  
11. Clique em **Runtimes de Integração** na parte superior para navegar para a página anterior para ver todos os runtimes de integração associados à fábrica de dados.  

## <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Runtime de integração de SSIS do Azure no portal

1. Na IU do Azure Data Factory, mude para o separador **Editar**, clique em **Ligações**e, em seguida, mude para o separador **Runtimes de Integração** para ver os runtimes de integração existente na sua fábrica de dados. 
    ![Ver IRs existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Clique em **Novo** para criar um novo SSIS IR do Azure. 

    ![Runtime de integração através do menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Para criar um tempo de execução de integração de SSIS do Azure, clique em **Novo** conforme mostrado na imagem. 
3. Na janela de Configuração do Integration Runtime, selecione **pacotes SSIS existentes de migração lift-and-shift a executar no Azure**e, em seguida, clique em **Seguinte**.

    ![Especifique o tipo de runtime de integração](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Veja a secção [Aprovisionar um runtime de integração do Azure SSIS](#provision-an-azure-ssis-integration-runtime) para os restantes passos para configurar um SSIS IR do Azure. 

    
## <a name="deploy-ssis-packages"></a>Implementar pacotes de SSIS
Agora, utilize o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar os pacotes de SSIS no Azure. Ligue ao seu SQL server do Azure que aloja o catálogo de SSIS (SSISDB). O nome do servidor SQL do Azure está no formato `<servername>.database.windows.net` (para a Base de Dados SQL do Azure). 

Veja os artigos seguintes da documentação do SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Implementar, executar e monitorizar pacotes do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Connect to SSIS catalog on Azure (Ligar ao catálogo do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Agendar a execução de pacotes no Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Ligar a origens de dados no local com a autenticação do Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Create and start Azure-SSIS integration runtime (Criar e iniciar um runtime de integração Azure-SSIS)

Avance para o tutorial seguinte para saber como copiar dados do local para a cloud: 

> [!div class="nextstepaction"]
>[Copiar dados na cloud](tutorial-copy-data-portal.md)
