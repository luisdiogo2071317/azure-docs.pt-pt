---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/22/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 36d59638984c31a98067b95fded9626f31a7f278
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "34669356"
---
Máquinas de virtuais de série H do Azure são as mais recentes da computação de alto desempenho que VMS com o objetivo de necessidades computacionais avançadas, como modelagem molecular e dinâmicas de fluidos computacionais. Essas 8 e 16 VMs de vCPU são criadas com o Intel Haswell E5 2667 V3 processador tecnologia com memória DDR4 e armazenamento temporário baseado em SSD. 

Além do poder de CPU substancial, a série H oferece diversas opções para funcionamento em rede RDMA de baixa latência através de FDR InfiniBand e várias configurações de memória para suportar requisitos computacionais de memória intensiva.



## <a name="h-series"></a>Série H

ACU: 290-300

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Discos de dados máximos | Débito máximo do disco: IOPS | NICs máximos |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32 x 500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64 x 500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32 x 500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64 x 500 |4  |
| Standard_H16r <sup>1</sup> |16 |112 |2000 |64 |64 x 500 |4  |
| Standard_H16mr <sup>1</sup> |16 |224 |2000 |64 |64 x 500 |4 |

<sup>1</sup> para aplicações de MPI, rede de back-end RDMA dedicada é ativada pela rede FDR InfiniBand, que proporciona o ultra baixa latência e alta largura de banda.

<br>






