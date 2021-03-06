---
title: Estados de LiveEvent e de faturação nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico apresenta uma visão geral de Estados de LiveEvent de serviços de multimédia do Azure e de faturação.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 17fead25840e45f98478a6eb6c146bcc261dfe75
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158014"
---
# <a name="live-event-states-and-billing"></a>Estados de evento em direto e de faturação

Nos serviços de multimédia do Azure, um evento Live começa assim que o seu estado faz a transição para de faturação **em execução**. Para parar o evento em direto de faturação, terá de parar o evento em direto.

Quando **LiveEventEncodingType** no seu [evento em direto](https://docs.microsoft.com/rest/api/media/liveevents) está definida como Standard, os serviços de multimédia automática seja fechado desativar qualquer evento em direto que ainda está na **em execução** estado 12 horas após o feed de entrada é perdido e há nenhuma **Live saída**em execução. No entanto, será serão cobrados a hora do evento em direto foi no **em execução** estado.

## <a name="states"></a>Estados

O evento em direto pode ter um dos seguintes Estados.

|Estado|Descrição|
|---|---|
|**Parado**| Este é o estado inicial do evento Live após criação (a menos que o início automático foi definido como true.) Ocorre uma faturação sem neste estado. Neste estado, as propriedades de evento em direto podem ser atualizadas, mas não é permitida a transmissão em fluxo.|
|**A partir de**| O evento em direto está a ser iniciado e os recursos estão a ser alocados. Ocorre uma faturação sem neste estado. Transmissão em fluxo a pedido ou atualizações não são permitidas durante este estado. Se ocorrer um erro, devolve o evento em direto para o estado parado.|
|**Em execução**| O evento Live recursos teriam sido alocados, ingestão e URLs de pré-visualização ter sido gerado, e é capaz de receber transmissões em fluxo. Neste momento, a faturação é Active Directory. Tem de chamar explicitamente Stop do recurso de evento em direto para parar a faturação ainda mais.|
|**A parar**| O evento em direto está a ser parado e recursos estão a ser desaprovisionados. Ocorre uma faturação sem neste estado transitório. Transmissão em fluxo a pedido ou atualizações não são permitidas durante este estado.|
|**A eliminar**| O evento em direto está a ser eliminado. Ocorre uma faturação sem neste estado transitório. Transmissão em fluxo a pedido ou atualizações não são permitidas durante este estado.|

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de transmissão em fluxo em direto](live-streaming-overview.md)
- [Tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md)
