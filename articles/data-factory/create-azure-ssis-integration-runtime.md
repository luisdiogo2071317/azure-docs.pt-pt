---
title: "Criar o tempo de execução de integração personalizada alojada no Azure Data Factory | Microsoft Docs"
description: Saiba como pode utilizar a atividade de procedimento armazenado do SQL Server para invocar um procedimento armazenado numa SQL Database do Azure ou do Azure SQL Data Warehouse a partir de um pipeline do Data Factory.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 7796df75d811ad34967aee66478eae992fd449fe
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Criar um tempo de execução de integração do Azure-SSIS no Azure Data Factory
Este artigo fornece os passos para o aprovisionamento de um tempo de execução de integração do Azure-SSIS no Azure Data Factory. Em seguida, pode utilizar o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar pacotes de SQL Server Integration Services (SSIS) neste runtime no Azure.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-introduction.md).

Tutorial: [Tutorial: implementar pacotes de SQL Server Integration Services (SSIS) para o Azure](tutorial-deploy-ssis-packages-azure.md) mostra-lhe como criar um tempo de execução de integração do Azure SSIS (IR) ao utilizar a SQL Database do Azure como o arquivo para o catálogo SSIS. Este artigo expande o tutorial e mostra como fazer o seguinte: 

- Utilize o Azure geridos instância do SQL (pré-visualização privada) para alojar um catálogo SSIS (base de dados SSISDB).
- Associação do Azure-SSIS IR a uma rede virtual do Azure (VNet). 

Para obter informações concetuais sobre associar uma resposta a incidentes SSIS do Azure para uma VNet e como configurar uma VNet no portal do Azure, consulte [associar IR Azure SSIS vnet](join-azure-ssis-integration-runtime-virtual-network.md). 

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**. Se não tiver uma subscrição, pode criar uma conta de [avaliação gratuita](http://azure.microsoft.com/pricing/free-trial/).
- **Servidor de Base de Dados SQL do Azure** ou **Instância Gerida do SQL Server (pré-visualização privada) (Pré-visualização Privada Expandida)**. Se ainda não tiver um servidor de base de dados, crie um no portal do Azure antes de começar. Este servidor aloja a base de dados do Catálogo de SSIS (SSISDB). Recomendamos que crie o servidor de base de dados na mesma região do Azure que o integration runtime. Esta configuração permite que o integration runtime escreva registos de execução no SSISDB sem cruzamento entre regiões do Azure. Tenha em atenção para baixo o escalão de preço do seu servidor SQL do Azure. Para obter uma lista dos escalões de preços suportados para a SQL Database do Azure, consulte [dos limites de recursos de base de dados SQL](../sql-database/sql-database-resource-limits.md).
- **Rede Virtual Clássica (VNet) (opcional)**. Tem de ter uma Rede Virtual do Azure (VNet) se, pelo menos, uma das seguintes condições for verdadeira:
    - Está a alojar a base de dados do Catálogo de SSIS numa Instância Gerida do SQL Server (pré-visualização privada) que faz parte de uma VNet.
    - Pretende ligar a arquivos de dados no local a partir de pacotes de SSIS em execução no integration runtime do Azure-SSIS.
- **Azure PowerShell**. Siga as instruções em [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps). Vai utilizar o PowerShell para executar um script para aprovisionar um integration runtime do Azure-SSIS que executa pacotes de SSIS na cloud. 

> [!NOTE]
> Para obter uma lista das regiões suportadas pelo Azure Data Factory V2 e o Integration Runtime Azure-SSIS, veja [Products available by region](https://azure.microsoft.com/regions/services/) (Produtos disponíveis por região). Expanda **Dados + Análise** para ver **Data Factory V2** e **Integration Runtime do SSIS**.

## <a name="use-azure-portal"></a>Utilizar o portal do Azure

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).    
2. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. No **nova fábrica de dados** página, introduza **MyAzureSsisDataFactory** para o **nome**. 
      
     ![Nova página da fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se receber o erro seguinte, altere o nome da fábrica de dados (por exemplo, yournameMyAzureSsisDataFactory) e tente criar novamente. Consulte [fábrica de dados - as regras de nomenclatura](naming-rules.md) artigo para regras de nomenclatura artefactos do Data Factory de dados.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
      Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Apenas as localizações que são suportadas para a criação de fábricas de dados são apresentadas na lista.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Depois de concluída a criação, consulte o **Data Factory** página conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Clique em **autor & Monitor** para iniciar a Interface de utilizador de fábrica (IU) de dados no separador. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionar um tempo de execução de integração do Azure SSIS

1. Na página de introdução de get, clique em **configurar o tempo de execução do SSIS integração** mosaico. 

   ![Configurar o mosaico de tempo de execução de integração de SSIS](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. No **definições gerais** página do **a configuração de tempo de execução de integração**, efetue os seguintes passos: 

   ![Definições gerais](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Especifique um **nome** para o tempo de execução de integração.
    2. Selecione o **localização** para o tempo de execução de integração. São apresentadas apenas as localizações suportadas.
    3. Selecione o **tamanho do nó** que está a ser configurado com o tempo de execução SSIS.
    4. Especifique o **número de nós** no cluster.
    5. Clique em **Seguinte**. 
1. No **as definições de SQL**, efetue os seguintes passos: 

    ![Definições de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Especifique o Azure **subscrição** que tem o servidor de SQL do Azure. 
    2. Selecione o seu servidor SQL do Azure para o **ponto final do servidor de base de dados do catálogo**.
    3. Introduza o **administrador** nome de utilizador.
    4. Introduza o **palavra-passe** para o administrador.  
    5. Selecione o **camada de serviço** para a base de dados SSISDB. O valor predefinido é básico.
    6. Clique em **Seguinte**. 
1.  No **definições avançadas** página, selecione um valor para o **máximo execuções por nó Parallel**.   

    ![Definições avançadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Este passo é **opcional**. Se tiver uma rede virtual clássica (VNet) que pretende que o tempo de execução de integração para aderir, selecione o **selecionar uma VNet para o tempo de execução de integração do Azure SSIS para associar e permitir que os serviços do Azure configurar permissões/definições da VNet** opção e, efetue os seguintes passos: 

    ![Definições avançadas com VNet](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Especifique o **subscrição** que tenha a VNet clássica. 
    2. Selecione o **VNet**. <br/>
    4. Selecione o **sub-rede**.<br/> 
1. Clique em **concluir** para iniciar a criação de tempo de execução de integração de SSIS do Azure. 

    > [!IMPORTANT]
    > - Este processo demora cerca de 20 minutos a concluir
    > - O serviço Data Factory estabelece ligação à SQL Database do Azure para preparar a base de dados do catálogo de SSIS (SSISDB). O script também configura permissões e definições para a sua VNet, caso sejam especificadas, e associa a nova instância do integration runtime do Azure-SSIS à VNet.
7. No **ligações** janela, mude para **tempos de execução de integração** se for necessário. Clique em **atualizar** para atualizar o estado. 

    ![Estado de criação](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Utilize as ligações em **ações** coluna para monitorizar, início/paragem, editar ou eliminar o tempo de execução de integração. Utilize a última ligação para ver o código JSON para o tempo de execução de integração. Os botões de editar e eliminar estão ativados, apenas quando a resposta a incidentes está parada. 

    ![Azure SSIS IR - ações](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Clique em **Monitor** ligação em **ações**.  

    ![Azure SSIS IR - detalhes](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Se tiver ocorrido um **erro** associado a resposta a incidentes SSIS do Azure, consulte o número de erros nesta página e a ligação para ver detalhes sobre o erro. Por exemplo, se o catálogo de SSIS já existe no servidor de base de dados, verá um erro que indica a existência de base de dados SSISDB.  
11. Clique em **tempos de execução de integração** na parte superior para navegar para fazer uma cópia da página anterior para ver todos os tempos de execução integração associados com a fábrica de dados.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS integração tempos de execução e no portal

1. Na IU de fábrica de dados do Azure, mude para o **editar** separador, clique em **ligações**e, em seguida, mude para **tempos de execução de integração** separador para ver os tempos de execução de integração existente na sua fábrica de dados. 
    ![Ver IRs existente](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Clique em **novo** para criar um novo IR. de SSIS do Azure 

    ![Tempo de execução de integração através do menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Para criar um tempo de execução de integração de SSIS do Azure, clique em **novo** conforme mostrado na imagem. 
3. Na janela do programa de configuração de tempo de execução de integração, selecione **pacotes SSIS existentes de comparação de precisão e shift para ser executado no Azure**e, em seguida, clique em **seguinte**.

    ![Especifique o tipo de tempo de execução de integração](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Consulte o [aprovisionar um tempo de execução de integração do Azure SSIS](#provision-an-azure-ssis-integration-runtime) restante na secção passos para configurar um IR. SSIS do Azure

## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell

### <a name="create-variables"></a>Criar variáveis
Defina variáveis para utilizar nos scripts neste tutorial:

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

# Remove these the following two OPTIONAL variables if you are using Azure SQL Database. 
# These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# Get the following information from the properties page for your Classic Virtual Network in the Azure portal
# It should be in the format: $VnetId = "/subscriptions/<Azure Subscription ID>/resourceGroups/<Azure Resource Group>/providers/Microsoft.ClassicNetwork/virtualNetworks/<Class Virtual Network Name>"

# OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

```
### <a name="log-in-and-select-subscription"></a>Iniciar sessão e selecionar a subscrição
Adicione o seguinte código o script para iniciar sessão e selecione a sua subscrição do Azure: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Validar a ligação à base de dados
Adicione o seguinte script para validar a sua server.database.windows.net de servidor da SQL Database do Azure ou o ponto final do servidor gerido instância do SQL do Azure (pré-visualização privada). 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

### <a name="configure-virtual-network"></a>Configurar uma rede virtual
Adicione o seguinte script para configurar automaticamente permissões/definições de rede virtual para o integration runtime do Azure-SSIS para associar.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

### <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo. O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup` na localização `westeurope`.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Execute o comando seguinte para criar uma fábrica de dados:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Criar um integration runtime
Execute o seguinte comando para criar um tempo de execução de integração do Azure-SSIS executado pacotes SSIS no Azure: utilizar o script da secção com base no tipo de base de dados (vs SQL Database do Azure. Instância de gerido SQL do Azure (pré-visualização privada)) está a utilizar. 

#### <a name="azure-sql-database-to-host-the-ssisdb-database-ssis-catalog"></a>Base de dados de SQL do Azure para alojar a base de dados do SSISDB (catálogo SSIS) 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode
```

Não é necessário transmitir valores para VNetId e a sub-rede, a menos que precisa de acesso a dados no local, ou seja, tem as origens de dados no local, os destinos nos seus pacotes SSIS. Tem de transmitir o valor do parâmetro CatalogPricingTier. Para obter uma lista dos escalões de preços suportados para a SQL Database do Azure, consulte [dos limites de recursos de base de dados SQL](../sql-database/sql-database-resource-limits.md).

#### <a name="azure-sql-managed-instance-private-preview-to-host-the-ssisdb-database"></a>Instância do Azure SQL Server gerido (pré-visualização privada) para alojar a base de dados do SSISDB

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

Tem de passar valores para parâmetros VnetId e a sub-rede com o Azure geridos instância do SQL (pré-visualização privada) que associa uma VNet. O parâmetro CatalogPricingTier aplicável geridos instância do SQL do Azure. 

### <a name="start-integration-runtime"></a>Iniciar o integration runtime
Execute o seguinte comando para iniciar o integration runtime do Azure-SSIS: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Este comando demora entre **20 e 30 minutos** a concluir. 


### <a name="full-script"></a>Script completo
Eis o script completo que cria uma resposta a incidentes SSIS do Azure e associa-a para uma VNet. Este script parte do princípio de que está a utilizar o Azure SQL geridos instância (MI) para alojar o catálogo SSIS. 

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two OPTIONAL variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName

write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```



## <a name="deploy-ssis-packages"></a>Implementar pacotes de SSIS
Agora, utilize o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar os pacotes de SSIS no Azure. Ligue ao seu SQL server do Azure que aloja o catálogo de SSIS (SSISDB). O nome do SQL server do Azure está no formato: &lt;servername&gt;.database.windows.net (para a Base de Dados SQL do Azure). Veja o artigo [Implementar pacotes](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) para obter instruções. 

## <a name="next-steps"></a>Passos Seguintes
Consulte os outros tópicos do Azure-SSIS IR nesta documentação:

- [Tempo de execução do Azure-SSIS integração](concepts-integration-runtime.md#azure-ssis-integration-runtime). Este artigo fornece informações concetuais sobre tempos de execução de integração em geral, incluindo o IR. SSIS do Azure 
- [Tutorial: implementar pacotes do SSIS no Azure](tutorial-deploy-ssis-packages-azure.md). Este artigo disponibiliza instruções passo a passo para criar um IR Azure-SSIS e utiliza uma base de dados da Base de Dados SQL do Azure para alojar o catálogo do SSIS. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Monitorizar um IR Azure-SSIS) Este artigo mostra como obter informações sobre um IR Azure-SSIS e descrições dos estados nas informações devolvidas. 
- [Manage an Azure-SSIS IR](manage-azure-ssis-integration-runtime.md) (Gerir um IR Azure-SSIS). Este artigo mostra-lhe como parar, iniciar ou remover um IR Azure-SSIS. Mostra também como aumentá-lo horizontalmente mediante a adição de mais nós ao mesmo. 
- [Join an Azure-SSIS IR to a VNet](join-azure-ssis-integration-runtime-virtual-network.md) (Associar um IR Azure-SSIS a uma VNet). Este artigo disponibiliza informações concetuais sobre como associar um IR Azure-SSIS a uma rede virtual do Azure (VNet). Também descreve os passos para utilizar o portal do Azure para configurar a VNet, de modo a que o IR Azure-SSIS se possa associar à mesma. 