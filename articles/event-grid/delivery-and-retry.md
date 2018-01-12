---
title: Azure de entrega de eventos grelha e tente novamente
description: Descreve como grelha de eventos do Azure oferece eventos e como se processa mensagens undelivered.
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/10/2018
ms.author: darosa
ms.openlocfilehash: fe9089334deceb38186add56ce3fb1d6ecc20363
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Entrega de mensagens de grelha de eventos e tente novamente 

Este artigo descreve a forma como a grelha de eventos do Azure processa eventos quando entrega não está a ser confirmada.

Grelha de evento fornece entrega durável. Fornece cada mensagem, pelo menos, uma vez para cada subscrição. Os eventos são enviados para o webhook registado de cada subscrição imediatamente. Se um webhook não reconhecer a receção de um evento dentro de 60 segundos, da primeira tentativa de entrega, eventos grelha tentativas de entrega do evento.

## <a name="message-delivery-status"></a>Estado de entrega de mensagens

Grelha de evento utiliza códigos de resposta HTTP para confirmar a receção de eventos. 

### <a name="success-codes"></a>Códigos de êxito

Os códigos de resposta HTTP seguintes indicam que um evento tem foi entregue com êxito para o webhook. Grelha de evento considera entrega concluída.

- 200 OK
- Aceite 202

### <a name="failure-codes"></a>Códigos de falha

Os códigos de resposta HTTP seguintes indicam que falhou uma tentativa de entrega de eventos. Grelha de eventos tentará novamente enviar o evento. 

- Pedido de 400 incorreta
- 401 não autorizado
- 404 Não Encontrado
- Tempo limite do pedido 408
- 414 URI demasiado longo
- Erro interno do servidor 500
- Serviço 503 não está disponível
- Tempo limite do 504 gateway

Qualquer código de resposta ou falta de uma resposta indica uma falha. Grelha de evento repete as tentativas de entrega. 

## <a name="retry-intervals"></a>Repita intervalos

Grelha de evento utiliza uma política de repetição de término exponencial para entrega de eventos. Se o webhook não responde ou devolve um código de falha, o evento grelha repete entrega na agenda seguinte:

1. 10 segundos
2. 30 segundos
3. um minuto
4. 5 minutos
5. 10 minutos
6. 30 minutos
7. 1 hora

Grelha de eventos adiciona uma pequena aleatoriedade para todos os intervalos de repetição.

## <a name="retry-duration"></a>Repita a duração

Durante a pré-visualização do Azure eventos grelha expira todos os eventos que não são fornecidos no prazo de duas horas.

## <a name="next-steps"></a>Passos Seguintes

* Para ver o estado de entregas de eventos, consulte [entrega de mensagens de grelha de eventos do Monitor](monitor-event-delivery.md).
* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).