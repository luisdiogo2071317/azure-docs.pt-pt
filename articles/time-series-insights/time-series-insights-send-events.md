---
title: Como enviar eventos para um ambiente de informações de séries de tempo do Azure | Microsoft Docs
description: Este tutorial explica como criar e configurar o hub de eventos e executar uma aplicação de exemplo para eventos de emissão para ser mostrada no informações de séries de tempo do Azure.
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MarkMcGeeAtAquent
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 04/09/2018
ms.openlocfilehash: b418d1114cf6b906dcdee46bbf7e094cbc4a0521
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Enviar eventos para um ambiente do Time Series Insights com um hub de eventos
Este artigo explica como criar e configurar o hub de eventos e execute um exemplo de aplicação para eventos de push. Se tiver um hub de eventos existentes com eventos no formato JSON, ignore este tutorial e ver o seu ambiente no [Insights de séries de tempo](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Configurar um hub de eventos
1. Para criar um hub de eventos, siga as instruções na [documentação](../event-hubs/event-hubs-create.md) dos Hubs de Eventos.

2. Procurar **hub de eventos** na barra de procura. Clique em **Event Hubs** na lista devolvida.

3. Selecione o seu hub de eventos, clicando no respetivo nome.

4. Em **entidades** na janela Configuração meio, clique em **Event Hubs** novamente.

5. Selecione o nome do hub de eventos de configurá-lo.

  ![Selecionar o grupo de consumidores do hub de eventos](media/send-events/consumer-group.png)

6. Em **entidades**, selecione **grupos de consumidores**.
 
7. Confirme que cria um grupo de consumidores que seja utilizado exclusivamente pela sua origem de eventos do Time Series Insights.

   > [!IMPORTANT]
   > Certifique-se de que este grupo de consumidores não é utilizado por nenhum outro serviço (como uma tarefa do Stream Analytics ou outro ambiente do Time Series Insights). Se o grupo de consumidores é utilizado por outros serviços, a operação de leitura é afetado negativamente para este ambiente e os outros serviços. A utilização de “$Default” como o grupo de consumidores pode levar à potencial reutilização por parte de outros leitores.

8. Sob o **definições** cabeçalho, selecione **políticas de acesso de partilha**.

9. No hub de eventos, crie **MySendPolicy** que é utilizada para enviar eventos na amostra csharp.

  ![Selecione Políticas de acesso partilhado e clique no botão Adicionar](media/send-events/shared-access-policy.png)  

  ![Adicionar uma política de acesso partilhado nova](media/send-events/shared-access-policy-2.png)  

## <a name="add-time-series-insights-reference-data-set"></a>Adicionar o conjunto de dados de referência de informações de séries de tempo 
Utilizar dados de referência na TSI contextualizes os dados de telemetria.  Nesse contexto adiciona significado aos seus dados e torna mais fácil para agregação e de filtro.  As associações TSI referenciam a dados em tempo de entrada e retroactively não é possível associar estes dados.  Por conseguinte, é fundamental para adicionar dados de referência antes de adicionar uma origem de evento com dados.  Os dados, como o tipo de sensor ou localização estão dimensões útil que poderá associar a um dispositivo/tag/sensor ID para facilitar o setor e o filtro.  

> [!IMPORTANT]
> É fundamental ter um conjunto de dados de referência configurado ao carregar os dados históricos.

Certifique-se de que tem os dados de referência no local quando efetuar em massa de dados históricos de carregamento para TSI.  Tenha em atenção, TSI irá iniciar imediatamente a leitura de uma origem de evento associado a um se essa origem de evento tem de dados.  É útil de espera para associar uma origem de evento a TSI até que tem os dados de referência, especialmente se essa origem de evento tem de dados no mesmo. Em alternativa, pode aguardar para enviar dados para essa origem de evento até o conjunto de dados de referência está no local.

Para gerir os dados de referência, existe a interface de utilizador baseadas na web no Explorador de TSI, e há uma API c# programática. TSI Explorer tem um utilizador visual beneficiar de carregamento de ficheiros ou referência colar existente conjuntos de dados como formato JSON ou CSV. Com a API, pode criar uma aplicação personalizada quando necessário.

Para obter mais informações sobre a gestão de dados de referência no Insights de séries de tempo, consulte o [artigo de referência de dados](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="create-time-series-insights-event-source"></a>Criar a origem de eventos do Time Series Insights
1. Se não tiver criado uma origem de eventos, siga [estas instruções](time-series-insights-how-to-add-an-event-source-eventhub.md) para criá-la.

2. Especifique **deviceTimestamp** como o nome da propriedade timestamp – esta propriedade é utilizada como o timestamp real na amostra c#. O nome da propriedade de carimbo de data/hora é sensível a maiúsculas e minúsculas e os valores têm de estar no formato __aaaa-MM-ddTHH:mm:ss.FFFFFFFK__, se forem enviados como JSON para um hub de eventos. Se a propriedade não existir no evento, é utilizada a hora a que o evento foi colocado em fila no hub de eventos.

  ![Crie a origem de eventos](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a>Código de exemplo para eventos push
1. Vá para a política de hub de eventos com o nome **MySendPolicy**. Copiar o **cadeia de ligação** com a chave de política.

  ![Copie a cadeia de ligação MySendPolicy](media/send-events/sample-code-connection-string.png)

2. Execute o código seguinte que enviará 600 eventos por cada um dos três dispositivos. Atualize `eventHubConnectionString` com a sua cadeia de ligação.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
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

Um objeto JSON com uma matriz JSON aninhada que contenha dois objetos JSON:
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
Tenha em atenção de que a propriedade "localização" é copiada através para cada evento.

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

### <a name="json-shaping-strategies"></a>Estratégias moldar JSON
Vamos utilizar o seguinte exemplo de um evento como inicial ponto e, em seguida, discutir problemas relacionados com ele e estratégias para mitigar os problemas.

#### <a name="payload-1"></a>Payload 1:
```json
[{
            "messageId": "LINE_DATA",
            "deviceId": "FXXX",
            "timestamp": 1522355650620,
            "series": [{
                        "chId": 3,
                        "value": -3750.0
                  }, {
                        "chId": 13,
                        "value": 0.58015072345733643
                  }, {
                        "chId": 11,
                        "value": 800.0
                  }, {
                        "chId": 21,
                        "value": 0.0
                  }, {
                        "chId": 14,
                        "value": -999.0
                  }, {
                        "chId": 37,
                        "value": 2.445906400680542
                  }, {
                        "chId": 39,
                        "value": 0.0
                  }, {
                        "chId": 40,
                        "value": 1.0
                  }, {
                        "chId": 1,
                        "value": 1.0172575712203979
                  }
            ],
            "EventProcessedUtcTime": "2018-03-29T20:36:21.3245900Z",
            "PartitionId": 2,
            "EventEnqueuedUtcTime": "2018-03-29T20:34:11.0830000Z",
            "IoTHub": {
                  "MessageId": "<17xxx2xx-36x0-4875-9x1x-x428x41x1x68>",
                  "CorrelationId": "<x253x5xx-7xxx-4xx3-91x4-xxx3bx2xx0x3>",
                  "ConnectionDeviceId": "AAAA-ZZ-001",
                  "ConnectionDeviceGenerationId": "<123456789012345678>",
                  "EnqueuedTime": "2018-03-29T20:34:10.7990000Z",
                  "StreamId": null
            }
      }
]
 ```

Se enviar esta matriz de eventos como um payload para TSI, será guardado como um evento por cada valor de medida. Se o fizer, pode criar um excesso de eventos, que não pode ser o ideal. Tenha em atenção que pode utilizar os dados de referência na TSI para adicionar nomes significativos como propriedades.  Por exemplo, pode criar o conjunto de dados de referência com a propriedade de chave = chId:  

chId medidas unidade 24 motor petróleo pressão PSI 25 CALC Pump taxa bbl por minuto

Para obter mais informações sobre a gestão de dados de referência no Insights de séries de tempo, consulte o [artigo de referência de dados](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

Outro problema com o primeiro payload é que essa timestamp é em milissegundos. TSI aceita apenas ISO formatado carimbos. É uma solução deixar o comportamento de timestamp predefinido no TSI, que consiste em utilizar timestamp colocados em fila.

Como alternativa para o payload acima, vamos ver outro exemplo.  

#### <a name="payload-2"></a>Payload de 2:
```json
{
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "STATE Engine State": 1,
      "unit": "NONE"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "MPC_AAAA-ZZ-001",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Well Head Px 1": -494162.8515625,
      "unit": "psi"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate": 0,
      "unit": "bbl/min"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Engine Fuel Pressure": 0,
      "unit": "psi"
}, {
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "Engine Oil Pressure": 0.58015072345733643,
      "unit": "psi"
}
```

Como Payload 1, TSI armazenará cada cada valor de medida como um evento exclusivo.  A diferença notória é que será lida a TSI o *timestamp* como corretamente aqui, como ISO.  

Se precisar de reduzir o número de eventos enviados, poderia enviar as informações da seguinte.  

#### <a name="payload-3"></a>Payload 3:
```json
{
      "line": "Line01",
      "station": "Station 11",
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate": 0,
      "CALC Pump Rate.unit": "bbl/min"
      "Engine Oil Pressure": 0.58015072345733643,
      "Engine Oil Pressure.unit": "psi"
      "Engine Fuel Pressure": 0,
      "Engine Fuel Pressure.unit": "psi"
}
```
Uma sugestão final é inferior.

#### <a name="payload-4"></a>Payload 4:
```json
{
              "line": "Line01",
              "station": "Station 11",
              "gatewayid": "AAAA-ZZ-001",
              "deviceid": "F12XX",
              "timestamp": "2018-03-29T20:34:15.0000000Z",
              "CALC Pump Rate": {
                           "value": 0,
                           "unit": "bbl/min"
              },
              "Engine Oil Pressure": {
                           "value": 0.58015072345733643,
                           "unit": "psi"
              },
              "Engine Fuel Pressure": {
                           "value": 0,
                           "unit": "psi"
              }
}
```

Este exemplo mostra a saída após pelo JSON:

```json
{
      "line": "Line01",
      "station": "Station 11",,
      "gatewayid": "AAAA-ZZ-001",
      "deviceid": "F12XX",
      "timestamp": "2018-03-29T20:34:15.0000000Z",
      "CALC Pump Rate.value": 0,
      "CALC Pump Rate.unit": "bbl/min"
      "Engine Oil Pressure.value": 0.58015072345733643,
      "Engine Oil Pressure.unit": "psi"
      "Engine Fuel Pressure.value": 0,
      "Engine Fuel Pressure.unit": "psi"
}
```

Tem a liberdade para definir propriedades diferentes para cada um dos canais no interior do seu próprio objeto json, mantendo a contagem de eventos baixa. Esta abordagem simplificada ocupam mais espaço, que é importante a ter em consideração. Capacidade TSI baseia-se nos eventos e tamanho, o que ocorrer primeiro.

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Ver o seu ambiente no Explorador de informações de séries de tempo](https://insights.timeseries.azure.com).
