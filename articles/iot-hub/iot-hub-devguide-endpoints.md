---
title: Compreender os pontos finais do IoT Hub do Azure | Documentos da Microsoft
description: Guia do programador - informações de referência sobre o IoT Hub pontos finais com acesso de dispositivo e voltado para o serviço.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: e6b4ee3425f6a490f33f998cab4f33734b23df22
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982108"
---
# <a name="reference---iot-hub-endpoints"></a>Referência - pontos finais do IoT Hub

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>Nomes do IoT Hub

Pode encontrar o nome de anfitrião do hub IoT que aloja os pontos finais no portal no seu hub **descrição geral** página. Por predefinição, o nome DNS de um hub IoT é semelhante a: `{your iot hub name}.azure-devices.net`.

Pode utilizar o DNS do Azure para criar um nome DNS personalizado para o seu hub IoT. Para obter mais informações, veja [Utilizar o DNS do Azure para oferecer definições de domínio personalizado para um serviço do Azure](../dns/dns-custom-domain.md).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lista de pontos finais incorporados do IoT Hub

O IoT Hub do Azure é um serviço de multi-inquilino, que expõe sua funcionalidade para vários atores. O diagrama seguinte mostra vários pontos de extremidade que expõe o IoT Hub.

![Pontos finais do Hub IoT][img-endpoints]

A lista seguinte descreve os pontos de extremidade:

* **Fornecedor de recursos**. O fornecedor de recursos do IoT Hub expõe um [do Azure Resource Manager] [ lnk-arm] interface. Essa interface permite que os proprietários de subscrições do Azure para criar e eliminar os hubs IoT e para atualizar as propriedades do hub IoT. As propriedades do IoT Hub regem [políticas de segurança ao nível do hub][lnk-accesscontrol], em vez de controlo de acesso ao nível do dispositivo e as opções de funcionais para mensagens na cloud para o dispositivo e dispositivo-para-cloud. O fornecedor de recursos do IoT Hub também permite-lhe [Exportar identidades do dispositivo][lnk-importexport].
* **Gestão de identidades de dispositivo**. Cada hub IoT expõe um conjunto de pontos finais de HTTPS REST para gerir identidades de dispositivos (criar, obter, atualizar e eliminar). [Identidades do dispositivo] [ lnk-device-identities] são utilizados para controlo de acesso e autenticação do dispositivo.
* **Gestão do dispositivo duplo**. Cada hub IoT expõe um conjunto de ponto final de REST de HTTPS de serviço com acesso à consulta e atualização [dispositivos duplos] [ lnk-twins] (etiquetas e propriedades de atualização).
* **Tarefas de gestão**. Cada hub IoT expõe um conjunto de ponto final de REST de HTTPS de serviço com acesso à consultar e gerir [trabalhos][lnk-jobs].
* **Pontos finais do dispositivo**. Para cada dispositivo no registo de identidade, o IoT Hub expõe um conjunto de pontos de extremidade:

  * *Enviar mensagens dispositivo-para-cloud*. Um dispositivo utiliza este ponto final para [enviar mensagens dispositivo-para-cloud][lnk-d2c].
  * *Receber mensagens da cloud para dispositivo*. Um dispositivo utiliza este ponto final para receber um alvo [mensagens na cloud para o dispositivo][lnk-c2d].
  * *Iniciar carregamentos de ficheiros*. Um dispositivo utiliza este ponto final para receber um URI de SAS do armazenamento do Azure a partir do IoT Hub para [carregar um ficheiro][lnk-upload].
  * *Obter e atualizar as propriedades dos dispositivos duplos*. Um dispositivo utiliza este ponto final para aceder aos respetivos [dispositivo duplo][lnk-twins]do propriedades.
  * *Receber pedidos de método direto*. Um dispositivo utiliza este ponto final para escutar [método direto][lnk-methods]do pedidos.

    Estes pontos finais expostos por meio [MQTT v3.1.1][lnk-mqtt], HTTPS 1.1, e [AMQP 1.0] [ lnk-amqp] protocolos. Também está disponível através de AMQP [WebSockets] [ lnk-websockets] na porta 443.

* **Pontos finais de serviço**. Cada hub IoT expõe um conjunto de pontos finais para a sua solução de back-end comunicar com os seus dispositivos. Com uma exceção, estes pontos finais são expostos apenas com o [AMQP] [ lnk-amqp] protocolo. O ponto final de invocação de método é exposto através do protocolo HTTPS.
  
  * *Receber mensagens dispositivo-para-cloud*. Este ponto final é compatível com [os Hubs de eventos do Azure][lnk-event-hubs]. Um serviço de back-end pode utilizá-lo para ler os [mensagens dispositivo-para-cloud] [ lnk-d2c] enviados pelos seus dispositivos. Pode criar pontos finais personalizados no seu hub IoT para além deste ponto final incorporado.
  * *Enviar mensagens da cloud para dispositivo e receber confirmações de entrega*. Estes pontos finais ativar a sua solução de back-end enviar fiável [mensagens na cloud para o dispositivo][lnk-c2d]e para receber o confirmações de expiração ou entrega correspondente.
  * *Receber notificações de ficheiro*. Este ponto final de mensagens permite-lhe receber notificações de quando os dispositivos com êxito a carregar um ficheiro. 
  * *Direcionar a invocação de método*. Este ponto final permite que um serviço de back-end invocar um [método direto] [ lnk-methods] num dispositivo.
  * *Receber eventos de monitorização de operações*. Este ponto final permite-lhe receber eventos de monitorização, se seu hub IoT tiver sido configurado para emiti-los de operações. Para obter mais informações, consulte [monitorização de operações do IoT Hub][lnk-operations-mon].

O [SDKs do Azure IoT] [ lnk-sdks] artigo descreve as várias formas de aceder a estes pontos finais.

Todos os pontos de extremidade do IoT Hub utilizam o [TLS] [ lnk-tls] protocolo e nenhum ponto final nunca é exposto em canais não encriptados/desprotegido.

## <a name="custom-endpoints"></a>Pontos finais personalizados

Pode ligar os serviços do Azure existentes na sua subscrição ao seu hub IoT para atuar como pontos finais para encaminhamento de mensagens. Estes pontos finais funcionam como pontos finais de serviço e são utilizados como sinks para rotas de mensagens. Dispositivos não é possível escrever diretamente para os pontos finais adicionais. Saiba mais sobre [roteamento de mensagens](../iot-hub/iot-hub-devguide-messages-d2c.md).

Atualmente, o IoT Hub suporta os seguintes serviços do Azure como pontos finais adicionais:

* Contentores de armazenamento do Azure
* Hubs de Eventos
* Filas de Service Bus
* Tópicos de Service Bus

Para os limites no número de pontos de extremidade pode adicionar, ver [Quotas e limitação][lnk-devguide-quotas].

## <a name="field-gateways"></a>Gateways de campo

Numa solução de IoT, um *gateway de campo* encontra-se entre os dispositivos e os pontos finais do IoT Hub. Ele está normalmente localizado perto dos seus dispositivos. Os dispositivos comunicam diretamente com o gateway de campo com um protocolo suportado pelos dispositivos. O gateway de campo liga-se para um ponto de final do IoT Hub com um protocolo suportado pelo IoT Hub. Um gateway de campo pode ser um dispositivo de hardware dedicado ou um computador de baixa energia com o software de gateway personalizada.

Pode usar [do Azure IoT Edge] [ lnk-iot-edge] para implementar um gateway de campo. IoT Edge oferece funcionalidade como comunicações de multiplexação de vários dispositivos para a mesma ligação do IoT Hub.

## <a name="next-steps"></a>Passos Seguintes

Outros tópicos de referência neste guia de programador do IoT Hub incluem:

* [Linguagem de consulta do IoT Hub para dispositivos duplos, tarefas e encaminhamento de mensagens][lnk-devguide-query]
* [Quotas e limitação][lnk-devguide-quotas]
* [Suporte para MQTT de Hub IoT][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
