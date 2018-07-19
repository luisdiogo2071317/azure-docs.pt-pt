---
title: Compreender o ponto de final incorporado do IoT Hub do Azure | Documentos da Microsoft
description: Guia do desenvolvedor – descreve como utilizar as mensagens de dispositivo para a nuvem à marca de ponto final do incorporada, compatível com o Event Hub.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 912bb0dd3e48e53134ad848119ae7428b380b88d
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39124941"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Ler mensagens do dispositivo para cloud a partir do ponto final incorporado

Por predefinição, as mensagens são roteadas para o ponto de final de serviço com acesso à incorporado (**mensagens/eventos**) que é compatível com [dos Hubs de eventos][lnk-event-hubs]. Este ponto final está apenas exposto a utilizar atualmente o [AMQP] [ lnk-amqp] protocolo na porta 5671. Um IoT hub expõe as propriedades seguintes para que possa controlar compatível com o Event Hub mensagens ponto final incorporado **mensagens/eventos**.

| Propriedade            | Descrição |
| ------------------- | ----------- |
| **Contagem de partições** | Defina esta propriedade durante a criação para definir o número de [partições] [ lnk-event-hub-partitions] para ingestão de eventos do dispositivo para a cloud. |
| **Período de retenção**  | Esta propriedade especifica o período de tempo em dias as mensagens são mantidas pelo IoT Hub. A predefinição é um dia, mas ele pode ser aumentado para sete dias. |

IoT Hub também permite-lhe gerir grupos de consumidores sobre o dispositivo para cloud incorporado recebem o ponto final.

Por predefinição, todas as mensagens que não coincidem explicitamente uma regra de roteamento de mensagem são escritas para o ponto final incorporado. Se desativar esta rota de contingência, as mensagens que não coincidem explicitamente quaisquer regras de roteamento de mensagem são ignoradas.

Pode modificar o período de retenção, por meio de programação utilizando o [fornecedor de recursos do IoT Hub REST APIs][lnk-resource-provider-apis], ou com o [portal do Azure] [ lnk-management-portal].

IoT Hub expõe os **mensagens/eventos** ponto final incorporado para os seus serviços de back-end ler as mensagens de dispositivo-para-cloud recebidas pelo hub. Este ponto final é o evento compatível com o Hub, que permite-lhe utilizar qualquer um dos mecanismos do serviço de Hubs de eventos oferece suporte para a leitura de mensagens.

## <a name="read-from-the-built-in-endpoint"></a>Ler a partir do ponto final incorporado

Quando utiliza a [SDK do Service Bus para .NET] [ lnk-servicebus-sdk] ou o [Hubs de eventos - anfitrião do processador de eventos][lnk-eventprocessorhost], pode utilizar qualquer ligação do IoT Hub cadeias de caracteres com as permissões corretas. Em seguida, utilize **mensagens/eventos** como o nome do Hub de eventos.

Ao utilizar SDKs (ou integrações de produtos) que não estão cientes do IoT Hub, tem de obter um ponto de final compatível com o Event Hub e o nome compatível com o Event Hub:

1. Inicie sessão para o [portal do Azure] [ lnk-management-portal] e navegue até ao seu hub IoT.
1. Clique em **Pontos Finais**.
1. Na **pontos finais incorporados** secção, clique em **eventos**. 
1. É aberta uma página de propriedades, que contém os seguintes valores: **ponto final de compatível com o Event Hub**, **nome compatível com o Event Hub**, **partições**,  **Período de retenção**, e **grupos de consumidores**.

    ![Definições do dispositivo para a nuvem][img-eventhubcompatible]

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

Para obter mais informações sobre pontos finais do IoT Hub, veja [pontos finais do IoT Hub][lnk-endpoints].

O [inícios Rápidos] [ lnk-get-started] mostram-lhe como enviar mensagens dispositivo-para-cloud a partir de dispositivos simulados e ler as mensagens a partir do ponto final incorporado. Para obter mais detalhes, consulte a [mensagens de dispositivo-para-cloud do Hub de IoT de processo utilizar rotas] [ lnk-d2c-tutorial] tutorial.

Se quiser encaminhe suas mensagens do dispositivo para a cloud para os pontos finais personalizados, veja [utilizar rotas de mensagens e os pontos finais personalizados para mensagens de dispositivo-para-cloud][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
