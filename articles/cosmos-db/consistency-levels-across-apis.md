---
title: Níveis de consistência e APIs do Azure Cosmos DB | Documentos da Microsoft
description: Compreender os níveis de consistência através de APIs no Azure Cosmos DB.
keywords: consistência, do azure cosmos db, azure, modelos, mongodb, cassandra, gráfico, tabela, do Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: ed08b90b9e216ee8713bfe445e98144bf2ba02d4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244193"
---
# <a name="consistency-levels-and-cosmos-db-apis"></a>Níveis de consistência e APIs do Cosmos DB

Os modelos de cinco consistência são suportados nativamente pela API do SQL, que é a API de predefinição ao utilizar o Cosmos DB. Além da API de SQL, o Cosmos DB também fornece suporte nativo para APIs compatíveis do protocolo de transmissão para bases de dados populares, como o MongoDB, Apache Cassandra, Gremlin e as tabelas do Azure. Esses bancos de dados oferecem nenhuma definidas precisamente modelos de consistência nem apoiado pelo SLA garante para os níveis de consistência e oferecem, normalmente, apenas um subconjunto dos modelos de cinco consistência que o Cosmos DB oferece. Para a API de SQL, Gremlin API e API de tabela, é utilizado o nível de consistência predefinido configurado na conta do Cosmos.

A tabela seguinte mostra o mapeamento entre a consistência dos dados solicitado por um controlador de cliente de sistemas operacionais para o Apache Cassandra 4.x e invocação MongoDB 3.4 ao utilizar a API de Cassandra e a API de MongoDB, respectivamente e os níveis de consistência correspondentes do Cosmos DB.

## <a id="cassandra-mapping"></a>Mapeamento de níveis de consistência do Apache Cassandra e o Cosmos DB

A tabela abaixo mostra o mapeamento para consistência de leitura entre o cliente do Apache Cassandra 4.x e o Cosmos DB "Padrão" um nível de consistência para ambos os uma implementação de várias regiões e única região.

| **O Apache Cassandra 4.x** | **Cosmos DB (várias regiões)** | **Cosmos DB (única região)** |
| - | - | - |
| UM, DOIS, TRÊS | Prefixo Consistente | Prefixo Consistente |
| LOCAL_ONE | Prefixo Consistente | Prefixo Consistente |
| QUÓRUM, TODAS, SÉRIE | Estagnação limitada (predefinição) ou seguro (em pré-visualização privada) | Forte |
| LOCAL_QUORUM | Estagnação Limitada | Forte |
| LOCAL_SERIAL | Estagnação Limitada | Forte |

## <a id="mongo-mapping"></a>Mapeamento entre os níveis de consistência de invocação MongoDB 3.4 e o Cosmos DB

A tabela abaixo mostra o mapeamento para "preocupações leitura" de invocação MongoDB 3.4 e Cosmos DB o nível de consistência "Padrão" para ambos os uma implementação de várias regiões e única região.

| **Invocação MongoDB 3.4** | **Cosmos DB (várias regiões)** | **Cosmos DB (única região)** |
| - | - | - |
| Linearizable | Forte | Forte |
| Maioria | Estagnação Limitada | Forte |
| Local | Prefixo Consistente | Prefixo Consistente |

## <a name="next-steps"></a>Passos Seguintes

Leia mais sobre níveis de consistência e a compatibilidade entre o Cosmos DB APIs com as APIs de código-fonte aberto nos seguintes artigos:

* [Desvantagens de desempenho e disponibilidade para vários níveis de consistência](consistency-levels-tradeoffs.md)
* [Funcionalidades do MongoDB suportadas pela API do MongoDB do Cosmos DB](mongodb-feature-support.md)
* [Funcionalidades de Apache Cassandra suportadas pela API de Cassandra do Cosmos DB](cassandra-support.md)