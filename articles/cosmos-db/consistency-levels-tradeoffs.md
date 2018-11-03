---
title: Desvantagens de desempenho e disponibilidade para vários níveis de consistência no Azure Cosmos DB | Documentos da Microsoft
description: Desvantagens de desempenho e disponibilidade para vários níveis de consistência no Azure Cosmos DB.
keywords: consistência, desempenho, do azure cosmos db, azure, do Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 8f36026c7e5802994b8cf22d60c6ecea052e6382
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963052"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Desvantagens de desempenho e disponibilidade para vários níveis de consistência no Azure Cosmos DB

Bases de dados distribuídas depender de replicação para elevada disponibilidade, latência baixa ou ambos, tornam o compromisso fundamental entre a consistência de leitura versus disponibilidade, latência e débito. O Azure Cosmos DB se aproxima de consistência de dados como uma gama de opções em vez dos dois extremos de consistência eventual e forte. O cosmos DB proporciona aos desenvolvedores escolher entre os cinco modelos de consistência bem definidos entre o espetro de consistência (mais forte para o mais fraco) – **forte**, **estagnação limitada**, **sessão** , **prefixo consistente**, e **eventual**. Cada um dos modelos de cinco consistência fornece as vantagens e desvantagens de desempenho e disponibilidade e são apoiados por SLAs abrangentes.

## <a name="consistency-levels-and-latency"></a>Níveis de consistência e a latência

- O **latência de leitura** para todos os níveis de consistência é sempre garantida ao ser inferior a 10 milissegundos em 99 por cento dos e é apoiado pelo SLA. A média (no percentil 50th) ler a latência normalmente é 2 milissegundos ou menos.

- Exceto para as contas do Cosmos que abrangem várias regiões e são configuradas com a consistência forte, o **latência de escrita** para a consistência restante níveis é sempre garantida para ser inferior a 10 milissegundos no 99th percentil. Esta latência de escrita conta com o SLA. A latência média (no percentil 50th) de escrita é normalmente 5 milissegundos ou menos.

- Para as contas do Cosmos com várias regiões configurados com consistência forte (atualmente em pré-visualização), o **latência de escrita** é garantido que estará menos de < (2 * ida e volta tempo/RTT) + 10 milissegundos no percentil 99. O RTT entre qualquer uma das duas regiões mais distantes à conta do Cosmos. A latência RTT exata é uma função de velocidade da luz distância e a topologia de rede do Azure exata. Redes do Azure não oferecem qualquer latência SLAs de RTT entre quaisquer duas regiões do Azure. Latências de replicação do cosmos DB são apresentadas no portal do Azure para a sua conta do Cosmos, permitindo-lhe monitorizar as latências de replicação entre várias regiões à sua conta do Cosmos.

## <a name="consistency-levels-and-throughput"></a>Níveis de consistência e o débito

- Para o mesmo número de unidades de pedido, a sessão, prefixo consistente e níveis de consistência eventual fornecem o débito em comparação com a forte e estagnação limitada de leitura de aproximadamente 2 X.

- Para um determinado tipo de operação de escrita como inserir, substituir, upsert, eliminar, etc., o débito de escrita para unidades de pedido é idêntico para todos os níveis de consistência.

## <a name="consistency-levels-and-durability"></a>Níveis de consistência e durabilidade

Antes de uma operação de escrita é reconhecida para o cliente, os dados de maneira duradoura consolidados por um quórum de réplicas na região que aceita as operações de escrita. Além disso, se o contentor estiver configurado com a política de indexação consistente, o índice é atualizado também de forma síncrona, replicado e maneira duradoura consolidado pelo quórum de réplicas, antes da confirmação de operação de escrita é enviada ao cliente.

A tabela seguinte resume a janela de perda de dados potenciais em caso de desastre regional para as contas do Cosmos que abrangem várias regiões.

| **Nível de consistência** | **Janela potencial de perda de dados em caso de desastre regional** |
| - | - |
| Forte | Zero |
| Estagnação Limitada | Limitado à "janela de envelhecimento" que configura na conta do Cosmos. |
| Sessão | Até 5 segundos |
| Prefixo Consistente | Até 5 segundos |
| Eventual | Até 5 segundos |

## <a name="next-steps"></a>Passos Seguintes

Em seguida pode saber mais sobre a distribuição global e os compromissos de consistência geral em sistemas distribuídos usando os seguintes artigos:

* [Vantagens e desvantagens de consistência no design de sistemas de banco de dados distribuída Moderno](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Elevada disponibilidade](high-availability.md)
* [SLA do cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
