---
title: Desvantagens de desempenho e disponibilidade para vários níveis de consistência | Documentos da Microsoft
description: Desvantagens de desempenho e disponibilidade para vários níveis de consistência no Azure Cosmos DB.
keywords: consistência, desempenho, do azure cosmos db, azure, do Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 061a7c223d0e1c6fa7384d7defe9f84f470236ce
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244074"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels"></a>Desvantagens de desempenho e disponibilidade para vários níveis de consistência

Bases de dados distribuídas depender de replicação para elevada disponibilidade, latência baixa ou ambos, tornam o compromisso fundamental entre a consistência de leitura versus availability1, latency2 e débito. O Azure Cosmos DB se aproxima de consistência de dados como uma gama de opções em vez dos dois extremos de consistência eventual e forte. O cosmos DB proporciona aos desenvolvedores escolher entre cinco modelos de consistência bem definidos no espectro de consistência (mais forte para o mais fraco) – **forte**, **estagnação limitada**, **sessão** , **prefixo consistente**, e **eventual**. Cada um dos modelos de cinco consistência fornece claras as compensações de disponibilidade e o desempenho e são apoiados por SLAs abrangentes.

## <a name="consistency-levels-and-latency"></a>Níveis de consistência e a latência

- O **latência de leitura** para todos os níveis de consistência é sempre garantida ao ser inferior a 10 milissegundos em 99 por cento dos e é apoiado pelo SLA. A média (no percentil 50th) ler a latência normalmente é 2 milissegundos ou menos.
- Exceto as contas do Cosmos em várias regiões e configurado com a consistência forte, o **latência de escrita** para todos os níveis de consistência é sempre garantida ao ser inferior a 10 milissegundos em 99 por cento dos e conta com o SLA. A latência média (no percentil 50th) de escrita é normalmente 5 milissegundos ou menos.
- Para as contas do Cosmos com várias regiões configurados com consistência forte (atualmente em pré-visualização), o **latência de escrita** é garantido que estará menos de < (2 * RTT) + 10 milissegundos no percentil 99. O RTT entre o mais distante de qualquer uma das duas regiões à conta do Cosmos. A latência RTT exata é uma função de velocidade da luz distância e a topologia de rede do Azure exata. Redes do Azure não oferecem qualquer latência SLAs de RTT entre quaisquer duas regiões do Azure. Latências de replicação do cosmos DB são apresentadas no portal do Azure para a sua conta do Cosmos, permitindo-lhe monitorizar as latências de replicação entre várias regiões à sua conta do Cosmos.

## <a name="consistency-levels-and-throughput"></a>Níveis de consistência e o débito

- Para a mesma quantidade de RU, sessão, prefixo consistente e a consistência eventual fornecem o débito em comparação comparado a forte e estagnação limitada de leitura de aproximadamente 2 X.
- Para um determinado tipo de operação de escrita (por exemplo, inserir, substituir, Upsert, eliminar, etc.), o débito de escrita para RUs é idêntico para todos os níveis de consistência.

## <a name="consistency-levels-and-durability"></a>Níveis de consistência e durabilidade

Antes de uma operação de escrita é reconhecida para o cliente, os dados de maneira duradoura consolidados por um quórum de réplicas numa região aceitar as gravações. Além disso, se o contentor estiver configurado com a política de indexação consistente, o índice é atualizado também de forma síncrona, replicado e consolidado pelo quórum de réplicas de maneira duradoura antes da operação de escrita é reconhecida para o cliente. 

A tabela seguinte resume a janela de potencial de perda de dados em caso de desastre regional para as contas do Cosmos em várias regiões.

| **Nível de consistência** | **Janela de potencial perda de dados em caso de desastre regional** |
| - | - |
| Forte | Zero |
| Estagnação Limitada | Limitado à "janela de envelhecimento" configurada pelo desenvolvedor na conta do Cosmos |
| Sessão | Até 5 segundos |
| Prefixo Consistente | Até 5 segundos |
| Eventual | Até 5 segundos |

## <a name="next-steps"></a>Passos Seguintes

Em seguida pode saber mais sobre a distribuição global e os compromissos de consistência geral em sistemas distribuídos usando os seguintes artigos:

* [Vantagens e desvantagens de consistência no design de sistemas de banco de dados distribuída Moderno](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Elevada disponibilidade](high-availability.md)
* [SLA do cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
