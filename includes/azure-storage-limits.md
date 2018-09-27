---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a777151216cb70b696da088b8a0d34779ebc39
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396132"
---
A tabela seguinte descreve os limites predefinidos para o armazenamento do Azure. O *entrada* limite refere-se a todos os dados (pedidos) a ser enviados para uma conta de armazenamento. O *saída* limite refere-se a todos os dados (respostas) recebidos de uma conta de armazenamento.

| Recurso | Limite Predefinido |
| --- | --- |
| Número de contas de armazenamento por região por subscrição, incluindo contas standard e premium | 200 |
| Capacidade de conta de armazenamento máximo<sup>1</sup> | 500 TiB |
| Número máx. de contentores de BLOBs, blobs, partilhas de ficheiros, tabelas, filas, entidades ou mensagens por conta de armazenamento | Sem limite |
| Taxa máxima de pedidos<sup>1</sup> por conta de armazenamento | 20 000 pedidos por segundo |
| Entrada de Max<sup>1</sup> por conta de armazenamento (regiões E.u.a.) | 10 Gbps, se o RA-GRS/GRS ativado, 20 Gbps para LRS/ZRS<sup>2</sup> |
| Saída de Max<sup>1</sup> por conta de armazenamento (regiões E.u.a.) | 20 Gbps se o RA-GRS/GRS ativado, 30 Gbps para LRS/ZRS |
| Entrada de Max<sup>1</sup> por conta de armazenamento (regiões de não-US) | 5 Gbps, se o RA-GRS/GRS ativado, 10 Gbps para LRS/ZRS<sup>2</sup> |
| Saída de Max<sup>1</sup> por conta de armazenamento (regiões de não-US) | 10 Gbps, se o RA-GRS/GRS ativado, 15 Gbps para LRS/ZRS |

<sup>1</sup> contas de armazenamento do azure suportam limites mais elevados de capacidade, taxa de pedidos, entrada e saída por pedido. Para obter mais informações sobre os limites de aumento, consulte [contas de armazenamento maiores e maior escala a anunciar](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/). Para pedir um aumento de limites de conta, contacte [suporte do Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup>[replicação de armazenamento do azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy) as opções incluem:
* **RA-GRS**: armazenamento geograficamente redundante de acesso de leitura. Se o RA-GRS for ativado, os destinos de saída para a localização secundária são idênticos para a localização principal.
* **GRS**: armazenamento georredundante. 
* **O ZRS**: armazenamento com redundância de zona.
* **LRS**: armazenamento localmente redundante. 

