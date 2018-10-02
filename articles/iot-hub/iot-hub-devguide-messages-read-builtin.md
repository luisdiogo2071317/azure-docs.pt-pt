---
title: Compreender o ponto de final incorporado do IoT Hub do Azure | Documentos da Microsoft
description: Guia do desenvolvedor – descreve como utilizar o ponto final compatível com o Event Hub incorporado, para ler mensagens do dispositivo para a cloud.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: e43d00fde0f76efa4398865757c44d94592b8291
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585516"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Ler mensagens de dispositivo para cloud a partir do ponto final incorporado

Por predefinição, as mensagens são roteadas para o ponto de final de serviço com acesso à incorporado (**mensagens/eventos**) que é compatível com [dos Hubs de eventos](http://azure.microsoft.com/documentation/services/event-hubs/
). Este ponto final está apenas exposto a utilizar atualmente o [AMQP](https://www.amqp.org/) protocolo na porta 5671. Um IoT hub expõe as propriedades seguintes para que possa controlar compatível com o Event Hub mensagens ponto final incorporado **mensagens/eventos**.

| Propriedade            | Descrição |
| ------------------- | ----------- |
| **Contagem de partições** | Defina esta propriedade durante a criação para definir o número de [partições](../event-hubs/event-hubs-features.md#partitions) para ingestão de eventos do dispositivo para a cloud. |
| **Período de retenção**  | Esta propriedade especifica o período de tempo em dias as mensagens são mantidas pelo IoT Hub. A predefinição é um dia, mas ele pode ser aumentado para sete dias. |

IoT Hub também permite-lhe gerir grupos de consumidores sobre o dispositivo para cloud incorporado recebem o ponto final.

Se estiver a utilizar [roteamento de mensagens](iot-hub-devguide-messages-d2c.md) e o [rota contingência](iot-hub-devguide-messages-d2c.md#fallback-route) é ativada, todas as mensagens que não correspondam a uma consulta em qualquer rota são escritas para o ponto final incorporado. Se desativar esta rota de contingência, as mensagens que não correspondam a qualquer consulta são ignoradas.

Pode modificar o período de retenção, por meio de programação utilizando o [fornecedor de recursos do IoT Hub REST APIs](/rest/api/iothub/iothubresource), ou com o [portal do Azure](https://portal.azure.com).

IoT Hub expõe os **mensagens/eventos** ponto final incorporado para os seus serviços de back-end ler as mensagens de dispositivo-para-cloud recebidas pelo hub. Este ponto final é o evento compatível com o Hub, que permite-lhe utilizar qualquer um dos mecanismos do serviço de Hubs de eventos oferece suporte para a leitura de mensagens.

## <a name="read-from-the-built-in-endpoint"></a>Ler a partir do ponto final incorporado

Quando utiliza a [SDK do Service Bus para .NET](https://www.nuget.org/packages/WindowsAzure.ServiceBus) ou o [Hubs de eventos - anfitrião do processador de eventos](..//event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md), pode utilizar quaisquer cadeias de ligação do IoT Hub com as permissões corretas. Em seguida, utilize **mensagens/eventos** como o nome do Hub de eventos.

Ao utilizar SDKs (ou integrações de produtos) que não estão cientes do IoT Hub, tem de obter um ponto de final compatível com o Event Hub e o nome compatível com o Event Hub:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) e navegue até ao seu hub IoT.

2. Clique em **pontos finais incorporados**.

3. O **eventos** secção contém os seguintes valores: **endpoint compatível com o Event Hub**, **nome compatível com o Event Hub**, **partições**, **Período de retenção**, e **grupos de consumidores**.

    ![Definições do dispositivo para a nuvem](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

O SDK do Hub IoT requer o nome de ponto final do IoT Hub, que é **mensagens/eventos** conforme mostrado na **pontos de extremidade**.

Se o SDK que está a utilizar requer uma **Hostname** ou **espaço de nomes** valor, remova o esquema do **endpoint compatível com o Event Hub**. Por exemplo, se for de ponto final compatível com o Event Hub **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, o **nome do anfitrião** seria  **iothub-ns-myiothub-1234.servicebus.windows.net**. O **espaço de nomes** seria **iothub-ns-myiothub-1234**.

Em seguida, pode utilizar qualquer política de acesso partilhado que tenha as **ServiceConnect** permissões para ligar ao Hub de eventos especificado.

Se precisar de criar uma cadeia de ligação do Hub de eventos com as informações anteriores, utilize o seguinte padrão:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

Os SDKs e integrações que pode utilizar com pontos finais de compatível com o Event Hub IoT Hub expõe inclui os itens na lista seguinte:

* [Cliente de Java dos Hubs de eventos](https://github.com/Azure/azure-event-hubs-java).
* [Spout do Apache Storm](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Pode ver o [spout origem](https://github.com/apache/storm/tree/master/external/storm-eventhubs) no GitHub.
* [Integração do Apache Spark](../hdinsight/spark/apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre pontos finais do IoT Hub, veja [pontos finais do IoT Hub](iot-hub-devguide-endpoints.md).

* O [inícios Rápidos](quickstart-send-telemetry-node.md) mostram-lhe como enviar mensagens dispositivo-para-cloud a partir de dispositivos simulados e ler as mensagens a partir do ponto final incorporado. 

Para obter mais detalhes, consulte a [mensagens de dispositivo-para-cloud do Hub de IoT de processo utilizar rotas](tutorial-routing.md) tutorial.

* Se quiser encaminhe suas mensagens do dispositivo para a cloud para os pontos finais personalizados, veja [utilizar rotas de mensagens e os pontos finais personalizados para mensagens de dispositivo-para-cloud](iot-hub-devguide-messages-read-custom.md).