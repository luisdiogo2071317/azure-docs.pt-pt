---
title: incluir ficheiro
description: incluir ficheiro
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: c98a2146a019817152be9fae76638dbaa4d9de3d
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49458886"
---
| **Recurso** | **Limite Predefinido** | **Limite Máximo** |
| --- | --- | --- |
| Contas do Batch por região por subscrição | 1 - 3 |50 |
| Núcleos dedicados por conta do Batch | 10 - 100 | N/A<sup>1</sup> |
| Núcleos de prioridade baixa por conta do Batch | 10 - 100 | N/D<sup>2</sup> |
| Tarefas ativas e agendas de trabalhos<sup>3</sup> por conta do Batch | 100 - 300 | 1000<sup>4</sup> |
| Conjuntos por conta do Batch | 20 - 100 | 500<sup>4</sup> |

> [!NOTE]
> Limites predefinidos variam consoante o tipo de subscrição que vai utilizar para criar uma conta do Batch. As quotas de núcleos mostradas destinam-se a contas do Batch no modo de serviço do Batch. [Ver as quotas na sua conta do Batch](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup> pode ser aumentado o número de núcleos dedicados por conta do Batch, mas o número máximo é não especificado. Contacte o suporte do Azure para discutir as opções de aumento.

<sup>2</sup> pode ser aumentado o número de núcleos de prioridade baixa por conta do Batch, mas o número máximo é não especificado. Contacte o suporte do Azure para discutir as opções de aumento.

<sup>3</sup> tarefas concluídas e agendas de tarefas não são limitadas.

<sup>4</sup> suporte do Azure de contacto para pedir um aumento para lá deste limite.
