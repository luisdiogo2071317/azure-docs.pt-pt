---
title: Quotas e limitações v3 de Media Services do Azure | Microsoft Docs
description: Este tópico descreve as quotas e limitações v3 de Media Services do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 06/13/2018
ms.author: juliako
ms.openlocfilehash: 75bfb0d5d29f0b8e038e68af08130564b72a05bf
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266758"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Quotas e limitações v3 de Media Services do Azure

Este artigo descreve as quotas e limitações na v3 de Media Services do Azure.

| Recurso | Limite Predefinido | 
| --- | --- | 
| Ativos por conta de Media Services do Azure | 1 000 000|
| JobInputs por tarefa | 50 |
| JobOutputs por tarefa | 20 (fixo) |
| Tamanho dos ficheiros| Em alguns cenários, não há um limite no tamanho máximo de ficheiro suportado para processamento nos Media Services. <sup>(1)</sup> |
| Tarefas por conta de Media Services | 500 000 <sup>(2)</sup>|
| Listar as transformações|A resposta, com 1000 transformações por página de paginação|
| Tarefas de listagem|A resposta, 500 tarefas por página de paginação|
| LiveEvents por conta de Media Services |5|
| Contas dos Media Services numa subscrição único | 25 (fixo) |
| StreamingPolicies | 1,000,000<sup>(3)</sup> |
| LiveOutputs no estado por LiveEvent de execução |3|
| LiveOutputs no estado de paragem por LiveEvent |50|
| Número de ficheiros por JobInput|10|
| Contas de armazenamento | 100<sup>(4)</sup> (fixo) |
| Pontos finais de transmissão em fluxo em execução estado por conta de Media Services|2|
| Transforma por conta de Media Services | 100 |
| TransformOutputs numa transformação| 20|
| StreamingLocators exclusivo associado um recurso de uma só vez | 20<sup>(5)</sup> |

<sup>1</sup> o tamanho máximo suportado para um blob único atualmente é até 5 TB de armazenamento de Blobs do Azure. No entanto, os limites adicionais aplicam-se nos serviços de suporte de dados do Azure com base nos tamanhos VM que são utilizados pelo serviço. Se o ficheiro de origem é superior a 260-GB, a tarefa provavelmente irá falhar. Se tiver conteúdo de 4K, que é maior do que o limite de 260 GB, contacte-nos amshelp@microsoft.com para potenciais mitigações suportar o seu cenário.

<sup>2</sup> este número inclui tarefas em fila, concluídas, Active Directory e canceladas. Não inclui tarefas eliminadas. 

Qualquer registo de tarefas na sua conta mais antiga do que 90 dias serão eliminados automaticamente, mesmo que o número total de registos é inferior a quota máxima. 

<sup>3</sup> quando utilizar um personalizado [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), deve conceber um conjunto limitado dessas políticas para a sua conta de serviço de multimédia e utilizá-las novamente para o seu StreamingLocators sempre que a encriptação mesma opções e protocolos são necessárias. Não deve criar uma nova StreamingPolicy para cada StreamingLocator.

<sup>4</sup> as contas de armazenamento tem de ser da mesma subscrição do Azure.

<sup>5</sup> StreamingLocators não foram concebidos para gerir o controlo de acesso por utilizador. Para conceder direitos de acesso diferentes a utilizadores individuais, utilize soluções de Gestão de Direitos Digitais (Digital Rights Management, DRM).

## <a name="support-ticket"></a>Pedido de suporte

Para os recursos que não sejam fixos, pode pedir para as quotas gerado, abrindo um [suporta permissão](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Inclui informações detalhadas no pedido no alterações de quota pretendido, cenários de caso de utilização e regiões necessárias. <br/>**Não** crie contas dos Serviços de Multimédia do Azure adicionais numa tentativa de obter limites superiores.

## <a name="next-steps"></a>Passos Seguintes

[Descrição geral](media-services-overview.md)
