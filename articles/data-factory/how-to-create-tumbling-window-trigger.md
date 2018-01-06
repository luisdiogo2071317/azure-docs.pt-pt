---
title: Como criar os acionadores de janela em cascata no Azure Data Factory | Microsoft Docs
description: "Saiba como criar um acionador no Azure Data Factory que é executado um pipeline numa janela em cascata."
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
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: a3b056ae4bb4eda26fec58ca3b6bed7f0744e36e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Como criar um acionador que é executado um pipeline numa janela em cascata
Este artigo fornece os passos para criar, iniciar e monitorizar um acionador de janela em cascata. Para obter informações gerais sobre acionadores e os tipos de suportamos, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Acionadores de janela em cascata são um tipo de accionador que é acionado num intervalo de tempo periódicas a partir de uma hora de início especificada, mantendo o estado. Em cascata windows são uma série de intervalos de tempo de tamanho fixo, sem sobreposição e contíguos. Um acionador de janela em cascata tem uma relação de 1:1, com um pipeline e só pode referenciar um pipeline único.

## <a name="tumbling-window-trigger-type-properties"></a>Em cascata as propriedades de tipo de Acionador de janela
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count":  <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "parameter1": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter2": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
                }
            }
      }    
}
```  

A tabela seguinte fornece uma descrição geral de alto nível dos elementos principais relacionadas com a periodicidade e agendamento de um acionador de janela em cascata.

| **Nome JSON** | **Descrição** | **Valores permitidos** | **Necessário** |
|:--- |:--- |:--- |:--- |
| **tipo** | Tipo do acionador. Isto é fixado como "TumblingWindowTrigger." | Cadeia | Sim |
| **runtimeState** | <readOnly>Os valores possíveis: Foi iniciado, parado, desativada | Cadeia | Sim, só de leitura |
| **frequência** |O *frequência* cadeia que representa a unidade de frequência com que o acionador voltar a ocorrer. Os valores suportados são "minutos" e "hora." Se a hora de início tem partes de data que são mais granulares do que a frequência, serão levados em consideração para os limites de janela de computação. Para ex: se a frequência é de hora a hora e a hora de início é 2016-04-01T10:10:10Z, a primeira janela é (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z.)  | Cadeia. Os tipos suportados "minutos", "hora" | Sim |
| **intervalo** |O *intervalo* é um número inteiro positivo e indica que o intervalo para o *frequência* que determina a frequência de acionamento será executado. Por exemplo, se *intervalo* é 3 e *frequência* é "horas", o acionador voltar a ocorrer a cada 3 horas. | Número inteiro | Sim |
| **startTime**|*startTime* é uma data / hora. *startTime* é a primeira ocorrência e pode pertencer ao passado. O primeiro intervalo de Acionador será (startTime, startTime + intervalo). | DateTime | Sim |
| **endTime**|*endTime* é uma data / hora. *endTime* é a última ocorrência e pode pertencer ao passado. | DateTime | Sim |
| **atraso** | Especifique o atraso antes do processamento de dados do início da janela. O pipeline executar é iniciado depois do tempo de execução esperado + atraso. Atraso define a forma como período de tempo o acionador aguarda vencido tempo antes de acionar executar de novo. Não alterar a hora de início da janela. | TimeSpan (exemplo: 00:10:00, implica atraso de 10 minutos) |  Não. Predefinição é "00: 00:00" |
| **simultaneidade máx.** | Número de execuções de Acionador simultâneas que são desencadeado para windows que estão prontos. Exemplo: se estamos a tentar preenchimento hora a hora de ontem, que será 24 windows. Se simultaneidade = 10, acionador eventos são desencadeados apenas para o primeiro 10 do windows (00:00-01:00 - 10-09:00:00). Depois de concluídas as primeiras 10 execuções de pipeline accionadas, execuções de Acionador são desencadeadas para o seguinte 10 (10:00-11:00-19:00-20:00). Continuar com o exemplo de concorrência = 10, se existirem 10 windows estiver prontos, será 10 execuções do pipeline. Se existir apenas 1 janela pronto, só será 1 execução de pipeline. | Número inteiro | Sim. Valores possíveis 1 a 50 |
| **retryPolicy: contagem** | O número de tentativas antes da execução de pipeline está marcado como "Falha"  | Número inteiro |  Não. Predefinição é 0 tentativas |
| **retryPolicy: intervalInSeconds** | O atraso entre as tentativas de repetição em segundos | Número inteiro |  Não. Predefinição é 30 segundos |

### <a name="using-system-variables-windowstart-and-windowend"></a>Utilizando variáveis de sistema: WindowStart e WindowEnd

Se pretender utilizar WindowStart e WindowEnd do acionador de janela em cascata na sua **pipeline** definição (ou seja, para a parte de uma consulta), tem de transmitir as variáveis como parâmetros para o pipeline de **acionador**definição, desta forma:
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "MyWindowStart": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "MyWindowEnd": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    }
                }
            }
      }    
}
```  

Em seguida, na definição do pipeline, para utilizar os valores WindowStart e WindowEnd, utilize os parâmetros em conformidade de "MyWindowStart" e "MyWindowEnd"

### <a name="notes-on-backfill"></a>Notas sobre o preenchimento
Quando existem várias janelas para execução (esp. num cenário de preenchimento), a ordem de execução do windows é determinista e será intervalos mais antigos a mais recente. Não é possível alterar este comportamento a partir de agora.

### <a name="updating-an-existing-triggerresource"></a>Atualizar um TriggerResource existente
* Se a frequência (ou tamanho da janela) do acionador for alterado, o estado do windows já processado será *não* ser reposto. O acionador continuará acionadas para os windows a partir do último que foi executado com o novo tamanho de janela.
* Se a hora de fim de Acionador alterações (adicionadas ou atualizadas), o estado do windows já processada será *não* ser reposto. O acionador simplesmente respeitará a hora de fim de novo. Se a hora de fim antes do windows já foi executado, irá parar o acionador. Caso contrário, irá parar quando for detetada a nova hora de fim.

## <a name="sample-using-azure-powershell"></a>Exemplo com o Azure PowerShell
Esta secção mostra como utilizar o Azure PowerShell para criar, iniciar e monitorizar um acionador.

1. Crie um ficheiro JSON com o nome MyTrigger.json na pasta C:\ADFv2QuickStartPSH\ com o seguinte conteúdo:

   > [!IMPORTANT]
   > Definir **startTime** para a hora UTC atual e **endTime** para uma hora anteriores a UTC atual do tempo antes de guardar o ficheiro JSON.

    ```json   
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```  
2. Criar um acionador, utilizando o **conjunto AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    
3. Confirm that the status of the trigger is **Stopped** by using the **Get-AzureRmDataFactoryV2Trigger** cmdlet.

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

## <a name="next-steps"></a>Passos Seguintes
Para obter informações detalhadas sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md#triggers).
