---
title: Níveis de consistência e APIs do Azure Cosmos DB | Documentos da Microsoft
description: Compreenda os níveis de consistência em APIs no Azure Cosmos DB.
keywords: consistência, do azure cosmos db, azure, modelos, mongodb, cassandra, gráfico, tabela, do Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 22ddfbf86dacdd5d7f80d1fa7826d6ae11c4de45
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52680400"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Níveis de consistência e APIs do Azure Cosmos DB

Cinco modelos de consistência oferecidos pelo Azure Cosmos DB são suportados nativamente pelo Azure Cosmos DB SQL API. Quando utiliza o Azure Cosmos DB, a API de SQL é a predefinição. 

O Azure Cosmos DB também fornece suporte nativo para conexão APIs compatíveis com o protocolo para bases de dados populares. Bases de dados incluem o armazenamento de MongoDB, Apache Cassandra, Gremlin e tabelas do Azure. Esses bancos de dados não oferecem precisamente os modelos de consistência definidos ou garantias apoiado pelo SLA para níveis de consistência. Normalmente, eles fornecem apenas um subconjunto dos modelos de cinco consistência oferecidos pelo Azure Cosmos DB. Para a API de SQL, o API do Gremlin e a API de tabela, é utilizado o nível de consistência predefinido configurado na conta do Azure Cosmos DB. 

As secções seguintes mostram o mapeamento entre a consistência de dados solicitada por um controlador de cliente de sistemas operacionais para o Apache Cassandra 4.x e de invocação MongoDB 3.4. Este documento também mostra os níveis de consistência correspondentes do Azure Cosmos DB para o Apache Cassandra e MongoDB.

## <a id="cassandra-mapping"></a>Mapeamento entre os níveis de consistência do Apache Cassandra e o Azure Cosmos DB

Esta tabela mostra o mapeamento de "consistência de leitura" entre o cliente do Apache Cassandra 4.x e o nível predefinido de consistência no Azure Cosmos DB. A tabela mostra as implementações de várias regiões e única região.

| **O Apache Cassandra 4.x** | **O Azure Cosmos DB (várias regiões)** | **O Azure Cosmos DB (região única)** |
| - | - | - |
| UM, DOIS, TRÊS | Prefixo consistente | Prefixo consistente |
| LOCAL_ONE | Prefixo consistente | Prefixo consistente |
| QUÓRUM, TODAS, SÉRIE | Estagnação limitada é a predefinição. Forte está em pré-visualização privada. | Forte |
| LOCAL_QUORUM | Estagnação limitada | Forte |
| LOCAL_SERIAL | Estagnação limitada | Forte |

## <a id="mongo-mapping"></a>Mapeamento entre os níveis de consistência de invocação MongoDB 3.4 e o Azure Cosmos DB

A tabela seguinte mostra o mapeamento de "ler preocupações" entre invocação MongoDB 3.4 e o nível predefinido de consistência no Azure Cosmos DB. A tabela mostra as implementações de várias regiões e única região.

| **Invocação MongoDB 3.4** | **O Azure Cosmos DB (várias regiões)** | **O Azure Cosmos DB (região única)** |
| - | - | - |
| Linearizable | Forte | Forte |
| Maioria | Estagnação limitada | Forte |
| Local | Prefixo consistente | Prefixo consistente |

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os níveis de consistência e a compatibilidade entre as APIs do Azure Cosmos DB com as APIs de código-fonte aberto. Consulte os seguintes artigos:

* [Desvantagens de desempenho e disponibilidade para vários níveis de consistência](consistency-levels-tradeoffs.md)
* [Funcionalidades do MongoDB suportadas pela API de MongoDB do Azure Cosmos DB](mongodb-feature-support.md)
* [Recursos do Apache Cassandra suportados a API de Cassandra do Azure Cosmos DB](cassandra-support.md)