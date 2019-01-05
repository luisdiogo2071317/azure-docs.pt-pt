---
title: Presença regional com o Azure Cosmos DB
description: Este artigo explica sobre a presença regional do Azure Cosmos DB e ambientes de cloud diferentes.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 5a4523405f7a9182bb5123ebacaebc9a9e5ae9a9
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54038668"
---
# <a name="regional-presence-of-azure-cosmos-db"></a>Presença regional do Azure Cosmos DB

Atualmente, o Azure está disponível em [54 regiões](https://azure.microsoft.com/global-infrastructure/regions/) em todo o mundo. O Azure Cosmos DB é um serviço fundamental no Azure e está disponível em todas as regiões onde o Azure está disponível.

[![Regiões onde o Azure Cosmos DB está disponível](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

O cosmos DB está disponível em todos os cinco ambientes distintos de cloud do Azure disponíveis para clientes:

* **Azure público** nuvem, o que está disponível globalmente.

* **O Azure na China** está disponível através de uma parceria única entre a Microsoft e a 21Vianet, um dos maiores fornecedores de internet no país.

* **O Azure Alemanha** fornece serviços através de um modelo de consignação de dados, o que garante que o cliente dados permanecem na Alemanha sob o controle do T-Systems International GmbH, uma subsidiária da Deutsche telecomunicações, que funcionam como os dados na Alemanha.

* **O Azure Government** está disponível em quatro regiões nos Estados Unidos para organismos públicos dos E.U.A. e respetivos parceiros. 

* **O Azure Government para o departamento de defesa** está disponível em duas regiões nos Estados Unidos para o departamento de defesa.

## <a name="regional-presence-with-global-distribution"></a>Presença regional com a distribuição global

Diferentes APIs expostas pelo Azure Cosmos DB (incluindo o armazenamento SQL, MongoDB, Cassandra, Gremlin e tabelas do Azure) estão disponíveis em todas as regiões do Azure. Por exemplo, pode ter o MongoDB e APIs de Cassandra expostos pelo Azure Cosmos DB não apenas em todas as regiões globais do Azure, mas também em regiões do Azure garante a soberania: como China, Alemanha, administração pública e o departamento de defesa regiões (DoD).

Azure Cosmos DB é um [globalmente distribuída](distribute-data-globally.md) base de dados. Pode associar qualquer número de regiões do Azure com a sua conta do Cosmos do Azure e os dados são automática e transparente replicados. Pode adicionar ou remover uma região para a sua conta do Cosmos do Azure em qualquer altura. Com a capacidade de distribuição global chave na mão e o protocolo de replicação dominado por várias, Azure Cosmos DB oferece menos de 10 ms de leitura e escrita latências no percentil 99, 99,999 ler e escrever a disponibilidade e capacidade de dimensionar de forma elástica aprovisionado taxa de transferência para leituras e escritas em todas as regiões à conta do Cosmos do Azure. O Azure Cosmos DB, também oferece cinco modelos de consistência bem definidos e pode optar por aplicar um modelo de consistência específicos aos seus dados. Por fim, o Azure Cosmos DB é o único serviço de base de dados do setor que fornece um abrangente contrato de nível de serviço (SLA) abrangente débito, latência no percentil de 99, elevada disponibilidade e consistência.

## <a name="next-steps"></a>Passos Seguintes

Agora pode saber sobre os diferentes conceitos do Azure Cosmos DB com os seguintes artigos:

* [Distribuição de dados globais](distribute-data-globally.md)
* [Como gerir uma conta do Azure Cosmos DB](manage-account.md)
* [Débito de aprovisionamento para contentores do Cosmos do Azure e bases de dados](set-throughput.md)
* [SLA do cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
