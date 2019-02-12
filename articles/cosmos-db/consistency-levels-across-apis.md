---
title: Níveis de consistência e APIs do Azure Cosmos DB
description: Compreender os níveis de consistência através de APIs no Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: b620ca76cfea296e504afffd91852308a01575db
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002013"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Níveis de consistência e APIs do Azure Cosmos DB

Cinco modelos de consistência oferecidos pelo Azure Cosmos DB são suportados nativamente pela API do SQL. Quando utiliza o Azure Cosmos DB, a API de SQL é a predefinição. 

O Azure Cosmos DB também fornece suporte nativo para conexão APIs compatíveis com o protocolo para bases de dados populares. Bases de dados incluem o armazenamento de MongoDB, Apache Cassandra, Gremlin e tabelas do Azure. Esses bancos de dados não oferecem precisamente os modelos de consistência definidos ou garantias apoiado pelo SLA para níveis de consistência. Normalmente, eles fornecem apenas um subconjunto dos modelos de cinco consistência oferecidos pelo Azure Cosmos DB. Para a API de SQL, o API do Gremlin e a API de tabela, é utilizado o nível de consistência predefinido configurado na conta do Cosmos do Azure. 

As secções seguintes mostram o mapeamento entre a consistência de dados solicitada por um controlador de cliente de sistemas operacionais para Apache Cassandra, MongoDB e os níveis de consistência correspondente no Azure Cosmos DB.

## <a id="cassandra-mapping"></a>Mapeamento entre os níveis de consistência do Apache Cassandra e o Azure Cosmos DB

Tabela a seguir descreve a combinação de consistência vários, é possível usar em relação a API de Cassandra e o mapeamento de nível de consistência nativo equivalente do Cosmos DB. Combinação de todos os de escrita do Apache Cassandra e modos de leitura são suportados nativamente pelo Cosmos DB. Em todas as combinações de escrita do Apache Cassandra e o modelo de consistência de leitura, o Cosmos DB irá fornecer garante a consistência igual ou superior a Apache Cassandra. Além disso, o Cosmos DB fornece uma durabilidade superior garante que o Apache Cassandra até mesmo no modo mais fraco de gravação.

A tabela seguinte mostra os **escrever mapeamento de consistência** entre o Azure Cosmos DB e Cassandra:

| Cassandra | Azure Cosmos DB | Garantia |
| - | - | - |
|TODOS|Forte  | Transação Atómica |
| EACH_QUORUM   | Forte    | Transação Atómica | 
| QUORUM, SERIAL |  Forte |    Transação Atómica |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | Prefixo Consistente |Prefixo consistente global |
| EACH_QUORUM   | Forte    | Transação Atómica |
| QUORUM, SERIAL |  Forte |    Transação Atómica |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | Prefixo Consistente | Prefixo consistente global |
| QUORUM, SERIAL | Forte   | Transação Atómica |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | Prefixo Consistente | Prefixo consistente global |
| LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE    | Estagnação Limitada | <ul><li>Estagnação limitada.</li><li>Na maioria das versões K ou hora de t por trás.</li><li>Leia mais recente confirmada valor na região.</li></ul> |
| ONE, LOCAL_ONE, ANY   | Prefixo Consistente | Prefixo consistente de por região |

A tabela seguinte mostra os **mapeamento de consistência de leitura** entre o Azure Cosmos DB e Cassandra:

| Cassandra | Azure Cosmos DB | Garantia |
| - | - | - |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO, ONE, LOCAL_ONE | Forte  | Transação Atómica|
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |Forte |   Transação Atómica |
|LOCAL_ONE, ONE | Prefixo Consistente | Prefixo consistente global |
| ALL, QUORUM, SERIAL   | Forte    | Transação Atómica |
| LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  Prefixo Consistente   | Prefixo consistente global |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM |    Prefixo Consistente   | Prefixo consistente global |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |Forte |   Transação Atómica |
| LOCAL_ONE, ONE    | Prefixo Consistente | Prefixo consistente global|
| TUDO, o QUÓRUM, SERIAL forte transação Atómica
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |Prefixo Consistente  | Prefixo consistente global |
|TODOS    |Forte |Transação Atómica |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  |Prefixo Consistente  |Prefixo consistente global|
|TUDO, o QUÓRUM, SERIAL forte transação Atómica
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |Prefixo Consistente  |Prefixo consistente global |
|TODOS    |Forte | Transação Atómica |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | Prefixo Consistente | Prefixo consistente global |
| QUORUM, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  Estagnação Limitada   | <ul><li>Estagnação limitada.</li><li>Na maioria das versões K ou hora de t por trás. </li><li>Leia mais recente confirmada valor na região.</li></ul>
| LOCAL_ONE, ONE |Prefixo Consistente | Prefixo consistente de por região |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | Prefixo Consistente | Prefixo consistente de por região |


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
* [Funcionalidades do MongoDB suportadas pela API do Azure Cosmos DB para o MongoDB](mongodb-feature-support.md)
* [Recursos do Apache Cassandra suportados a API de Cassandra do Azure Cosmos DB](cassandra-support.md)