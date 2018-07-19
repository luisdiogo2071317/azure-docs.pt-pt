---
title: Compreender a mensagens de dispositivo-para-cloud do IoT Hub do Azure | Documentos da Microsoft
description: Guia para programadores - como utilizar mensagens de dispositivo para a cloud com o IoT Hub. Inclui informações sobre o envio de dados de telemetria e não telemtry e, usando o roteamento para entregar mensagens.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: be87b00f27f0d0b25cd77a0634ab1c653a85e5ac
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126447"
---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a>Enviar mensagens de dispositivo para a nuvem ao IoT Hub

Para enviar telemetria de séries temporais e alertas dos seus dispositivos para a sua solução de back-end, envie mensagens de dispositivo para a cloud a partir do seu dispositivo ao seu hub IoT. Para uma discussão sobre outras opções de dispositivo-para-cloud suportado pelo IoT Hub, veja [documentação de orientação do dispositivo-para-cloud comunicações][lnk-d2c-guidance].

Enviar mensagens dispositivo-para-cloud através de um ponto de final voltado para o dispositivo (**/devices/ {deviceId} / mensagens/eventos**). Regras de encaminhamento, em seguida, encaminhe suas mensagens para um dos pontos finais com acesso de serviço no seu hub IoT. Regras de encaminhamento usam os cabeçalhos e o corpo das mensagens dispositivo-para-cloud para determinar onde roteá-los. Por predefinição, as mensagens são roteadas para o ponto de final de serviço com acesso à incorporado (**mensagens/eventos**), que é compatível com [dos Hubs de eventos][lnk-event-hubs]. Portanto, pode usar o padrão [integração dos Hubs de eventos e SDKs] [ lnk-compatible-endpoint] para receber mensagens dispositivo-para-cloud no seu back-end de solução.

IoT Hub implementa com um padrão de mensagens de transmissão em fluxo de mensagens do dispositivo para a cloud. Mensagens do dispositivo-para-cloud do IoT Hub são mais semelhantes [os Hubs de eventos] [ lnk-event-hubs] *eventos* que [do Service Bus] [ lnk-servicebus] *mensagens* em que existe um grande volume de eventos, passando por meio do serviço que pode ser lido por vários leitores.

Dispositivo-para-cloud mensagens com o IoT Hub com as seguintes características:

* Mensagens dispositivo-para-cloud são mantidos no padrão de um hub IoT e durável **mensagens/eventos** ponto final até sete dias.
* Mensagens do dispositivo para a cloud podem ter um máximo de 256 KB e podem ser agrupadas em lotes para otimizar envia. Lotes podem ser, no máximo, 256 KB.
* Conforme explicado no [controlar o acesso ao IoT Hub] [ lnk-devguide-security] secção, o IoT Hub permite o controlo de acesso e autenticação por dispositivo.
* IoT Hub permite-lhe criar até 10 pontos de extremidade personalizados. As mensagens são entregues para os pontos finais com base nas rotas configuradas no seu hub IoT. Para obter mais informações, consulte [regras de encaminhamento](iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).
* O IoT Hub permite milhões de dispositivos ligados em simultâneo (consulte [Quotas e limitação][lnk-quotas]).
* IoT Hub não permite a criação de partições arbitrário. Mensagens do dispositivo para cloud são particionadas com base no respetivo tipo de origem **deviceId**.

Para obter mais informações sobre as diferenças entre o IoT Hub e dos Hubs de eventos, consulte [comparação do IoT Hub do Azure e Hubs de eventos][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Enviar o tráfego de sem ser de telemetria

Muitas vezes, além de telemetria, os dispositivos enviam mensagens e pedidos que necessitem de execução separada e manipulação na solução de back-end. Por exemplo, alertas críticos que devem acionar uma ação específica no back-end. Pode escrever um [regra de encaminhamento] [ lnk-devguide-custom] para enviar esses tipos de mensagens para um ponto de extremidade dedicado para o processamento em fluxo com base no cabeçalho da mensagem ou um valor no corpo da mensagem.

Para obter mais informações sobre a melhor maneira de processar esse tipo de mensagem, consulte a [Tutorial: como processar mensagens de dispositivo-para-cloud do IoT Hub] [ lnk-d2c-tutorial] tutorial.

## <a name="route-device-to-cloud-messages"></a>Encaminhar mensagens do dispositivo para a cloud

Tem duas opções para encaminhar mensagens dispositivo-para-cloud às suas aplicações de back-end:

* Utilizar o incorporado [ponto final de compatível com o Event Hub] [ lnk-compatible-endpoint] para ativar aplicações de back-end ler as mensagens de dispositivo-para-cloud recebidas pelo hub. Para saber mais sobre o ponto final compatível com o Event Hub incorporado, veja [ler mensagens do dispositivo para cloud a partir do ponto final incorporado][lnk-devguide-builtin].
* Utilize as regras de encaminhamento para enviar mensagens para os pontos finais personalizados no seu IoT hub. Os pontos finais personalizados permitem que as aplicações de back-end permite ler mensagens do dispositivo para a cloud com os Hubs de eventos, filas do Service Bus ou tópicos do Service Bus. Para saber mais sobre pontos finais de encaminhamento e personalizados, consulte [utilizar os pontos finais personalizados e regras de encaminhamento para mensagens de dispositivo-para-cloud][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Propriedades anti-spoofing

Para evitar falsificação nas mensagens do dispositivo para a cloud, IoT Hub do dispositivo os carimbos de todas as mensagens com as seguintes propriedades:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Os dois primeiros contêm os **deviceId** e **generationId** do dispositivo de origem, como por [propriedades de identidade de dispositivo][lnk-device-properties].

O **ConnectionAuthMethod** propriedade contém um objeto JSON serializado com as seguintes propriedades:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Passos Seguintes

Para informações sobre os SDKs que pode utilizar para enviar mensagens do dispositivo para a nuvem, consulte [SDKs do Azure IoT][lnk-sdks].

O [inícios Rápidos] [ lnk-get-started] mostram-lhe como enviar mensagens dispositivo-para-cloud a partir de dispositivos simulados. Para obter mais detalhes, consulte a [mensagens de dispositivo-para-cloud do Hub de IoT de processo utilizar rotas] [ lnk-d2c-tutorial] tutorial.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: quickstart-send-telemetry-node.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: tutorial-routing.md
