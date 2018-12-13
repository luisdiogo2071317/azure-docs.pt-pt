---
title: Como enviar eventos para um ambiente do Azure Time Series Insights | Documentos da Microsoft
description: Este tutorial explica como criar e configurar o hub de eventos e executar uma aplicação de exemplo para enviar eventos para ser mostrada no Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: c583c2211297acd83f88d23b2b8cbd9f8207927f
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867613"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Enviar eventos para um ambiente do Time Series Insights com um hub de eventos

Este artigo explica como criar e configurar o hub de eventos e executa uma aplicação de exemplo para enviar eventos. Se tiver um hub de eventos existente com eventos no formato JSON, ignorar este tutorial e ver o seu ambiente no [do Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Configurar um Hub de eventos

1. Para a criação do Hub de eventos, siga as instruções do Hub de eventos [documentação](https://docs.microsoft.com/azure/event-hubs/).
1. Procure `Event Hub` na barra de pesquisa. Clique em **os Hubs de eventos** na lista devolvida.
1. Selecione o seu Hub de eventos ao clicar no respetivo nome.
1. Quando cria um Hub de eventos, realmente está a criar um espaço de nomes do Hub de eventos.  Se ainda tiver que criar um Hub de eventos no espaço de nomes, tem de criar um em entidades.  

    ![atualizado][1]

1. Depois de criar um Hub de eventos, clique no respetivo nome.
1. Sob **entidades** na janela de configuração média, clique em **dos Hubs de eventos** novamente.
1. Selecione o nome do Hub de eventos para configurá-lo.

    ![grupo de consumidores][2]

1. Sob **entidades**, selecione **grupos de consumidores**.
1. Certifique-se de que cria um grupo de consumidores que seja utilizado exclusivamente pela origem de evento TSI.

    > [!IMPORTANT]
    > Certifique-se de que este grupo de consumidores não é utilizado por nenhum outro serviço (por exemplo, a tarefa do Stream Analytics ou outro ambiente do TSI). Se o grupo de consumidores for utilizado por outros serviços, a operação de leitura é afetada negativamente neste ambiente e os outros serviços. Se estiver a utilizar `$Default` como o grupo de consumidores pode levar à potencial reutilização por outros leitores.

1. Sob o **configurações** cabeçalho, selecione **políticas de acesso de partilha**.
1. No hub de eventos, crie **MySendPolicy** que é utilizado para enviar eventos no C# exemplo.

    ![partilhado = um acesso][3]

    ![partilhado-access-dois][4]

## <a name="add-time-series-insights-instances"></a>Adicionar instâncias do Time Series Insights

A atualização do TSI usa instâncias para adicionar dados contextuais para dados telemétricos recebidos. Os dados que estão associados ao tempo de consulta utilizando um **ID de série de tempo**. O **ID de série de tempo** para os windmills de exemplo é projeto `Id`. Para saber mais sobre as instâncias de série de tempo e **IDs de série de tempo**, leia [modelos de série de tempo](./time-series-insights-update-tsm.md).

### <a name="create-time-series-insights-event-source"></a>Criar a origem de eventos do Time Series Insights

1. Se não tiver criado uma origem de eventos, siga [estas instruções](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub) para criá-la.
1. Especifique a `timeSeriesId` – veja [modelos de série de tempo](./time-series-insights-update-tsm.md) para saber mais sobre **IDs de série de tempo**.

### <a name="push-events-sample-windmills"></a>Enviar eventos (windmills de exemplo)

1. Procure o hub de eventos na barra de pesquisa. Clique em **os Hubs de eventos** na lista devolvida.
1. Selecione o seu hub de eventos ao clicar no respetivo nome.
1. Aceda a **partilhados políticas de acesso** e, em seguida **RootManageSharedAccessKey**. Copiar o **ligação chave primária sting**

   ![connection-string][5]

1. Aceda a https://tsiclientsample.azurewebsites.net/windFarmGen.html. Esta solução é executada windmill simulado dispositivos.
1. Cole a cadeia de ligação que copiou no passo três no **cadeia de ligação do Hub de eventos**.

    ![connection-string][6]

1. Clique em **clique aqui para iniciar**. O simulador também irá gerar um JSON de instância que pode utilizar diretamente.
1. Regresse ao seu Hub de eventos. Deverá ver os novos eventos que está a ser recebidos pelo hub:

   ![telemetria][7]

<a id="json"></a>

## <a name="supported-json-shapes"></a>Formas JSON suportadas

### <a name="sample-1"></a>Exemplo 1

#### <a name="input"></a>Input

Um objeto JSON simples.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```

#### <a name="output---one-event"></a>Saída - um evento

|ID|carimbo de data/hora|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Exemplo 2

#### <a name="input"></a>Input

Uma matriz JSON com dois objetos JSON. Cada objeto JSON será convertido num evento.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```

#### <a name="output---two-events"></a>Saída - dois eventos

|ID|carimbo de data/hora|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Exemplo 3

#### <a name="input"></a>Input

Um objeto JSON com uma matriz JSON aninhada que contém dois objetos JSON:
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```

#### <a name="output---two-events"></a>Saída - dois eventos

Tenha em atenção que a propriedade "location" é copiada para cada evento.

|localização|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Exemplo 4

#### <a name="input"></a>Input

Um objeto JSON com uma matriz JSON aninhada que contém dois objetos JSON. Essa entrada demonstra que as propriedades globais podem ser representadas pelo objeto JSON complexo.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```

#### <a name="output---two-events"></a>Saída - dois eventos

|localização|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ver o seu ambiente no Explorador do Time Series Insights](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/send-events/updated.png
[2]: media/send-events/consumer-group.png
[3]: media/send-events/shared-access-policy.png
[4]: media/send-events/shared-access-policy-2.png
[5]: media/send-events/sample-code-connection-string.png
[6]: media/send-events/updated_two.png
[7]: media/send-events/telemetry.png