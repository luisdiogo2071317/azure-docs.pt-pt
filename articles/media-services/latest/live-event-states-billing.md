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
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: 588aeede123848900fac6fab663dd1f6c6c169b6
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53719426"
---
# <a name="liveevent-states-and-billing"></a>Estados de LiveEvent e faturação

Nos serviços de multimédia do Azure, um LiveEvent começa assim que o seu estado faz a transição para de faturação **em execução**. Para parar o LiveEvent de faturação, terá de parar o LiveEvent.

Quando **LiveEventEncodingType** no seu [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) está definida como Standard, os serviços de multimédia automática seja fechado desativar qualquer LiveEvent que ainda está na **em execução** estado 12 horas após o feed de entrada é perdido e há nenhuma **LiveOutput**em execução. No entanto, será serão cobrados a hora a LiveEvent estava a ser o **em execução** estado.

## <a name="states"></a>Estados

O LiveEvent pode ter um dos seguintes Estados.

|Estado|Descrição|
|---|---|
|**Parado**| Este é o estado inicial do LiveEvent após criação (a menos que o início automático foi definido como true.) Ocorre uma faturação sem neste estado. Neste estado, as propriedades de LiveEvent podem ser atualizadas, mas não é permitida a transmissão em fluxo.|
|**A partir de**| O LiveEvent está a ser iniciada e os recursos estão a ser alocados. Ocorre uma faturação sem neste estado. Transmissão em fluxo a pedido ou atualizações não são permitidas durante este estado. Se ocorrer um erro, devolve o LiveEvent para o estado parado.|
|**Em execução**| Tenham sido gerados LiveEvent recursos teriam sido alocados, ingestão e URLs de pré-visualização e é capaz de receber transmissões em fluxo. Neste momento, a faturação é Active Directory. Deve chamar explicitamente o parar no recurso LiveEvent para parar a faturação ainda mais.|
|**A parar**| Está a ser parado a LiveEvent e recursos estão a ser desaprovisionados. Ocorre uma faturação sem neste estado transitório. Transmissão em fluxo a pedido ou atualizações não são permitidas durante este estado.|
|**A eliminar**| O LiveEvent está a ser eliminado. Ocorre uma faturação sem neste estado transitório. Transmissão em fluxo a pedido ou atualizações não são permitidas durante este estado.|

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de transmissão em fluxo em direto](live-streaming-overview.md)
- [Tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md)
