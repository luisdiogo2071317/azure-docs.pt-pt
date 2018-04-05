---
title: Implementar pacotes de SSIS no Azure | Microsoft Docs
description: Este artigo explica como implementar pacotes de SSIS no Azure e criar um runtime de integração do Azure-SSIS ao utilizar o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: hero-article
ms.date: 01/29/2018
ms.author: douglasl
ms.openlocfilehash: aca9f822bf3fd3b26e554240a4fee2474b89143d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Implementar pacotes de SQL Server Integration Services no Azure
Este tutorial disponibiliza os passos para utilizar o portal do Azure para aprovisionar um integration runtime (IR) Azure-SSIS no Azure Data Factory. Em seguida, pode utilizar o SQL Server Data Tools ou o SQL Server Management Studio para implementar pacotes do SQL Server Integration Services (SSIS) neste runtime no Azure. Para obter informações concetuais sobre IRs Azure-SSIS, veja [Descrição geral do runtime de integração Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Neste tutorial, vai concluir os seguintes passos:

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Aprovisionar um runtime de integração Azure-SSIS.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está em disponibilidade geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar. 
- **Servidor da Base de Dados SQL do Azure**. Se ainda não tiver um servidor de base de dados, crie um no portal do Azure antes de começar. O Azure Data Factory cria o Catálogo de SSIS (base de dados SSISDB) neste servidor de base de dados. Recomendamos que crie o servidor de base de dados na mesma região do Azure que o integration runtime. Esta configuração permite que o runtime de integração escreva registos de execução na base de dados de SSISDB sem cruzamento entre regiões do Azure. 
- Confirme que a definição **Permitir acesso aos serviços do Azure** está ativada para o servidor da base de dados. Para obter mais informações, veja [Proteger a base de dados SQL do Azure](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Para ativar esta definição com o PowerShell, veja [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
- Adicione o endereço IP do computador cliente ou um intervalo de endereços IP que inclua o endereço IP do computador cliente à lista de endereços IP do cliente nas definições de firewall para o servidor da base de dados. Para obter mais informações, veja [Regras de firewall ao nível do servidor da Base de Dados SQL e ao nível da base de dados](../sql-database/sql-database-firewall-configure.md).
- Certifique-se de que o servidor da Base de Dados SQL do Azure não tem um Catálogo de SSIS (base de dados SSISDB). O aprovisionamento de um IR Azure-SSIS não suporta a utilização de um Catálogo de SSIS existente.

> [!NOTE]
> - Pode criar uma fábrica de dados da versão 2 nas seguintes regiões: E.U.A. Leste, E.U.A. Leste 2, Sudeste Asiático e Europa Ocidental. 
> - Pode criar um IR do Azure-SSIS nas seguintes regiões: E.U.A. Leste, E.U.A. Leste 2, E.U.A. Central, Europa do Norte, Europa Ocidental e Leste da Austrália. 

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. Inicie sessão no [portal do Azure](https://portal.azure.com/).    
3. Selecione **Novo** no menu da esquerda, selecione **Dados + Análise** e, em seguida, selecione **Data Factory**. 
   
   ![Seleção do Data Factory no painel "Novo"](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Na página **Nova fábrica de dados**, introduza **MyAzureSsisDataFactory** em **Nome**. 
      
   ![Página "Nova fábrica de dados"](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser *globalmente exclusivo*. Se receber o seguinte erro, altere o nome da fábrica de dados (por exemplo, **&lt;oseunome&gt;MyAzureSsisDataFactory**) e tente criá-la novamente. Para regras de nomenclatura de artefactos do Data Factory, veja o artigo [Data Factory – Regras de Nomenclatura](naming-rules.md).
  
   `Data factory name “MyAzureSsisDataFactory” is not available`
4. Em **Subscrição**, selecione a sua subscrição do Azure na qual pretende criar a fábrica de dados. 
5. Para **Grupo de Recursos**, siga um destes passos:
     
   - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista. 
   - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
   Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
6. Em **Versão**, selecione **V2 (Pré-visualização)**.
7. Em **Localização**, selecione a localização para a fábrica de dados. A lista mostra apenas as localizações que são suportadas para a criação de fábricas de dados.
8. Selecione **Afixar ao dashboard**.     
9. Selecione **Criar**.
10. No dashboard, é apresentado o mosaico seguinte, com o estado **A implementar fábrica de dados**: 

   ![Mosaico "A Implementar o Data Factory"](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
11. Após concluir a criação, verá a página **Fábrica de dados**.
   
   ![Home page da fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
12. Selecione **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Data Factory num separador à parte. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionar um runtime de integração Azure-SSIS

1. Na página **Introdução**, selecione o mosaico **Configurar Runtime de Integração do SSIS**. 

   ![Mosaico "Configurar Runtime de Integração do SSIS"](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Na página **Definições Gerais** da **Configuração do Runtime de Integração**, conclua os seguintes passos: 

   ![Definições gerais](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Para **Nome**, especifique um nome para o runtime de integração.

   b. Para **Localização**, selecione a localização para o runtime de integração. Apenas são apresentadas as localizações suportadas.

   c. Para **Tamanho do Nó**, selecione o tamanho do nó que será configurado com o runtime do SSIS.

   d. Para **Número de Nós**, especifique o número de nós no cluster.
   
   e. Selecione **Seguinte**. 
3. Na página **Definições de SQL**, conclua os seguintes passos: 

   ![Definições de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Para **Subscrição**, especifique a subscrição do Azure que tem o servidor da base de dados do Azure.

   b. Para **Ponto Final do Servidor da Base de Dados do Catálogo**, selecione o servidor da base de dados do Azure.

   c. Para **Nome de Utilizador de Administrador**, introduza o nome de utilizador do administrador.

   d. Para **Palavra-passe de Administrador**, introduza a palavra-passe do administrador.

   e. Para **Camada do Servidor da Base de Dados do Catálogo**, selecione a camada de serviço para a base de dados SSISDB. O valor predefinido é **Básico**.

   f. Selecione **Seguinte**. 
4. Na página **Definições Avançadas**, selecione um valor para **Máximo de Execuções Paralelas por Nó**.   

   ![Definições avançadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Este passo é *opcional*. Se tiver uma rede virtual (criada através do modelo de implementação clássico ou do Azure Resource Manager) à qual quer que o runtime de integração seja associado, selecione a caixa de verificação **Selecionar uma VNet para o runtime de integração Azure-SSIS para associar e permitir aos serviços do Azure configurar permissões/definições da VNet**. Em seguida, conclua os seguintes passos: 

   ![Definições avançadas com uma rede virtual](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

   a. Para **Subscrição**, especifique a subscrição que tem a rede virtual.

   b. Para **Nome da VNet**, selecione o nome da rede virtual.

   c. Para **Nome da Sub-rede**, selecione o nome da sub-rede da rede virtual. 
6. Selecione **Concluir** para iniciar a criação do runtime de integração Azure-SSIS. 

   > [!IMPORTANT]
   > Este processo demora cerca de 20 minutos a concluir.
   >
   > O serviço Data Factory liga à sua Base de Dados SQL do Azure para preparar o Catálogo de SSIS (base de dados SSISDB). O script também configura as permissões e definições da rede virtual, se for especificado. Também é associado à nova instância do runtime de integração Azure-SSIS para a rede virtual.
   > 
   > Quando aprovisionar uma instância de um IR Azure-SSIS, o Pacote de Funcionalidades do Azure para SSIS e o Acesso Redistribuível também são instalados. Estes componentes fornecem conectividade aos ficheiros do Excel e acesso e a várias origens de dados do Azure, para além das origens de dados suportadas pelos componentes incorporados. Não pode instalar componentes de terceiros para SSIS neste momento. (Esta restrição inclui componentes de terceiros da Microsoft, tais como os componentes Oracle e Teradata da Attunity e os componentes BI do SAP).

7. No separador **Ligações**, mude para **Runtimes de Integração**, se for necessário. Selecione **Atualizar** para atualizar o estado. 

   ![Estado de criação, com o botão "Atualizar"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Utilize as ligações na coluna **Ações** para iniciar/parar, editar ou eliminar o runtime de integração. Utilize a última ligação para ver o código JSON para o runtime de integração. Os botões de editar e eliminar estão ativados, apenas quando a resposta a incidentes está parada. 

   ![Ligações na coluna "Ações"](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

## <a name="create-an-azure-ssis-integration-runtime"></a>Criar um integration runtime do Azure-SSIS

1. Na IU do Azure Data Factory, mude para o separador **Editar**, selecione **Ligações** e, em seguida, mude para o separador **Runtimes de Integração** para ver os runtimes de integração existentes na sua fábrica de dados. 
   ![Seleções para visualização de IRs existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
2. Selecione **Novo** para criar um IR Azure-SSIS. 

   ![Runtime de integração através do menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
3. Na janela **Configuração do Runtime de Integração**, selecione **Pacotes SSIS existentes de migração lift-and-shift a executar no Azure** e, em seguida, selecione **Seguinte**.

   ![Especifique o tipo de runtime de integração](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Para os restantes passos para configurar um IR Azure-SSIS, veja a secção [Aprovisionar um runtime de integração Azure-SSIS](#provision-an-azure-ssis-integration-runtime). 

 
## <a name="deploy-ssis-packages"></a>Implementar pacotes de SSIS
Agora, utilize o SQL Server Data Tools ou o SQL Server Management Studio para implementar os pacotes SSIS no Azure. Ligue ao servidor da base de dados do Azure que aloja o Catálogo de SSIS (base de dados SSISDB). O nome do servidor da base de dados do Azure está no formato `<servername>.database.windows.net` (para a Base de Dados SQL do Azure). 

Veja os artigos seguintes da documentação do SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Implementar, executar e monitorizar pacotes do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Ligar ao Catálogo de SSIS no Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Agendar a execução de pacotes no Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Ligar a origens de dados no local com a autenticação do Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Aprovisionar um runtime de integração Azure-SSIS.

Para saber como copiar dados do local para a cloud, avance para o tutorial seguinte: 

> [!div class="nextstepaction"]
> [Copiar dados na cloud](tutorial-copy-data-portal.md)
