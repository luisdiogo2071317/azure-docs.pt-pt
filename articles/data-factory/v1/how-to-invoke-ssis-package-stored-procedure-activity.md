---
title: Invocar pacotes SSIS utilizando o Azure Data Factory - atividade de procedimento armazenado | Microsoft Docs
description: Este artigo descreve como invocar um pacote de SQL Server Integration Services (SSIS) a partir de um pipeline do Azure Data Factory com atividade de procedimento armazenado.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: ff0d1e5d644926864641ceb3e3c3a102167c1fd2
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Invocar um pacote SSIS utilizando a atividade de procedimento armazenado no Azure Data Factory
Este artigo descreve como invocar um pacote SSIS de um pipeline do Azure Data Factory através da utilização de uma atividade de procedimento armazenado. 

> [!NOTE]
> Este artigo aplica-se versão 1 do Data Factory, que é geralmente disponível. Se estiver a utilizar a versão 2 do serviço do Data Factory, o que está em pré-visualização pública, consulte [pacotes SSIS invocar utilizando a atividade de procedimento armazenado na versão 2](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="azure-sql-database"></a>Base de Dados SQL do Azure 
As instruções neste artigo utilizam uma base de dados SQL do Azure que aloja o catálogo SSIS. Também pode utilizar uma instância de geridos de SQL no Azure (pré-visualização privada).

### <a name="create-an-azure-ssis-integration-runtime"></a>Criar um integration runtime do Azure-SSIS
Criar um tempo de execução de integração do Azure-SSIS se não tiver uma, seguindo as instruções passo a passo no [Tutorial: pacotes SSIS implementar](../tutorial-deploy-ssis-packages-azure.md). Tem de criar uma fábrica de dados da versão 2 para criar um tempo de execução de integração de SSIS do Azure. 

### <a name="azure-powershell"></a>Azure PowerShell
Instalar os módulos do PowerShell do Azure mais recentes ao seguir as instruções em [como instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados
O procedimento seguinte fornece os passos para criar uma fábrica de dados. Criar um pipeline com uma atividade de procedimento armazenado neste data Factory. A atividade de procedimento armazenado executa um procedimento armazenado na base de dados SSISDB para executar o pacote de SSIS.

1. Defina uma variável para o nome do grupo de recursos que vai utilizar nos comandos do PowerShell mais tarde. Copie o texto do comando seguinte para o PowerShell, especifique um nome para o [Grupo de recursos do Azure](../../azure-resource-manager/resource-group-overview.md) com aspas duplas e execute o comando. Por exemplo: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Se o grupo de recursos já existir, pode não substituí-lo. Atribua outro valor à variável `$ResourceGroupName` e execute novamente o comando
2. Para criar o grupo de recursos do Azure, execute o comando abaixo: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Se o grupo de recursos já existir, pode não substituí-lo. Atribua outro valor à variável `$ResourceGroupName` e execute novamente o comando. 
3. Defina uma variável para o nome da fábrica de dados. 

    > [!IMPORTANT]
    >  Atualize o nome da fábrica de dados para que seja globalmente exclusivo. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Para criar a fábrica de dados, execute o seguinte **New-AzureRmDataFactory** cmdlet, utilizando a propriedade de localização e ResourceGroupName da variável $ResGrp: 
    
    ```powershell       
    $df = New-AzureRmDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    $df 
    ```

Tenha em atenção os seguintes pontos:

* O nome do Azure Data Factory deve ser globalmente exclusivo. Se receber o erro seguinte, altere o nome e tente novamente.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Para criar instâncias do Data Factory, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ser um membro das funções **contribuidor** ou **proprietário**, ou um **administrador** da subscrição do Azure.
* Atualmente, a versão 2 do Data Factory só permite criar fábricas de dados nas regiões E.U.A. Leste, E.U.A. Leste 2 e Europa Ocidental. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras regiões.

### <a name="create-an-azure-sql-database-linked-service"></a>Criar um serviço ligado da Base de Dados SQL do Azure
Crie um serviço ligado para ligar a base de dados SQL do Azure que aloja o catálogo SSIS à fábrica de dados. Fábrica de dados utiliza informações neste serviço ligado para ligar à base de dados SSISDB e executa um procedimento armazenado para ser executado um pacote SSIS. 

1. Crie um ficheiro JSON com o nome **AzureSqlDatabaseLinkedService.json** no **C:\ADF\RunSSISPackage** pasta com o seguinte conteúdo: (criar a pasta ADFv2TutorialBulkCopy se já existir.)

    > [!IMPORTANT]
    > Substitua &lt;servername&gt;, &lt;databasename&gt;, &lt;username&gt;@&lt;servername&gt; e &lt;password&gt; pelos valores da sua Base de Dados SQL do Azure antes de guardar o ficheiro.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<AZURE SQL SERVER NAME>.database.windows.net,1433;Database=SSISDB;User ID=<USERNAME>;Password=<PASSWORD>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. No **Azure PowerShell**, mude para o **C:\ADF\RunSSISPackage** pasta.
3. Execute o **New-AzureRmDataFactoryLinkedService** cmdlet para criar o serviço ligado: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzureRmDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

## <a name="create-an-output-dataset"></a>Criar um conjunto de dados de saída
Este conjunto de dados de saída é um conjunto de dados fictício que pauta a agenda do pipeline. Tenha em atenção que a frequência é definida como hora e o intervalo está definida para 1. Por conseguinte, o pipeline é executado depois de uma hora no pipeline de início e de fim. 

1. Crie um ficheiro de OuputDataset.json com o seguinte conteúdo: 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. Execute o **New-AzureRmDataFactoryDataset** cmdlet para criar um conjunto de dados. 

    ```powershell
    New-AzureRmDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

## <a name="create-a-pipeline-with-stored-procedure-activity"></a>Criar um pipeline com atividade de procedimento armazenado 
Neste passo, cria um pipeline com uma atividade de procedimento armazenado. A atividade invoca o procedimento de sp_executesql armazenados para executar o seu pacote SSIS. 

1. Crie um ficheiro JSON com o nome **MyPipeline.json** no **C:\ADF\RunSSISPackage** pasta com o seguinte conteúdo:

    > [!IMPORTANT]
    > Substitua &lt;nome da pasta&gt;, &lt;nome do PROJETO&gt;, &lt;nome do pacote&gt; com nomes de pasta, o projeto e o pacote no catálogo de SSIS antes de guardar o ficheiro. 

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'SsisAdfTest', @project_name=N'MyProject', @package_name=N'Package.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. Para criar o pipeline: **RunSSISPackagePipeline**, execute o **conjunto AzureRmDataFactoryPipeline** cmdlet.

    ```powershell
    $DFPipeLine = New-AzureRmDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

## <a name="create-a-pipeline-run"></a>Criar uma execução de pipeline
Utilize o **Invoke-AzureRmDataFactoryPipeline** cmdlet para executar o pipeline. O cmdlet devolve o ID de execução do pipeline para monitorização futura.

```powershell
$RunId = New-AzureRmDataFactoryPipeline $df -File ".\MyPipeline.json"
```

## <a name="monitor-the-pipeline-run"></a>Monitorizar a execução do pipeline.

2. Executar **Get-AzureRmDataFactorySlice** para obter detalhes sobre todos os setores da saída conjunto de dados * *, que é a tabela de saída do pipeline.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Repare que a StartDateTime que especificar aqui é a mesma hora especificada no JSON do pipeline. 
3. Execute **Get-AzureRmDataFactoryRun** para obter os detalhes das execuções de um setor específico da atividade.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Pode continuar a executar este cmdlet até ver o setor no estado **Pronto** ou **Falhou**. Quando estiver no estado Pronto, verifique a pasta **partitioneddata** no contentor **adfgetstarted** do armazenamento de blobs dos dados de saída.  A criação de um cluster do HDInsight a pedido costuma ser um pouco demorada.

    Pode executar a consulta seguinte na base de dados do SSISDB no seu servidor SQL do Azure para verificar se o pacote foi executado. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Passos seguintes
Para obter detalhes sobre a atividade de procedimento armazenado, consulte o [atividade de procedimento armazenado](data-factory-stored-proc-activity.md) artigo.

