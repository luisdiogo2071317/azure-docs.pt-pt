---
title: Aprovisionar o Integration Runtime do Azure-SSIS com o PowerShell | Microsoft Docs
description: Saiba como aprovisionar o runtime de integração do Azure-SSIS no Azure Data Factory com o PowerShell, para que possa implementar e executar pacotes SSIS no Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: tutorial
ms.date: 06/27/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 30e02f0b852ef0af8105a69575ea2c88e1265639
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43109749"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>Aprovisionar o Integration Runtime do Azure-SSIS no Azure Data Factory com o PowerShell
Este tutorial disponibiliza os passos para o aprovisionamento de um integration runtime (IR) Azure-SSIS no Azure Data Factory. Em seguida, pode utilizar o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar e executar pacotes de SQL Server Integration Services (SSIS) neste runtime no Azure. Neste tutorial, vai executar os seguintes passos:

> [!NOTE]
> Este artigo utiliza o Azure PowerShell para aprovisionar um IR SSIS do Azure. Para utilizar a interface de utilizador (IU) do Data Factory para aprovisionar um IR SSIS do Azure, veja [Tutorial: Criar um runtime de integração do SSIS do Azure](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Criar um integration runtime do Azure-SSIS
> * Iniciar o integration runtime do Azure-SSIS
> * Implementar pacotes de SSIS
> * Rever o script completo

## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar. Para obter informações concetuais sobre o IR Azure-SSIS, veja [Azure-SSIS integration runtime overview (Descrição geral do runtime de integração Azure-SSIS)](concepts-integration-runtime.md#azure-ssis-integration-runtime). 
- **Servidor da Base de Dados SQL do Azure**. Se ainda não tiver um servidor de base de dados, crie um no portal do Azure antes de começar. Este servidor aloja a base de dados do Catálogo de SSIS (SSISDB). Recomendamos que crie o servidor de base de dados na mesma região do Azure que o integration runtime. Esta configuração permite que o integration runtime escreva registos de execução no SSISDB sem cruzamento entre regiões do Azure. 
    - Com base no servidor de base de dados selecionado, pode criar a SSISDB em seu nome como uma base de dados única, pertencendo a um Conjunto Elástico, ou numa Instância Gerida (Pré-visualização) e acessível na rede pública ou ao associar a uma rede virtual. Para obter documentação de orientação sobre como escolher o tipo de servidor de base de dados para alojar o SSISDB, veja [Comparar Base de Dados SQL e Instância Gerida (Pré-visualização)](create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview). Se utilizar a Base de Dados SQL do Azure com pontos finais do serviço de rede virtual/Instância Gerida (Pré-visualização) para alojar a SSISDB ou pedir acesso a dados no local, tem de associar o seu IR do Azure-SSIS a uma rede virtual, veja [Criar IR do Azure-SSIS numa rede virtual](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Confirme que a definição “**Permitir acesso aos serviços do Azure**” está **Ativada** para o seu servidor da base de dados. Esta definição não se aplica ao utilizar a Base de Dados SQL do Azure com pontos finais do serviço de rede virtual/Instância Gerida (Pré-visualização) para alojar a SSISDB. Para obter mais informações, veja [Proteger a base de dados SQL do Azure](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Para ativar esta definição com o PowerShell, veja [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1). 
    - Adicione o endereço IP do computador cliente ou um intervalo de endereços IP que inclua o endereço IP do computador cliente à lista de endereços IP do cliente nas definições de firewall para o servidor de base de dados. Para obter mais informações, veja [Regras de firewall ao nível do servidor da Base de Dados SQL e ao nível da base de dados](../sql-database/sql-database-firewall-configure.md). 
    - Pode ligar ao servidor de base de dados através da autenticação SQL com as credenciais de administrador do servidor ou a autenticação do Azure Active Directory (AAD) com a sua Identidade de Serviço Gerida do Azure Data Factory (MSI).  Para esta última opção, tem de adicionar o MSI do Data Factory a um grupo do AAD com permissões de acesso ao servidor de base de dados, veja [Criar IR do Azure-SSIS com a autenticação do AAD](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Certifique-se de que o servidor da Base de Dados SQL do Azure não tem um Catálogo de SSIS (base de dados SSISDB). O aprovisionamento do Azure-SSIS IR não suporta a utilização de um Catálogo de SSIS existente. 
- **Azure PowerShell**. Siga as instruções em [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps). Vai utilizar o PowerShell para executar um script para aprovisionar um integration runtime do Azure-SSIS que executa pacotes de SSIS na cloud. 

> [!NOTE]
> - Para obter uma lista de regiões do Azure em que o Data Factory está atualmente disponível, selecione as regiões que lhe interessam na página seguinte e, em seguida, expanda **Analytics** para localizar **Data Factory**: [Produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/). 
> - Para obter uma lista de regiões do Azure em que o Integration Runtime Azure-SSIS está atualmente disponível, selecione as regiões que lhe interessam na página seguinte e, em seguida, expanda **Analytics** para localizar **Integration Runtime Azure-SSIS**: [Produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="launch-windows-powershell-ise"></a>Iniciar o ISE do Windows PowerShell
Inicie o **ISE do Windows PowerShell** com privilégios administrativos. 

## <a name="create-variables"></a>Criar variáveis
Copie e cole o script seguinte; especifique valores para as variáveis. Para obter uma lista dos **escalões de preço** suportados na Base de Dados SQL do Azure, veja [Limites de recursos da Base de Dados SQL](../sql-database/sql-database-resource-limits.md).

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B". For standard/premium/Elastic Pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"
```

## <a name="validate-the-connection-to-database"></a>Validar a ligação à base de dados
Adicione o script seguinte para validar o servidor da Base de Dados SQL, `<servername>.database.windows.net`. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

Para criar uma base de dados SQL do Azure como parte do script, veja o exemplo seguinte: 

Defina valores para as variáveis que ainda não foram definidos. Por exemplo: SSISDBServerName, FirewallIPAddress. 

```powershell
New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="log-in-and-select-subscription"></a>Iniciar sessão e selecionar a subscrição
Adicione o seguinte código ao script para iniciar sessão e selecionar a sua subscrição do Azure: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo. O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup` na localização `westeurope`.

Se o grupo de recursos já existir, não copie este código para o script. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Execute o comando seguinte para criar uma fábrica de dados:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Criar um integration runtime
Execute o seguinte comando para criar um integration runtime do Azure-SSIS que execute pacotes de SSIS no Azure: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
  
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier
```

## <a name="start-integration-runtime"></a>Iniciar o integration runtime
Execute o seguinte comando para iniciar o integration runtime do Azure-SSIS: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Este comando demora entre **20 e 30 minutos** a concluir. 

## <a name="deploy-ssis-packages"></a>Implementar pacotes de SSIS
Agora, utilize o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar os pacotes de SSIS no Azure. Ligue ao seu SQL server do Azure que aloja o catálogo de SSIS (SSISDB). O nome do servidor da Base de Dados SQL do Azure está no formato `<servername>.database.windows.net`. 

Veja os artigos seguintes da documentação do SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Implementar, executar e monitorizar pacotes do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Connect to SSIS catalog on Azure (Ligar ao catálogo do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Agendar a execução de pacotes no Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Ligar a origens de dados no local com a autenticação do Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="full-script"></a>Script completo
O script do PowerShell nesta secção configura uma instância do integration runtime do Azure-SSIS na cloud, que executa pacotes de SSIS. Depois de executar este script com êxito, pode implementar e executar pacotes do SSIS na cloud do Microsoft Azure com o SSISDB alojado na Base de Dados SQL do Azure.

1. Inicie o Windows PowerShell Integrated Scripting Environment (ISE).
2. No ISE, execute o seguinte comando a partir da linha de comandos.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Copie o script do PowerShell nesta secção e cole-o no ISE.
4. Indique os valores adequados para todos os parâmetros no início do script.
5. Execute o script. O comando `Start-AzureRmDataFactoryV2IntegrationRuntime` quase no fim do script é executado durante **20 a 30 minutos**.

> [!NOTE]
> - O script liga ao seu servidor da Base de Dados SQL do Azure ou para preparar a base de dados do Catálogo do SSIS (SSISDB).

> - Quando aprovisionar uma instância do IR do Azure-SSIS, o Pacote de Funcionalidades do Azure para SSIS e o Acesso Redistribuível também são instalados. Estes componentes fornecem conectividade aos ficheiros do Excel e acesso e a várias origens de dados do Azure, para além das origens de dados suportadas pelos componentes incorporados. Também pode instalar componentes adicionais. Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

Para obter uma lista dos **escalões de preço** suportados na Base de Dados SQL do Azure, veja [Limites de recursos da Base de Dados SQL](../sql-database/sql-database-resource-limits.md). 

Para obter uma lista das regiões suportadas pelo Azure Data Factory V2 e o Integration Runtime Azure-SSIS, veja [Products available by region](https://azure.microsoft.com/regions/services/) (Produtos disponíveis por região). Expanda **Dados + Análise** para ver **Data Factory V2** e **Integration Runtime do SSIS**.

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B". For standard/premium/Elastic Pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
    
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
    
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-virtual-network"></a>Associar um IR Azure-SSIS a uma rede virtual
Se utilizar a Base de Dados SQL do Azure com pontos finais de serviço de rede virtual/Instância Gerida (Pré-visualização) associada a uma rede virtual para alojar a SSISDB, também tem de associar o runtime de integração do Azure-SSIS à mesma rede virtual. O Azure Data Factory permite-lhe associar o seu runtime de integração Azure-SSIS a uma rede virtual. Para obter mais informações, veja [Join Azure-SSIS integration runtime to a virtual network (Associar um runtime de integração do Azure-SSIS a uma rede virtual)](join-azure-ssis-integration-runtime-virtual-network.md).

Para obter um script completo para criar um runtime de integração do Azure-SSIS que é associado a uma rede virtual, veja [Create an Azure-SSIS integration runtime (Criar um runtime de integração do Azure-SSIS)](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Monitorizar e gerir o IR Azure-SSIS
Veja os artigos seguintes para obter detalhes sobre a monitorização e a gestão de IR Azure-SSIS. 

- [Monitor an Azure-SSIS integration runtime (Monitorizar um runtime de integração Azure-SSIS)](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Manage an Azure-SSIS integration runtime (Gerir um runtime de integração Azure-SSIS)](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como: 

> [!div class="checklist"]
> * Criar uma fábrica de dados.
> * Criar um integration runtime do Azure-SSIS
> * Iniciar o integration runtime do Azure-SSIS
> * Implementar pacotes de SSIS
> * Rever o script completo

Avance para o tutorial seguinte para saber como copiar dados do local para a cloud: 

> [!div class="nextstepaction"]
>[Copiar dados na cloud](tutorial-copy-data-dot-net.md)
