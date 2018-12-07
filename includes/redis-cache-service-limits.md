---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 8a9322862d63d856f96729e5784b88e0ef098fb6
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53018497"
---
| Recurso | Limite |
| --- | --- |
| Tamanho da cache |530 GB |
| Bases de Dados |64 |
| Máx. de clients conectados |40,000 |
| Cache do Azure para réplicas de Redis (para elevada disponibilidade) |1 |
| Partições horizontais numa cache premium com clustering |10 |

Limita a Cache do Azure do Azure para Redis e tamanhos são diferentes para cada escalão de preço. Para ver os escalões de preço e seus tamanhos associados, consulte [Cache do Azure do Azure para Redis preços](https://azure.microsoft.com/pricing/details/cache/).

Para obter mais informações sobre a Cache do Azure do Azure para limites de configuração do Redis, consulte [configuração do servidor predefinido Redis](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Porque a configuração e gestão de Cache do Azure do Azure para instâncias de Redis é feito pela Microsoft, nem todos os comandos da Redis são suportados na Cache do Azure do Azure para Redis. Para obter mais informações, consulte [Redis comandos não suportados na Cache do Azure do Azure para Redis](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

