---
title: Débito de aprovisionamento para o Azure Cosmos DB
description: Saiba como configurar o débito aprovisionado para os seus contentores do Azure Cosmos DB e as bases de dados.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: a97032344b904442ed3606c6297251578c3b4ff7
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263898"
---
# <a name="provision-throughput-on-azure-cosmos-containers-and-databases"></a>Débito de aprovisionar em contentores de Cosmos do Azure e bases de dados

Uma base de dados do Cosmos do Azure é uma unidade de gestão para um conjunto de contentores. Uma base de dados consiste num conjunto de contentores de esquema desconhecido. Um contentor do Cosmos do Azure é a unidade de escalabilidade para o débito e armazenamento. Um contentor é particionado horizontalmente através de um conjunto de máquinas dentro de uma região do Azure e é distribuído por todas as regiões do Azure à sua conta do Cosmos do Azure.

O Azure Cosmos DB permite-lhe configurar o débito em duas granularidades - **contentores do Azure Cosmos** e **bases de dados do Azure Cosmos**.

# <a name="setting-throughput-on-a-azure-cosmos-container"></a>A definição de débito num contentor do Cosmos do Azure  

O débito aprovisionado num contentor do Azure Cosmos é reservado exclusivamente para o contentor. O contentor recebe o débito aprovisionado o tempo todo. O débito aprovisionado num contentor com suporte financeiro suporte dos SLAs. Para configurar o débito num contentor, consulte [como aprovisionar o débito num contentor do Azure Cosmos](how-to-provision-container-throughput.md).

A definição de débito aprovisionado num contentor é a opção amplamente usada. Enquanto pode dimensionar o débito para um contentor ao aprovisionar qualquer quantidade de débito (RUs), não é possível especificar seletivamente a taxa de transferência para a partição ou partições lógicas. Quando a carga de trabalho em execução numa partição lógica consome mais do que o débito que foi alocado para a partição lógica específica, as operações irão obter taxa limitado. Quando ocorre a limitação de velocidade, pode aumentar o débito para o contêiner inteiro ou repita a operação. Para obter mais informações sobre a criação de partições, consulte [partições lógicas](partition-data.md).

Recomenda-se que configure o débito com a granularidade de contentor quando pretender que o desempenho garantido para o contentor.

Débito aprovisionado num contentor do Azure Cosmos é distribuído uniformemente por todas as partições lógicas do contentor. Uma vez que um ou mais partições lógicas de um contentor são alojadas por uma partição de recursos, as partições físicas pertencem exclusivamente para o contentor e o débito aprovisionado no contentor de suporte. A imagem seguinte mostra como uma partição de recursos hospeda um ou mais partições lógicas de um contentor:

![Partição de recursos](./media/set-throughput/resource-partition.png)

# <a name="setting-throughput-on-a-azure-cosmos-database"></a>A definição de débito num banco de dados do Cosmos do Azure

Quando aprovisionar o débito numa base de dados do Cosmos do Azure, o débito é compartilhado entre todos os contentores na base de dados, a menos que tiver especificado um débito aprovisionado em contentores específicos. O débito de base de dados entre seus contêineres de partilha é análogo ao que aloja uma base de dados num cluster de máquinas. Uma vez que todos os contentores dentro de uma base de dados partilham os recursos disponíveis numa máquina, naturalmente não obtém um desempenho previsível em qualquer contentor específico. Para configurar o débito numa base de dados, consulte [como configurar o débito aprovisionado num banco de dados do Azure Cosmos](how-to-provision-database-throughput.md).

Débito de definição numa base de dados do Cosmos do Azure garante que recebe o débito aprovisionado o tempo todo. Desde que todos os contentores dentro do compartilhamento de base de dados, o débito aprovisionado, o Azure Cosmos DB não fornece que garantias de qualquer débito previsível para um determinado contêiner nessa base de dados. A parte da taxa de transferência que pode receber um contentor específico está dependente:

* O número de contentores
* A escolha de chaves de partição para vários contêineres e
* A distribuição da carga de trabalho em várias partições lógicas dos contentores. 

Recomenda-se que configure o débito numa base de dados quando pretende partilhar a taxa de transferência em vários contentores, mas não pretender dedicar a taxa de transferência a nenhum contêiner em especial. Seguem-se alguns exemplos em que é preferencial para aprovisionar a taxa de transferência ao nível da base de dados:

* A partilha de débito aprovisionado de uma base de dados num conjunto de contentores é útil para uma aplicação multi-inquilino. Cada utilizador pode ser representado por um contentor do Azure Cosmos distinto.

* Partilhar o débito aprovisionado de uma base de dados num conjunto de contentores é útil quando estiver a migrar uma base de dados NoSQL (por exemplo, o MongoDB, Cassandra) alojado de um cluster de VMs ou de servidores físicos no local ao Azure Cosmos DB. Pode considerar o débito aprovisionado configurado na sua base de dados do Cosmos do Azure como um equivalente lógico (mas mais econômico e elástica) para que a capacidade de computação do seu cluster do MongoDB ou o Cassandra.  

Num determinado período de tempo, o débito alocado a um contentor numa base de dados é distribuído por todas as partições lógicas nesse contentor. Quando tiver o débito aprovisionado num banco de dados de partilha de contentores, não pode aplicar seletivamente a taxa de transferência para um contentor específico ou de uma partição lógica. Se a carga de trabalho numa partição lógica consome mais do que a taxa de transferência que é atribuída a uma partição lógica específica, suas operações será limitado de taxa. Quando ocorre a limitação de velocidade, pode aumentar o débito para o contêiner inteiro ou repita a operação. Para obter mais informações sobre a criação de partições, consulte [partições lógicas](partition-data.md).

Várias partições lógicas partilha o débito aprovisionado para um banco de dados podem ser hospedadas numa partição de recursos único. Enquanto uma única partição lógica de um contentor sempre tem um âmbito dentro de uma partição de recursos, partições lógicas do "L" em contentores de "C" partilha o débito aprovisionado das bases de dados podem ser mapeadas e alojadas em partições físicas de 'R'. A imagem seguinte mostra como uma partição de recursos pode alojar uma ou mais partições lógicas que pertencem a contentores diferentes dentro de uma base de dados:

![Partição de recursos](./media/set-throughput/resource-partition2.png)

## <a name="setting-throughput-on-a-azure-cosmos-database-and-a-container"></a>Definir débito de uma base de dados do Cosmos do Azure e um contentor

Pode combinar os dois modelos, aprovisionamento de débito no banco de dados e o contentor é permitido. O exemplo seguinte mostra como aprovisionar o débito num banco de dados do Cosmos do Azure e um contentor:

* Pode criar uma base de dados do Cosmos do Azure com o nome 'Z' com o débito aprovisionado de RUs "K". 
* Em seguida, cria cinco contentores com o nome A, B, C, D e E na base de dados.
* Pode configurar explicitamente o 'P' RUs do débito aprovisionado no contentor "B".
* O débito de RUs "K" é compartilhado entre os quatro contentores – A, C, D e E. A quantidade exata de débito disponível para A, C, D ou E irá variar e não há nenhum SLA para a taxa de transferência de cada contentor individuais.
* O contentor 'B' é garantido que tenha o débito de RUs 'P' todo o tempo e ele é suporte dos SLAs.

## <a name="comparison-of-models"></a>Comparação dos modelos

|**Quota**  |**Débito aprovisionado num banco de dados**  |**Débito aprovisionado num contentor**|
|---------|---------|---------|
|RUs mínimo |400 |400|
|RUs mínimas por contentor|100|400|
|RUs mínimas necessária para consumir a 1 GB de armazenamento|40|40|
|RUs máximos|Ilimitado, na base de dados|Ilimitado, no contentor|
|RUs atribuído/disponíveis para um contentor específico|Não existem garantias. RUs atribuídos a um determinado contêiner dependem as propriedades como - escolha de chaves de partição de contentores que partilhem o débito, distribuição de carga de trabalho, número de contentores. |Todos os RUs configurados no contentor exclusivamente estão reservados para o contentor.|
|Armazenamento máximo para um contentor|Ilimitado|Ilimitado|
|Débito máximo por partição lógica de um contentor|10 mil RUs|10 mil RUs|
|Armazenamento máximo (dados + índice) por partição lógica de um contentor|10 GB|10 GB|

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [partições lógicas](partition-data.md)
* Saiba [como aprovisionar o débito num contentor do Cosmos do Azure](how-to-provision-container-throughput.md)
* Saiba [como aprovisionar o débito, numa base de dados do Cosmos do Azure](how-to-provision-database-throughput.md)

