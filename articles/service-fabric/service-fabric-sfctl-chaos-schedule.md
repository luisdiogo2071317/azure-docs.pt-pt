---
title: Azure Service Fabric CLI - sfctl chaos agenda de | Microsoft Docs
description: Descreve os comandos de agenda do Service Fabric CLI sfctl chaos.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: 0d09338f71d71d07ab0e037d4736cfaa1f3cff85
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764035"
---
# <a name="sfctl-chaos-schedule"></a>agenda de chaos sfctl
Obter e definir a agenda de chaos.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| Introdução | Obter a agenda de Chaos que define quando e como executar Chaos. |
| definir | Defina a agenda de Chaos a serem utilizadas pelo Chaos. |

## <a name="sfctl-chaos-schedule-get"></a>obter a agenda de chaos sfctl
Obter a agenda de Chaos que define quando e como executar Chaos.

Obtém a versão da agenda de Chaos em utilização e a agenda de Chaos que define quando e como executar Chaos.

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos agenda conjunto
Defina a agenda de Chaos a serem utilizadas pelo Chaos.

Defina a agenda de Chaos atualmente em utilização por Chaos. Chaos automaticamente ação irá agendar a execução com base na agenda Chaos. A versão na agenda entrada fornecida tem de corresponder à versão da agenda Chaos no servidor. Se a versão fornecida não corresponde à versão no servidor, a agenda de Chaos não está atualizada. Se a versão fornecida corresponde à versão no servidor, a agenda de Chaos está atualizada e a versão da agenda Chaos no servidor é incrementada cópias de segurança por um e encapsula num wrapper novamente para 0 após 2 147 483 647. Se estiver a executar Chaos quando esta chamada é efetuada, a chamada falhará.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -dicionário de parâmetros chaos | JSON codificado lista que representam um mapeamento de nomes de cadeia para ChaosParameters a ser utilizado pelas tarefas. |
| -data de expiração-utc | A data e hora para quando deixar de utilizar a agenda para agendar Chaos.  Predefinição\: 9999-12-31T23\:59\:59.999Z. |
| – tarefas | Lista JSON codificado de ChaosScheduleJobs que representa o quando executar Chaos e com os parâmetros para executar Chaos com. |
| -data de início-utc | A data e hora para quando começar a utilizar a agenda para agendar Chaos.  Predefinição\: 1601-01-01T00\:00\:00.000Z. |
| -versão | O número de versão da agenda. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

### <a name="examples"></a>Exemplos

O comando seguinte define uma agenda (assumindo que a agenda atual tem uma versão de 0), que inicia no 2016-01-01 e expira em 2038-01-01, que é executada Chaos 24 horas do dia, 7 dias por semana. Será agendada Chaos no cluster para essa hora.

    sfctl chaos schedule set --version 0 --start-date-utc "2016-01-01T00:00:00.000Z" --expiry-date-utc "2038-01-01T00:00:00.000Z"
    --chaos-parameters-dictionary 
    [  
    {  
        "Key":"adhoc",
        "Value":{  
            "MaxConcurrentFaults":3,
            "EnableMoveReplicaFaults":true,
            "ChaosTargetFilter":{  
                "NodeTypeInclusionList":[  
                "N0010Ref",
                "N0020Ref",
                "N0030Ref",
                "N0040Ref",
                "N0050Ref"
                ]
            },
            "MaxClusterStabilizationTimeoutInSeconds":60,
            "WaitTimeBetweenIterationsInSeconds":15,
            "WaitTimeBetweenFaultsInSeconds":30,
            "TimeToRunInSeconds":"600",
            "Context":{  
                "Map":{  
                "test":"value"
                }
            },
            "ClusterHealthPolicy":{  
                "MaxPercentUnhealthyNodes":0,
                "ConsiderWarningAsError":true,
                "MaxPercentUnhealthyApplications":0
            }
        }
    }
    ]
    --jobs 
    [  
    {  
        "ChaosParameters":"adhoc",
        "Days":{  
            "Sunday":true,
            "Monday":true,
            "Tuesday":true,
            "Wednesday":true,
            "Thursday":true,
            "Friday":true,
            "Saturday":true
        },
        "Times":[  
            {  
                "StartTime":{  
                "Hour":0,
                "Minute":0
                },
                "EndTime":{  
                "Hour":23,
                "Minute":59
                }
            }
        ]
    }
    ]

## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI de recursos de infraestrutura de serviço.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).