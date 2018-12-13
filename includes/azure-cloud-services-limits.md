---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: eb1fe7f83ed83efe078be0aadf26cc7db6f498e2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886388"
---
| Recurso | Limite Predefinido | Limite Máximo |
| --- | --- | --- |
| [Funções da Web/trabalho por implementação](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Pontos finais de entrada de instância](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) por implementação |25 |25 |
| [Pontos finais de entrada](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) por implementação |25 |25 |
| [Pontos finais internos](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) por implementação |25 |25 |
| [Hospedado certificados de serviço](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementação |199 |199 |

<sup>1</sup>cada serviço em nuvem com as funções da Web/trabalho pode ter duas implementações, uma para produção e uma para teste. Observe também que este limite refere-se para o número de funções distintas (configuração) e não o número de instâncias por função (dimensionamento).

