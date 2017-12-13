---
title: Como criar acionadores no Azure Data Factory | Microsoft Docs
description: Saiba como criar um acionador no Azure Data Factory que executa um pipeline com base numa agenda.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: eed286c01604ab0e9ac2113d56cbce268503668d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Como criar um acionador que executa um pipeline com base numa agenda
Este artigo fornece os passos para criar, iniciar e monitorizar um acionador. Para obter informações concetuais sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell
Esta secção mostra como utilizar o Azure PowerShell para criar, iniciar e monitorizar um acionador. Se pretender ver este trabalho de exemplo, primeiro percorrer o [início rápido: criar uma fábrica de dados com o Azure PowerShell](quickstart-create-data-factory-powershell.md). Em seguida, adicione o seguinte código ao método principal, que cria e inicia um acionador de agenda que é executado a cada 15 minutos. O acionador está associado um pipeline (**Adfv2QuickStartPipeline**) que criar como parte do início rápido.

1. Crie um ficheiro JSON com o nome MyTrigger.json na pasta C:\ADFv2QuickStartPSH\ com o seguinte conteúdo: 

    > [!IMPORTANT]
    > Definir **startTime** para a hora UTC atual e **endTime** para uma hora anteriores a UTC atual do tempo antes de guardar o ficheiro JSON.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```
    
    No fragmento JSON: 
    - O **tipo** do acionador está definido como **ScheduleTrigger**. 
    - O **frequência** está definido como **minuto** e **intervalo** está definido como **15**. Por conseguinte, o acionador é executado o pipeline entre os tempos de início e de fim a cada 15 minutos. 
    - O **endTime** é uma hora após o **startTime**, por isso, o acionador é executado o pipeline de 15 minutos, 30 minutos e, depois da startTime de 45 minutos. Não se esqueça de atualizar a startTime a hora UTC atual e endTime para uma hora anteriores a startTime.  
    - O acionador está associado a **Adfv2QuickStartPipeline** pipeline. Para associar várias pipelines com um acionador, adicione mais **pipelineReference** secções. 
    - O pipeline no início rápido, demora dois **parâmetros**. Por conseguinte, a passagem dos valores para esses parâmetros de Acionador. 
2. Criar um acionador, utilizando o **conjunto AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Confirme que o estado do acionador é **parado** utilizando o **Get-AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
4. Inicie o acionador utilizando o **início AzureRmDataFactoryV2Trigger** cmdlet: 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
5. Confirme que o estado do acionador é **iniciado** utilizando o **Get-AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
6.  Obter execuções de Acionador com o PowerShell, utilizando o **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Para obter informações sobre a execução do acionador, execute o seguinte comando periodicamente: atualização **TriggerRunStartedAfter** e **TriggerRunStartedBefore** valores para fazer corresponder os valores na definição do acionador . 

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```

    Para monitorizar o acionador executa/pipeline executa no portal do Azure, consulte [monitorizar o pipeline é executada](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-net-sdk"></a>Utilizar o .NET SDK
Esta secção mostra como utilizar o .NET SDK para criar, iniciar e monitorizar um acionador. Se pretender ver este código a funcionar, primeiro percorrer o [início rápido para criar uma fábrica de dados com o .NET SDK](quickstart-create-data-factory-dot-net.md). Em seguida, adicione o seguinte código ao método principal, que cria e inicia um acionador de agenda que é executado a cada 15 minutos. O acionador está associado um pipeline (**Adfv2QuickStartPipeline**) que criar como parte do início rápido. 

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method. 
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Para monitorizar um acionador em execução, adicione o seguinte código antes do último `Console.WriteLine` instrução: 

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Para monitorizar o acionador executa/pipeline executa no portal do Azure, consulte [monitorizar o pipeline é executada](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-python-sdk"></a>Utilizar SDK Python
Esta secção mostra como utilizar o Python SDK para criar, iniciar e monitorizar um acionador. Se pretender ver este código a funcionar, primeiro percorrer o [início rápido para criar uma fábrica de dados utilizando o Python SDK](quickstart-create-data-factory-python.md). Em seguida, adicione o bloco de código seguinte após o bloco de código "monitorizar o pipeline de execução" no script de python. Este código cria um acionador de agenda que é executado a cada 15 minutos, entre o início especificado e a hora de fim. Atualize o start_time para a hora UTC atual e end_time para uma hora posterior à hora UTC atual. 

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC') 
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Para monitorizar o acionador executa/pipeline executa no portal do Azure, consulte [monitorizar o pipeline é executada](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-resource-manager-template"></a>Utilize o Gestor de recursos de modelo
Pode utilizar um modelo Azure Resource Manager para criar um acionador. Para obter instruções passo a passo, consulte [criar um Azure data factory com o modelo do Resource Manager](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>A hora de início de Acionador de passar para um pipeline
Na versão 1, o Azure Data Factory suportado ler ou escrever dados particionados utilizando variáveis de sistema do SliceStart/SliceEnd/WindowStart/WindowEnd. Na versão 2, pode conseguir este comportamento, utilizando um parâmetro de pipeline e agendada/hora a hora de início do acionador como um valor do parâmetro. No exemplo seguinte, hora agendada o acionador é transmitida como um valor para o scheduledRunTime de parâmetro do pipeline. 

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Para obter mais informações, consulte [como de leitura ou escrita particionada dados](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Passos seguintes
Para obter informações detalhadas sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md#triggers).