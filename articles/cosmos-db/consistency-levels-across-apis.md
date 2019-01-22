---
title: Níveis de consistência e APIs do Azure Cosmos DB
description: Compreender os níveis de consistência através de APIs no Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: a506c696cdb9ca6c6221b54c63d2446b7cb86a69
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430571"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Níveis de consistência e APIs do Azure Cosmos DB

Cinco modelos de consistência oferecidos pelo Azure Cosmos DB são suportados nativamente pelo Azure Cosmos DB SQL API. Quando utiliza o Azure Cosmos DB, a API de SQL é a predefinição. 

O Azure Cosmos DB também fornece suporte nativo para conexão APIs compatíveis com o protocolo para bases de dados populares. Bases de dados incluem o armazenamento de MongoDB, Apache Cassandra, Gremlin e tabelas do Azure. Esses bancos de dados não oferecem precisamente os modelos de consistência definidos ou garantias apoiado pelo SLA para níveis de consistência. Normalmente, eles fornecem apenas um subconjunto dos modelos de cinco consistência oferecidos pelo Azure Cosmos DB. Para a API de SQL, o API do Gremlin e a API de tabela, é utilizado o nível de consistência predefinido configurado na conta do Azure Cosmos DB. 

As secções seguintes mostram o mapeamento entre a consistência de dados solicitada por um controlador de cliente de sistemas operacionais para o Apache Cassandra 4.x e de invocação MongoDB 3.4. Este documento também mostra os níveis de consistência correspondentes do Azure Cosmos DB para o Apache Cassandra e MongoDB.

## <a id="cassandra-mapping"></a>Mapeamento entre os níveis de consistência do Apache Cassandra e o Azure Cosmos DB

Esta tabela mostra o mapeamento de consistência entre o Apache Cassandra e níveis de consistência no Azure Cosmos DB. Para cada uma leitura de Cassandra e níveis de consistência de escrita, a consistência nível correspondente do Cosmos DB fornece mais forte, ou seja, mais estritas garantias.


<table>
<tr> 
  <th rowspan="2">Nível de consistência de Cassandra</th> 
  <th rowspan="2">Nível de consistência do cosmos DB</th> 
  <th colspan="3">Mapeamento de consistência de escrita</th> 
  <th colspan="3">Mapeamento de consistência de leitura</th> 
</tr> 


 
 <tr> 
  <th>Cassandra</th> 
  <th>BD do Cosmos</th> 
  <th>Garantia</th> 
  <th>Do Cassandra</th> 
  <th>Para o Cosmos DB</th> 
  <th>Garantia</th> 
 </tr> 
 
  <tr> 
  <td rowspan="6">TODOS</td> 
  <td rowspan="6">Forte</td> 
  <td>TODOS</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
  <td>ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO, ONE, LOCAL_ONE</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
 </tr> 
 
 <tr> 
  <td rowspan="2">EACH_QUORUM</td> 
  <td rowspan="2">Forte</td> 
  <td rowspan="2">Transação Atómica</td> 
  <td>ALL, QUORUM, SERIAL,  LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO</td> 
  <td>Forte</td> 
  <td >Transação Atómica</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>
 

 <tr> 
  <td rowspan="2">QUORUM, SERIAL</td> 
  <td rowspan="2">Forte</td> 
  <td rowspan="2">Transação Atómica</td> 
  <td>ALL, QUORUM, SERIAL</td> 
  <td>Forte</td> 
  <td >Transação Atómica</td> 
 </tr> 

 <tr>
   <td>LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td>
   <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>
 
 
 <tr> 
 <td>LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, <b>ANY</b></td> 
  <td>Prefixo Consistente</td> 
  <td>Prefixo consistente global</td> 
  <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td> 
  <td>Prefixo Consistente</td> 
  <td>Prefixo consistente global</td>
 </tr> 
 
 
  <tr> 
  <td rowspan="6">EACH_QUORUM</td> 
  <td rowspan="6">Forte</td> 
  <td rowspan="2">EACH_QUORUM</td> 
  <td rowspan="2">Forte</td> 
  <td rowspan="2">Transação Atómica</td> 
  <td>ALL, QUORUM, SERIAL,  LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>
 
 
 
 <tr> 
  <td rowspan="2">QUORUM, SERIAL</td> 
  <td rowspan="2">Forte</td> 
  <td rowspan="2">Transação Atómica</td> 
  <td>ALL, QUORUM, SERIAL</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>
 
 
  <tr> 
  <td rowspan="2">LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY</td> 
  <td rowspan="2">Prefixo Consistente</td> 
  <td rowspan="2">Prefixo consistente global</td> 
  <td>TODOS</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>


  <tr> 
  <td rowspan="4">QUÓRUM</td> 
  <td rowspan="4">Forte</td> 
  <td rowspan="2">QUORUM, SERIAL</td> 
  <td rowspan="2">Forte</td> 
  <td rowspan="2">Transação Atómica</td> 
  <td>ALL, QUORUM, SERIAL</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>
 
 
 <tr> 
  <td rowspan="2">LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY</td> 
  <td rowspan="2">Prefixo Consistente </td> 
  <td rowspan="2">Prefixo consistente global </td> 
  <td>TODOS</td> 
  <td>Forte</td> 
  <td>Transação Atómica</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente global</td>
 </tr>
 
 <tr> 
  <td rowspan="4">LOCAL_QUORUM, THREE, TWO</td> 
  <td rowspan="4">Estagnação Limitada</td> 
  <td rowspan="2">LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td> 
  <td rowspan="2">Estagnação Limitada</td> 
  <td rowspan="2">Estagnação limitada.<br/>
Na maioria das versões K ou hora de t por trás.<br/>
Leia mais recente confirmada valor na região. 
</td> 
  
  <td>QUORUM, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td> 
  <td>Estagnação Limitada</td> 
  <td>Estagnação limitada.<br/>
Na maioria das versões K ou hora de t por trás. <br/>
Leia mais recente confirmada valor na região. </td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE</td>
  <td>Prefixo Consistente</td>
   <td>Prefixo consistente de por região</td>
 </tr>
 
 
 <tr> 
  <td>ONE, LOCAL_ONE, ANY</td> 
  <td>Prefixo Consistente </td> 
  <td >Prefixo consistente de por região </td> 
  <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td> 
  <td>Prefixo Consistente</td> 
  <td>Prefixo consistente de por região</td> 
 </tr> 
</table>

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