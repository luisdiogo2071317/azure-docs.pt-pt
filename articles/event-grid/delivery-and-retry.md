---
title: Entrega do Event Grid e repetição do Azure
description: Descreve como o Azure Event Grid fornece eventos e como ele lida com as mensagens não serão entregues.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: 189484291dd337535fe6988f919326b6e997b290
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506291"
---
# <a name="event-grid-message-delivery-and-retry"></a>Entrega de mensagens do Event Grid e tente novamente 

Este artigo descreve como o Azure Event Grid processa eventos quando entrega não é reconhecida.

Event Grid fornece entrega durável. Ele fornece a cada mensagem, pelo menos, uma vez para cada subscrição. Eventos são enviados para o webhook registado de cada subscrição imediatamente. Se um webhook não reconhece o recebimento de um evento em 60 segundos, da primeira tentativa de entrega, o Event Grid repete a entrega do evento. 

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
- 414 URI demasiado longo
- 500 Erro de Servidor Interno
- 503 Serviço Indisponível
- 504 Tempo Limite do Gateway

Se a grelha de eventos receber um erro que indica que o ponto final está temporariamente indisponível, tentará novamente enviar o evento. Se a grelha de eventos receber um erro que indica a entrega nunca será concluída com êxito e uma [tiver sido configurado o ponto final de mensagens não entregues](manage-event-delivery.md), envia o evento para o ponto final de mensagens não entregues. 

## <a name="retry-intervals-and-duration"></a>Intervalos de repetição e a duração

Grelha de eventos utiliza uma política de repetição de término exponencial para a entrega de eventos. Se o webhook não responde ou retorna um código de falha, o Event Grid repete a entrega na agenda seguinte:

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