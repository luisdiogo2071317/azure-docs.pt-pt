---
title: Criar o tempo de execução do Azure-SSIS integração no Azure Data Factory | Microsoft Docs
description: Saiba como criar um tempo de execução de integração do Azure-SSIS no Azure Data Factory para que possa implementar e executar pacotes SSIS no Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f75d08074fc6020541226318d6422da373136a2d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268151"
---
# <a name="create-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Criar o tempo de execução de integração do Azure-SSIS no Azure Data Factory
Este artigo fornece os passos para o aprovisionamento de um tempo de execução de integração do Azure-SSIS no Azure Data Factory. Em seguida, pode utilizar o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar e executar os pacotes do SQL Server Integration Services (SSIS) neste tempo de execução no Azure.

O tutorial [Tutorial: implementar pacotes de SQL Server Integration Services (SSIS) para o Azure](tutorial-create-azure-ssis-runtime-portal.md) mostra-lhe como criar um tempo de execução de integração do Azure SSIS (IR) ao utilizar a SQL Database do Azure para alojar o catálogo de SSIS. Este artigo expande o tutorial e mostra-lhe como efetue os seguintes procedimentos: 

- Opcionalmente, utilize SQL Database do Azure com pontos finais do serviço de rede virtual/gerido instância (pré-visualização) como o servidor de base de dados para alojar o seu catálogo SSIS (base de dados SSISDB). Como pré-requisito, precisa de associar a sua resposta a incidentes SSIS do Azure a uma rede virtual e configurar definições e as permissões de rede virtual conforme necessário, consulte [associação do Azure-SSIS IR para uma rede virtual](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).  
- Opcionalmente, utilize autenticação do Azure Active Directory (AAD) com o Azure dados fábrica geridos serviço de identidade (MSI) para IR de SSIS do Azure para ligar ao servidor de base de dados. Como pré-requisito, terá de adicionar o MSI da fábrica de dados para um grupo AAD com permissões de acesso para o servidor de base de dados, consulte [autenticação ativar o AAD para IR de Azure SSIS](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir).

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-introduction.md).


## <a name="overview"></a>Descrição geral
Este artigo mostra formas diferentes de aprovisionamento de uma resposta a incidentes SSIS do Azure:

- [Portal do Azure](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Modelo do Azure Resource Manager](#azure-resource-manager-template)

Quando cria uma resposta a incidentes SSIS do Azure, o Data Factory estabelece ligação à SQL Database do Azure para preparar a base de dados do catálogo de SSIS (SSISDB). O script também configura as permissões e definições da sua rede virtual, se for especificado e associa a nova instância do Azure-SSIS integração runtime para a rede virtual.

Quando aprovisionar uma instância do IR do Azure-SSIS, o Pacote de Funcionalidades do Azure para SSIS e o Acesso Redistribuível também são instalados. Estes componentes fornecem conectividade aos ficheiros do Excel e acesso e a várias origens de dados do Azure, para além das origens de dados suportadas pelos componentes incorporados. Também pode instalar componentes adicionais. Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**. Se não tiver uma subscrição, pode criar uma conta de [avaliação gratuita](http://azure.microsoft.com/pricing/free-trial/).
- **Azure base de dados do SQL server/gerido instância (pré-visualização)**. Se ainda não tiver um servidor de base de dados, crie um no portal do Azure antes de começar. Este servidor aloja a base de dados do Catálogo de SSIS (SSISDB). Recomendamos que crie o servidor de base de dados na mesma região do Azure que o integration runtime. Esta configuração permite que o integration runtime escreva registos de execução no SSISDB sem cruzamento entre regiões do Azure. Com base no servidor de base de dados selecionada, SSISDB pode ser criada em seu nome como uma base de dados autónomo, parte de um conjunto elástico ou numa instância geridos (pré-visualização) e é acessível na rede pública ou ao associar uma rede virtual. Para obter uma lista dos escalões de preços suportados para a SQL Database do Azure, consulte [dos limites de recursos de base de dados SQL](../sql-database/sql-database-resource-limits.md).

    Certifique-se de que a base de dados do Azure SQL server/gerido instância (pré-visualização) não tem um catálogo de SSIS (SSIDB da base de dados). O aprovisionamento do Azure-SSIS IR não suporta a utilização de um Catálogo de SSIS existente.
- **Rede virtual clássica/Azure Resource Manager (opcional)**. Tem de ter uma rede virtual do Azure se, pelo menos, uma das seguintes condições for verdadeira:
    - Estão a alojar a base de dados SSIS catálogo na base de dados do Azure SQL com pontos finais do serviço de rede virtual/gerido instância (pré-visualização) que está no interior de uma rede virtual.
    - Pretende ligar a arquivos de dados no local a partir de pacotes de SSIS em execução no integration runtime do Azure-SSIS.
- **Azure PowerShell**. Siga as instruções em [como instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps), se utilizar o PowerShell para executar um script para aprovisionar Azure SSIS integração runtime que executa pacotes SSIS na nuvem. 

> [!NOTE]
> - Pode criar uma fábrica de dados da versão 2 nas seguintes regiões: E.U.A. Leste, E.U.A. Leste 2, Sudeste Asiático e Europa Ocidental. 
> - Pode criar um IR do Azure-SSIS nas seguintes regiões: E.U.A. Leste, E.U.A. Leste 2, E.U.A. Central, E.U.A. Oeste 2, Europa do Norte, Europa Ocidental, Sul do Reino Unido e Leste da Austrália.

## <a name="azure-portal"></a>Portal do Azure
Nesta secção, utilize o portal do Azure, especificamente, a IU de fábrica de dados, para criar um IR. SSIS do Azure 

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. Inicie sessão no [Portal do Azure](https://portal.azure.com/).    
3. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
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

### <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionar um runtime de integração de SSIS do Azure

1. Na página de introdução, clique no mosaico **Configurar o Integration Runtime do SSIS**. 

   ![Configurar o mosaico do Integration Runtime do SSIS](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Na página **Definições Gerais** da **Configuração do Runtime de Integração**, conclua os seguintes passos: 

    ![Definições gerais](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Para **nome**, introduza o nome do seu tempo de execução de integração.
    
    b. Para **Descrição**, introduza a descrição do seu tempo de execução de integração.
    
    c. Para **localização**, selecionar a localização do seu tempo de execução de integração. Apenas são apresentadas as localizações suportadas. Recomendamos que selecione a mesma localização do seu servidor de base de dados para o anfitrião SSISDB.
    
    d. Para **tamanho de nó**, selecione o tamanho de nó do cluster de tempo de execução de integração. Tamanhos de nó suportados apenas são apresentados. Selecione um tamanho de nó grande (trabalho de dimensionamento), se pretender executar muitas computação/memória – pacotes intensivas.
    
    e. Para **número de nó**, selecione o número de nós do cluster de tempo de execução de integração. Números de nó suportados apenas são apresentados. Selecione um cluster de grandes dimensões com vários nós (de escalamento horizontal), se pretender executar muitos pacotes em paralelo.
     
    f. Para **edição/licença**, selecione a edição/licença do SQL Server para o tempo de execução de integração: Standard ou Enterprise. Selecione Enterprise, se pretender utilizar as funcionalidades avançadas/premium no seu tempo de execução de integração. 
    
    g. Para **guardar dinheiro**, selecione a opção de Azure híbrida benefício (AHB) para o tempo de execução de integração: ou No. Selecione Sim, se pretender traga a sua própria licença do SQL Server com o Software Assurance beneficiar das reduções de custos com utilização híbrida.
    
    h. Clique em **Seguinte**. 
3. Na página **Definições de SQL**, conclua os seguintes passos: 

    ![Definições de SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Para **subscrição**, selecione a subscrição do Azure com o servidor de base de dados para o anfitrião SSISDB. 
    
    b. Para **localização**, selecione a localização do seu servidor de base de dados para o anfitrião SSISDB. Recomendamos que selecione a mesma localização do seu tempo de execução de integração.
    
    c. Para **ponto final do servidor de base de dados do catálogo**, selecione o ponto final do seu servidor de base de dados para o anfitrião SSISDB. Com base no servidor de base de dados selecionada, SSISDB pode ser criada em seu nome como uma base de dados autónomo, parte de um conjunto elástico ou numa instância geridos (pré-visualização) e é acessível na rede pública ou ao associar uma rede virtual.
    
    d. No **autenticações AAD de utilização...**  caixa de verificação, selecione o método de autenticação para o servidor de base de dados para o anfitrião SSISDB: SQL Server ou do Azure Active Directory (AAD) com o Azure dados fábrica geridos serviço de identidade (MSI). Se selecionar esta opção, terá de adicionar o MSI da fábrica de dados para um grupo AAD com permissões de acesso para o servidor de base de dados, consulte [autenticação ativar o AAD para IR de Azure SSIS](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    
    e. Para **nome de utilizador de Admin**, introduza o nome de utilizador do SQL Server authentication para o servidor de base de dados SSISDB do anfitrião.
    
    f. Para **palavra-passe de administrador**, introduza a palavra-passe de autenticação de SQL Server para o servidor de base de dados para o anfitrião SSISDB.  
    
    g. Para **camada de serviço de base de dados de catálogo**, selecione a camada de serviço para o servidor de base de dados para o anfitrião SSISDB: escalão padrão/Basic/Premium ou o nome do conjunto elástico.
    
    h. Clique em **Testar ligação** e se tiver êxito, clique em **seguinte**. 
4.  No **definições avançadas** página, conclua os seguintes passos:   

    ![Definições avançadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
    
    a. Para **máximo execuções por nó Parallel**, selecione o número máximo de pacotes para executar em simultâneo por nó do cluster de tempo de execução de integração. Suportado apenas são apresentados os números de pacote. Selecione um número reduzido, se pretender utilizar mais do que um núcleo para executar um único pacote de grande/pesado-peso que é computação/memória-intensiva. Selecione um número elevado, se pretender executar um ou mais pacotes de pequena/leve-ponderação num único núcleo. 
    
    b. Para **URI de SAS do contentor de configuração personalizada**, opcionalmente, introduza do contentor de Blob de armazenamento do Azure, a assinatura de acesso partilhado (SAS) Uniform Resource identificador (URI) onde o script de configuração e os ficheiros associados são armazenados, consulte [Configuração personalizada para IR de Azure SSIS](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 
5. No **selecionar uma rede virtual...**  caixa de verificação, selecione se pretende associar o seu tempo de execução de integração a uma rede virtual. A verificar se utilizar a SQL Database do Azure com pontos finais do serviço de rede virtual/gerido instância (pré-visualização) para alojar SSISDB ou necessitam de acesso a dados no local; ou seja, tem de origens de dados no local, os destinos nos seus pacotes SSIS, consulte [associar IR SSIS do Azure a uma rede virtual](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Se selecionar esta opção, conclua os seguintes passos:


    ![Definições avançadas com a rede virtual](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    a. Para **subscrição**, selecione a subscrição do Azure que tenha a sua rede virtual. 
    
    b. Para **localização**, a mesma localização do seu tempo de execução de integração está selecionada.
    
    b. Para **tipo**, selecione o tipo de rede virtual: clássico ou do Azure Resource Manager. Recomendamos que selecione a rede virtual do Azure Resource Manager, desde clássico rede virtual será preterida em breve.
    
    c. Para **nome da VNet**, selecione o nome da sua rede virtual. Esta rede virtual deve ser a mesma rede virtual utilizada para a SQL Database do Azure com pontos finais do serviço de rede virtual/gerido instância (pré-visualização) para alojar SSISDB e/ou a uma ligada à sua rede no local.

    d. Para **nome de sub-rede**, selecione o nome de sub-rede da rede virtual. Esta rede virtual deve ser uma sub-rede diferente daquela utilizado para a instância geridos (pré-visualização) para o anfitrião SSISDB.
6. Clique em **VNet validação** e se tiver êxito, clique em **concluir** para iniciar a criação do seu tempo de execução de integração de SSIS do Azure. 

    > [!IMPORTANT]
    > - Este processo demora cerca de 20 minutos a concluir
    > - O serviço Data Factory liga à sua Base de Dados SQL do Azure ou para preparar a base de dados do Catálogo do SSIS (SSISDB). O script também configura as permissões e definições da sua rede virtual, se for especificado e associa a nova instância do Azure-SSIS integração runtime para a rede virtual.
7. Na janela **Ligações**, mude para **Runtimes de Integração** se for necessário. Clique em **Atualizar** para atualizar o estado. 

    ![Estado da criação](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Utilize as ligações em **ações** coluna para iniciar/parar, editar ou eliminar o tempo de execução de integração. Utilize a última ligação para ver o código JSON para o runtime de integração. Os botões de editar e eliminar estão ativados, apenas quando a resposta a incidentes está parada. 

    ![Azure SSIS IR - ações](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Runtime de integração de SSIS do Azure no portal

1. Na IU do Azure Data Factory, mude para o separador **Editar**, clique em **Ligações**e, em seguida, mude para o separador **Runtimes de Integração** para ver os runtimes de integração existente na sua fábrica de dados. 
    ![Ver IRs existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Clique em **Novo** para criar um novo SSIS IR do Azure. 

    ![Runtime de integração através do menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Para criar um tempo de execução de integração de SSIS do Azure, clique em **Novo** conforme mostrado na imagem. 
3. Na janela de Configuração do Integration Runtime, selecione **pacotes SSIS existentes de migração lift-and-shift a executar no Azure**e, em seguida, clique em **Seguinte**.

    ![Especifique o tipo de runtime de integração](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Veja a secção [Aprovisionar um runtime de integração do Azure SSIS](#provision-an-azure-ssis-integration-runtime) para os restantes passos para configurar um SSIS IR do Azure.

## <a name="azure-powershell"></a>Azure PowerShell
Nesta secção, utilizar o Azure PowerShell para criar um IR. SSIS do Azure

### <a name="create-variables"></a>Criar variáveis
Defina variáveis para utilizar nos scripts neste tutorial:

```powershell
### Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# You can create a data factory of version 2 in the following regions: East US, East US 2, Southeast Asia, and West Europe. 
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# You can create an Azure-SSIS IR in the following regions: East US, East US 2, Central US, West US 2, North Europe, West Europe, UK South, and Australia East.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance (Preview)/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = “[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance (Preview)

### SSISDB info
$SSISDBServerEndpoint = “[your Azure SQL Database server name or Managed Instance (Preview) name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance (Preview)]"
```

### <a name="log-in-and-select-subscription"></a>Iniciar sessão e selecionar a subscrição
Adicione o seguinte código o script para iniciar sessão e selecione a sua subscrição do Azure: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Validar a ligação à base de dados
Adicione o seguinte script para validar o ponto final de servidor de SQL Database do Azure. 

```powershell
if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    if(![string]::IsNullOrEmpty($SSISDBPricingTier))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=“ + $SSISDBServerAdminPassword
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_"  ;
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
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
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Execute o comando seguinte para criar uma fábrica de dados.

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Criar um integration runtime
Execute os seguintes comandos para criar um tempo de execução de integração do Azure-SSIS executado pacotes SSIS no Azure. 

Se não utilizar SQL Database do Azure com pontos finais do serviço de rede virtual/gerido instância (pré-visualização) para alojar SSISDB nem necessitam de acesso a dados no local, pode omitir parâmetros VNetId e sub-rede ou passar valores vazios para os mesmos. Caso contrário, não pode omiti-los e deve passar valores válidos da sua configuração de rede virtual, consulte [associar IR SSIS do Azure a uma rede virtual](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Se utilizar a instância geridos (pré-visualização) para anfitrião SSISDB, pode omitir CatalogPricingTier parâmetro ou transmita um valor vazio para o mesmo. Caso contrário, não pode omiti-lo e deve transmita um valor válido na lista de suportado escalões de preço da base de dados do Azure SQL, consulte [dos limites de recursos de base de dados SQL](../sql-database/sql-database-resource-limits.md). 

Se utilizar a autenticação do Azure Active Directory (AAD) com o Azure dados fábrica geridos serviço de identidade (MSI) para ligar ao servidor de base de dados, pode omitir CatalogAdminCredential parâmetro, mas tem de adicionar o MSI da fábrica de dados para um grupo AAD com acesso permissões para o servidor de base de dados, consulte [autenticação ativar o AAD para IR de Azure SSIS](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir).

```powershell               
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}
```

Tem de passar valores para parâmetros VnetId e a sub-rede com o Azure geridos instância do SQL (pré-visualização) que é associado uma rede virtual. O parâmetro CatalogPricingTier não é aplicável para Azure geridos instância do SQL (pré-visualização). 

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

Eis o script completo que cria um tempo de execução de integração de SSIS do Azure. 

```powershell
### Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# You can create a data factory of version 2 in the following regions: East US, East US 2, Southeast Asia, and West Europe. 
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# You can create an Azure-SSIS IR in the following regions: East US, East US 2, Central US, West US 2, North Europe, West Europe, UK South, and Australia East.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance (Preview)/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = “[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance (Preview)

### SSISDB info
$SSISDBServerEndpoint = “[your Azure SQL Database server name or Managed Instance (Preview) name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance (Preview)]"

### Log in and select subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database
if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    if(![string]::IsNullOrEmpty($SSISDBPricingTier))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=“ + $SSISDBServerAdminPassword
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_"  ;
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
    }
}

### Configure virtual network
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}

### Start integration runtime   
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Modelo Azure Resource Manager
Nesta secção, utilize o modelo Azure Resource Manager para criar o tempo de execução de integração de SSIS do Azure. Eis uma exemplo de instruções: 

1. Crie um ficheiro JSON com o seguinte modelo Azure Resource Manager. Substitua os valores na angled Retos (local proprietários da) com os seus próprios valores. 

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2017-09-01-preview",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D4_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```
    
2. Para implementar o modelo Azure Resource Manager, execute o comando de New-AzureRmResourceGroupDeployment conforme mostrado no exemplo seguinte, onde ADFTutorialResourceGroup é o nome do grupo de recursos e adftutorialarm. JSON é o ficheiro que contém um JSON definição para a sua fábrica de dados e IR. SSIS do Azure 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Este comando cria a fábrica de dados e IR SSIS do Azure no mesmo, mas não inicia o IR. 
3. Para iniciar a sua resposta a incidentes SSIS do Azure, execute o comando de início AzureRmDataFactoryV2IntegrationRuntime: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ``` 

## <a name="deploy-ssis-packages"></a>Implementar pacotes de SSIS
Agora, utilize o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar os pacotes de SSIS no Azure. Ligar ao seu servidor de base de dados que aloja o catálogo SSIS (SSISDB). O nome do servidor de base de dados está no formato: &lt;nome do servidor de base de dados do Azure SQL&gt;. database.windows.net ou &lt;nome da instância geridos (pré-visualização). Prefixo DNS&gt;. database.windows.net. Veja o artigo [Implementar pacotes](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) para obter instruções.

## <a name="next-steps"></a>Passos Seguintes
Consulte os outros tópicos do Azure-SSIS IR nesta documentação:

- [Tempo de execução do Azure-SSIS integração](concepts-integration-runtime.md#azure-ssis-integration-runtime). Este artigo fornece informações concetuais sobre tempos de execução de integração em geral, incluindo o IR. SSIS do Azure 
- [Tutorial: implementar pacotes do SSIS no Azure](tutorial-create-azure-ssis-runtime-portal.md). Este artigo disponibiliza instruções passo a passo para criar um IR Azure-SSIS e utiliza uma base de dados da Base de Dados SQL do Azure para alojar o catálogo do SSIS. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Monitorizar um IR Azure-SSIS) Este artigo mostra como obter informações sobre um IR Azure-SSIS e descrições dos estados nas informações devolvidas. 
- [Manage an Azure-SSIS IR](manage-azure-ssis-integration-runtime.md) (Gerir um IR Azure-SSIS). Este artigo mostra-lhe como parar, iniciar ou remover um IR Azure-SSIS. Mostra também como aumentá-lo horizontalmente mediante a adição de mais nós ao mesmo. 
- [Associar uma resposta a incidentes SSIS do Azure a uma rede virtual](join-azure-ssis-integration-runtime-virtual-network.md). Este artigo fornece informações concetuais sobre a associar a sua resposta a incidentes SSIS do Azure a uma rede virtual do Azure. Também fornece passos para utilizar o portal do Azure para configurar a rede virtual, para que o Azure SSIS IR juntar-se a rede virtual. 

