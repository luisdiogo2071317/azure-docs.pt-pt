---
title: Aguardar atividade no Azure Data Factory | Documentos da Microsoft
description: A atividade de espera coloca em pausa a execução do pipeline para o período de tempo especificado.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 731df55a11f4671670a65dac8a83927d81da454c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015802"
---
# <a name="wait-activity-in-azure-data-factory"></a>Aguardar atividade no Azure Data Factory
Quando utiliza uma atividade Aguardar num pipeline, este aguarda o período de tempo especificado antes de continuar a execução das atividades subsequentes. 

## <a name="syntax"></a>Sintaxe

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Propriedades do tipo

Propriedade | Descrição | Valores permitidos | Necessário
-------- | ----------- | -------------- | --------
nome | Nome do `Wait` atividade. | Cadeia | Sim
tipo | Tem de ser definido como **aguarde**. | Cadeia | Sim
waitTimeInSeconds | O número de segundos que o pipeline aguarda antes de continuar com o processamento. | Número inteiro | Sim

## <a name="example"></a>Exemplo

> [!NOTE]
> Esta seção fornece definições de JSON e comandos de exemplo de PowerShell para executar o pipeline. Para obter instruções com instruções passo a passo para criar um pipeline de fábrica de dados com as definições do Azure PowerShell e JSON, veja [tutorial: criar uma fábrica de dados com o Azure PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Pipeline com atividade de espera
Neste exemplo, o pipeline tem duas atividades: **Até** e **aguarde**. A atividade de espera é configurada para esperar por um segundo. O pipeline é executado a atividade Web num loop com um segundo a aguardar o tempo entre cada execução. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
    }
}

```

## <a name="next-steps"></a>Passos Seguintes
Consulte outras atividades de fluxo de controle suportadas pelo Data Factory: 

- [Atividade Se Condição](control-flow-if-condition-activity.md)
- [Atividade Executar Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Atividade Obter Metadados](control-flow-get-metadata-activity.md)
- [Atividade de Pesquisa](control-flow-lookup-activity.md)
- [Atividade Web](control-flow-web-activity.md)
- [Atividade Until](control-flow-until-activity.md)
