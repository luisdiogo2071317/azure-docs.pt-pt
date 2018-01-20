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
ms.openlocfilehash: 749deb6549e0ac90da4b44424026c897108a4bb7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Invocar um pacote SSIS utilizando a atividade de procedimento armazenado no Azure Data Factory
Este artigo descreve como invocar um pacote SSIS de um pipeline do Azure Data Factory através da utilização de uma atividade de procedimento armazenado. 

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [pacotes SSIS invocar utilizando a atividade de procedimento armazenado na versão 1](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="azure-sql-database"></a>Base de Dados SQL do Azure 
As instruções neste artigo utilizam uma base de dados SQL do Azure que aloja o catálogo SSIS. Também pode utilizar uma instância de geridos de SQL no Azure (pré-visualização privada).

## <a name="create-an-azure-ssis-integration-runtime"></a>Criar um integration runtime do Azure-SSIS
Criar um tempo de execução de integração do Azure-SSIS se não tiver uma, seguindo as instruções passo a passo no [Tutorial: pacotes SSIS implementar](tutorial-deploy-ssis-packages-azure.md).

## <a name="data-factory-ui-azure-portal"></a>IU de fábrica de dados (portal do Azure)
Esta secção utiliza IU da fábrica de dados para criar um pipeline do Data Factory com uma atividade de procedimento armazenado que invoca um pacote SSIS.

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Primeiro passo é criar uma fábrica de dados utilizando o portal do Azure. 

1. Navegue para o [portal do Azure](https://portal.azure.com). 
2. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. No **nova fábrica de dados** página, introduza **ADFTutorialDataFactory** para o **nome**. 
      
     ![Nova página da fábrica de dados](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o seguinte erro para o campo de nome, altere o nome da fábrica de dados (por exemplo, Seunomeadftutorialdatafactory). Consulte [fábrica de dados - as regras de nomenclatura](naming-rules.md) artigo para regras de nomenclatura artefactos do Data Factory de dados.
  
     ![Nome não está disponível - erro](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
    Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Apenas as localizações que são suportadas pela fábrica de dados são apresentadas na lista pendente. Os dados armazena (Storage do Azure, SQL Database do Azure, etc.) e computações (HDInsight, etc.) utilizadas pela fábrica de dados podem ser noutras localizações.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Depois de concluída a criação, consulte o **Data Factory** página conforme mostrado na imagem.
   
    ![Home page da fábrica de dados](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Clique em **autor & Monitor** mosaico para iniciar a aplicação de interface (IU) do utilizador do Azure Data Factory num separador separado. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Criar um pipeline com atividade de procedimento armazenado
Neste passo, utilize a IU da fábrica de dados para criar um pipeline. Adicionar uma atividade de procedimento armazenado para o pipeline e configurá-lo para executar o pacote SSIS utilizando o procedimento sp_executesql armazenados. 

1. Na página de introdução de get, clique em **criar pipeline**: 

    ![Obter a página de introdução](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. No **atividades** caixa de ferramentas, expanda **base de dados SQL**e arrastar largar **procedimento armazenado** atividade para a superfície de desginer do pipeline. 

    ![Atividade de procedimento armazenado de arrastar e largar](./media/how-to-invoke-ssis-package-stored-procedure-activity/drag-drop-sproc-activity.png)
3. Na janela Propriedades para a atividade de procedimento armazenado, mude para o **conta SQL** separador e clique em **+ novo**. Criar uma ligação à base de dados SQL do Azure que aloja o catálogo de SSIS (SSIDB da base de dados). 
   
    ![Novo botão de serviço ligado](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-linked-service-button.png)
4. No **novo serviço ligado** janela, efetue os seguintes passos: 

    1. Selecione **base de dados SQL do Azure** para **tipo**.
    2. Selecione o seu servidor de SQL do Azure que aloja a base de dados do SSISDB para o **nome do servidor** campo.
    3. Selecione **SSISDB** para **nome de base de dados**.
    4. Para **nome de utilizador**, introduza o nome de utilizador que tenha acesso à base de dados.
    5. Para **palavra-passe**, introduza a palavra-passe do utilizador. 
    6. Testar a ligação à base de dados clicando **Testar ligação** botão.
    7. Guarde o serviço ligado ao clicar no **guardar** botão. 

        ![Serviço ligado da Base de Dados SQL do Azure](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-settings.png)
5. Na janela de propriedades, mude para o **procedimento armazenado** separador do **conta SQL** separador e efetuar os passos seguintes: 

    1. Para o **nome de procedimento armazenado** campo Enter `sp_executesql` . 
    2. Clique em **+ novo** no **armazenados parâmetros de procedimento** secção. 
    3. Para **nome** do parâmetro, introduza **instrução INSERT**. 
    4. Para **tipo** do parâmetro, introduza **cadeia** . 
    5. Para **valor** do parâmetro, introduza a seguinte consulta SQL.

        Na consulta SQL, especifique os valores corretos para o **nome_da_pasta**, **project_name**, e **package_name** parâmetros. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END
        ```

        ![Serviço ligado da Base de Dados SQL do Azure](./media/how-to-invoke-ssis-package-stored-procedure-activity/stored-procedure-settings.png)
6. Para validar a configuração do pipeline, clique em **validar** na barra de ferramentas. Para fechar o **relatório de validação de Pipeline**, clique em  **>>** .

    ![Validar o pipeline](./media/how-to-invoke-ssis-package-stored-procedure-activity/validate-pipeline.png)
7. Publicar o pipeline à fábrica de dados clicando **publicar todas as** botão. 

    ![Publicar](./media/how-to-invoke-ssis-package-stored-procedure-activity/publish-all-button.png)    

### <a name="run-and-monitor-the-pipeline"></a>Executar e monitorizar o pipeline
Nesta secção, acionar uma execução de pipeline e, em seguida, monitorizá-lo. 

1. Para acionar um pipeline executar, clique em **acionador** na barra de ferramentas e clique em **acionar agora**. 

    ![Acionar agora](./media/how-to-invoke-ssis-package-stored-procedure-activity/trigger-now.png)
2. Mudar para o **Monitor** separador no lado esquerdo. Consulte o pipeline de execução e o respetivo estado, juntamente com outras informações (por exemplo, o tempo de execução iniciar). Para atualizar a vista, clique em **atualizar**.

    ![Execuções de pipeline](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)
3. Clique em **execuções de atividade de vista** ligação no **ações** coluna. Ver apenas uma atividade executar como do pipeline tem apenas uma atividade (atividade de procedimento armazenado).

    ![Execuções de atividade](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-runs.png) 4. pode executar o seguinte **consulta** contra o SSISDB da base de dados no seu servidor SQL do Azure para verificar se o pacote foi executado. 

    ```sql
    select * from catalog.executions
    ```

    ![Certifique-se de execuções de pacote](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

Também pode criar um acionador agendado para o pipeline, para que o pipeline é executado numa agenda (houly, diária, etc.). Por exemplo, consulte [criar uma fábrica de dados - a IU de fábrica de dados](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
Nesta secção, utiliza o Azure PowerShell para criar um pipeline do Data Factory com uma atividade de procedimento armazenado que invoca um pacote SSIS. 

Instale os módulos do Azure PowerShell mais recentes ao seguir as instruções em [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps). 

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Pode utilizar a mesma fábrica de dados que tenha a resposta a incidentes SSIS do Azure ou criar uma fábrica de dados separada. O procedimento seguinte fornece os passos para criar uma fábrica de dados. Criar um pipeline com uma atividade de procedimento armazenado neste data Factory. A atividade de procedimento armazenado executa um procedimento armazenado na base de dados SSISDB para executar o pacote de SSIS. 

1. Defina uma variável para o nome do grupo de recursos que vai utilizar nos comandos do PowerShell mais tarde. Copie o texto do comando seguinte para o PowerShell, especifique um nome para o [Grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) com aspas duplas e execute o comando. Por exemplo: `"adfrg"`. 
   
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

5. Para criar a fábrica de dados, execute o seguinte cmdlet **Set-AzureRmDataFactoryV2**, utilizando a propriedade de Location e ResourceGroupName da variável $ResGrp: 
    
    ```powershell       
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Tenha em atenção os seguintes pontos:

* O nome do Azure Data Factory deve ser globalmente exclusivo. Se receber o erro seguinte, altere o nome e tente novamente.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Para criar instâncias do Data Factory, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ser um membro das funções **contribuidor** ou **proprietário**, ou um **administrador** da subscrição do Azure.
* Atualmente, a versão 2 do Data Factory só permite criar fábricas de dados nas regiões E.U.A. Leste, E.U.A. Leste 2 e Europa Ocidental. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras regiões.

### <a name="create-an-azure-sql-database-linked-service"></a>Criar um serviço ligado da Base de Dados SQL do Azure
Crie um serviço ligado para ligar a base de dados SQL do Azure que aloja o catálogo SSIS à fábrica de dados. Fábrica de dados utiliza informações neste serviço ligado para ligar à base de dados SSISDB e executa um procedimento armazenado para ser executado um pacote SSIS. 

1. Crie um ficheiro JSON com o nome **AzureSqlDatabaseLinkedService.json** no **C:\ADF\RunSSISPackage** pasta com o seguinte conteúdo: 

    > [!IMPORTANT]
    > Substitua &lt;servername&gt;, &lt;username&gt;, e &lt;palavra-passe&gt; com valores do seu SQL Database do Azure antes de guardar o ficheiro.

    ```json
    {
        "name": "AzureSqlDbLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            }
        }
    }
    ```

2. No **Azure PowerShell**, mude para o **C:\ADF\RunSSISPackage** pasta.

3. Execute o cmdlet **Set-AzureRmDataFactoryV2LinkedService** para criar o serviço ligado: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Criar um pipeline com atividade de procedimento armazenado 
Neste passo, cria um pipeline com uma atividade de procedimento armazenado. A atividade invoca o procedimento de sp_executesql armazenados para executar o seu pacote SSIS. 

1. Crie um ficheiro JSON com o nome **RunSSISPackagePipeline.json** no **C:\ADF\RunSSISPackage** pasta com o seguinte conteúdo:

    > [!IMPORTANT]
    > Substitua &lt;nome da pasta&gt;, &lt;nome do PROJETO&gt;, &lt;nome do pacote&gt; com nomes de pasta, o projeto e o pacote no catálogo de SSIS antes de guardar o ficheiro. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [
                {
                    "name": "My SProc Activity",
                    "description":"Runs an SSIS package",
                    "type": "SqlServerStoredProcedure",
                    "linkedServiceName": {
                        "referenceName": "AzureSqlDbLinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "typeProperties": {
                        "storedProcedureName": "sp_executesql",
                        "storedProcedureParameters": {
                            "stmt": {
                                "value": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER NAME>', @project_name=N'<PROJECT NAME>', @package_name=N'<PACKAGE NAME>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                            }
                        }
                    }
                }
            ]
        }
    }
    ```

2. Para criar o pipeline: **RunSSISPackagePipeline**, execute o **conjunto AzureRmDataFactoryV2Pipeline** cmdlet.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Segue-se o resultado do exemplo:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>Criar uma execução de pipeline
Utilize o **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet para executar o pipeline. O cmdlet devolve o ID de execução do pipeline para monitorização futura.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>Monitorizar a execução do pipeline.

Execute o seguinte script do PowerShell para verificar continuamente o estado de execução do pipeline até que este termine de copiar os dados. Copie/cole o seguinte script na janela do PowerShell e prima ENTER. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

### <a name="create-a-trigger"></a>Criar um acionador
No passo anterior, invocar a pipeline a pedido. Também pode criar um acionador de agenda para executar o pipeline com base numa agenda (hora a hora, diária, etc..).

1. Crie um ficheiro JSON com o nome **MyTrigger.json** no **C:\ADF\RunSSISPackage** pasta com o seguinte conteúdo: 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. No **Azure PowerShell**, mude para o **C:\ADF\RunSSISPackage** pasta.
3. Execute o **conjunto AzureRmDataFactoryV2Trigger** cmdlet para criar o acionador. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Por predefinição, o acionador está no estado parado. Inicie o acionador executando o **início AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Confirme que o acionador foi iniciado executando o **Get-AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. Após a hora seguinte, execute o seguinte comando. Por exemplo, se a hora atual é UTC às 15:25, execute o comando 4 PM UTC. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    Pode executar a consulta seguinte na base de dados do SSISDB no seu servidor SQL do Azure para verificar se o pacote foi executado. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Passos Seguintes
Também pode monitorizar o pipeline com o portal do Azure. Para obter instruções passo a passo, consulte [monitorizar o pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).
