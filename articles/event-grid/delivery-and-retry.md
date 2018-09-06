---
title: Entrega do Event Grid e repetição do Azure
description: Descreve como o Azure Event Grid fornece eventos e como ele lida com as mensagens não serão entregues.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2a9ff23e5182c8cb7c91ad93e368f61f258c84f8
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841597"
---
# <a name="event-grid-message-delivery-and-retry"></a>Entrega de mensagens do Event Grid e tente novamente 

Este artigo descreve como o Azure Event Grid processa eventos quando entrega não é reconhecida.

Event Grid fornece entrega durável. Ele fornece a cada mensagem, pelo menos, uma vez para cada subscrição. Eventos são enviados para o webhook registado de cada subscrição imediatamente. Se um webhook não confirmar a receção de um evento em 60 segundos, da primeira tentativa de entrega, o Event Grid repete a entrega do evento. 

Atualmente, Event Grid envia individualmente cada evento para os subscritores. O subscritor recebe uma matriz com um único evento.

## <a name="message-delivery-status"></a>Estado de entrega da mensagem

Grelha de eventos utiliza códigos de resposta HTTP para confirmar a receção de eventos. 

### <a name="success-codes"></a>Códigos de êxito

Os seguintes códigos de resposta HTTP indicam que um evento tem foi entregue com êxito para o webhook. Grelha de eventos considera a entrega completo.

- 200 OK
- Aceite 202

### <a name="failure-codes"></a>Códigos de falha

Os seguintes códigos de resposta HTTP indicam que não foi uma tentativa de entrega de eventos.

- 400 pedido inválido
- 401 não autorizado
- 404 Não Encontrado
- 408 tempo limite do pedido
- Entidade do pedido 413 demasiado grande
- 414 URI demasiado longo
- 429 demasiados pedidos
- 500 Erro de Servidor Interno
- 503 Serviço Indisponível
- 504 Tempo Limite do Gateway

Se tiver [configurado um ponto de extremidade de mensagens não entregues](manage-event-delivery.md) e grelha de eventos receber qualquer um de um 400 ou 413 código de resposta, o Event Grid envia imediatamente o evento para o ponto de extremidade de mensagens não entregues. Caso contrário, o Event Grid tentará novamente todos os erros.

## <a name="retry-intervals-and-duration"></a>Intervalos de repetição e a duração

Grelha de eventos utiliza uma política de repetição de término exponencial para a entrega de eventos. Se o webhook não responder ou retorna um código de falha, o Event Grid repete a entrega na agenda seguinte:

1. 10 segundos
2. 30 segundos
3. 1 minuto
4. 5 minutos
5. 10 minutos
6. 30 minutos
7. 1 hora

Grelha de eventos adiciona uma pequeno randomização para todos os intervalos de repetição. Depois de uma hora, a entrega de eventos será repetida uma vez por hora.

Por predefinição, o Event Grid expira todos os eventos que não são entregues no prazo de 24 horas. Pode [personalizar a política de repetição](manage-event-delivery.md) durante a criação de uma subscrição de evento. Fornecer o número máximo de tentativas de entrega (a predefinição é 30) e o evento time-to-live (a predefinição é 1440 minutos).

## <a name="dead-letter-events"></a>Eventos de mensagens não entregues

Quando o Event Grid não é possível entregar um evento, ele pode enviar o evento não serão entregues para uma conta de armazenamento. Este processo é conhecido como mensagens não entregues. Para ver os eventos não serão entregues, pode selecioná-los da localização de mensagens não entregues. Para obter mais informações, consulte [inutilizado letras e políticas de repetição](manage-event-delivery.md).

## <a name="next-steps"></a>Passos Seguintes

* Para ver o estado de entregas de eventos, consulte [entrega de mensagens do Monitor Event Grid](monitor-event-delivery.md).
* Para personalizar as opções de entrega de eventos, veja [definições de entrega de gerir o Event Grid](manage-event-delivery.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para começar rapidamente com o Event Grid, veja [criar e encaminhar eventos personalizados com o Azure Event Grid](custom-event-quickstart.md).