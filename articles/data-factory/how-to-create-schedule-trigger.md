---
title: Criar os acionadores de agenda no Azure Data Factory | Microsoft Docs
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
ms.date: 01/23/2018
ms.author: shlo
ms.openlocfilehash: 51e2dddbe66ca372d89fc8efeb24bdab9fe6a442
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Criar um acionador que executa um pipeline com base numa agenda
Este artigo fornece informações sobre o acionador de agenda e os passos para criar, iniciar e monitorizar um acionador de agenda. Para outros tipos de acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md).

Ao criar um acionador de agendamento, especifique uma agenda (data, periodicidade de início, terminar data etc.) para o acionador e associar com um pipeline. Os pipelines e os acionadores têm uma relação muitos para muitos. Múltiplos acionadores podem arrancar um pipeline individual. Um acionador único pode arrancar vários pipelines.

> [!NOTE]
> Este artigo aplica-se ao Azure Data Factory versão 2, que está atualmente em pré-visualização. Se estiver a utilizar o Azure Data Factory versão 1, que é geralmente disponível (DG), consulte [introdução ao Azure Data Factory versão 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

As secções seguintes fornecem passos para criar um acionador de agenda de formas diferentes. 

## <a name="data-factory-ui"></a>IU do Data Factory
Pode criar um **acionador de agenda** agendar um pipeline para executar periodicamente (hora a hora, diariamente, etc.). 

> [!NOTE]
> Para instruções completas de criação de um pipeline e um acionador de agendamento, associar o acionador de pipeline, executar e monitorizar o pipeline, consulte [início rápido: criar uma fábrica de dados utilizando a IU da fábrica de dados](quickstart-create-data-factory-portal.md).

1. Mude para o separador **Editar**. 

    ![Mudar para o separador Editar](./media/how-to-create-schedule-trigger/switch-edit-tab.png)
1. Clique em **Acionador**, no menu, e clique em **Novo/Editar**. 

    ![Menu Novo acionador](./media/how-to-create-schedule-trigger/new-trigger-menu.png)
2. Na página **Adicionar Acionadores**, clique em **Escolher acionador...** e clique em **Novo**. 

    ![Adicionar acionadores - acionador novo](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)
3. No **novo acionador** página, efetue os seguintes passos: 

    1. Confirme que **agenda** está selecionado para **tipo**. 
    2. Especifique a datetime de início do acionador para **data de início (UTC)**. Está definido para o atual datetime por predefinição. 
    3. Especifique **periodicidade** para o acionador. Selecione um dos valores da lista pendente (cada minuto, hora a hora, diariamente, semanalmente e mensais). Introduza o multiplicador na caixa de texto. Por exemplo, se quiser acionar a executar uma vez para cada 15 minutos, selecione **minuto cada**e introduza **15** na caixa de texto. 
    4. Para o **final** campo, se não pretender especificar um datetime de fim para o acionador, selecione **não final**. Para especificar uma extremidade data hora, selecione **no data**, especifique o datetime de fim e clique em **aplicar**. Nenhuma execução de pipeline tem custos associados. Se estiver a testar, pode pretender certificar-se de que o pipeline é acionado apenas duas vezes. No entanto, certifique-se de que há tempo suficiente para o pipeline ser executado entre a hora de publicação e a hora de fim. O acionador só entra em vigor depois de publicar a solução no Data Factory e não quando guarda o acionador na IU.

        ![Definições do acionador](./media/how-to-create-schedule-trigger/trigger-settings.png)
4. No **novo acionador** janela, verifique o **ativado** opção e clique em **seguinte**. Pode utilizar esta caixa de verificação para desativar o acionador mais tarde. 

    ![Definições do acionador - botão seguinte](./media/how-to-create-schedule-trigger/trigger-settings-next.png)
5. Na página **Novo Acionador**, reveja a mensagem de aviso e clique em **Concluir**.

    ![Definições do acionador - botão concluir](./media/how-to-create-schedule-trigger/new-trigger-finish.png)
6. Clique em **Publicar** para publicar as alterações no Data Factory. Depois de publicar as alterações à fábrica de dados, o acionador não inicia a acionar a execução de pipeline. 

    ![Botão Publicar](./media/how-to-create-schedule-trigger/publish-2.png)
8. Mude para o separador **Monitorizar**, no lado esquerdo. Clique em **Atualizar** para atualizar a lista. Pode ver que o pipeline é executado accionado pelo acionador agendado. Repare nos valores na coluna **Acionado Por**. Se utilizar **acionador agora** opção, pode ver o acionador manual executar na lista. 

    ![Monitorizar execuções acionadas](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)
9. Clique na seta para baixo junto a **Execuções do Pipeline** para mudar para a vista **Execuções Acionadas**. 

    ![Monitorizar execuções acionadas](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell
Esta secção mostra como utilizar o Azure PowerShell para criar, iniciar e monitorizar um acionador de agenda. Para ver este trabalho de exemplo, primeiro percorrer o [início rápido: criar uma fábrica de dados utilizando o Azure PowerShell](quickstart-create-data-factory-powershell.md). Em seguida, adicione o seguinte código ao método principal, que cria e inicia um acionador de agenda que é executado a cada 15 minutos. O acionador está associado a um pipeline com o nome **Adfv2QuickStartPipeline** que criar como parte do início rápido.

1. Crie um ficheiro JSON com o nome **MyTrigger.json** na pasta C:\ADFv2QuickStartPSH\ com o seguinte conteúdo:

    > [!IMPORTANT]
    > Antes de guardar o ficheiro JSON, defina o valor da **startTime** elemento para a hora UTC atual. Defina o valor da **endTime** elemento a uma hora posterior à hora UTC atual.

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
    - O **tipo** elemento do acionador está definido como "ScheduleTrigger."
    - O **frequência** elemento é definido como "Minutos" e o **intervalo** elemento é definido como 15. Por conseguinte, o acionador é executado o pipeline entre os tempos de início e de fim a cada 15 minutos.
    - O **endTime** elemento é uma hora após o valor da **startTime** elemento. Por conseguinte, o acionador é executado o pipeline de 15 minutos, 30 minutos e 45 minutos após a hora de início. Não se esqueça de atualizar a hora de início para o atual a hora UTC e a hora de fim para uma hora posterior à hora de início. 
    - O acionador está associado a **Adfv2QuickStartPipeline** pipeline. Para associar várias pipelines com um acionador, adicione mais **pipelineReference** secções.
    - O pipeline no início rápido, demora dois **parâmetros** valores: **inputPath** e **outputPath**. Por conseguinte, a passagem dos valores para estes parâmetros de Acionador.

2. Criar um acionador, utilizando o **conjunto AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

3. Confirme que o estado do acionador é **parado** utilizando o **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Inicie o acionador utilizando o **início AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Confirme que o estado do acionador é **iniciado** utilizando o **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6.  Obter o acionador é executada no Azure PowerShell, utilizando o **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Para obter informações sobre a execução do acionador, execute o seguinte comando periodicamente. Atualização do **TriggerRunStartedAfter** e **TriggerRunStartedBefore** valores para fazer corresponder os valores existentes na sua definição do acionador:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Para monitorizar o acionador é executado e o pipeline é executado no portal do Azure, consulte [monitorizar o pipeline é executado](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="net-sdk"></a>SDK .NET
Esta secção mostra como utilizar o SDK .NET para criar, iniciar e monitorizar um acionador. Para ver este trabalho de exemplo, primeiro percorrer o [início rápido: criar uma fábrica de dados utilizando o SDK .NET](quickstart-create-data-factory-dot-net.md). Em seguida, adicione o seguinte código ao método principal, que cria e inicia um acionador de agenda que é executado a cada 15 minutos. O acionador está associado a um pipeline com o nome **Adfv2QuickStartPipeline** que criar como parte do início rápido.

Para criar e iniciar um acionador de agenda que é executado a cada 15 minutos, adicione o seguinte código ao método principal:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Para monitorizar um acionador em execução, adicione o seguinte código antes do último `Console.WriteLine` instrução no exemplo:

```csharp
            // Check that the trigger runs every 15 minutes
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

Para monitorizar o acionador é executado e o pipeline é executado no portal do Azure, consulte [monitorizar o pipeline é executado](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="python-sdk"></a>SDK Python
Esta secção mostra como utilizar o SDK Python para criar, iniciar e monitorizar um acionador. Para ver este trabalho de exemplo, primeiro percorrer o [início rápido: criar uma fábrica de dados utilizando o SDK Python](quickstart-create-data-factory-python.md). Em seguida, adicione o bloco de código seguinte após o bloco de código "monitorizar o pipeline de execução" no script de Python. Este código cria um acionador de agenda que é executado a cada 15 minutos, entre o início especificado e a hora de fim. Atualização do **start_time** variável para a hora UTC atual e o **end_time** variável para uma hora posterior à hora UTC atual.

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

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Para monitorizar o acionador é executado e o pipeline é executado no portal do Azure, consulte [monitorizar o pipeline é executado](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Modelo Azure Resource Manager
Pode utilizar um modelo Azure Resource Manager para criar um acionador. Para obter instruções passo a passo, consulte [criar um Azure data factory utilizando um modelo do Resource Manager](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>A hora de início de Acionador de passar para um pipeline
O Azure Data Factory versão 1 suporta ler ou escrever dados particionados utilizando as variáveis de sistema: **SliceStart**, **SliceEnd**, **WindowStart**e **WindowEnd**. No Azure Data Factory versão 2, pode conseguir este comportamento, utilizando um parâmetro de pipeline. A hora de início e a hora agendada para o acionador estão definidas como o valor para o parâmetro de pipeline. No exemplo seguinte, a hora agendada para o acionador é transmitida como um valor para o pipeline **scheduledRunTime** parâmetro:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    

Para obter mais informações, consulte as instruções no [como de leitura ou escrita particionada dados](how-to-read-write-partitioned-data.md).

## <a name="json-schema"></a>JSON schema
A seguinte definição JSON mostra como criar um acionador de agenda com o agendamento e de periodicidade:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
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
>  O **parâmetros** propriedade é uma propriedade obrigatória do **pipelines** elemento. Se o pipeline não assume quaisquer parâmetros, tem de incluir uma definição de JSON vazia para o **parâmetros** propriedade.


### <a name="schema-overview"></a>Descrição geral de esquema
A tabela seguinte fornece uma descrição dos elementos de esquema principais que estão relacionadas com a periodicidade e agendamento de um acionador de alto nível:

| Propriedade JSON | Descrição |
|:--- |:--- |
| **startTime** | Um valor de data / hora. Para agendas simples, o valor da **startTime** propriedade aplica-se a primeira ocorrência. Para agendas complexas, o acionador inicia não define que o especificado **startTime** valor. |
| **endTime** | A data de fim e a hora para o acionador. Não executar o acionador após a data de fim especificado e a hora. O valor da propriedade não pode estar no passado. Esta propriedade é opcional. |
| **timeZone** | O fuso horário. Atualmente, apenas o UTC fuso horário é suportado. |
| **recurrence** | Um objeto de periodicidade que especifica as regras de periodicidade para o acionador. O objeto de periodicidade suporta o **frequência**, **interva**l, **endTime**, **contagem**, e **agenda**elementos. Quando um objeto de periodicidade estiver definido, o **frequência** elemento é necessário. Os outros elementos do objeto de periodicidade são opcionais. |
| **frequency** | A unidade da frequência com que o acionador voltar a ocorrer. Os valores suportados incluem "minutos", "horas", "dia", "semanas" e "mês". |
| **interval** | Um número inteiro positivo que indica o intervalo para o **frequência** valor, que determina a frequência de execução do acionador. Por exemplo, se o **intervalo** é 3 e o **frequência** é "semanas", o acionador voltar a ocorrer todos os 3 semanas. |
| **schedule** | A agenda de periodicidade para o acionador. Um acionador com uma determinada **frequência** valor altera a periodicidade com base numa agenda de periodicidade. O **agenda** propriedade contém as modificações para a periodicidade que se baseiam em minutos, horas, dias da semana, os dias do mês e o número da semana.


### <a name="schema-defaults-limits-and-examples"></a>As predefinições de esquema, os limites e exemplos

| Propriedade JSON | Tipo | Necessário | Valor predefinido | Valores válidos | Exemplo |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Cadeia | Sim | Nenhum | Datas-Horas ISO 8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Objeto | Sim | Nenhum | Objeto de periodicidade | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Número | Não | 1 | 1 a 1000 | `"interval":10` |
| **endTime** | Cadeia | Sim | Nenhum | Um valor de data / hora que representa uma data futura. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Objeto | Não | Nenhum | Objeto da agenda | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>propriedade de startTime
A tabela seguinte mostra como as **startTime** propriedade controla um acionador executar:

| valor de startTime | Periodicidade sem agenda | Periodicidade com agenda |
|:--- |:--- |:--- |
| Hora de início no passado | Calcula o tempo de execução futuro primeiro após a hora de início e executa a essa hora.<br/><br/>Executa as execuções subsequentes com base em calcular a partir do último tempo de execução.<br/><br/>Veja o exemplo a seguir à tabela. | O acionador inicia _não sooner que_ a hora de início especificada. A primeira ocorrência é baseada na agenda que é calculada a partir da hora de início.<br/><br/>Executa as execuções subsequentes com base na agenda de periodicidade. |
| Hora de início no futuro ou no presente | É executada uma vez à hora de início especificada.<br/><br/>Executa as execuções subsequentes com base em calcular a partir do último tempo de execução. | O acionador inicia _não mais cedo_ especificado que a hora de início. A primeira ocorrência é baseada na agenda que é calculada a partir da hora de início.<br/><br/>Executa as execuções subsequentes com base na agenda de periodicidade. |

Vamos ver um exemplo do que acontece quando a hora de início é no passado, com uma periodicidade, mas nenhuma agenda. Partem do princípio de que a hora atual é `2017-04-08 13:00`, a hora de início é `2017-04-07 14:00`, e a periodicidade é dois dias. (O **periodicidade** valor é definido pela definição do **frequência** propriedade como "dia" e o **intervalo** propriedade para 2.) Tenha em atenção que o **startTime** valor está no passado e ocorre antes da hora atual.

Nestas condições, a primeira execução é em `2017-04-09 at 14:00`. O motor do Scheduler calcula as ocorrências de execução a partir da hora de início. Quaisquer instâncias no passado são eliminadas. O motor utiliza a instância seguinte que ocorre no futuro. Neste cenário, a hora de início é `2017-04-07 at 2:00pm`, pelo que a instância seguinte é dois dias a esse tempo, o que é `2017-04-09 at 2:00pm`.

O primeiro tempo de execução é o mesmo mesmo quando o **startTime** valor é `2017-04-05 14:00` ou `2017-04-01 14:00`. Após a primeira execução, as execuções subsequentes são calculadas utilizando a agenda. Por conseguinte, as execuções subsequentes são em `2017-04-11 at 2:00pm`, em seguida, `2017-04-13 at 2:00pm`, em seguida, `2017-04-15 at 2:00pm`, e assim sucessivamente.

Por fim, quando as horas ou minutos não se encontram definidos na agenda para um acionador, as horas ou minutos da primeira execução são utilizados como as predefinições.

### <a name="schedule-property"></a>propriedade de agenda
Um lado, a utilização de uma agenda pode limitar o número de execuções de Acionador. Por exemplo, se um acionador com uma frequência mensal estiver agendado para ser executado apenas no dia 31, o acionador é executado apenas nos meses que tenham 31 dias.

Por outro lado, as agendas também podem expandir o número de execuções de acionadores. Por exemplo, um acionador com uma frequência de mensal tem agendadas para serem executadas em dias do mês, 1 e 2, é executada nos 1ª e 2nd dias do mês, em vez de uma vez por mês.

Se vários **agenda** elementos especificados, a ordem de avaliação é desde a maior para a definição de agenda mais pequena. A avaliação começa por dia da semana número e, em seguida, mês dia de semana, hora e por fim, minutos.

A tabela seguinte descreve o **agenda** elementos detalhadamente:


| Elemento JSON | Descrição | Valores válidos |
|:--- |:--- |:--- |
| **minutos** | Minutos da hora em que o acionador é executado. | <ul><li>Número inteiro</li><li>Matriz de números inteiros</li></ul>
| **horas** | Horas do dia em que o acionador é executado. | <ul><li>Número inteiro</li><li>Matriz de números inteiros</li></ul> |
| **weekDays** | Dias da semana em que executa o acionador. O valor pode ser especificado com uma frequência semanal apenas. | <ul><li>Segunda, Terça, quarta-feira, Quinta-feira, sexta-feira, Sábado, Domingo</li><li>Matriz de valores de dia (tamanho máximo de matriz é 7)</li><li>Dia valores não são sensíveis a maiúsculas</li></ul> |
| **monthlyOccurrences** | Dias do mês em que executa o acionador. O valor pode ser especificado com uma frequência mensal apenas. | <ul><li>Matriz de **monthlyOccurence** objetos: `{ "day": day,  "occurrence": occurence }`.</li><li>O **dia** atributo é o dia da semana em que executa o acionador. Por exemplo, um **monthlyOccurrences** propriedade com um **dia** valor `{Sunday}` significa cada Domingo do mês. O **dia** atributo é necessário.</li><li>O **ocorrência** atributo é a ocorrência de especificado **dia** durante o mês. Por exemplo, um **monthlyOccurrences** propriedade com **dia** e **ocorrência** valores de `{Sunday, -1}` significa Domingo último do mês. O **ocorrência** atributo é opcional.</li></ul> |
| **dias do mês** | Dia do mês em que executa o acionador. O valor pode ser especificado com uma frequência mensal apenas. | <ul><li>Qualquer valor <= -1 e >= -31</li><li>Qualquer valor >= 1 e <= 31</li><li>Matriz de valores</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Exemplos de agendas de periodicidade do acionador
Esta secção fornece exemplos de agendas de periodicidade e centra-se no **agenda** objeto e os respetivos elementos.

Os exemplos partem do princípio de que o **intervalo** valor é 1 e essa o **frequência** valor é correto, de acordo com a definição de agenda. Por exemplo, não pode ter um **frequência** valor de "dia" e tem também uma modificação de "dias do mês" no **agenda** objeto. Restrições de como estas são mencionadas na tabela na secção anterior.

| Exemplo | Descrição |
|:--- |:--- |
| `{"hours":[5]}` | Execute todos os dias às 5:00. |
| `{"minutes":[15], "hours":[5]}` | Execute 5:15 AM todos os dias. |
| `{"minutes":[15], "hours":[5,17]}` | Execute 5 15 da Manhã e as 17:15:00 todos os dias. |
| `{"minutes":[15,45], "hours":[5,17]}` | Executar em 5:15 AM, 5:45 AM, as 17:15:00 e as 17:45:00 todos os dias. |
| `{"minutes":[0,15,30,45]}` | Execute a cada 15 minutos. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Execute a cada hora. Este acionador é executado de hora em hora. Os minutos são controlados mediante a **startTime** valor, quando é especificado um valor. Se um valor não especificado, os minutos são controlados pela hora de criação. Por exemplo, se a hora de início ou a hora de criação (que se aplica) for 12:25 PM, o acionador é executada às 00:25, 01:25, 02:25,... e 23:25.<br/><br/>Esta agenda é equivalente ao facto de ter um acionador com um **frequência** valor de "hora," um **intervalo** valor de 1 e não **agenda**.  Este agendamento pode ser utilizado com diferentes **frequência** e **intervalo** valores para criar outros acionadores. Por exemplo, quando o **frequência** valor é "mês", executa a agenda uma só vez por mês, em vez de todos os dias, quando o **frequência** valor é "dia". |
| `{"minutes":[0]}` | Execute a cada hora em hora. Este acionador é executada a cada hora a hora de início às 12:00, 1:00:00, 2 horas da Manhã, e assim sucessivamente.<br/><br/>Esta agenda é equivalente a um acionador com um **frequência** valor de "hora" e um **startTime** valor de zero minutos ou não **agenda** mas um **frequência**  valor de "dia". Se o **frequência** valor for "semana" ou "mês", a agenda executa um dia, uma semana ou um dia, mês, apenas, respetivamente. |
| `{"minutes":[15]}` | Execute em 15 minutos decorridos desde a cada hora. Este acionador é executada a cada hora em 15 minutos decorridos desde a hora de início às 00:15, 1:15:00, 2:15 AM e outros e terminado em 23:15:00. |
| `{"hours":[17], "weekDays":["saturday"]}` | Execute as 17:00:00 em Saturdays todas as semanas. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Execute as 17:00:00 na segunda-feira, Quartas e sexta-feira todas as semanas. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Execute as 17:15:00 e as 17:45:00 na segunda-feira, Quartas e sexta-feira todas as semanas. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Execute a cada 15 minutos em dias da semana. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Execute a cada 15 minutos em dias da semana entre as 09:00:00 e 4:45 PM. |
| `{"weekDays":["tuesday", "thursday"]}` | Execute em executado às Terças e às quintas-o do momento de início especificada. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Executar às 6:00 do dia de cada mês 28th (assumindo um **frequência** valor de "mês"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Execute às 6:00:00 no último dia do mês. Para executar um acionador no último dia de um mês, utilize -1 em vez de dia 28, 29, 30 ou 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Execute às 6:00:00 no primeiro e último dia de cada mês. |
| `{monthDays":[1,14]}` | Execute no primeiro e 14 dia de cada mês da hora de início especificada. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Execute o primeiro sexta-feira de cada mês às 5:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Execute o primeiro sexta-feira de cada mês da hora de início especificada. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Execute o terceiro sexta-feira do fim do mês, todos os meses, a hora de início especificada. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Execute na primeira e última sexta-feira de cada mês às 5 15 da Manhã. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Execute na primeira e última sexta-feira de cada mês da hora de início especificada. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Execute o quinto sexta-feira de cada mês da hora de início especificada. Quando não houver nenhuma sexta-feira quinta num mês, o pipeline não é executado, uma vez que tem agendado para ser executado apenas em sextas quinta. Para executar o acionador no último ocorrer sexta-feira do mês, considere a utilização de -1 em vez de 5 para o **ocorrência** valor. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Executado a cada 15 minutos na última sexta-feira do mês. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Executar em 5:15 AM, 5:45 AM, as 17:15:00 e 5:45 PM o terceiro quarta-feira de cada mês. |


## <a name="next-steps"></a>Passos Seguintes
Para obter informações detalhadas sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md#triggers).
