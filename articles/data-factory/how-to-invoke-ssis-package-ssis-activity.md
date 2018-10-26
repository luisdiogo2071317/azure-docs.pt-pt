---
title: Execute o pacote do SSIS com a atividade de execução nas pacotes de SSIS - Azure | Documentos da Microsoft
description: Este artigo descreve como executar um pacote de SQL Server Integration Services (SSIS) num pipeline do Azure Data Factory através da atividade para executar pacotes SSIS.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/16/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: cda439973c584a57cadc30de7fb931732682de00
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092462"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Executar um pacote do SSIS com a atividade para executar o pacote de SSIS no Azure Data Factory
Este artigo descreve como executar um pacote do SSIS num pipeline do Azure Data Factory com uma atividade de executar o pacote do SSIS. 

## <a name="prerequisites"></a>Pré-requisitos

**Base de Dados SQL do Azure**. As instruções neste artigo utilizam uma base de dados SQL do Azure que aloja o catálogo do SSIS. Também pode utilizar uma instância de gerida de base de dados do Azure SQL.

## <a name="create-an-azure-ssis-integration-runtime"></a>Criar um integration runtime do Azure-SSIS
Criar um runtime de integração Azure-SSIS se não tiver uma ao seguir as instruções passo a passo na [Tutorial: pacotes do SSIS implementar](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Executar um pacote no portal do Azure
Nesta secção, vai utilizar IU do Data Factory para criar um pipeline do Data Factory com uma atividade de executar o pacote do SSIS que executa um pacote do SSIS.

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Primeiro passo é criar uma fábrica de dados com o portal do Azure. 

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. Navegue para o [portal do Azure](https://portal.azure.com). 
3. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory). Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Nome não disponível - erro](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
    Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Só aparecem na lista pendente as localizações que o Data Factory suporta. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras localizações.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
    ![Home page da fábrica de dados](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a aplicação de interface de utilizador (IU) do Azure Data Factory num separador à parte. 

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Criar um pipeline com uma atividade de executar o pacote do SSIS
Neste passo, vai utilizar a IU do Data Factory para criar um pipeline. Adicionar uma atividade de executar o pacote do SSIS para o pipeline e configurá-lo para executar o pacote do SSIS. 

1. Na página de introdução, clique em **criar pipeline**: 

    ![Página Introdução](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. Na **atividades** caixa de ferramentas, expanda **gerais**e arraste e largue o **executar pacotes do SSIS** atividade para a superfície de desenho do pipeline. 

   ![Arraste a atividade de executar o pacote do SSIS para a superfície de desenho](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Sobre o **gerais** separador de propriedades para a atividade de executar o pacote do SSIS, forneça um nome e descrição para a atividade. Defina o tempo limite opcional e repita a valores.

    ![Definir as propriedades no separador Geral](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Na **definições** separador de propriedades para a atividade executar pacotes do SSIS, selecione o Runtime de integração Azure-SSIS é associado a `SSISDB` base de dados em que o pacote é implementado. Forneça o caminho de pacote no `SSISDB` base de dados no formato `<folder name>/<project name>/<package name>.dtsx`. Opcionalmente, especifique a execução de 32 bits e um nível de registo predefinidas ou personalizadas e fornecer um caminho de ambiente no formato `<folder name>/<environment name>`.

    ![Definir as propriedades no separador Definições](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

5. Para validar a configuração do pipeline, clique em **Validate** na barra de ferramentas. Para fechar o **Relatório de Validação do Pipeline**, clique em **>>**.

6. Publicar o pipeline no Data Factory clicando **publicar tudo** botão. 

### <a name="optionally-parameterize-the-activity"></a>Opcionalmente, parametrizar a atividade

Opcionalmente, atribuir valores, expressões ou funções, o que podem referenciar variáveis de sistema do Data Factory, a seu projeto ou pacote de parâmetros no formato JSON através do botão "Ver código-fonte" na parte inferior da sua caixa de atividade de executar o pacote do SSIS ou o "código" botão no canto superior direito da sua área de pipeline. Por exemplo, pode atribuir parâmetros do pipeline de fábrica de dados ao seu projeto do SSIS ou parâmetros de pacote, conforme mostrado nas capturas de ecrã seguintes:

![Editar script JSON para a atividade de executar o pacote do SSIS](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters.png)

![Adicionar parâmetros para a atividade de executar o pacote do SSIS](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters2.png)

![Adicionar parâmetros para a atividade de executar o pacote do SSIS](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters2.png)

### <a name="run-the-pipeline"></a>Executar o pipeline
Nesta secção, acionar uma execução de pipeline e, em seguida, monitorizá-lo. 

1. Para acionar uma execução de pipeline, clique em **acionador** na barra de ferramentas e clique em **acionar agora**. 

    ![Acionar agora](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Na janela **Executar Pipeline**, selecione **Concluir**. 

### <a name="monitor-the-pipeline"></a>Monitorizar o pipeline

1. Mude para o separador **Monitorizar**, no lado esquerdo. Verá a execução do pipeline e o respetivo estado, juntamente com outras informações (por exemplo, a hora de início da execução). Para atualizar a vista, clique em **Atualizar**.

    ![Execuções de pipeline](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Clique na ligação **Ver Execuções de Atividades**, na coluna **Ações**. Pode ver apenas uma atividade executar conforme o pipeline tem apenas uma atividade (atividade a executar o pacote do SSIS).

    ![Execuções de atividade](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Pode executar o seguinte procedimento **consulta** contra o SSISDB de base de dados no seu servidor SQL do Azure para verificar se o pacote executado. 

    ```sql
    select * from catalog.executions
    ```

    ![Certifique-se de execuções de pacote](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. Também pode obter o ID de execução do SSISDB a partir da saída da execução de atividade do pipeline e utilize o ID para verificar os registos de execução mais abrangentes e mensagens de erro no SSMS.

    ![Obter o ID de execução.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>O pipeline com um acionador de agenda

Também pode criar um acionador agendado para o seu pipeline, para que o pipeline é executado com base numa agenda (hora a hora, diária, etc.). Por exemplo, veja [criar uma fábrica de dados - IU do Data Factory](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>Executar um pacote com o PowerShell
Nesta secção, utilizar o Azure PowerShell para criar um pipeline do Data Factory com uma atividade de executar o pacote do SSIS que executa um pacote do SSIS. 

Instale os módulos do Azure PowerShell mais recentes ao seguir as instruções em [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps). 

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados
Pode utilizar a fábrica de dados mesmo que tenha o IR Azure-SSIS ou criar uma fábrica de dados separado. O procedimento seguinte fornece passos para criar uma fábrica de dados. Criar um pipeline com uma atividade de executar o pacote do SSIS nesta fábrica de dados. A atividade de executar o pacote do SSIS é executado o pacote do SSIS. 

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
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName `
                                            -Location $ResGrp.Location `
                                            -Name $dataFactoryName 
    ```

Tenha em atenção os seguintes pontos:

* O nome do Azure Data Factory deve ser globalmente exclusivo. Se receber o erro seguinte, altere o nome e tente novamente.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Para criar instâncias do Data Factory, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ser um membro das funções **contribuidor** ou **proprietário**, ou um **administrador** da subscrição do Azure.
* Para obter uma lista de regiões do Azure em que o Data Factory está atualmente disponível, selecione as regiões que lhe interessam na página seguinte e, em seguida, expanda **Analytics** para localizar **Data Factory**: [Produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/). Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras regiões.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Criar um pipeline com uma atividade de executar o pacote do SSIS 
Neste passo, vai criar um pipeline com uma atividade de executar o pacote do SSIS. A atividade executa seu pacote do SSIS. 

1. Crie um ficheiro JSON com o nome **RunSSISPackagePipeline.json** no **C:\ADF\RunSSISPackage** pasta com conteúdo semelhante ao seguinte exemplo:

    > [!IMPORTANT]
    > Substitua os nomes de objetos, descrições e caminhos, propriedade e valores de parâmetros, as palavras-passe e outros valores das variáveis antes de guardar o ficheiro. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [{
                "name": "mySSISActivity",
                "description": "My SSIS package/activity description",
                "type": "ExecuteSSISPackage",
                "typeProperties": {
                    "connectVia": {
                        "referenceName": "myAzureSSISIR",
                        "type": "IntegrationRuntimeReference"
                    },
                    "runtime": "x64",
                    "loggingLevel": "Basic",
                    "packageLocation": {
                        "packagePath": "FolderName/ProjectName/PackageName.dtsx"            
                    },
                    "environmentPath":   "FolderName/EnvironmentName",
                    "projectParameters": {
                        "project_param_1": {
                            "value": "123"
                        }
                    },
                    "packageParameters": {
                        "package_param_1": {
                            "value": "345"
                        }
                    },
                    "projectConnectionManagers": {
                        "MyAdonetCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "abc"
                                }
                            }
                        }
                    },
                    "packageConnectionManagers": {
                        "MyOledbCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "def"
                                }
                            }
                        }
                    },
                    "propertyOverrides": {
                        "\\PackageName.dtsx\\MaxConcurrentExecutables ": {
                            "value": 8,
                            "isSensitive": false
                        }
                    }
                },
                "policy": {
                    "timeout": "0.01:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30
                }
            }]
        }
    }
    ```

2.  No Azure PowerShell, mude para o `C:\ADF\RunSSISPackage` pasta.

3. Para criar o pipeline **RunSSISPackagePipeline**, execute o **Set-AzureRmDataFactoryV2Pipeline** cmdlet.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                   -ResourceGroupName $ResGrp.ResourceGroupName `
                                                   -Name "RunSSISPackagePipeline"
                                                   -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Segue-se o resultado do exemplo:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="run-the-pipeline"></a>Executar o pipeline
Utilize o **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet para executar o pipeline. O cmdlet devolve o ID de execução do pipeline para monitorização futura.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>Monitorizar o pipeline

Execute o seguinte script do PowerShell para verificar continuamente o estado de execução do pipeline até que este termine de copiar os dados. Copie/cole o seguinte script na janela do PowerShell e prima ENTER. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

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

Também pode monitorizar o pipeline com o portal do Azure. Para obter instruções passo a passo, consulte [monitorizar o pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>O pipeline com um acionador de agenda
No passo anterior, executou a pipeline a pedido. Também pode criar um acionador de agenda para executar o pipeline com base numa agenda (hora a hora, diária, etc.).

1. Crie um ficheiro JSON com o nome **MyTrigger.json** na **C:\ADF\RunSSISPackage** pasta com o seguinte conteúdo: 

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
2. Na **do Azure PowerShell**, mude para o **C:\ADF\RunSSISPackage** pasta.
3. Executar o **Set-AzureRmDataFactoryV2Trigger** cmdlet, que cria o acionador. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                    -DataFactoryName $DataFactory.DataFactoryName `
                                    -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Por predefinição, o acionador está no estado parado. Inicie o acionador ao executar o **Start-AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                      -DataFactoryName $DataFactory.DataFactoryName `
                                      -Name "MyTrigger" 
    ```
5. Confirme que o acionador é iniciado ao executar o **Get-AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                    -DataFactoryName $DataFactoryName `
                                    -Name "MyTrigger"     
    ```    
6. Após a hora seguinte, execute o seguinte comando. Por exemplo, se a hora atual é de 3:25 PM UTC, execute o comando 4 PM UTC. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                       -DataFactoryName $DataFactoryName `
                                       -TriggerName "MyTrigger" `
                                       -TriggerRunStartedAfter "2017-12-06" `
                                       -TriggerRunStartedBefore "2017-12-09"
    ```

    Pode executar a consulta seguinte na base de dados SSISDB no seu servidor SQL do Azure para verificar se o pacote executado. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Passos Seguintes
Consulte a seguinte mensagem de blogue:
-   [Modernize e expandir os fluxos de trabalho ETL/ELT com atividades SSIS no ADF pipelines](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
