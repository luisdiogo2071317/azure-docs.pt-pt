---
title: Como depurar UDFs no duplos Digital do Azure | Documentos da Microsoft
description: Orientação sobre como depurar UDFs no duplos Digital do Azure
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: stefanmsft
ms.openlocfilehash: 852b2d35ae605f5529d162d52655fd258ca07c5a
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946101"
---
# <a name="how-to-debug-issues-with-user-defined-functions-in-azure-digital-twins"></a>Como depurar problemas com as funções definidas pelo utilizador no duplos Digital do Azure

Este artigo resume como diagnosticar funções definidas pelo utilizador. Em seguida, ele identifica alguns dos cenários mais comuns encontrados ao trabalhar com eles.

## <a name="debug-issues"></a>Depurar problemas

Saber como diagnosticar quaisquer problemas que possam surgir dentro de sua instância de duplos Digital do Azure irá ajudá-lo para efetivamente identificar o problema, a causa do problema e uma solução.

### <a name="enable-log-analytics-for-your-instance"></a>Ativar a análise de registo para a sua instância

Os registos e métricas para a sua instância de duplos Digital do Azure são expostas através do Azure Monitor. A seguinte documentação pressupõe que criou um [do Azure Log Analytics](../log-analytics/log-analytics-queries.md) área de trabalho por meio da [Portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md), da funcionalidade [da CLI do Azure](../log-analytics/log-analytics-quick-create-workspace-cli.md), ou através de [ PowerShell](../log-analytics/log-analytics-quick-create-workspace-posh.md).

> [!NOTE]
> Poderá haver um atraso de 5 minutos, ao enviar eventos para **do Log Analytics** pela primeira vez.

Leia o artigo ["Recolher e consumir dados de registo dos seus recursos do Azure"](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) para ativar as definições de diagnóstico para a sua instância de duplos Digital do Azure através do Portal, a CLI do Azure ou o PowerShell. Certifique-se selecionar todas as categorias de registo, métricas e sua área de trabalho do Log Analytics do Azure.

### <a name="trace-sensor-telemetry"></a>Telemetria de sensores de rastreio

Certifique-se de que as definições de diagnóstico são ativada na sua instância de duplos Digital do Azure, estão selecionadas todas as categorias de registo e que os registos estão a ser enviados para o Azure Log Analytics.

Para fazer corresponder uma mensagem de telemetria de sensor para seus respectivos registos, pode especificar um ID de correlação nos dados de evento a ser enviados. Para tal, defina o `x-ms-client-request-id` propriedade para um GUID.

Depois de enviar telemetria, abrir o Log Analytics do Azure para consultar os registos com o conjunto de ID de correlação:

```Kusto
AzureDiagnostics
| where CorrelationId = 'yourCorrelationIdentifier'
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourCorrelationIdentifier* | O ID de correlação que foi especificada nos dados de eventos |

Se iniciar sessão sua função definida pelo usuário, esses logs irão aparecer na sua instância do Azure Log Analytics com a categoria `UserDefinedFunction`. Para obtê-las, introduza a seguinte condição de consulta no Azure Log Analytics:

```Kusto
AzureDiagnostics
| where Category = 'UserDefinedFunction'
```

Para obter mais informações sobre as operações de consulta poderosa, consulte [introdução às consultas](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="identify-common-issues"></a>Identificar problemas comuns

Diagnóstico e a identificação de problemas comuns são importantes quando a sua solução de resolução de problemas. Vários problemas comuns encontrados durante o desenvolvimento de funções definidas pelo utilizador são resumidas abaixo.

### <a name="ensure-a-role-assignment-was-created"></a>Certifique-se de que foi criada uma atribuição de função

Sem uma atribuição de função criada dentro de API de gestão, a função definida pelo utilizador não terá acesso a executar quaisquer ações como enviar notificações, obtenção de metadados, e calculada de definição de valores na topologia.

Verificar se uma atribuição de função que existe para a função definida pelo utilizador através da API de gestão:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/roleassignments?path=/&traverse=Down&objectId=yourUserDefinedFunctionId
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourManagementApiUrl* | O caminho do URL completo para a API de gestão  |
| *yourUserDefinedFunctionId* | O ID da função definida pelo utilizador a obter atribuições de funções para|

Se nenhuma atribuição de função é recuperada, siga este artigo no [como criar uma atribuição de função para a função definida pelo utilizador](./how-to-user-defined-functions.md).

### <a name="check-if-the-matcher-will-work-for-a-sensors-telemetry"></a>Verifica se na ferramenta de correspondência irá funcionar para a telemetria de um sensor

Com a seguinte chamada em relação a API de gestão dos suas instâncias gémeos Digital do Azure, será capaz de determinar se um determinado na ferramenta de correspondência aplica-se para o sensor de determinado.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/matchers/yourMatcherIdentifier/evaluate/yourSensorIdentifier?enableLogging=true
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourManagementApiUrl* | O caminho do URL completo para a API de gestão  |
| *yourMatcherIdentifier* | O ID da na ferramenta de correspondência que deseja avaliar |
| *yourSensorIdentifier* | O ID de sensor que deseja avaliar |

Resposta:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-will-trigger"></a>Verifique o que irá acionar um sensor

Com a seguinte chamada em relação a API de gestão dos suas instâncias gémeos Digital do Azure, será capaz de determinar os identificadores das suas funções definidas pelo utilizador que irão ser acionados por telemetria de entrada do sensor determinado:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/sensors/yourSensorIdentifier/matchers?includes=UserDefinedFunctions
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourManagementApiUrl* | O caminho do URL completo para a API de gestão  |
| *yourSensorIdentifier* | O ID do sensor que irão enviar telemetria |

Resposta:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Problema com a receção das notificações

Quando não receber notificações a partir de dentro da função definida pelo utilizador acionada, certifique-se de que o parâmetro de tipo de objeto de topologia corresponde ao tipo de identificador de que está a ser utilizado.

**Incorreto** exemplo:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Este cenário surge porque o identificador utilizado refere-se a um sensor enquanto o tipo de objeto de topologia especificado é "Espaço".

**Correto** exemplo:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

A maneira mais fácil para não executar para este problema é usar o `Notify` método no objeto de metadados.

Exemplo:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Exceções comuns de diagnóstico

Se ativar as definições de diagnóstico, pode encontrar essas exceções comuns:

1. **Limitação**: se a sua função definida pelo utilizador excede a taxa de execução limites descritos na [limites do serviço](./concepts-service-limits.md) artigo, serão limitado. Limitação envolve não existem operações adicionais em execução com êxito até os limites de expirarem.

1. **Dados não encontrados**: se a sua função definida pelo utilizador tenta aceder aos metadados que não existem, a operação falhará.

1. **Não autorizado**: se a sua função definida pelo utilizador não tiver uma atribuição de função definido ou não tem permissões suficientes para aceder a determinadas metadados da topologia de, a operação falhará.

## <a name="next-steps"></a>Passos Seguintes

Aprenda a ativar [registos e monitorização](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) no duplos Digital do Azure.
