---
title: Criação de partições e dimensionamento horizontal no Azure Cosmos DB
description: Saiba mais sobre a criação de partições como funciona no Azure Cosmos DB, como configurar a criação de partições e chaves de partição e como escolher a chave de partição correta para a sua aplicação.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: 38f587fc24478beff3ab236207de3ed8a892c915
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998953"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Criação de partições e dimensionamento horizontal no Azure Cosmos DB

Este artigo explica sobre as partições físicas e lógicas no Azure Cosmos DB e as melhores práticas para dimensionamento e a criação de partições. 

## <a name="logical-partitions"></a>Partições lógicas

Uma partição lógica consiste num conjunto de itens com a mesma chave de partição. Por exemplo, considere um contentor onde todos os itens contêm uma `City` propriedade, em seguida, pode usar `City` como a chave de partição para o contentor. Grupos de itens com valores específicos para o `City` como, por exemplo "Londres", "Paris", "NYC" etc. serão formam uma partição lógica distinta.

No Azure Cosmos DB, um contentor é a unidade fundamental de escalabilidade. Os dados adicionados para o contentor e a taxa de transferência que for aprovisionado no contentor são automaticamente particionada (horizontalmente) num conjunto de partições lógicas. Eles são divididos em partições com base na chave de partição que especificou para o contentor do Cosmos. Para obter mais informações, consulte [como especificar a chave de partição para o contentor do Cosmos](how-to-create-container.md) artigo.

Uma partição lógica define o âmbito das transações da base de dados. Pode atualizar itens dentro de uma partição lógica através de uma transação com o isolamento do instantâneo.

Quando novos itens são adicionados ao contêiner ou se o débito aprovisionado no contentor for aumentado, as novas partições lógicas transparente são criadas pelo sistema.

## <a name="physical-partitions"></a>Partições físicas

Um contentor do Cosmos é dimensionado ao distribuir os dados e o débito num grande número de partições lógicas. Internamente, uma ou mais partições lógicas estão mapeadas para uma **partição física** que consiste num conjunto de réplicas, também conhecido como um conjunto de réplicas. Cada conjunto de réplicas aloja uma instância do motor de base de dados do Cosmos. Um conjunto de réplicas torna os dados armazenados na partição física durável, de elevada disponibilidade e consistente. Uma partição física suporta uma quantidade fixa, máxima de armazenamento e RUs. Cada réplica que inclui a partição física herda a quota de armazenamento. E todas as réplicas de uma partição física coletivamente suportam a taxa de transferência alocada para a partição física. A imagem seguinte mostra como lógica partições são mapeadas para as partições físicas que são distribuídas globalmente:

![Criação de partições de Cosmos DB do Azure](./media/partition-data/logical-partitions.png)

Débito aprovisionado para um contentor é dividido igualmente entre as partições físicas. Por isso uma estrutura de chave de partição que não a distribuir uniformemente os pedidos de débito pode criar partições de "acesso frequentes". Partições muito ativas podem resultar na utilização de limitação de velocidade e ineficiente do débito aprovisionado.

Ao contrário das partições lógicas, as partições físicas são uma implementação interna do sistema. Não é possível controlar seu tamanho, posicionamento, a contagem ou o mapeamento entre as partições lógicas e as partições físicas. No entanto, pode controlar o número de partições lógicas e a distribuição de dados e débito, escolhendo a chave de partição correta.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, fornecido uma descrição geral da criação de partições de dados e as melhores práticas para dimensionamento e a criação de partições no Azure Cosmos DB. 

* Saiba mais sobre [débito aprovisionado no Azure Cosmos DB](request-units.md)
* Saiba mais sobre [distribuição global no Azure Cosmos DB](distribute-data-globally.md)
* Saiba mais sobre [escolher uma chave de partição](partitioning-overview.md#choose-partitionkey)
* Saiba [como aprovisionar o débito num contentor do Cosmos](how-to-provision-container-throughput.md)
* Saiba [como aprovisionar o débito, numa base de dados do Cosmos](how-to-provision-database-throughput.md)
