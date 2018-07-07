---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 512f251a91a035d3d48566c414076b1a5b6d8805
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37907105"
---
Armazenamento de tamanhos de VM otimizados oferecem débito de disco elevados e e/s e são ideais para bases de dados de grandes volumes de dados SQL e NoSQL. Este artigo fornece informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento e débito de rede da largura de banda para cada tamanho neste agrupamento. 

A série Ls oferece até 32 vCPUs, com o [processador Intel® Xeon® E5 v3 família](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). A série Ls tem o mesmo desempenho de CPU que a série G/GS e dispõe de 8 GiB de memória por vCPU.  

## <a name="ls-series"></a>Série Ls

ACU: 180-240

O armazenamento Premium: suportado

Cache de armazenamento Premium: Não suportado
 
| Tamanho          | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Discos de dados máximos | Débito do armazenamento temporário máximo: IOPS / MBps | Débito máximo do disco não colocado em cache: IOPS/MBps | NICs. Máx. / esperado de largura de banda de rede (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4    | 32   | 678   | 16    | 20,000 / 200   | 5,000 / 125        | 2 / 4,000  | 
| Standard_L8s   | 8    | 64   | 1,388 | 32   | 40,000 / 400   | 10,000 / 250       | 4 / 8,000  | 
| Standard_L16s  | 16   | 128  | 2,807 | 64   | 80,000 / 800   | 20,000 / 500       | 8 / 16,000 | 
| Standard_L32s <sup>1</sup> | 32   | 256  | 5,630 | 64   | 160,000 / 1,600   | 40,000 / 1,000     | 8 / 20,000 | 
 

O débito máximo do disco possível com VMs da série Ls pode estar limitado pelo número, tamanho e repartição de quaisquer discos de anexados. Para obter mais detalhes, veja [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho de máquinas virtuais do Azure](../articles/virtual-machines/windows/premium-storage.md).

<sup>1</sup> instância está isolada do hardware dedicado de um único cliente.

## <a name="size-table-definitions"></a>Definições da tabela de dimensionamento

- A capacidade de armazenamento é apresentada em unidades de GiB ou 1024^3 bytes. Quando comparar discos medidos em GB (1000^3 bytes) com discos medidos em GiB (1024^3), não se esqueça de que os números de capacidade especificados em GiB podem aparecer mais pequenos. Por exemplo, 1023 GiB = 1098,4 GB
- O débito do disco é medido em operações de entrada/saída por segundo (IOPS) e MBps, em que MBps = 10^6 bytes/seg.
- Se quiser obter o melhor desempenho para as suas VMs, deve limitar o número de discos de dados de 2 discos por vCPU.
- **Esperado de largura de banda de rede** é o máximo agregado [largura de banda alocada por tipo de VM](../articles/virtual-network/virtual-machine-network-throughput.md) em todas as NICs, para todos os destinos. Os limites superiores não são garantidos, mas foram concebidos para fornecer orientações para selecionar o tipo de VM correto para a aplicação pretendida. O desempenho de rede real irá depender de vários fatores, incluindo congestionamento, cargas e definições da rede. Para obter mais informações sobre a otimização do débito de rede, veja [Otimizar o débito de rede para Windows e Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Para alcançar o desempenho de rede esperado no Linux ou no Windows, poderá ser necessário selecionar uma versão específica ou otimizar a VM. Para obter mais informações, veja [Como fazer um teste fiável de um débito de máquina virtual](../articles/virtual-network/virtual-network-bandwidth-testing.md).
