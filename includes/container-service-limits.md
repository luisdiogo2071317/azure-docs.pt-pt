---
title: incluir ficheiro
description: incluir ficheiro
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4251f379c517d5ccfd0430987e3d5280208590ff
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400305"
---
| Recurso | Limite predefinido |
| --- | :--- |
| Número máximo de clusters por subscrição | 100 |
| Número máximo de nós por cluster | 100 |
| Número máximo de pods por nó: [Redes básicas][basic-networking] com o Kubenet | 110 |
| Número máximo de pods por nó: [Redes avançadas][advanced-networking] com o Azure CNI | Implementação da CLI do Azure: 30<sup>1</sup><br />Modelo do Resource Manager: 30<sup>1</sup><br />Implementação do portal: 30 |

<sup>1</sup> Quando implementa um cluster do AKS com a CLI do Azure ou um modelo do Resource Manager, este valor é configurável até **110 pods por nó**. Não pode configurar pods máximos por nó, depois de já ter implementado um cluster do AKS, ou se implementar um cluster com o portal do Azure.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
