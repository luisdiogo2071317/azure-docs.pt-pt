---
title: "Disponibilidade da região e recursos do Azure Container Instances | Azure Docs"
description: "Detete as regiões do Azure que suportam a implementação de instâncias de contentor e os limites de memória e CPU dessas instâncias."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2017
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