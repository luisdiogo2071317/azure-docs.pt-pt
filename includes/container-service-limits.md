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
ms.openlocfilehash: 23c25953d2f493d2dd799bfd11dbbb69db002d1b
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55736033"
---
| Recurso | Limite predefinido |
| --- | :--- |
| Número máximo de clusters por subscrição | 100 |
| Número máximo de nós por cluster | 100 |
| Máx. pods por nó: [Rede básica] [ basic-networking] com Kubenet | 110 |
| Máx. pods por nó: [Advanced networking] [ advanced-networking] com CNI do Azure | Implementação de CLI do Azure: 30<sup>1</sup><br />Modelo do Resource Manager: 30<sup>1</sup><br />Implementação do portal: 30 |

<sup>1</sup> Quando implementa um cluster do AKS com a CLI do Azure ou um modelo do Resource Manager, este valor é configurável até **110 pods por nó**. Não pode configurar pods máximos por nó, depois de já ter implementado um cluster do AKS, ou se implementar um cluster com o portal do Azure.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
