---
title: Fluxo registos de diagnóstico do Azure para um hub de eventos
description: Saiba como transmitir os registos de diagnóstico do Azure para um hub de eventos.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 806ebe80ad49f2f908bf46549bb4abc533a6d516
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936705"
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hub"></a>Fluxo registos de diagnóstico do Azure para um hub de eventos
**[Os registos de diagnóstico do Azure](monitoring-overview-of-diagnostic-logs.md)**  pode transmissão em fluxo em tempo real para qualquer aplicação utilizando a opção "Exportar para os Event Hubs" incorporada no Portal ou ativar o ID da regra de autorização de Hub de eventos na definição de diagnóstico através do Azure Cmdlets do PowerShell ou do Azure CLI 2.0.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>O que pode fazer com os Hubs de eventos e registos de diagnóstico
Seguem-se apenas algumas formas poderá utilizar a capacidade de transmissão em fluxo para os registos de diagnóstico:

* **Transmitir os registos para sistemas 3rd de registo e telemetria de terceiros** – pode transmitir todos os seus registos de diagnóstico para um hub de eventos único para os dados de registo de pipe para uma ferramenta de análise terceiros SIEM ou de registo.
* **Ver o estado de funcionamento do serviço através da transmissão em fluxo "path frequente" dados ao Power BI** – utilizar Event Hubs, Stream Analytics e Power BI, pode facilmente transformar os dados de diagnóstico para quase em tempo real das informações no seus serviços do Azure. [Este artigo de documentação fornece uma excelente descrição geral de como configurar os Event Hubs, processar os dados com o Stream Analytics e utilizar o Power BI como uma saída](../stream-analytics/stream-analytics-power-bi-dashboard.md). Eis algumas sugestões para obter configurado com os registos de diagnóstico:

  * Um hub de eventos para uma categoria de registos de diagnóstico é criado automaticamente ao selecionar a opção no portal ou ativá-la através do PowerShell, pelo que pretende selecionar o hub de eventos no espaço de nomes com o nome que começa com **insights -**.
  * O seguinte código do SQL Server é um exemplo de consulta de Stream Analytics que pode utilizar para analisar todos os dados de registo na uma tabela de PowerBI:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Criar uma plataforma de registo e telemetria personalizada** – se já tiver uma plataforma de telemetria personalizada ou são apenas de pensar sobre como criar um, altamente dimensionável de publicação-subscrição natureza dos Event Hubs permite-lhe para a ingestão de forma flexível os registos de diagnóstico. [Consulte o guia de Dan Rosanova para utilizar os Hubs de eventos numa plataforma de telemetria do dimensionamento global](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Ative a transmissão em fluxo de registos de diagnóstico

Pode ativar a transmissão em fluxo de registos de diagnóstico programaticamente, através do portal, ou utilizando o [as APIs REST da Azure Monitor](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). Qualquer forma, pode cria uma definição de diagnóstico no que especificou um espaço de nomes de Event Hubs e as categorias de registo e métricas que pretende enviar para o espaço de nomes. Um hub de eventos é criado no espaço de nomes para cada categoria de registo que ativar. Um diagnóstico **categoria de registo** é um tipo de registo que pode recolher um recurso.

> [!WARNING]
> Ativar e os registos de diagnóstico a partir dos recursos de computação (por exemplo, VMs ou Service Fabric) de transmissão em fluxo [requer um conjunto diferente de passos](../event-hubs/event-hubs-streaming-azure-diags-data.md).

O espaço de nomes de Event Hubs não tem de estar na mesma subscrição, como o recurso emitir os registos, desde que o utilizador que configura a definição possui acesso RBAC adequado para ambas as subscrições.

> [!NOTE]
> Atualmente, o envio de métricas multidimensionais através das definições de diagnóstico não é suportado. As métricas com dimensões são exportadas como métricas dimensionais simples e agregadas em valores de dimensões.
>
> *Por exemplo*: a métrica “Mensagens Recebidas” num hub do Hub de Eventos pode ser explorada e representada ao nível da linha. No entanto, se for exportada através das definições de diagnóstico, a métrica será representada como todas as mensagens recebidas em todas as filas do hub do Hub de Eventos.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Registos de diagnóstico de fluxo através do portal

1. No portal, navegue para o Monitor do Azure e clique em **definições de diagnóstico**

    ![Secção de monitorização do Monitor do Azure](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Opcionalmente filtrar a lista por tipo de recurso ou grupo de recursos, em seguida, clique no recurso para o qual pretende configurar uma definição de diagnóstico.

3. Se não existem definições de existem no recurso que selecionou, são-lhe pedido para criar uma definição. Clique em "Ativar o diagnóstico".

   ![Adicionar definição de diagnóstico - sem definições existentes](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Se existirem definições existentes no recurso, irá ver uma lista das definições já configurada neste recurso. Clique em "Adicionar definição de diagnóstico".

   ![Adicionar definição de diagnóstico - existente definições](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Dê a definição de um nome e a caixa de verificação **fluxo para um hub de eventos**, em seguida, selecione um espaço de nomes de Event Hubs.

   ![Adicionar definição de diagnóstico - existente definições](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)

   O espaço de nomes selecionado será onde o hub de eventos é criado (se este é os primeiro tempo transmissão em fluxo os registos de diagnóstico) ou transmissão em fluxo para (se já existem recursos que são de transmissão em fluxo dessa categoria de registo para este espaço de nomes), e a política define as permissões que o mecanismo de transmissão em fluxo tem. Hoje em dia, a transmissão em fluxo para um hub de eventos requer permissões de gerir, enviar e escuta. Pode criar ou modificar as políticas de acesso partilhado do espaço de nomes dos Event Hubs no portal do separador Configurar para o espaço de nomes. Para atualizar uma destas definições de diagnóstico, o cliente tem de ter a permissão de ListKey na regra de autorização de Event Hubs. Também, opcionalmente, pode especificar um nome de hub de eventos. Se especificar um nome de hub de eventos, os registos são encaminhados para esse hub de eventos, em vez de para um hub de eventos recentemente criado por categoria de registo.

4. Clique em **Guardar**.

Após alguns instantes, a nova definição é apresentada na sua lista de definições para este recurso, e os registos de diagnóstico são transmissão em fluxo para esse hub de eventos, assim como novos dados de evento são gerados.

### <a name="via-powershell-cmdlets"></a>Através de Cmdlets do PowerShell

Para ativar a transmissão em fluxo através de [Cmdlets do PowerShell do Azure](insights-powershell-samples.md), pode utilizar o `Set-AzureRmDiagnosticSetting` cmdlet com os seguintes parâmetros:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -EventHubAuthorizationRuleId [your Event Hub namespace auth rule ID] -Enabled $true
```

O ID de regra de autorização de Hub de eventos é uma cadeia com este formato: `{Event Hub namespace resource ID}/authorizationrules/{key name}`, por exemplo, `/subscriptions/{subscription ID}/resourceGroups/{resource group}/providers/Microsoft.EventHub/namespaces/{Event Hub namespace}/authorizationrules/RootManageSharedAccessKey`. Atualmente não é possível selecionar um nome de hub de eventos específico com o PowerShell.

### <a name="via-azure-cli-20"></a>Através da CLI do Azure 2.0

Para ativar a transmissão em fluxo através de [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/monitor?view=azure-cli-latest), pode utilizar o [criar definições de diagnóstico do monitor az](https://docs.microsoft.com/en-us/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) comando.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Pode adicionar categorias adicionais no registo de diagnóstico adicionando dicionários para a matriz JSON transmitida como o `--logs` parâmetro.

O `--event-hub-rule` argumento utiliza o mesmo formato como o ID de regra de autorização de Hub de eventos, tal como explicado para o Cmdlet do PowerShell.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Como posso consumir os dados de registo dos Event Hubs?

Segue-se dados de saída de exemplo de Hubs de eventos:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Nome do elemento | Descrição |
| --- | --- |
| registos |Uma matriz de todos os eventos de registo este payload. |
| hora |Tempo em que o evento ocorreu. |
| categoria |Categoria de registo para este evento. |
| resourceId |ID de recurso do recurso que gerou este evento. |
| operationName |Nome da operação. |
| nível |Opcional. Indica o nível de eventos de registo. |
| propriedades |Propriedades do evento. |

Pode ver uma lista de todos os fornecedores de recursos que suportam a transmissão em fluxo para os Event Hubs [aqui](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Dados de sequência de recursos de computação

Também pode transmitir os registos de diagnóstico a partir dos recursos de computação com o agente do Windows Azure Diagnostics. [Consulte este artigo](../event-hubs/event-hubs-streaming-azure-diags-data.md) para saber como configurar essa opção.

## <a name="next-steps"></a>Passos Seguintes

* [Leia mais sobre os registos de diagnóstico do Azure](monitoring-overview-of-diagnostic-logs.md)
* [Introdução ao Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
