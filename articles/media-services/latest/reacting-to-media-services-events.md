---
title: A agir os eventos de Media Services do Azure | Microsoft Docs
description: Utilize a grelha de eventos do Azure para subscrever a eventos de Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 969957d53824bd70440e5529b83bc830bb5d9cc4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="reacting-to-media-services-events"></a>A agir os eventos de Media Services

Eventos de Media Services permitir que as aplicações de reagir a diferentes eventos (por exemplo, o estado alteração evento de tarefa) utilizando arquiteturas sem servidor modernas. Isto é feito sem a necessidade de código complicado ou serviços de consulta ineficaz e dispendiosa. Em vez disso, os eventos são enviadas por push através do [grelha de eventos do Azure](https://azure.microsoft.com/services/event-grid/) para processadores de eventos, tal como [das funções do Azure](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ou até as suas próprias Webhook e apenas pagamento para o utilizar. Para obter informações sobre preços, consulte [preços da grelha de evento](https://azure.microsoft.com/pricing/details/event-grid/).

Disponibilidade para eventos de serviços de suporte de dados está associada à grelha de evento [disponibilidade](../../event-grid/overview.md) e ficará disponível noutras regiões como sucede grelha de eventos.  

## <a name="available-media-services-events"></a>Eventos de Media Services disponíveis

Grelha de evento utiliza [subscrições de evento](../../event-grid/concepts.md#event-subscriptions) encaminhar mensagens de evento para subscritores.  Atualmente, as subscrições de eventos de Media Services podem incluir o tipo de evento seguinte:  

|Nome do evento|Descrição|
|----------|-----------|
| Microsoft.Media.JobStateChange| É desencadeado quando um Estado das alterações de tarefa. |

## <a name="event-schema"></a>Esquema de eventos

Os eventos de serviços de suporte de dados incluem todas as informações que necessárias para responder a alterações nos seus dados.  Pode identificar um evento de serviços de suporte de dados porque a propriedade eventType começa com "Microsoft.Media.".

Para obter mais informações, consulte [esquemas de eventos de Media Services](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Práticas de consumo de eventos

As aplicações que processam eventos de Media Services devem seguir alguns práticas recomendadas:

* Como várias subscrições podem ser configuradas para eventos de rota para o mesmo processador de eventos, é importante para não partem do princípio de eventos são de uma origem específica, mas o tópico da mensagem para se certificar de que são provenientes da conta do storage que está à espera de verificação.
* Da mesma forma, verifique se o eventType um estão preparados para o processo e não partem do princípio de que todos os eventos recebidos serão os tipos de que espera.
* Ignore os campos que não compreender.  Esta prática irão ajudá-lo a manter-se resiliente para novas funcionalidades que podem ser adicionadas no futuro.
* Utilize os prefixo e o sufixo de correspondências "assunto" para limitar os eventos para um determinado evento.

## <a name="next-steps"></a>Passos Seguintes

[Obter eventos de estado de tarefa](job-state-events-cli-how-to.md)
