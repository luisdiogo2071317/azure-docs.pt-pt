---
title: Origens de eventos de grelha de eventos do Azure
description: Descreve as origens de eventos suportados para a grelha de eventos do Azure
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: tomfitz
ms.openlocfilehash: f9c3bcb6b92b43fe5b5bad72c99e6ce199c17448
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34302131"
---
# <a name="event-sources-in-azure-event-grid"></a>Origens de eventos na grelha de eventos do Azure

Uma origem de evento é onde ocorre o evento. Vários serviços do Azure são automaticamente configurados para enviar eventos. Também pode criar aplicações personalizadas que enviam eventos. Aplicações personalizadas não precisam de ser alojado no Azure para utilizar a grelha de eventos para distribuição de eventos.

Este artigo fornece ligações para conteúdo para cada origem de evento.

## <a name="azure-subscriptions"></a>Subscrições do Azure

Subscreve eventos de subscrições do Azure para responder a alterações em recursos através de uma subscrição do Azure.

|Cargo |Descrição  |
|---------|---------|
| [Integração da automatização do Azure com grelha de eventos e equipas da Microsoft](ensure-tags-exists-on-new-virtual-machines.md) |Crie uma máquina virtual, o que envia um evento. O evento é acionado um runbook de automatização que a máquina virtual de etiquetas e aciona uma mensagem que é enviada para um canal Teams da Microsoft. |
| [Esquema de eventos](event-schema-subscriptions.md) | Mostra os campos em eventos de subscrição do Azure. |

## <a name="custom-topics"></a>Tópicos personalizados

Subscreve tópicos personalizados para responder a eventos de aplicações.

|Cargo  |Descrição  |
|---------|---------|
| [Criar e encaminhar eventos personalizados com a CLI do Azure](custom-event-quickstart.md) | Mostra como utilizar a CLI do Azure para enviar eventos personalizados. |
| [Criar e encaminhar eventos personalizados com o Azure PowerShell](custom-event-quickstart-powershell.md) | Mostra como utilizar o Azure PowerShell para enviar eventos personalizados. |
| [Criar e encaminhar eventos personalizados com o portal do Azure](custom-event-quickstart-portal.md) | Mostra como utilizar o portal para enviar eventos personalizados. |
| [Publicar tópico personalizado](post-to-custom-topic.md) | Mostra como publicar um evento para um tópico personalizado. |
| [Eventos personalizados de rota para o armazenamento de filas do Azure](custom-event-to-queue-storage.md) | Descreve como enviar eventos personalizados para um armazenamento de filas. |
| [Esquema de eventos](event-schema.md) | Mostra os campos de eventos personalizados. |

## <a name="event-hubs"></a>Event Hubs

Subscreve eventos de Event Hubs para responder a eventos de ficheiro de captura.

|Cargo  |Descrição  |
|---------|---------|
| [Transmitir macrodados em fluxo para um armazém de dados](event-grid-event-hubs-integration.md) | Quando os Event Hubs cria um ficheiro de captura, o evento grelha enviará um evento para uma aplicação de função. A aplicação obtém o ficheiro de captura e migra dados para um armazém de dados. |
| [Esquema de eventos](event-schema-event-hubs.md) | Mostra os campos em eventos de Event Hubs. |

## <a name="iot-hub"></a>IoT Hub

Subscrever ao IoT Hub eventos para responder ao dispositivo criado e eliminar eventos.

|Cargo  |Descrição  |
|---------|---------|
| [Enviar notificações por e-mail sobre eventos de IoT Hub do Azure utilizando as Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Uma aplicação lógica envia um e-mail de notificação, sempre que um dispositivo é adicionado ao seu IoT hub. |
| [Reagir a eventos de IoT Hub, utilizando a grelha de eventos para ações de Acionador](../iot-hub/iot-hub-event-grid.md) | Descrição geral de integrar os Hubs Iot a grelha de eventos. |
| [Esquema de eventos](event-schema-iot-hub.md) | Mostra os campos do IoT Hub de eventos. |

## <a name="media-services"></a>Serviços de Multimédia

Subscreve eventos de Media Services para responder a eventos de estado de tarefa.

|Cargo  |Descrição  |
|---------|---------|
| [A agir os eventos de Media Services](../media-services/latest/reacting-to-media-services-events.md) | Descrição geral da integração dos Media Services com o evento grelha. |
| [Eventos de Media Services do Azure de rota para um ponto final de web personalizado ao utilizar a CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Mostra como enviar eventos a partir dos Media Services. |
| [Esquema de eventos](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Mostra os campos em eventos de Media Services. |

## <a name="resource-groups"></a>Grupos de recursos

Subscreve eventos do grupo de recursos para responder a alterações em recursos através de um grupo de recursos.

|Cargo  |Descrição  |
|---------|---------|
| [Monitorizar alterações de máquina virtual com o Azure Event Grid e Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) | Uma aplicação lógica monitoriza as alterações a uma máquina virtual e envia mensagens de correio eletrónico sobre essas alterações. |
| [Esquema de Eventos](event-schema-resource-groups.md) | Mostra os campos no recurso de eventos de grupo. |

## <a name="service-bus"></a>Service Bus

Subscreve eventos de Service Bus para responder a mensagens sem um serviço de escuta do Active Directory.

|Cargo  |Descrição  |
|---------|---------|
| [Service Bus do Azure para exemplos de integração da grelha de eventos do Azure](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Grelha de eventos envia mensagens do tópico de barramento de serviço para a função de aplicação e aplicação lógica. |
| [Service Bus do Azure para descrição geral da integração de grelha de eventos](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Descrição geral da integração de Service Bus com o evento grelha. |
| [Esquema de eventos](event-schema-service-bus.md) | Mostra os campos em eventos de Service Bus. |

## <a name="storage"></a>Armazenamento

Subscreve eventos de armazenamento de BLOBs para responder a eventos criados e eliminados do blob.

|Cargo  |Descrição  |
|---------|---------|
| [Eventos de armazenamento de BLOBs de rota para um ponto de final web personalizado com a CLI do Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Mostra como utilizar a CLI do Azure para enviar eventos de armazenamento de Blobs. |
| [Eventos de armazenamento de BLOBs de rota para um ponto de final web personalizado com o PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Mostra como utilizar o Azure PowerShell para enviar eventos de armazenamento de Blobs. |
| [Reagir aos eventos de armazenamento de Blobs](../storage/blobs/storage-blob-event-overview.md) | Descrição geral da integração de armazenamento de Blobs com o evento grelha. |
| [Esquema de eventos](event-schema-blob-storage.md) | Mostra os campos em eventos de armazenamento de Blobs. |

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
