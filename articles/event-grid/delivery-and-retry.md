---
title: Azure de entrega de eventos grelha e tente novamente
description: Descreve como grelha de eventos do Azure oferece eventos e como se processa mensagens undelivered.
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: darosa
ms.openlocfilehash: 4eacb37d6e19b4b69d604aa84fd404479dead1ea
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
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

## <a name="monitoring"></a>Monitorização

Pode utilizar o portal para ver o estado de entregas de eventos.

Para ver as métricas para uma subscrição de evento, procure **subscrições de evento** nos serviços disponíveis e selecione-o.

![Pesquisa para subscrições de eventos](./media/delivery-and-retry/select-event-subscriptions.png)

Filtrar por tipo de evento, a subscrição e localização. Selecione **métricas** para a subscrição ver.

![Subscrições de eventos de filtro](./media/delivery-and-retry/filter-events.png)

Ver as métricas para o tópico de eventos e uma subscrição.

![Veja as métricas de eventos](./media/delivery-and-retry/subscription-metrics.png)

Se tiver publicado um tópico personalizado, pode ver as métricas para o mesmo. Selecione o grupo de recursos que contém o tópico e selecione o tópico.

![Selecione o tópico personalizado](./media/delivery-and-retry/select-custom-topic.png)

Ver as métricas para o tópico de evento personalizado.

![Veja as métricas de eventos](./media/delivery-and-retry/custom-topic-metrics.png)

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).