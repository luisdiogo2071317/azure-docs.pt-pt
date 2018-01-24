---
title: Criar os acionadores de janela em cascata no Azure Data Factory | Microsoft Docs
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
ms.openlocfilehash: 1f026683ebc9b3d2bc935cd78aa9d16684e7db40
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Criar um acionador que é executado um pipeline numa janela em cascata
Este artigo fornece os passos para criar, iniciar e monitorizar um acionador de janela em cascata. Para obter informações gerais sobre acionadores e os tipos suportados, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Este artigo aplica-se ao Azure Data Factory versão 2, que está atualmente em pré-visualização. Se estiver a utilizar o Azure Data Factory versão 1, que é geralmente disponível (DG), consulte [introdução ao Azure Data Factory versão 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Os acionadores de janela em cascata são um tipo de acionador que é acionado num intervalo de tempo periódico a partir de uma hora de início especificada, mantendo o estado. Em cascata windows são uma série de intervalos de tempo de tamanho fixo, sem sobreposição e contíguos. Um acionador de janela em cascata tem uma relação unívoca com um pipeline e só pode referenciar um pipeline único.

## <a name="tumbling-window-trigger-type-properties"></a>Em cascata as propriedades de tipo de Acionador de janela
Uma janela em cascata tem as seguintes propriedades de tipo do acionador:

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

A tabela seguinte fornece uma descrição dos elementos principais de JSON que estão relacionadas com a periodicidade e agendamento de um acionador de janela em cascata de alto nível:

| Elemento JSON | Descrição | Tipo | Valores permitidos | Necessário |
|:--- |:--- |:--- |:--- |:--- |
| **type** | O tipo do acionador. O tipo é o valor fixo "TumblingWindowTrigger." | Cadeia | "TumblingWindowTrigger" | Sim |
| **runtimeState** | O estado atual do acionador tempo de execução.<br/>**Tenha em atenção**: este elemento está \<readOnly >. | Cadeia | "Iniciado", "parado," "Desativado" | Sim |
| **frequency** | Uma cadeia que representa a unidade de frequência (minutos ou horas) em que o acionador voltar a ocorrer. Se o **startTime** valores de data são mais granulares do que o **frequência** valor, o **startTime** datas são consideradas quando são calculados os limites de janela. Por exemplo, se o **frequência** valor é de hora a hora e a **startTime** valor é 2016-04-01T10:10:10Z, a primeira janela é (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z). | Cadeia | "minutos", "horas"  | Sim |
| **interval** | Um número inteiro positivo que indica o intervalo para o **frequência** valor, que determina a frequência de execução do acionador. Por exemplo, se o **intervalo** é 3 e o **frequência** é "horas", o acionador voltar a ocorrer a cada 3 horas. | Número inteiro | Um número inteiro positivo. | Sim |
| **startTime**| A primeira ocorrência, que pode ser passado. O primeiro intervalo de Acionador é (**startTime**, **startTime** + **intervalo**). | DateTime | Um valor de DateTime. | Sim |
| **endTime**| A última ocorrência, que pode ser passado. | DateTime | Um valor de DateTime. | Sim |
| **delay** | A quantidade de tempo para atrasar o início do processamento de dados para a janela. O pipeline executar é iniciado depois do tempo de execução esperado e a quantidade de **atraso**. O **atraso** define quanto o acionador aguarda posterior à hora devida antes de acionar uma execução de novo. O **atraso** não alterar a janela **startTime**. Por exemplo, um **atraso** valor 10:00:00 implica um atraso de 10 minutos. | Timespan  | Um valor de tempo em que a predefinição é 00:00:00. | Não |
| **maxConcurrency** | O número de execuções de Acionador simultâneas que são desencadeado para windows que estão prontos. Por exemplo, para fazer uma cópia de preenchimento hora a hora é executado para resultados de ontem no 24 windows. Se **maxConcurrency** = 10, acionador eventos são desencadeados apenas para o primeiro 10 do windows (00:00-01:00 - 10-09:00:00). Depois de concluídas as primeiras 10 execuções de pipeline accionadas, execuções de Acionador são desencadeadas para os 10 junto windows (10:00-11:00-19:00-20:00). Continuar com este exemplo de **maxConcurrency** = 10, se existirem 10 windows estiver prontos, existem 10 total pipeline é executado. Se existir apenas 1 janela pronto, apenas há 1 execução de pipeline. | Número inteiro | Um número inteiro entre 1 e 50. | Sim |
| **retryPolicy: Count** | O número de tentativas antes da execução de pipeline está marcado como "Falhado".  | Número inteiro | Um número inteiro, onde a predefinição é 0 (nenhum tentativas). | Não |
| **retryPolicy: intervalInSeconds** | O atraso entre tentativas de repetição especificada em segundos. | Número inteiro | O número de segundos, em que a predefinição é 30. | Não |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart e WindowEnd variáveis do sistema

Pode utilizar o **WindowStart** e **WindowEnd** variáveis do sistema do acionador de janela em cascata na sua **pipeline** definição (ou seja, para a parte de uma consulta). As variáveis do sistema de passar como parâmetros para o pipeline de **acionador** definição. O exemplo seguinte mostra como estas variáveis de passar como parâmetros:

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

Para utilizar o **WindowStart** e **WindowEnd** valores de variáveis do sistema na definição do pipeline, utilize os parâmetros "MyWindowStart" e "MyWindowEnd", em conformidade.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>Ordem de execução do windows num cenário de preenchimento
Quando existem várias janelas para execução (especialmente num cenário de preenchimento), a ordem de execução para o windows é determinista, de intervalos mais antigos a mais recente. Atualmente, não é possível alterar este comportamento.

### <a name="existing-triggerresource-elements"></a>Elementos de TriggerResource existentes
Os pontos seguintes aplicam-se a existente **TriggerResource** elementos:

* Se o valor para o **frequência** elemento (ou tamanho da janela) das alterações acionador, o estado do windows que já são processados é *não* repor. Continua o acionador a acionar para os windows a partir da janela do último foi executado utilizando o novo tamanho da janela.
* Se o valor para o **endTime** é o elemento de Acionador alterações (adicionadas ou atualizadas), o estado do windows que já são processadas *não* repor. O acionador honra novo **endTime** valor. Se o novo **endTime** valor é antes do windows que já é executado, deixa de Acionador. Caso contrário, o acionador interrompe quando a nova **endTime** é encontrado valor.

## <a name="sample-for-azure-powershell"></a>Exemplo para o Azure PowerShell
Esta secção mostra como utilizar o Azure PowerShell para criar, iniciar e monitorizar um acionador.

1. Crie um ficheiro JSON com o nome **MyTrigger.json** na pasta C:\ADFv2QuickStartPSH\ com o seguinte conteúdo:

   > [!IMPORTANT]
   > Antes de guardar o ficheiro JSON, defina o valor da **startTime** elemento para a hora UTC atual. Defina o valor da **endTime** elemento a uma hora posterior à hora UTC atual.

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

6. Obter o acionador é executada no Azure PowerShell, utilizando o **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Para obter informações sobre a execução do acionador, execute o seguinte comando periodicamente. Atualização do **TriggerRunStartedAfter** e **TriggerRunStartedBefore** valores para fazer corresponder os valores existentes na sua definição do acionador:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Para monitorizar a execução do acionador e o pipeline é executado no portal do Azure, consulte [monitorizar o pipeline é executado](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Passos Seguintes
Para obter informações detalhadas sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md#triggers).
