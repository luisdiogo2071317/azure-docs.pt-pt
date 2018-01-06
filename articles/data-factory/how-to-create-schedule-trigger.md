---
title: Como criar os acionadores de agenda no Azure Data Factory | Microsoft Docs
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
ms.openlocfilehash: 6b92e8402d372e29e264dc70b128124973d66bb9
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Como criar um acionador que executa um pipeline com base numa agenda
Este artigo fornece informações sobre o acionador de agenda e os passos para criar, iniciar e monitorizar um acionador. Para outros tipos de acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

### <a name="schedule-trigger-json-definition"></a>Acionador de agenda definição JSON
Quando cria um acionador de agendamento, pode especificar o agendamento e de periodicidade utilizando JSON, conforme mostrado no exemplo nesta secção.

Para que o acionador de agenda iniciar um execução de pipeline, inclua uma referência de pipeline do pipeline específico na definição do acionador. Os pipelines e os acionadores têm uma relação muitos para muitos. Múltiplos acionadores podem arrancar um pipeline individual. Um acionador único pode arrancar vários pipelines.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  A propriedade **parameters** é uma propriedade obrigatória dentro dos **pipelines**. Mesmo que o seu pipeline não assuma nenhum parâmetro, inclua um json vazio para os parâmetros, pois a propriedade tem de existir.


### <a name="overview-schedule-trigger-schema"></a>Descrição geral: Esquema de Acionador de agenda
A tabela seguinte mostra uma descrição geral de alto nível dos principais elementos relacionados com a periodicidade e o agendamento nos acionadores:

Propriedade JSON |     Descrição
------------- | -------------
startTime | startTime é uma Data-Hora. Para agendas simples, startTime é a primeira ocorrência. Para agendas complexas, o acionador não é iniciado antes de startTime.
endTime | Especifica a data-hora de fim do acionador. O acionador não é executado após esta hora. endTime não pode ser uma hora no passado. Esta propriedade é opcional.
timeZone | Atualmente, só é suportado UTC.
recurrence | O objeto de periodicidade especifica as regras de periodicidade do acionador. O objeto de periodicidade suporta os elementos frequência, intervalo, endTime, contagem e agenda. Se a periodicidade for definida, a frequência é necessária; os outros elementos de periodicidade são opcionais.
frequência | Representa a unidade da frequência com que o acionador voltar a ocorrer. Os valores suportados são `minute`, `hour`, `day`, `week`, ou `month`.
intervalo | O intervalo é um número inteiro positivo. Indica o intervalo da frequência que determina quantas vezes o acionador é executado. Por exemplo, se o intervalo for 3 e a frequência "semanal", o acionador repete-se de três em três semanas.
agenda | Os acionadores que tenham uma frequência especificada alteram a repetição com base numa agenda de periodicidade. As agendas contêm modificações baseadas em minutos, horas, dias de semana, dias do mês e número da semana.


### <a name="overview-schedule-trigger-schema-defaults-limits-and-examples"></a>Descrição geral: As predefinições de esquema de Acionador de agendamento, os limites e exemplos

Nome JSON | Tipo de valor | Necessário? | Valor predefinido | Valores válidos | Exemplo
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Cadeia | Sim | Nenhuma | Datas-Horas ISO 8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Objeto | Sim | Nenhum | Objeto de periodicidade | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
intervalo | Número | Não | 1 | 1 a 1000. | ```"interval":10```
endTime | Cadeia | Sim | Nenhum | Valor de data-hora que representa uma hora no futuro | `"endTime" : "2013-02-09T09:30:00-08:00"`
agenda | Objeto | Não | Nenhuma | Objeto da agenda | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Descrição aprofundada: startTime
A tabela seguinte mostra como é que startTime controla a forma como os acionadores são executados:

valor de startTime | Periodicidade sem agenda | Periodicidade com agenda
--------------- | --------------------------- | ------------------------
Hora de início no passado | Calcula a primeira hora de execução no futuro após a hora de início e executa nessa hora.<p>Executa as execuções subsequentes com base na determinação da última hora de execução.</p><p>Veja o exemplo a seguir à tabela.</p> | O acionador _não é acionado antes_ da hora de início especificada. A primeira ocorrência é baseada na agenda calculada a partir da hora de início. <p>Executa as execuções subsequentes com base na agenda de periodicidade.</p>
Hora de início no futuro ou no presente | Executa uma vez na hora de início especificada. <p>Executa as execuções subsequentes com base na determinação da última hora de execução.</p> | O acionador _não é acionado antes_ da hora de início especificada. A primeira ocorrência é baseada na agenda calculada a partir da hora de início.<p>Executa as execuções subsequentes com base na agenda de periodicidade.</p>

Vamos ver um exemplo do que acontece quando startTime está no passado, com periodicidade, mas sem agenda. Partamos do princípio de que a hora atual é `2017-04-08 13:00`, startTime é `2017-04-07 14:00` e a periodicidade é de dois em dois dias (definida com a frequência: dia e intervalo: 2). Repare que startTime está no passado e que ocorre antes da hora atual.

Nestas condições, a primeira execução é em `2017-04-09 at 14:00`. O motor do Scheduler calcula as ocorrências de execução a partir da hora de início. Quaisquer instâncias no passado são eliminadas. O motor utiliza a instância seguinte que ocorre no futuro. Por isso, neste caso, startTime é `2017-04-07 at 2:00pm`, pelo que a instância seguinte é dois dias a contar dessa hora, que é `2017-04-09 at 2:00pm`.

A primeira hora de execução é a mesma, mesmo que startTime seja `2017-04-05 14:00` ou `2017-04-01 14:00`. Após a primeira execução, as execuções subsequentes são calculadas com a agenda. Consequentemente, ocorrem em `2017-04-11 at 2:00pm`, depois em `2017-04-13 at 2:00pm` e, por fim, em `2017-04-15 at 2:00pm`.

Finalmente, se um acionador tiver uma agenda e se não forem definidas as horas e/ou os minutos na mesma, estas são predefinidas para as horas e/ou minutos da primeira execução, respetivamente.

### <a name="deep-dive-schedule"></a>Descrição aprofundada: agenda
Por um lado, as agendas podem limitar o número de execuções de acionadores. Por exemplo, se um acionador com a frequência “mês” tiver uma agenda que só é executada no dia 31, o acionador só é executado nos meses que têm 31 dias.

Por outro lado, as agendas também podem expandir o número de execuções de acionadores. Por exemplo, se um acionador com a frequência “mês” tiver uma agenda que é executada nos dias 1 e 2, o acionador é executado no primeiro e no segundo dias do mês em vez de uma vez por mês.

Se forem especificados vários elementos de agenda, a ordem de avaliação é do maior para o mais pequeno – número da semana, dia do mês, dia da semana, hora e minuto.

A tabela seguinte descreve os elementos de agenda detalhadamente:


Nome JSON | Descrição | Valores válidos
--------- | ----------- | ------------
minutes | Minutos da hora em que o acionador é executado. | <ul><li>Número inteiro</li><li>Matriz de números inteiros</li></ul>
hours | Horas do dia em que o acionador é executado. | <ul><li>Número inteiro</li><li>Matriz de números inteiros</li></ul>
weekDays | Dias da semana em que o acionador é executado. Só podem ser especificados com uma frequência semanal. | <ul><li>Segunda-feira, terça-feira, quarta-feira, quinta-feira, sexta-feira, sábado ou domingo</li><li>Matriz de qualquer um dos valores (tamanho da matriz máximo 7)</li></p>Não sensíveis a maiúsculas e minúsculas</p>
monthlyOccurrences | Determina em que dias do mês o acionador é executado. Só podem ser especificadas com uma frequência semanal. | Matriz de objetos de monthlyOccurence: `{ "day": day,  "occurrence": occurence }`. <p> O dia é o dia da semana em que o acionador é executado; por exemplo, `{Sunday}` é cada domingo do mês. Necessário.<p>A ocorrência é a ocorrência do dia durante o mês; por exemplo, `{Sunday, -1}` é o último domingo do mês. Opcional.
monthDays | Dia do mês em que o acionador é executado. Só podem ser especificadas com uma frequência semanal. | <ul><li>Qualquer valor <= -1 e >= -31</li><li>Qualquer valor >= 1 e <= 31</li><li>Uma matriz de valores</li>


## <a name="examples-recurrence-schedules"></a>Exemplos: agendas de periodicidade
Esta secção mostra exemplos de agendas de periodicidade, que se focam no objeto da agenda e nos subelementos.

As agendas de exemplo partem do princípio de que o intervalo está definido como 1. Além disso, também presumem a frequência certa de acordo com o que está definido na agenda. Por exemplo, não pode utilizar a frequência “dia” e ter a modificação "monthDays" na agenda. Estas restrições são mencionadas na tabela da secção anterior.

Exemplo | Descrição
------- | -----------
`{"hours":[5]}` | É executada às 05:00 Todos os Dias
`{"minutes":[15], "hours":[5]}` | É executada às 05:15 Todos os Dias
`{"minutes":[15], "hours":[5,17]}` | É executada às 05:15 e às 17:15 Todos os Dias
`{"minutes":[15,45], "hours":[5,17]}` | É executada às 05:15, às 05:45, às 17:15 e às 17:45 Todos os Dias
`{"minutes":[0,15,30,45]}` | É executada A Cada 15 Minutos
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | É executada de Hora em Hora. Este acionador é executado de hora em hora. O minuto é controlado por startTime, se for especificado; se não for especificado, é controlado pela hora de criação. Por exemplo, se a hora de início ou a hora de criação (a que se aplicar) for 12:25, o acionador é executado às 00:25, às 01:25, às 02:25,..., às 23:25. A agenda é equivalente a ter um acionador com a frequência de "horas", um intervalo de 1 e nenhuma agenda. A diferença é que esta agenda pode ser utilizada com diferentes frequências e intervalos para criar outros acionadores também. Por exemplo, se a frequência fosse "mês", a agenda seria executada uma só vez por mês, em vez de todos os dias se fosse "dia".
`{"minutes":[0]}` | É executada de hora em hora à hora certa. Este acionador também é executado de hora em hora, mas à hora certa (por exemplo, 00:00, 01:00, 02:00, etc.). Esta definição é equivalente a um acionador com a frequência "horas", startTime com zero minutos e nenhuma agenda se a frequência fosse "dia"; contudo se a frequência fosse “semana” ou “mês”, a agenda só seria executada uma vez por semana ou um dia por mês, respetivamente.
`{"minutes":[15]}` | É executada 15 minutos após cada hora. É executada de hora em hora, com início às 00:15, 01:15, 02:15, etc., e fim às 22:15 e 23:15.
`{"hours":[17], "weekDays":["saturday"]}` | É executada às 17:00 aos sábados todas as semanas
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | É executada às 17:00 à segunda-feira, quarta-feira e sexta-feira todas as semanas
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | É executada às 17:15 e às 17:45 à segunda-feira, quarta-feira e sexta-feira todas as semanas
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | É executada a cada 15 minutos nos dias de semana
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | É executada a cada 15 minutos nos dias de semana entre as 09:00 e as 16:45
`{"weekDays":["tuesday", "thursday"]}` | É executada às terças-feiras e quintas-feiras à hora de início especificada
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | É executada às 06:00 do dia 28 de cada mês (presumindo que a frequência é “mês”).
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | É executada às 06:00 no último dia do mês Se quiser executar um acionador no último dia de um mês, utilize -1 em vez de dia 28, 28, 30 ou 31
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | É executada às 06:00 no primeiro e último dias de todos os meses
`{monthDays":[1,14]}` | É executada no primeiro e no 14.º dia de todos os meses à hora de início especificada
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | É executada na primeira sexta-feira de cada mês às 05:00
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | É executada na primeira sexta-feira de cada mês à hora de início especificada
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | É executada na terceira sexta-feira do fim do mês, de cada mês, à hora de início
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | É executada na primeira e na última sexta-feira de todos os meses às 05:15
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | É executada na primeira e na última sexta-feira de cada mês à hora de início especificada
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | É executada na quinta sexta-feira de cada mês à hora especificada. Se o mês não tiver cinco sextas-feiras, o pipeline não é executado, pois está agendado para ser executado apenas à quinta sexta-feira.  Se quiser executar o acionador na última sexta-feira do mês, considere utilizar -1 em vez de 5 na ocorrência.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | É executada a cada 15 minutos na última sexta-feira do mês
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | É executada às 05:15, às 05:45 AM, às 17:15 e às 17:45 na terceira quarta-feira de cada mês


## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell
Esta secção mostra como utilizar o Azure PowerShell para criar, iniciar e monitorizar um acionador de programador. Se pretender ver este trabalho de exemplo, primeiro percorrer o [início rápido: criar uma fábrica de dados com o Azure PowerShell](quickstart-create-data-factory-powershell.md). Em seguida, adicione o seguinte código ao método principal, que cria e inicia um acionador de agenda que é executado a cada 15 minutos. O acionador está associado um pipeline (**Adfv2QuickStartPipeline**) que criar como parte do início rápido.

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

## <a name="next-steps"></a>Passos Seguintes
Para obter informações detalhadas sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md#triggers).
