---
title: incluir ficheiro
description: incluir ficheiro
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 71294824bd3dd5215c388cfcd44382c7eee123ad
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48874137"
---
| Recurso | Limite predefinido |
| --- | :--- |
| Número máximo de clusters por subscrição | 100 |
| Número máximo de nós por cluster | 100 |
| Número máximo de pods por nó: [Redes básicas][basic-networking] com o Kubenet | 110 |
| Número máximo de pods por nó: [Redes avançadas][advanced-networking] com o Azure CNI | Implementação da CLI do Azure: 30<sup>1</sup><br />Modelo do Resource Manager: 30<sup>1</sup><br />Implementação do portal: 30 |

<sup>1</sup> este valor é configurável na implementação de cluster ao implementar um cluster do AKS com a CLI do Azure ou um modelo do Resource Manager.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
