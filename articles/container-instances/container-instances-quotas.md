---
title: Quotas e disponibilidade das regiões do Azure Container Instances
description: As quotas predefinidas e a disponibilidade das regiões do serviço do Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 12/07/2018
ms.author: danlep
ms.openlocfilehash: a7b61702feb062c57fdec84f335ace44a47d0283
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249486"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Quotas e disponibilidade das regiões do Azure Container Instances

Todos os serviços do Azure incluem determinados limites e quotas predefinidos para os recursos e funcionalidades. As secções seguintes detalham os limites de recursos predefinidos para vários recursos do Azure Container Instances (ACI), bem como a disponibilidade do serviço ACI em regiões do Azure.

## <a name="service-quotas-and-limits"></a>Quotas e limites do serviço

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Disponibilidade de região

O Azure Container Instances está disponível nas seguintes regiões com a CPU especificada e os limites de memória.

| Localização | SO | CPU | Memória (GB) |
| -------- | -- | :---: | :-----------: |
| EUA Leste, Europa do Norte, Europa Ocidental, EUA Oeste, EUA Oeste 2 | Linux | 4 | 14 |
| Leste do Japão | Linux | 2 | 8 |
| Leste da Austrália, EUA Leste 2, Sudeste Asiático | Linux | 2 | 7 |
| Canadá Central, Índia Central, Leste asiático, Norte dos E.U.A., Centro-Sul dos E.U.A. | Linux | 2 | 3.5 |
| EUA Leste, Europa Ocidental, EUA Oeste | Windows | 4 | 14 |
| Leste da Austrália, Canadá Central, Índia Central, Ásia Oriental, E.U.A. Leste 2, leste do Japão, Norte dos E.U.A., Europa do Norte, E.U.A. Central Centro-Sul, Sudeste asiático, oeste dos E.U.A. 2 | Windows | 2 | 3.5 |

As instâncias de contentor criadas dentro destes limites de recursos estão sujeitas à disponibilidade numa região de implementação. Quando uma região está sob muita carga, pode ocorrer uma falha ao implementar instâncias. Para mitigar essa falha de implementação, experimente implementar instâncias com definições de memória e CPU inferior ou tente implementar mais tarde.

Informe a equipa das regiões adicionais necessárias ou do aumento dos limites da CPU/memória em [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para obter mais informações sobre a resolução de problemas de implementação de instância de contentor, veja [Troubleshoot deployment issues with Azure Container Instances (Resolver problemas de implementação com o Azure Container Instances)](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Passos Seguintes

Podem ser aumentados determinados limites e quotas predefinidos. Para pedir um aumento de um ou mais recursos que suportam tal aumento, submeta um [pedido de suporte do Azure][azure-support] (selecione "Quota" no **Tipo de problema**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
