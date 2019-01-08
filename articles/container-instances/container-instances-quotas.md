---
title: Quotas e disponibilidade das regiões do Azure Container Instances
description: As quotas predefinidas e a disponibilidade das regiões do serviço do Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 12/17/2018
ms.author: danlep
ms.openlocfilehash: 647890517e6f08a4602ebed8ee1057cb45f10cbe
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075503"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Quotas e disponibilidade das regiões do Azure Container Instances

Todos os serviços do Azure incluem determinados limites e quotas predefinidos para os recursos e funcionalidades. As secções seguintes detalham os limites de recursos predefinidos para vários recursos do Azure Container Instances (ACI), bem como a disponibilidade do serviço ACI em regiões do Azure.

## <a name="service-quotas-and-limits"></a>Quotas e limites do serviço

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Disponibilidade de região

O Azure Container Instances está disponível nas seguintes regiões com a CPU especificada e os limites de memória. Os valores são atualizados no momento da publicação. Para obter informações atualizadas, utilize o [capacidades de lista](/rest/api/container-instances/listcapabilities/listcapabilities) API. Limites de disponibilidade e o recurso poderão ser diferente ao utilizar o Azure Container Instances com um [rede virtual](container-instances-vnet.md) (pré-visualização) ou com [recursos GPU](container-instances-gpu.md) (pré-visualização).

| Localização | SO | CPU | Memória (GB) |
| -------- | -- | :---: | :-----------: |
| Canadá Central | Linux | 4 | 16 |
| EUA Leste, Europa do Norte, Europa Ocidental, EUA Oeste, EUA Oeste 2 | Linux | 4 | 14 |
| Leste do Japão | Linux | 2 | 8 |
| Leste da Austrália, EUA Leste 2, Sudeste Asiático | Linux | 2 | 7 |
| Índia central, Ásia Oriental, Norte dos E.U.A., Centro-Sul E.U.A. Central, Sul da Índia | Linux | 2 | 3.5 |
| EUA Leste, Europa Ocidental, EUA Oeste | Windows | 4 | 14 |
| Leste da Austrália, Canadá Central, Índia Central, Ásia Oriental, E.U.A. Leste 2, leste do Japão, Centro-Norte, Europa do Norte, EUA Centro-Sul, Sul da Índia, Sudeste asiático, E.U.A. oeste 2 | Windows | 2 | 3.5 |

As instâncias de contentor criadas dentro destes limites de recursos estão sujeitas à disponibilidade numa região de implementação. Quando uma região está sob muita carga, pode ocorrer uma falha ao implementar instâncias. Para mitigar essa falha de implementação, experimente implementar instâncias com definições de memória e CPU inferior ou tente implementar mais tarde.

Informe a equipa das regiões adicionais necessárias ou do aumento dos limites da CPU/memória em [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para obter mais informações sobre a resolução de problemas de implementação de instância de contentor, veja [Troubleshoot deployment issues with Azure Container Instances (Resolver problemas de implementação com o Azure Container Instances)](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Passos Seguintes

Podem ser aumentados determinados limites e quotas predefinidos. Para pedir um aumento de um ou mais recursos que suportam tal aumento, submeta um [pedido de suporte do Azure][azure-support] (selecione "Quota" no **Tipo de problema**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
