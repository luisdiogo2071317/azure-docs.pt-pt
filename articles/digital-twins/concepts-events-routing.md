---
title: Encaminhamento de eventos e mensagens duplos Digital do Azure | Documentos da Microsoft
description: Descrição geral do encaminhamento de eventos e mensagens para pontos finais de serviço duplos Digital do Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 9b861f0991b11637c7b27b645d4506e658ea53b3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324290"
---
# <a name="routing-events-and-messages"></a>Eventos de roteamento e de mensagens

Soluções de IoT unam-se, muitas vezes, vários serviços poderosos, incluindo armazenamento, análise e muito mais. Este artigo descreve como ligar aplicações de duplos Digital do Azure aos serviços de análise, IA e armazenamento do Azure para enriquecê-los com mais informações e funcionalidades.

## <a name="route-types"></a>Tipos de rota

Os gémeos digital oferece duas maneiras de integrar eventos de IoT para outros serviços do Azure ou para aplicativos de negócios:

* **Eventos de roteamento duplos Digital**: eventos de duplos Digital do Azure podem ser acionados quando um objeto no gráfico espaciais é alterada, quando forem recebidos dados de telemetria ou quando uma função definida pelo usuário cria uma notificação com base nas condições predefinidas. Os utilizadores podem enviar estes eventos para [os Hubs de eventos](https://azure.microsoft.com/services/event-hubs/), [tópicos do Service Bus](https://azure.microsoft.com/services/service-bus/), ou [grades de evento](https://azure.microsoft.com/services/event-grid/) para processamento adicional.

* **Telemetria do dispositivo de encaminhamento**: Além de eventos de roteamento, duplos Digital do Azure pode também encaminhar mensagens de telemetria do dispositivo sem formato para os Hubs de eventos para obter mais informações e análises. Esses tipos de mensagens não são processados por duplos Digital e apenas serem encaminhados para o **Hub de eventos**.

Os utilizadores podem especificar um ou mais pontos finais de saída para enviar eventos ou para reencaminhar mensagens. Eventos e de mensagens serão enviadas para os pontos finais de acordo com essas preferências de encaminhamento predefinidos. Em outras palavras, os utilizadores podem especificar determinado um ponto final para receber eventos da operação de gráfico, outro para receber eventos de telemetria do dispositivo e assim por diante.

![Encaminhamento de eventos de duplos digital][1]

Roteamento **os Hubs de eventos** manterá a ordem na qual telemetria as mensagens são enviadas, para que elas cheguem ao ponto de extremidade na mesma sequência como foram originalmente recebidos. **Event Grid** e **do Service Bus** não garantem que os pontos finais vão receber eventos na mesma ordem em que ocorreu. No entanto, o esquema de eventos incluem um carimbo que pode ser utilizado para identificar a ordem após os eventos deparar-se com o ponto final.

## <a name="route-implementation"></a>Implementação de rota

O serviço de duplos Digital do Azure suporta atualmente o seguinte procedimento **EndpointTypes**:

* **EventHub**: é o ponto de final do cadeia de ligação do Hub de eventos.
* **ServiceBus**: é o ponto de final de cadeia de ligação do Service Bus.
* **EventGrid**: é o ponto de final de cadeia de ligação Event Grid.

Atualmente, o duplos Digital do Azure suporta o seguinte **EventTypes** que serão enviadas para o ponto de final escolhido:

* **DeviceMessages**: são mensagens de telemetria enviadas a partir de dispositivos dos utilizadores e reencaminhadas pelo sistema.
* **TopologyOperation**: são operações que alterar o gráfico ou metadados do gráfico. Por exemplo, adicionar ou excluir uma entidade, como um espaço.
* **SpaceChange**: são as alterações num espaço calculada valor como resultado de uma mensagem de telemetria do dispositivo.
* **SensorChange**: são as alterações num sensor calculada valor como resultado de uma mensagem de telemetria do dispositivo.
* **UdfCustom**: são notificações personalizadas de uma função definida pelo utilizador.

> [!IMPORTANT]
> Nem todos os **EndpointTypes** suportam todos **EventTypes**.
> Consulte a tabela abaixo para o **EventTypes** que têm permissão para cada **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| **EventHub**|     X          |         X         |     X       |      X       |   X       |
| **serviceBus**|              |         X         |     X       |      X       |   X       |
| **EventGrid**|               |         X         |     X       |      X       |   X       |

>[!NOTE]
>Para obter mais detalhes sobre como criar pontos de extremidade e exemplos de esquema dos eventos, consulte [pontos de extremidade e de saída](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os limites de pré-visualização pública, leia [limites de pré-visualização de duplos Digital do Azure](concepts-service-limits.md).

Para experimentar um exemplo de duplos Digital do Azure, leia [guia de introdução de salas disponíveis](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png