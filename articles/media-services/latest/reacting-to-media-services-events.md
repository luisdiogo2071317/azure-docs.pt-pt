---
title: Reagir a eventos de serviços de multimédia do Azure | Documentos da Microsoft
description: Utilize o Azure Event Grid para subscrever a eventos de serviços de multimédia.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/19/2018
ms.author: juliako
ms.openlocfilehash: 143fec2ddb168b0fff0e419fa5767e9718637241
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465542"
---
# <a name="reacting-to-media-services-events"></a>Reagir a eventos de serviços de multimédia

Os eventos de serviços de multimédia permitem que os aplicativos reagir a eventos diferentes (por exemplo, o tarefa evento de alteração Estado), usando as modernas arquiteturas sem servidor. Ele faz isso sem a necessidade de código complicado ou serviços de consulta dispendiosa e ineficiente. Em vez disso, os eventos são enviados por meio [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) a manipuladores de eventos, tal como [as funções do Azure](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ou até mesmo para o seu próprio Webhook e pagar apenas o que usa. Para obter informações sobre preços, consulte [preços do Event Grid](https://azure.microsoft.com/pricing/details/event-grid/).

Disponibilidade para eventos de serviços de multimédia é associada ao Event Grid [disponibilidade](../../event-grid/overview.md) e ficará disponível noutras regiões como o Event Grid.  

## <a name="available-media-services-events"></a>Eventos de serviços de multimédia disponíveis

Grelha de eventos usa [subscrições de eventos](../../event-grid/concepts.md#event-subscriptions) encaminhar mensagens de eventos para os assinantes.  Atualmente, as subscrições de eventos dos serviços de multimédia podem incluir os seguintes eventos:  

|Nome do Evento|Descrição|
|----------|-----------|
| Microsoft.Media.JobStateChange| Gerado quando um Estado das alterações de tarefa. |
| Microsoft.Media.LiveEventConnectionRejected | Tentativa de ligação do codificador é rejeitada. |
| Microsoft.Media.LiveEventEncoderConnected | Codificador estabelece ligação com o evento em direto. |
| Microsoft.Media.LiveEventEncoderDisconnected | Codificador desliga. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Servidor de mídia ignora os segmentos de dados porque este seja muito tarde ou um timestamp sobreposto (timestamp de novos segmentos de dados é inferior à hora de fim do segmento de dados anterior). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Servidor de suporte de dados recebe o primeiro segmento de dados para cada faixa no stream ou ligação. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Servidor de suporte de dados Deteta áudio e fluxos de vídeo não estão sincronizados. Utilizar como um aviso porque a experiência do usuário não poderá ser afetada. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Servidor de suporte de dados Deteta qualquer um dos dois fluxos de vídeo vindo do codificador externa não estão sincronizadas. Utilizar como um aviso porque a experiência do usuário não poderá ser afetada. |
| Microsoft.Media.LiveEventIngestHeartbeat | Publicado a cada 20 segundos para cada faixa quando está a executar o evento em direto. Fornece ingestão de resumo de estado de funcionamento. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Servidor de suporte de dados Deteta descontinuidade na faixa de entrada. |

## <a name="event-schema"></a>Esquema de Eventos

Eventos de serviços de multimédia contêm todas as informações que necessárias para responder a alterações nos seus dados.  É possível identificar um evento de serviços de multimédia, porque a propriedade eventType começa com "Microsoft.Media.".

Para obter mais informações, consulte [esquemas de eventos dos serviços de multimédia](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Práticas recomendadas para o consumo de eventos

Aplicações que processam os eventos de serviços de multimédia devem seguir algumas práticas recomendadas:

* Como várias subscrições podem ser configuradas para encaminhar eventos para o mesmo manipulador de eventos, é importante não partem do princípio de eventos são de uma origem específica, mas para verificar o tópico da mensagem para se certificar de que trata da conta de armazenamento que está esperando.
* Da mesma forma, verifique se o eventType é um estão preparados para o processo e não partem do princípio de que todos os eventos recebidos serão os tipos esperados.
* Ignore campos que não compreende.  Essa prática ajuda a manter-se resiliente aos novos recursos que podem ser adicionados no futuro.
* Utilize as correspondências de prefixo e o sufixo "assunto" para limitar os eventos para um determinado evento.

## <a name="next-steps"></a>Passos Seguintes

[Obter eventos de estado de tarefa](job-state-events-cli-how-to.md)
