---
title: "Disponibilidade da região e recursos do Azure Container Instances"
description: "Detete as regiões do Azure que suportam a implementação de instâncias de contentor e os limites de memória e CPU dessas instâncias."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 08/31/2017
ms.author: marsma
ms.openlocfilehash: ace4eb6b284f2c1b2caeb54c1d686e68cacb1725
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Disponibilidade das regiões do Azure Container Instances

Durante a pré-visualização, o Azure Container Instances está disponível nas seguintes regiões com a CPU especificada e os limites de memória.

| Localização | SO | CPU | Memória (GB) |
| -------- | -- | :---: | :-----------: |
| Europa Ocidental, E.U.A. Oeste, E.U.A Leste | Linux | 2 | 7 |
| Europa Ocidental, E.U.A. Oeste, E.U.A Leste | Windows | 2 | 3.5 |

## <a name="resource-availability"></a>Disponibilidade dos recursos

As instâncias de contentor criadas dentro destes limites de recursos estão sujeitas à disponibilidade numa região de implementação. Quando uma região está sob muita carga, pode ocorrer uma falha ao implementar instâncias.

Para mitigar essa falha de implementação, experimente implementar instâncias com definições de memória e CPU inferior ou tente implementar mais tarde.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a resolução de problemas de implementação de instância de contentor, veja [Troubleshoot deployment issues with Azure Container Instances (Resolver problemas de implementação com o Azure Container Instances)](container-instances-troubleshooting.md).