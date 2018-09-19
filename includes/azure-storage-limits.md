---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/13/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: fb349c9ea2178d84e367e763797d5d93d3ae4c53
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46299135"
---
A tabela seguinte descreve os limites predefinidos para o armazenamento do Azure. O *entrada* limite refere-se a todos os dados (pedidos) a ser enviados para uma conta de armazenamento. O *saída* limite refere-se a todos os dados (respostas) recebidos de uma conta de armazenamento.

| Recurso | Limite Predefinido |
| --- | --- |
| Número de contas de armazenamento por região por subscrição, incluindo contas standard e premium | 200 |
| Capacidade de conta de armazenamento máximo | 500 TiB |
| Número máx. de contentores de BLOBs, blobs, partilhas de ficheiros, tabelas, filas, entidades ou mensagens por conta de armazenamento | Sem limite |
| Taxa de pedidos máxima por conta de armazenamento | 20 000 pedidos por segundo |
| Entrada máxima por conta de armazenamento (regiões E.u.a.) | 10 Gbps, se o RA-GRS/GRS ativado, 20 Gbps para LRS/ZRS<sup>1</sup> |
| Saída máxima por conta de armazenamento (regiões E.u.a.) | 50 Gbps |
| Entrada máxima por conta de armazenamento (regiões de não-US) | 5 Gbps, se o RA-GRS/GRS ativado, 10 Gbps para LRS/ZRS<sup>1</sup> |
| Saída máxima por conta de armazenamento (regiões de não-US) | 50 Gbps |

<sup>1</sup>[replicação de armazenamento do azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy) as opções incluem:
* **RA-GRS**: armazenamento geograficamente redundante de acesso de leitura. Se o RA-GRS for ativado, os destinos de saída para a localização secundária são idênticos para a localização principal.
* **GRS**: armazenamento georredundante. 
* **O ZRS**: armazenamento com redundância de zona.
* **LRS**: armazenamento localmente redundante. 

