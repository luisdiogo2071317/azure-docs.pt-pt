---
title: Quotas e disponibilidade das regiões do Azure Container Instances
description: As quotas predefinidas e a disponibilidade das regiões do serviço do Azure Container Instances.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 02/27/2018
ms.author: marsma
ms.openlocfilehash: 1bc890abc8b406ae75f292f37775e4cb62cf0473
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115280"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Quotas e disponibilidade das regiões do Azure Container Instances

Todos os serviços do Azure incluem determinados limites e quotas predefinidos para os recursos e funcionalidades. As secções seguintes detalham os limites de recursos predefinidos para vários recursos do Azure Container Instances (ACI), bem como a disponibilidade do serviço ACI em regiões do Azure.

## <a name="service-quotas-and-limits"></a>Quotas e limites do serviço

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Disponibilidade de região

O Azure Container Instances está disponível nas seguintes regiões com a CPU especificada e os limites de memória.

| Localização | SO | CPU | Memória (GB) |
| -------- | -- | :---: | :-----------: |
| E.U.A. Oeste, E.U.A. Leste, Europa Ocidental, Europa do Norte | Linux | 4 | 14 |
| E.U.A. Oeste 2, Sudeste Asiático | Linux | 2 | 7 |
| Leste da Austrália, E.U.A. Leste 2, E.U.A. Central | Linux | 1 | 1.5 |
| E.U.A. Oeste, E.U.A. Leste, Europa Ocidental, Europa do Norte | Windows | 4 | 14 |
| E.U.A. Oeste 2, Sudeste Asiático | Windows | 2 | 3.5 |

As instâncias de contentor criadas dentro destes limites de recursos estão sujeitas à disponibilidade numa região de implementação. Quando uma região está sob muita carga, pode ocorrer uma falha ao implementar instâncias. Para mitigar essa falha de implementação, experimente implementar instâncias com definições de memória e CPU inferior ou tente implementar mais tarde.

Informe a equipa das regiões adicionais necessárias ou do aumento dos limites da CPU/memória em [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para obter mais informações sobre a resolução de problemas de implementação de instância de contentor, veja [Troubleshoot deployment issues with Azure Container Instances (Resolver problemas de implementação com o Azure Container Instances)](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Passos seguintes

Podem ser aumentados determinados limites e quotas predefinidos. Para pedir um aumento de um ou mais recursos que suportam tal aumento, submeta um [pedido de suporte do Azure][azure-support] (selecione "Quota" no **Tipo de problema**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
