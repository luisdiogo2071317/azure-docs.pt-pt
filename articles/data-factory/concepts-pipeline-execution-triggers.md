---
title: "Execução de pipelines e acionadores no Azure Data Factory | Microsoft Docs"
description: "Este artigo disponibiliza informações sobre como executar um pipeline no Azure Data Factory a pedido ou mediante a criação de um acionador."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2018
ms.author: shlo
ms.openlocfilehash: fc34cfbab796c6e1e4cd25ce13dcc63c39c6699d
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Execução de pipelines e acionadores no Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Versão 1 - GA](v1/data-factory-scheduling-and-execution.md)
> * [Versão 2 - Pré-visualização](concepts-pipeline-execution-triggers.md)

Uma _execução de pipeline_ é um termo da versão 2 do Azure Data Factory que define uma instância de uma execução de pipeline. Por exemplo, imagine que tem um pipeline que é executado às 8:00, às 9:00 e às 10:00. Neste caso, existem três execuções separadas do pipeline (execuções de pipeline). Cada execução de pipeline tem um ID exclusivo, que é um GUID que as define exclusivamente. Normalmente, as execuções de pipeline são instanciadas pela transmissão de argumentos a parâmetros que são definidos nos pipelines. Há duas formas de executar um pipeline: manualmente ou com um _acionador_. Este artigo fornece detalhes sobre ambas a formas de executar um pipeline.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Azure Data Factory, atualmente disponível em modo de pré-visualização. Se estiver a utilizar a versão 1 do Azure Data Factory, que se encontra disponível de forma generalizada (GA), veja [Agendamento e execução na versão 1 do Azure Data Factory](v1/data-factory-scheduling-and-execution.md).

## <a name="manual-execution-on-demand"></a>Execução manual (a pedido)
À execução manual de um pipeline também se dá o nome de execução _a pedido_.

Por exemplo, vamos supor que tem um pipeline simples chamado **copyPipeline** que quer executar. O pipeline tem uma única atividade que copia a partir de uma pasta de origem do Armazenamento de Blobs do Azure para uma pasta de destino no mesmo armazenamento. A definição JSON que se segue mostra este pipeline de exemplo:

```json
{
  "name": "copyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}
```

Na definição JSON, o pipeline utiliza dois parâmetros: **sourceBlobContainer** e **sinkBlobContainer**. Transmita os valores para estes parâmetros no runtime.

Pode executar manualmente o pipeline através dos métodos que se seguem:
- SDK de .NET.
- Módulo Azure PowerShell.
- API REST.
- SDK de Python.

### <a name="the-rest-api"></a>API REST
O comando de exemplo que se segue mostra como executar manualmente o pipeline com a API REST:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```

Para obter um exemplo completo, veja [Início Rápido: criar uma fábrica de dados com a API REST](quickstart-create-data-factory-rest-api.md).

### <a name="azure-powershell"></a>Azure PowerShell
O comando de exemplo que se segue mostra como executar manualmente o pipeline com o Azure PowerShell:

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Transmita os parâmetros no corpo no payload de pedidos. No SDK de .NET, no Azure Powershell e no SDK de Python, os valores são transmitidos num dicionário que, por sua vez, é transmitido como um argumento para a chamada:

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

O payload de resposta é um ID exclusivo da execução de pipeline:

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```

Para obter um exemplo completo, veja [Início Rápido: criar uma fábrica de dados com o Azure PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="the-net-sdk"></a>SDK de .NET
A chamada de exemplo que se segue mostra como executar manualmente o pipeline com o SDK de .NET:

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Para obter um exemplo completo, veja [Início Rápido: criar uma fábrica de dados com o SDK de .NET](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> Pode utilizar o SDK de .NET para invocar pipelines do Azure Data Factory a partir de Funções do Azure, dos seus próprios serviços Web, etc.

<h2 id="triggers">Execução de acionadores</h2>
Os acionadores constituem a segunda forma de executar uma execução de pipeline. Representam uma unidade de processamento que determina quando é que uma execução de pipeline tem de arrancar. Atualmente, o Azure Data Factory suporta dois tipos de acionadores:
- Acionador de agenda: um acionador que invoca um pipeline com base numa agenda.
- Acionador de janela em cascata: um acionador que opera num intervalo periódico, ao mesmo tempo que mantém o estado. Neste momento, o Azure Data Factory não suporta acionadores baseados em eventos. Por exemplo, o acionador para uma execução de pipeline que responde a um evento de chegada de ficheiro.

Os pipelines e os acionadores têm uma relação muitos para muitos. Vários acionadores podem desencadear um único pipeline ou um só acionador pode desencadear vários pipelines. Na definição de acionador que se segue, a propriedade **pipelines** refere-se a uma lista de pipelines que são acionados pelo acionador especificado. A definição da propriedade inclui valores para os parâmetros do pipeline.

### <a name="basic-trigger-definition"></a>Definição de acionador básica

```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
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
```

## <a name="schedule-trigger"></a>Acionador de Agenda
Um acionador de agenda executa pipelines com base numa agenda. Este acionador suporta opções de calendário avançadas e periódicas. Por exemplo, o acionador suporta intervalos como "semanalmente" ou "Segunda-feira às 17:00 e quinta-feira às 21:00". O acionador de agenda é flexível porque o padrão do conjunto de dados é agnóstico e o acionador não distingue entre dados de série de tempo e sem ser de série de tempo.

Para obter mais informações sobre acionadores de agenda e exemplos, veja [Criar um acionador de agenda](how-to-create-schedule-trigger.md).

## <a name="tumbling-window-trigger"></a>Acionador de janela em cascata
Os acionadores de janela em cascata são um tipo de acionador que é acionado num intervalo de tempo periódico a partir de uma hora de início especificada, mantendo o estado. As janelas em cascata são uma série de intervalos de tempo com tamanho fixo, não sobrepostos e contínuos. Para obter mais informações sobre acionadores de janela em cascata e exemplos, veja [Criar um acionador de janela em cascata](how-to-create-tumbling-window-trigger.md).

## <a name="schedule-trigger-definition"></a>Definição de acionador de agenda
Quando cria um acionador de agenda, o agendamento e a periodicidade são especificados com uma definição JSON. 

Para que o acionador de agenda desencadeie uma execução de pipeline, inclua uma referência do pipeline em questão na definição do acionador. Os pipelines e os acionadores têm uma relação muitos para muitos. Múltiplos acionadores podem arrancar um pipeline individual. Um acionador único pode arrancar vários pipelines.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // How often to fire
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
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
> A propriedade **parameters** é uma propriedade obrigatória do elemento **pipelines**. Se o seu pipeline não utiliza parâmetros, deve incluir uma definição JSON vazia para a propriedade **parameters**.

### <a name="schema-overview"></a>Descrição geral do esquema
A tabela que se segue fornece uma descrição geral de alto nível dos principais elementos do esquema relacionados com a periodicidade e o agendamento de um acionador:

| Propriedade JSON | Descrição |
|:--- |:--- |
| **startTime** | Um valor de data/hora. Para agendamentos simples, o valor da propriedade **startTime** aplica-se à primeira ocorrência. Para agendamentos complexos, o acionador é iniciado imediatamente a seguir ao valor especificado em **startTime**. |
| **endTime** | A data e hora de fim do acionador. O acionador não é executado após a data e hora de fim especificadas. O valor da propriedade não pode situar-se no passado. <!-- This property is optional. --> |
| **timeZone** | O fuso horário. Atualmente, o fuso horário UTC é o único suportado. |
| **recurrence** | Um objeto de periodicidade que especifica as regras de periodicidade do acionador. O objeto de periodicidade suporta os elementos **frequency**, **interval**, **endTime**, **count** e **schedule**. Quando um objeto de periodicidade é definido, o elemento **frequency** é obrigatório. Os outros elementos do objeto de periodicidade são opcionais. |
| **frequency** | A unidade que se refere à frequência com que o acionador voltar a ocorrer. Os valores suportados incluem "minute", "hour", "day", "week" e "month". |
| **interval** | Um valor inteiro positivo que indica o intervalo do valor **frequency**, que determina o número de vezes que o acionador é executado. Por exemplo, se o valor de **interval** for 3 e o de **frequency** for "week", o acionador repete-se de três em três semanas. |
| **schedule** | A agenda de periodicidade do acionador. Um acionador que tenha um valor **frequency** especificado modifica a respetiva periodicidade com base numa agenda de periodicidade. A propriedade **schedule** contém modificações da periodicidade baseadas em minutos, horas, dias de semana, dias do mês e número da semana.

### <a name="schedule-trigger-example"></a>Exemplo de acionador Schedule

```json
{
    "properties": {
        "name": "MyTrigger",
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-01T09:00:00-08:00",
                "endTime": "2017-11-02T22:00:00-08:00"
            }
        },
        "pipelines": [{
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToBlobPipeline"
                },
                "parameters": {}
            },
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToAzureSQLPipeline"
                },
                "parameters": {}
            }
        ]
    }
}
```

### <a name="schema-defaults-limits-and-examples"></a>Predefinições, limites e exemplos do esquema

| Propriedade JSON | Tipo | Necessário | Valor predefinido | Valores válidos | Exemplo |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Cadeia | Sim | Nenhum | Datas-Horas ISO 8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Objeto | Sim | Nenhum | Objeto de periodicidade | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Número | Não | 1 | 1 a 1000 | `"interval":10` |
| **endTime** | Cadeia | Sim | Nenhum | Um valor de data/hora que representa uma hora no futuro. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Objeto | Não | Nenhuma | Objeto da agenda | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>Propriedade startTime
A tabela que se segue mostra o modo como a propriedade **startTime** controla a execução de um acionador:

| valor de startTime | Periodicidade sem agenda | Periodicidade com agenda |
|:--- |:--- |:--- |
| Hora de início no passado | Calcula a primeira hora de execução no futuro após a hora de início e é executada nessa hora.<br/><br/>Executa as execuções subsequentes com base no cálculo da última hora de execução.<br/><br/>Veja o exemplo a seguir à tabela. | O acionador é iniciado _imediatamente a seguir_  à hora de início especificada. A primeira ocorrência tem por base a agenda calculada a partir da hora de início.<br/><br/>Executa as execuções subsequentes com base na agenda de periodicidade. |
| Hora de início no futuro ou no presente | É executada uma vez na hora de início especificada.<br/><br/>Executa as execuções subsequentes com base no cálculo da última hora de execução. | O acionador é iniciado _imediatamente a seguir_  à hora de início especificada. A primeira ocorrência tem por base a agenda calculada a partir da hora de início.<br/><br/>Executa as execuções subsequentes com base na agenda de periodicidade. |

Vejamos um exemplo do que acontece quando a hora de início (startTime) se situa no passado, com periodicidade, mas sem agenda. Parta do princípio de que a hora atual é `2017-04-08 13:00`, a hora de início é `2017-04-07 14:00` e a periodicidade é de dois em dois dias. (O valor **recurrence** é especificado mediante a definição da propriedade **frequency** como "day" e da propriedade **interval** como 2.) Repare que o valor de **startTime** se situa no passado e ocorre antes da hora atual.

Nestas condições, a primeira execução é em `2017-04-09 at 14:00`. O motor do Scheduler calcula as ocorrências de execução a partir da hora de início. Quaisquer instâncias no passado são eliminadas. O motor utiliza a instância seguinte que ocorre no futuro. Neste cenário, a hora de início é `2017-04-07 at 2:00pm`, pelo que a instância seguinte é dali a dois dias a contar dessa hora, ou seja, `2017-04-09 at 2:00pm`.

A primeira hora de execução é igual, mesmo que o valor de **startTime** seja `2017-04-05 14:00` ou `2017-04-01 14:00`. Após a primeira execução, as execuções subsequentes são calculadas com base na agenda. Por conseguinte, as execuções subsequentes são a `2017-04-11 at 2:00pm`, depois a `2017-04-13 at 2:00pm`, a seguir a `2017-04-15 at 2:00pm` e assim sucessivamente.

Por último, quando as horas ou os minutos não se encontram definidos na agenda para um acionador, as horas ou minutos da primeira execução são utilizados como as predefinições.

### <a name="schedule-property"></a>Propriedade schedule
Por um lado, a utilização de agendas pode limitar o número de execuções de acionadores. Por exemplo, se um acionador com uma frequência mensal estiver agendado de modo a ser executado apenas no dia 31, o acionador só é executado nos meses que têm 31 dias.

Por outro lado, as agendas também podem expandir o número de execuções de acionadores. Por exemplo, um acionador com uma frequência mensal que esteja agendado de maneira a ser executado nos dias 1 e 2 do mês, é executado nos 1º e 2º dias do mês, em vez de uma vez por mês.

Se forem especificados vários elementos **schedule**, a ordem de avaliação é feita da definição de agenda maior para a mais pequena. A avaliação começa pelo número da semana, seguindo-se o dia do mês, o dia da semana, a hora e, por último, o minuto.

A tabela seguinte descreve os elementos de **schedule** de forma detalhada:

| Elemento JSON | Descrição | Valores válidos |
|:--- |:--- |:--- |
| **minutes** | Minutos da hora em que o acionador é executado. | <ul><li>Número inteiro</li><li>Matriz de números inteiros</li></ul>
| **hours** | Horas do dia em que o acionador é executado. | <ul><li>Número inteiro</li><li>Matriz de números inteiros</li></ul> |
| **weekDays** | Dias da semana em que o acionador é executado. O valor pode ser especificado com uma frequência semanal apenas. | <ul><li>Segunda-feira, terça-feira, quarta-feira, quinta-feira, sexta-feira, sábado, domingo</li><li>Matriz de valores de dia (o tamanho máximo da matriz é 7)</li><li>Os valores de dia não são sensíveis às maiúsculas e minúsculas</li></ul> |
| **monthlyOccurrences** | Dias do mês em que o acionador é executado. O valor pode ser especificado com uma frequência mensal apenas. | <ul><li>Matriz de objetos de **monthlyOccurence**: `{ "day": day,  "occurrence": occurence }`.</li><li>O atributo **day** é o dia da semana em que o acionador é executado. Por exemplo, uma propriedade **monthlyOccurrences** com um valor **day** igual a `{Sunday}` significa todos os domingos do mês. O atributo **day** é obrigatório.</li><li>O atributo **occurrence** é a ocorrência do valor **day** especificado durante o mês. Por exemplo, uma propriedade **monthlyOccurrences** com os valores **day** e **occurrence** iguais a `{Sunday, -1}` significa o último domingo do mês. O atributo **occurrence** é opcional.</li></ul> |
| **monthDays** | Dia do mês em que o acionador é executado. O valor pode ser especificado com uma frequência mensal apenas. | <ul><li>Qualquer valor <= -1 e >= -31</li><li>Qualquer valor >= 1 e <= 31</li><li>Matriz de valores</li></ul> |

## <a name="examples-of-trigger-recurrence-schedules"></a>Exemplos de agendas de periodicidade do acionador
Esta secção mostra exemplos de agendas de periodicidade e centra-se no objeto **schedule** e respetivos elementos.

Os exemplos partem do princípio de que o valor **interval** é igual a 1 e que o valor **frequency** está correto de acordo com a definição da agenda. Por exemplo, não pode ter um valor **frequency** igual a "day" e ter simultaneamente uma modificação "monthDays" no objeto **schedule**. Este género de restrições é mencionado na tabela da secção anterior.

| Exemplo | Descrição |
|:--- |:--- |
| `{"hours":[5]}` | Executar todos os dias às 5:00. |
| `{"minutes":[15], "hours":[5]}` | Executar todos os dias às 5:15. |
| `{"minutes":[15], "hours":[5,17]}` | Executar todos os dias às 5:15 e 17:15. |
| `{"minutes":[15,45], "hours":[5,17]}` | Executar todos os dias às 5:15, 5:45, 17:15 e 17:45. |
| `{"minutes":[0,15,30,45]}` | Executar de 15 em 15 minutos. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Executar hora a hora. Este acionador é executado de hora em hora. Os minutos são controlados pelo valor de **startTime**, quando é especificado um valor. Se não for especificado qualquer valor, os minutos são controlados pela hora de criação. Por exemplo, se a hora de início ou a hora de criação (a que se aplicar) for 12:25, o acionador é executado às 00:25, 01:25, 02:25 e assim sucessivamente até às 23:25.<br/><br/>Esta agenda equivale a ter um acionador com um valor **frequency** igual a "hour", um valor **interval** igual a 1 e zero propriedades **schedule**. Esta agenda pode ser utilizada com valores **frequency** e **interval** diferentes para criar outros acionadores. Por exemplo, quando o valor **frequency** é igual a "month", a agenda é executada apenas uma vez por mês, em vez de todos os dias, como acontece quando o valor **frequency** é igual a "day". |
| `{"minutes":[0]}` | Executar de hora a hora, à hora certa. Este acionador é executado de hora a hora, à hora certa, começando às 00:00, 1:00, 2:00 e assim sucessivamente.<br/><br/>Esta agenda equivale a ter um acionador com um valor **frequency** igual a "hour" e um valor **startTime** de zero minutos, ou zero propriedades **schedule**, mas um valor **frequency** igual a "day". Se o valor **frequency** for igual a "week" ou "month", a agenda é executada um dia por semana ou um dia por mês apenas, respetivamente. |
| `{"minutes":[15]}` | Executar de hora a hora, 15 minutos após a hora. Este acionador é executado de hora a hora, 15 minutos após a hora, começando às 00:15, 1:15, 2:15 e assim sucessivamente até terminar às 23:15. |
| `{"hours":[17], "weekDays":["saturday"]}` | Executar às 17:00 de sábado todas as semanas. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Executar às 17:00 de segunda-feira, quarta-feira e sexta-feira todas as semanas. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Executar às 17:15 e 17:45 de segunda-feira, quarta-feira e sexta-feira todas as semanas. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Executar de 15 em 15 minutos nos dias de semana. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Executar de 15 em 15 minutos nos dias de semana, entre as 9:00 e as 16:45. |
| `{"weekDays":["tuesday", "thursday"]}` | Executar às terças-feiras e quintas-feiras à hora de início especificada. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Executar às 6:00 do dia 28 de cada mês (pressupondo um valor **frequency** igual a "month"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Executar às 6:00 no último dia do mês. Para executar um acionador no último dia de um mês, utilize -1 em vez de dia 28, 29, 30 ou 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Executar às 6:00 no primeiro e último dias de cada mês. |
| `{monthDays":[1,14]}` | Executar no primeiro e 14.º dias de cada mês à hora de início especificada. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Executar na primeira sexta-feira de cada mês às 5:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Executar na primeira sexta-feira de cada mês à hora de início especificada. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Executar na terceira sexta-feira do fim do mês, todos os meses, à hora de início especificada. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Executar na primeira e última sextas-feiras de cada mês às 5:15. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Executar na primeira e última sextas-feiras de cada mês à hora de início especificada. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Executar na quinta sexta-feira de cada mês à hora de início especificada. Quando um mês não tiver cinco sextas-feiras, o pipeline não é executado, uma vez que está agendado para ser executado apenas à quinta sexta-feira. Para executar o acionador na última sexta-feira do mês, considere utilizar -1 em vez de 5 para o valor **occurrence**. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Executar de 15 em 15 minutos na última sexta-feira do mês. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Executar às 5:15, 5:45, 17:15 e 17:45 na terceira quarta-feira de cada mês. |

## <a name="trigger-type-comparison"></a>Comparação de tipos de acionador
Tanto o acionador de janela em cascata como o acionador de agenda operam em heartbeats de tempo, por isso, o que os diferencia?

A tabela que se segue oferece uma comparação entre o acionador de janela em cascata e o acionador de agenda:

|  | Acionador de &nbsp;janela&nbsp; em cascata | Acionador de &nbsp;agenda |
|:--- |:--- |:--- |
| **Cenários de&nbsp;preenchimento de fundo** | Suportado. As execuções de pipeline podem ser agendadas para períodos de tempo no passado. | Não suportado. As execuções de pipeline só podem ser executadas em períodos de tempo do presente e do futuro. |
| **Fiabilidade** | 100% de fiabilidade. As execuções de pipeline podem ser agendadas para todos os períodos de tempo a partir de uma data de início especificada sem lacunas. | Menos fiável. |
| **Capacidade&nbsp;de repetição** | Suportado. As execuções de pipeline com falhas têm uma política de repetição predefinida de 0 ou uma política especificada pelo utilizador na definição do acionador. Repete automaticamente quando as execuções de pipeline falham devido a limites de simultaneidade/servidor/limitação (ou seja, códigos de estado 400: Erro de Utilizador, 429: Demasiados Pedidos e 500: Erro Interno do Servidor). | Não suportado. |
| **Simultaneidade** | Suportado. Os utilizadores podem definir explicitamente limites de simultaneidade para o acionador. Permite entre 1 e um máximo de 50 execuções de pipeline acionadas em simultâneo. | Não suportado. |
| **Variáveis do&nbsp;sistema** | Suporta a utilização das variáveis do sistema **WindowStart** e **WindowEnd**. Os utilizadores podem aceder a `triggerOutputs().windowStartTime` e `triggerOutputs().windowEndTime` como variáveis do sistema de acionador na definição do acionador. Os valores são utilizados como a hora de início e a hora de fim do período de tempo, respetivamente. Por exemplo, para um acionador de janela em cascata executado de hora a hora, para o período de tempo das 1:00 às 2:00, a definição é `triggerOutputs().WindowStartTime = 2017-09-01T01:00:00Z` e `triggerOutputs().WindowEndTime = 2017-09-01T02:00:00Z`. | Não suportado. |
| **Relação pipeline para acionador** | Suporta uma relação um para um. Apenas um pipeline pode ser acionado. | Suporta relações muitos para muitos. Múltiplos acionadores podem arrancar um pipeline individual. Um acionador único pode arrancar vários pipelines. | 

## <a name="next-steps"></a>Passos seguintes
Veja os tutoriais seguintes:

- [Início Rápido: criar uma fábrica de dados com o SDK de .NET](quickstart-create-data-factory-dot-net.md)
- [Criar um acionador de agenda](how-to-create-schedule-trigger.md)
- [Criar um acionador de janela em cascata](how-to-create-tumbling-window-trigger.md)
