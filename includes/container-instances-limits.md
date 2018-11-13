---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 44bdaec78e1fad574f29a5945b07041b588aaff8
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572571"
---
| Recurso | Limite Predefinido |
| --- | :--- |
| Grupos de contentores por [subscrição](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Número de contentores por grupo de contentores | 60 |
| Número de volumes por grupo de contentores | 20 |
| Portas por IP | 5 |
| Contentor cria por hora |300<sup>1</sup> |
| Contentor cria por 5 minutos | 100<sup>1</sup> |
| Contentor elimina por hora | 300<sup>1</sup> |
| Contentor elimina por 5 minutos | 100<sup>1</sup> |
| Vários contentores por grupo de contentores | Apenas Linux<sup>2</sup> |
| Volumes dos Ficheiros do Azure | Apenas Linux<sup>2</sup> |
| Volumes do GitRepo | Apenas Linux<sup>2</sup> |
| Volumes secretos | Apenas Linux<sup>2</sup> |

<sup>1</sup> Crie um [pedido de suporte do Azure][azure-support] para pedir um aumento de limite.<br />
<sup>2</sup> O suporte do Windows para esta funcionalidade está planeada.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
