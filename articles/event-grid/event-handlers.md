---
title: Processadores de eventos de grelha de eventos do Azure
description: Descreve os processadores de eventos suportados para a grelha de eventos do Azure
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: tomfitz
ms.openlocfilehash: 7c012bdf025a352788aec2d2d70bab33d7914577
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849547"
---
# <a name="event-handlers-in-azure-event-grid"></a>Processadores de eventos na grelha de eventos do Azure

Um processador de eventos é o local onde o evento é enviado. O processador demora algumas medidas adicionais para processar o evento. Vários serviços do Azure são automaticamente configurados para processar eventos. Também pode utilizar quaisquer webhook para processar eventos. O webhook não precisa de ser alojado no Azure para processar eventos. Grelha de evento só suporta pontos finais de webhook HTTPS.

Este artigo fornece ligações para conteúdo para o processador de eventos.

## <a name="azure-automation"></a>Automatização do Azure

Utilize a automatização do Azure para processar eventos com runbooks automatizados.

|Cargo  |Descrição  |
|---------|---------|
|[Integração da automatização do Azure com grelha de eventos e equipas da Microsoft](ensure-tags-exists-on-new-virtual-machines.md) |Crie uma máquina virtual, o que envia um evento. O evento é acionado um runbook de automatização que a máquina virtual de etiquetas e aciona uma mensagem que é enviada para um canal Teams da Microsoft. |

## <a name="azure-functions"></a>Funções do Azure

Utilize as funções do Azure para resposta sem servidor eventos.

Se utilizar as Funções do Azure como o processador, utilize o acionador do Event Grid em vez dos acionadores HTTP genéricos. O Event Grid valida automaticamente os acionadores de função do Event Grid. Com os acionadores HTTP genéricos, tem de implementar a [resposta de validação](security-authentication.md#webhook-event-delivery).

|Cargo  |Descrição  |
|---------|---------|
| [Acionador de grelha de eventos para as funções do Azure](../azure-functions/functions-bindings-event-grid.md) | Descrição geral de utilizar o acionador de grelha de evento nas funções. |
| [Utilizar o Event Grid para automatizar o redimensionamento de imagens carregadas](resize-images-on-storage-blob-upload-event.md) | Utilizadores carregar imagens através da aplicação web para a conta de armazenamento. Quando é criado um blob de armazenamento, o evento grelha enviará um evento para a aplicação de função, redimensiona a imagem carregada. |
| [Transmitir macrodados em fluxo para um armazém de dados](event-grid-event-hubs-integration.md) | Quando os Event Hubs cria um ficheiro de captura, o evento grelha enviará um evento para uma aplicação de função. A aplicação obtém o ficheiro de captura e migra dados para um armazém de dados. |
| [Service Bus do Azure para exemplos de integração da grelha de eventos do Azure](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Grelha de eventos envia mensagens do tópico de barramento de serviço para a função de aplicação e aplicação lógica. |

## <a name="event-hubs"></a>Event Hubs

Utilize os Hubs de eventos quando a sua solução obtém os eventos mais rapidamente do que pode processar os eventos. A aplicação processa os eventos provenientes dos Hubs de eventos-la própria agenda. Pode dimensionar o seu processamento para processar os eventos de entrada de eventos.

|Cargo  |Descrição  |
|---------|---------|
| [Encaminhar eventos personalizados para os Hubs de eventos do Azure com a CLI do Azure e a grelha de eventos](custom-event-to-eventhub.md) | Envia um evento personalizado para um hub de eventos para processamento por uma aplicação. |

## <a name="hybrid-connections"></a>Ligações Híbridas

Utilize as ligações de híbridas de reencaminhamento do Azure para enviar eventos para aplicações que estão dentro de uma rede empresarial e não tem um ponto final acessível publicamente.

|Cargo  |Descrição  |
|---------|---------|
| [Enviar eventos da ligação híbrida](custom-event-to-hybrid-connection.md) | Envia um evento personalizado para uma ligação híbrida existente para processar uma aplicação de serviço de escuta. |

## <a name="logic-apps"></a>Aplicações Lógicas

Utilize as Logic Apps para automatizar os processos de negócio para responder a eventos.

|Cargo  |Descrição  |
|---------|---------|
| [Monitorizar alterações de máquina virtual com o Azure Event Grid e Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) | Uma aplicação lógica monitoriza as alterações a uma máquina virtual e envia mensagens de correio eletrónico sobre essas alterações. |
| [Enviar notificações por e-mail sobre eventos de IoT Hub do Azure utilizando as Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Uma aplicação lógica envia um e-mail de notificação, sempre que um dispositivo é adicionado ao seu IoT hub. |
| [Service Bus do Azure para exemplos de integração da grelha de eventos do Azure](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Grelha de eventos envia mensagens do tópico de barramento de serviço para a função de aplicação e aplicação lógica. |

## <a name="queue-storage"></a>Armazenamento de filas

Utilize o armazenamento de filas para receber eventos que têm de ser solicitados. Poderá utilizar o armazenamento de filas quando tiver um processo de execução longa que demora demasiado tempo a responder. Através do envio de eventos para o armazenamento de filas, a aplicação pode solicitar e processar os eventos na sua própria agenda.

|Cargo  |Descrição  |
|---------|---------|
| [Eventos personalizados de rota para o armazenamento de filas do Azure com a CLI do Azure e a grelha de eventos](custom-event-to-queue-storage.md) | Descreve como enviar eventos personalizados para um armazenamento de filas. |

## <a name="webhooks"></a>WebHooks

Utilize webhooks para pontos finais personalizáveis que respondem a eventos.

|Cargo  |Descrição  |
|---------|---------|
| [Receber eventos para um ponto final HTTP](receive-events.md) | Descreve como validar um ponto final de HTTP para receber eventos a partir de uma subscrição de evento e receber e anular a serialização de eventos. |

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
