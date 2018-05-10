---
title: Limites de memória e simultaneidade - Azure SQL Data Warehouse | Microsoft Docs
description: Ver os limites de memória e simultaneidade atribuídos a vários níveis de desempenho e classes de recursos no Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/07/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 46d41e3ee85deb20f189bc9c82a255178f3d7eee
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Limites de memória e simultaneidade para o Azure SQL Data Warehouse
Ver os limites de memória e simultaneidade atribuídos a vários níveis de desempenho e classes de recursos no Azure SQL Data Warehouse. Para obter mais informações e para aplicar estas capacidades ao seu plano de gestão de carga de trabalho, consulte [classes de recursos para a gestão de carga de trabalho](resource-classes-for-workload-management.md). 

Existem atualmente duas gerações disponíveis no armazém de dados do SQL Server – Gen1 e Gen2. Recomendamos que utilize Gen2 do SQL Server do armazém de dados para obter o melhor desempenho para a carga de trabalho do armazém de dados. Gen2 introduz uma nova cache do disco de estado sólido NVMe que mantém os dados acedidos com mais frequência próximo as CPUs. Esta ação remove a e/s remoto para as cargas de trabalho mais intensivas e pedir o seu trabalho. Para além de desempenho, Gen2 oferece o nível de escala maior, permitindo-lhe dimensionar até 30 000 unidades de armazém de dados e fornecer armazenamento columnar ilimitado. Iremos suportar a geração anterior (Gen1) do SQL Data Warehouse ainda e manter as funcionalidades da mesmas; No entanto, Aconselhamo-lo para [atualizar para Gen2](upgrade-to-latest-generation.md) convier mais antigo. 

## <a name="data-warehouse-capacity-settings"></a>Definições de capacidade do armazém de dados
As tabelas seguintes mostram a capacidade máxima para o armazém de dados em níveis de desempenho diferentes. Para alterar o nível de desempenho, consulte [dimensionar a computação - portal](quickstart-scale-compute-portal.md).

### <a name="gen2"></a>Ger2

Gen2 fornece 2,5 x mais memória por consulta que o Gen1. Esta memória adicional ajuda Gen2 fornecer o desempenho dele rápida.  Os níveis de desempenho para o intervalo de Gen2 de DW1000c a DW30000c. 

| Nível de desempenho | Nós de computação | Distribuições por nó de computação | Memória por do armazém de dados (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW1000c           | 2             | 30                             |   600                          |
| DW1500c           | 3             | 20                             |   900                          |
| DW2000c           | 4             | 15                             |  1200                          |
| DW2500c           | 5             | 12                             |  1500                          |
| DW3000c           | 6             | 10                             |  1800                          |
| DW5000c           | 10            | 6                              |  3000                          |
| DW6000c           | 12            | 5                              |  3600                          |
| DW7500c           | 15            | 4                              |  4500                          |
| DW10000c          | 20            | 3                              |  6000                          |
| DW15000c          | 30            | 2                              |  9000                          |
| DW30000c          | 60            | 1                              | 18000                          |

O DWU Gen2 máximo é DW30000c, que tem 60 nós de computação e de uma distribuição por nó de computação. Por exemplo, um armazém de dados a 600 TB no DW30000c processa aproximadamente 10 TB por nó de computação.

### <a name="gen1"></a>Ger1

Os níveis de serviço para o intervalo de Gen1 entre DW100 a DW6000. 

| Nível de desempenho | Nós de computação | Distribuições por nó de computação | Memória por do armazém de dados (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100             | 1             | 60                             |  24                            |
| DW200             | 2             | 30                             |  48                            |
| DW300             | 3             | 20                             |  72                            |
| DW400             | 4             | 15                             |  96                            |
| DW500             | 5             | 12                             | 120                            |
| DW600             | 6             | 10                             | 144                            |
| DW1000            | 10            | 6                              | 240                            |
| DW1200            | 12            | 5                              | 288                            |
| DW1500            | 15            | 4                              | 360                            |
| DW2000            | 20            | 3                              | 480                            |
| DW3000            | 30            | 2                              | 720                            |
| DW6000            | 60            | 1                              | 1440                           |

## <a name="concurrency-maximums"></a>Valores máximos de concorrência
Para garantir que cada consulta tem recursos suficientes para executar de forma eficiente, SQL Data Warehouse controla a utilização de recursos através da atribuição de ranhuras de concorrência para cada consulta. O sistema coloca consultas para uma fila onde estes aguardar suficiente [ranhuras de concorrência](resource-classes-for-workload-management.md#concurrency-slots) estão disponíveis. Ranhuras de concorrência também determinam a atribuição de prioridades de CPU. Para obter mais informações, consulte [analisar a carga de trabalho](analyze-your-workload.md)

### <a name="gen2"></a>Ger2
 
**Classes de recurso estático**

A tabela seguinte mostra as consultas em simultâneo máximas e ranhuras de concorrência para cada [classe de recursos estático](resource-classes-for-workload-management.md).  

| Nível de Serviço | Consultas em simultâneo máximas | Ranhuras de concorrência disponíveis |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Classes de recursos dinâmicos**

> [!NOTE]
> A classe de recursos de smallrc no Gen2 dinamicamente adiciona memória como o nível de serviço aumenta e só suporta um consultas em simultâneo de 32 máximo.  As ranhuras de simultaneidade e a memória utilizada pelo smallrc aumenta à medida que aumenta de nível de serviço. 
>
>

A tabela seguinte mostra as consultas em simultâneo máximas e ranhuras de concorrência para cada [classe de recursos dinâmicos](resource-classes-for-workload-management.md). Ao contrário Gen1, classes de recursos dinâmicos em Gen2 são verdadeiramente dinâmicas.  Gen2 utiliza uma alocação de percentagem de memória de 3-10-22-70 para classes de recursos de pequena-média-grande-xlarge em todos os níveis de serviço.

| Nível de Serviço | Consultas em simultâneo máximas | Ranhuras de concorrência disponíveis | Ranhuras utilizadas pelo smallrc | Ranhuras utilizadas pelo mediumrc | Ranhuras utilizadas pelo largerc | Ranhuras utilizadas pelo xlargerc |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW1000c       | 32                         |   40                        | 1                     |  4                     |  8                    |  28                    |
| DW1500c       | 32                         |   60                        | 1                     |  6                     |  13                   |  42                    |
| DW2000c       | 32                         |   80                        | 2                     |  8                     |  17                   |  56                    |
| DW2500c       | 32                         |  100                        | 3                     | 10                     |  22                   |  70                    |
| DW3000c       | 32                         |  120                        | 3                     | 12                     |  26                   |  84                    |
| DW5000c       | 32                         |  200                        | 6                     | 20                     |  44                   | 140                    |
| DW6000c       | 32                         |  240                        | 7                     | 24                     |  52                   | 168                    |
| DW7500c       | 32                         |  300                        | 9                     | 30                     |  66                   | 210                    |
| DW10000c      | 32                         |  400                        | 12                    | 40                     |  88                   | 280                    |
| DW15000c      | 32                         |  600                        | 18                    | 60                     | 132                   | 420                    |
| DW30000c      | 32                         | 1200                        | 36                    | 120                    | 264                   | 840                    |



#### <a name="gen1"></a>Ger1

Classes de recurso estático

A tabela seguinte mostra as consultas em simultâneo máximas e ranhuras de concorrência para cada [classe de recursos estático](resource-classes-for-workload-management.md) no **Gen1**.

| Nível de serviço  | Consultas em simultâneo máximas | Ranhuras de concorrência máximo |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:-------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100         | 4                          |   4                       | 1         | 2          | 4          | 4          |  4         |  4         |  4         |   4        |
| DW200         | 8                          |   8                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW300         | 12                         |  12                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400         | 16                         |  16                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500         | 20                         |  20                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW600         | 24                         |  24                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000        | 32                         |  40                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1200        | 32                         |  48                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500        | 32                         |  60                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000        | 48                         |  80                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000        | 64                         | 120                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW6000        | 128                        | 240                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

Classes de recursos dinâmicos
> [!NOTE]
> A classe de recursos de smallrc no Gen1 atribui uma quantidade fixa de memória por consulta, de forma semelhante a staticrc10 de classe de recurso estático.  Uma vez smallrc é estático, tem a capacidade de dimensionar a 128 consultas em simultâneo. 
>
>

A tabela seguinte mostra as consultas em simultâneo máximas e ranhuras de concorrência para cada [classe de recursos dinâmicos](resource-classes-for-workload-management.md) no **Gen1**.

| Nível de serviço  | Consultas em simultâneo máximas | Ranhuras de concorrência disponíveis | smallrc | mediumrc | largerc | xlargerc |
|:-------------:|:--------------------------:|:---------------------------:|:-------:|:--------:|:-------:|:--------:|
| DW100         |  4                         |   4                         | 1       |  1       |  2      |   4      |
| DW200         |  8                         |   8                         | 1       |  2       |  4      |   8      |
| DW300         | 12                         |  12                         | 1       |  2       |  4      |   8      |
| DW400         | 16                         |  16                         | 1       |  4       |  8      |  16      |
| DW500         | 20                         |  20                         | 1       |  4       |  8      |  16      |
| DW600         | 24                         |  24                         | 1       |  4       |  8      |  16      |
| DW1000        | 32                         |  40                         | 1       |  8       | 16      |  32      |
| DW1200        | 32                         |  48                         | 1       |  8       | 16      |  32      |
| DW1500        | 32                         |  60                         | 1       |  8       | 16      |  32      |
| DW2000        | 48                         |  80                         | 1       | 16       | 32      |  64      |
| DW3000        | 64                         | 120                         | 1       | 16       | 32      |  64      |
| DW6000        | 128                        | 240                         | 1       | 32       | 64      | 128      |


Quando for cumprida uma destes limiares, consultas novas são colocados em fila e executadas numa base first in, First Out.  Como um consultas concluída e o número de consultas e de ranhuras descerem abaixo os limites, o SQL Data Warehouse versões consultas em fila. 

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como tirar partido das classes de recursos para otimizar ainda mais a carga de trabalho, reveja os artigos seguintes:
* [Classes de recursos para a gestão de carga de trabalho](resource-classes-for-workload-management.md)
* [Analisar a carga de trabalho](analyze-your-workload.md)

