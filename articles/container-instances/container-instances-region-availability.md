---
title: "Disponibilidade da região e recursos do Azure Container Instances"
description: "Detete as regiões do Azure que suportam a implementação de instâncias de contentor e os limites de memória e CPU dessas instâncias."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 12/15/2017
ms.author: marsma
ms.openlocfilehash: ec7f469c47924f4ae22d6509996ca9cf498fc9ad
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Disponibilidade das regiões do Azure Container Instances

Durante a pré-visualização, o Azure Container Instances está disponível nas seguintes regiões com a CPU especificada e os limites de memória.

| Localização | SO | CPU | Memória (GB) |
| -------- | -- | :---: | :-----------: |
| Europa Ocidental, E.U.A. Oeste, E.U.A Leste | Linux | 4 | 14 |
| Europa Ocidental, E.U.A. Oeste, E.U.A Leste | Windows | 4 | 14 |

## <a name="resource-availability"></a>Disponibilidade dos recursos

As instâncias de contentor criadas dentro destes limites de recursos estão sujeitas à disponibilidade numa região de implementação. Quando uma região está sob muita carga, pode ocorrer uma falha ao implementar instâncias.

Para mitigar essa falha de implementação, experimente implementar instâncias com definições de memória e CPU inferior ou tente implementar mais tarde.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a resolução de problemas de implementação de instância de contentor, veja [Troubleshoot deployment issues with Azure Container Instances (Resolver problemas de implementação com o Azure Container Instances)](container-instances-troubleshooting.md).