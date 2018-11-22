---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2b6ba1dcddd435c42ad864b8e87175d0e98c9b3a
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279866"
---
| Recurso | Limite Predefinido | Limite Máximo |
| --- | --- | --- |
| [Funções da Web/trabalho por implementação](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Pontos finais de entrada de instância](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) por implementação |25 |25 |
| [Pontos finais de entrada](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) por implementação |25 |25 |
| [Pontos finais internos](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) por implementação |25 |25 |

<sup>1</sup>cada serviço em nuvem com as funções da Web/trabalho pode ter duas implementações, uma para produção e uma para teste. Observe também que este limite refere-se para o número de funções distintas (configuração) e não o número de instâncias por função (dimensionamento).

