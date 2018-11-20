---
title: Solicitar unidades e o débito no Azure Cosmos DB
description: Saiba mais sobre como especificar e estimar os requisitos da unidade de pedido no Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: d1ccffab0e05787e362c7b1cc2bd534c7f9a8ada
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160465"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unidades de pedido no Azure Cosmos DB

Com o Azure Cosmos DB, paga a taxa de transferência, aprovisionamento e o armazenamento que consumir numa base horária. Taxa de transferência tem de ser aprovisionada para se certificar de que recursos de sistema suficientes disponíveis para a base de dados do Cosmos do Azure o tempo todo para atingirem ou ultrapassarem o SLA do Azure Cosmos DB.

O Azure Cosmos DB suporta uma variedade de APIs (SQL, MongoDB, Cassandra, Gremlin e tabela). Cada API tem seu próprio conjunto de operações de banco de dados, que vão desde a simple ponto lê e escreve consultas complexas. Cada operação de base de dados consome recursos do sistema consoante a complexidade da operação.  O custo de todas as operações de base de dados é normalizado pelo Azure Cosmos DB e é expresso em termos de unidades de pedido (RUs). O custo para ler um item de 1 KB é 1 unidade de pedido (1 RU) e o RUs mínimas necessárias para consumir a 1 GB de armazenamento é 40. Todas as outras operações de base de dados da mesma forma são atribuídas um custo em termos de RUs. Independentemente da API está a utilizar para interagir com o seu contentor do Cosmos do Azure e a operação de base de dados (escrita, ler, consulta), os custos são sempre medidos em termos de RUs.

Pode pensar RU/s como a moeda para a taxa de transferência. RU/s é uma moeda com base na taxa, que abstrai os recursos do sistema, tais como CPU, memória e IOPS, que são necessárias para realizar as operações de base de dados suportadas pelo Azure Cosmos DB. A imagem seguinte mostra as unidades de pedido consumidas por operações de base de dados diferente:

![Unidades de pedido de consumir de operações de base de dados](./media/request-units/request-units.png)

Para gerir e planear a capacidade, o Azure Cosmos DB garante que o número de RUs para uma operação de base de dados especificada ao longo de um determinado conjunto de dados é determinístico. Pode controlar o número de RUs consumidos por qualquer operação de base de dados, examinando o cabeçalho de resposta. Assim que compreender os fatores que afetam os custos da unidade de pedido e requisitos de débito da sua aplicação, pode executar a aplicação de forma económica.

Enquanto é-lhe cobrada numa base horária, Aprovisiona o número de RUs para a sua aplicação numa base por segundo em incrementos de 100 RU/s. Para dimensionar o débito aprovisionado para a sua aplicação, pode aumentar ou diminuir o número de RUs (na incrementos ou diminui de 100 RUs) em qualquer altura, por meio de programação ou através do portal do Azure.

Pode aprovisionar o débito em duas granularidades distintos: 

* **Contentores**. Para obter mais informações, consulte [como aprovisionar o débito num contentor do Azure Cosmos.](how-to-provision-container-throughput.md)
* **Bases de dados**. Para obter mais informações, consulte [como aprovisionar o débito, numa base de dados do Cosmos do Azure.](how-to-provision-database-throughput.md)

## <a name="request-unit-considerations"></a>Considerações de unidade de pedido

Ao calcular o número de RU/s para aprovisionar, é importante considerar os seguintes fatores:

* **Tamanho do item** – à medida que aumenta de tamanho de um item, o número de RUs consumidos para ler ou escrever o item também aumenta.

* **Indexação do item** -por predefinição, cada item é indexado automaticamente. Menos unidades de pedido são consumidas se optar por não indexar alguns dos seus itens num contentor.

* **Contagem de propriedade do item** -supondo que o padrão em todas as propriedades de indexação, o número de RUs consumidas para escrever um item aumenta à medida que os aumentos de contagem de propriedade do item.

* **Propriedades indexadas** -uma política de índice em cada contentor determina quais propriedades são indexadas por predefinição. Pode reduzir o consumo de unidades de pedido para operações de escrita ao limitar o número de propriedades indexadas.

* **Consistência dos dados** -os níveis de consistência forte e estagnação limitada consumam aproximadamente 2 X RUs mais durante a realização de operações de leitura quando comparada com a que de outro Relaxada níveis de consistência.

* **Padrões de consulta** -a complexidade de uma consulta afeta quantas unidades de pedido são consumidas para uma operação. Definir o número de resultados da consulta, o número de predicados, a natureza dos predicados, o número de funções definidas pelo utilizador, o tamanho dos dados de origem, o tamanho do resultado e projeções de afetam o custo das operações de consulta. O Azure Cosmos DB garante que a mesma consulta nos mesmos dados custará sempre o mesmo número de RUs no repetições de execuções.

* **Utilização do script** – como consultas, procedimentos armazenados e acionadores consumam RUs com base na complexidade das operações que está a ser executadas. Ao desenvolver seu aplicativo, Inspecione o cabeçalho de cobrança do pedido para compreender melhor quanta capacidade de unidade de pedido consome de cada operação.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [aprovisionamento um débito de contentores do Cosmos do Azure e bases de dados](set-throughput.md)
* Saiba mais sobre [Llogical partições](partition-data.md)
* Saiba mais sobre [globalmente dimensionar o débito aprovisionado](scaling-throughput.md)
* Saiba [como aprovisionar o débito num contentor do Cosmos do Azure](how-to-provision-container-throughput.md)
* Saiba [como aprovisionar o débito, numa base de dados do Cosmos do Azure](how-to-provision-database-throughput.md)
