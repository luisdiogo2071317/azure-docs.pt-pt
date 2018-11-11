---
title: Dimensionar o débito no Azure Cosmos DB
description: Este artigo descreve como o Azure Cosmos DB dimensiona débito de forma elástica
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: d29f01c7f953ed211b429e41b844a01c67e41054
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282382"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Dimensionar o débito no Azure Cosmos DB

No Azure Cosmos DB, o débito aprovisionado é representado como pedido unidades por segundo (RU/s, plural: RUs). RUs avaliar o custo de leitura e escrita em relação a seu contentor do Cosmos como mostrado na imagem seguinte:

![Unidades de Pedido](./media/scale-throughput/figure1.png)

Pode aprovisionar RUs num contentor do Cosmos ou uma base de dados do Cosmos. RUs aprovisionadas num contentor são exclusivamente disponíveis para as operações executadas nesse contentor. RUs aprovisionadas numa base de dados são partilhados entre todos os contentores dentro dessa base de dados (exceto para os contentores que tenham exclusivamente atribuídas RUs).

Para dimensionar elasticamente o débito, pode aumentar ou diminuir o RU/s aprovisionada em qualquer altura. Para obter mais informações, consulte [débito aprovisionar procedimentos](set-throughput.md) e dimensionar bases de dados e contentores de Cosmos. Para globalmente dimensionar o débito, pode adicionar ou remover regiões na sua conta do Cosmos em qualquer altura. Para obter mais informações, consulte [procedimentos adicionar ou remover regiões à sua conta do Cosmos](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Associação de várias regiões com uma conta do Cosmos é importante em muitos cenários para alcançar a baixa latência e [elevada disponibilidade](high-availability.md) em todo o mundo.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>como o débito aprovisionado é distribuído em várias regiões

Se aprovisionar 'R' RUs num contêiner de Cosmos (ou da base de dados), o Cosmos DB garante que o RUs 'R' estão disponíveis no *cada* região associada à sua conta do Cosmos. Sempre que adicionar uma nova região à sua conta do Cosmos DB Aprovisiona automaticamente 'R' RUs na região recentemente adicionado. As operações executadas em relação a seu contentor do Cosmos são garantidas para obter o RUs 'R' em cada região. Não é possível atribuir seletivamente RUs numa região específica. O RUs aprovisionadas para um contentor do Cosmos (ou a base de dados) é aprovisionado para todas as regiões à sua conta do Cosmos.

Partindo do princípio de que um contentor do Cosmos está configurado com 'R' RUs e existem "n" regiões associadas com a conta do Cosmos, em seguida:

- Se a conta do Cosmos estiver configurada com uma região de escrita única, o total de RUs disponível globalmente no contentor = R x N.

- Se a conta do Cosmos estiver configurada com várias regiões de escrita, o total de RUs disponível globalmente no contentor = R x (N + 1). O RUs de R adicionais é aprovisionado automaticamente para os conflitos de atualização de processo e o tráfego de entropia anti em todas as regiões.

Sua escolha de [modelo de consistência](consistency-levels.md) afetará também o débito. Pode obter aproximadamente 2 x débito de leitura para a sessão, prefixo consistente e eventual consistência em comparação com a estagnação limitada ou consistência forte.

## <a name="next-steps"></a>Passos Seguintes

Em seguida pode saber como configurar o débito com a ajuda do seguinte artigo:

* [Obter e definir a taxa de transferência para contentores e bases de dados](set-throughput.md) 

