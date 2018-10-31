---
title: Níveis de consistência no Azure Cosmos DB | Documentos da Microsoft
description: Azure Cosmos DB tem cinco níveis de consistência para o ajudar a equilibrar eventual consistência, disponibilidade e latência vantagens e desvantagens.
keywords: a consistência eventual, do azure cosmos db, do azure, do Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5cb439f7fe6461fcef0d010535179e16e28c294a
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239175"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Níveis de consistência no Azure Cosmos DB

Bases de dados distribuídas depender de replicação para elevada disponibilidade, latência baixa ou ambos, tornam o compromisso fundamental entre a consistência de leitura versus disponibilidade, latência e débito. A maioria dos comercialmente disponíveis bases de dados distribuídas perguntar aos desenvolvedores escolher entre os dois modelos de consistência extreme: consistência forte e consistência eventual. Embora o [transação atómica](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) ou o modelo de consistência forte é o padrão-ouro da programação de dados, ele tem um preço acentuado de latência superior (em estado de repouso) e disponibilidade reduzida (durante falhas). Por outro lado, a consistência eventual oferece maior disponibilidade e um melhor desempenho, mas é extremamente difícil de programar.

Consistência de dados como uma gama de opções em vez dos dois extremos se aproxima do cosmos DB. Consistência forte e consistência eventual são duas extremidades do espectro, existem muitas escolhas de consistência ao longo do espetro de consistência. Estas opções de consistência permitem aos programadores fazer escolhas precisas e os compromissos de granularidade mais fina em relação a elevada disponibilidade ou desempenho. O cosmos DB proporciona aos desenvolvedores escolher entre cinco modelos de consistência bem definidos no espectro de consistência (mais forte para o mais fraco) – **forte**, **estagnação limitada**, **sessão** , **prefixo consistente**, e **eventual**. Cada um desses modelos de consistência é bem definidos, intuitiva e pode ser utilizada para cenários específicos do mundo real. Cada um dos modelos de cinco consistência fornecem as compensações claras de disponibilidade e o desempenho e são apoiados por SLAs abrangentes.

![A consistência é um espectro](./media/consistency-levels/five-consistency-levels.png)
**consistência é um espectro**

Tenha em atenção que os níveis de consistência são região desconhecida. Nível de consistência (e as garantias de consistência correspondente) da sua conta do Cosmos DB, é garantido para todas as operações de leitura, independentemente do seguinte:

- A região a partir do qual as leituras e gravações são atendidas
- O número de regiões à sua conta do Cosmos
- Se a sua conta está configurada com uma única ou várias regiões de escrita

## <a name="scope-of-the-read-consistency"></a>Âmbito de consistência de leitura

A consistência de leitura aplica-se para uma única operação de âmbito dentro do intervalo da chave de partição (ou seja, uma partição lógica) de leitura. A operação de leitura pode ser emitida por um cliente remoto ou um procedimento armazenado.

## <a name="configuring-the-default-consistency-level"></a>Configurar o nível de consistência predefinido

Pode configurar o **predefinido de nível de consistência** na sua conta do Cosmos DB em qualquer altura. O nível de consistência predefinido configurado na sua conta se aplica a todas as bases de dados do Cosmos (e contentores) nessa conta. Todas as leituras e consultas emitidas relativamente de um contentor ou uma base de dados irão utilizar esse nível de consistência por predefinição. Veja aqui para saber como [configurar o nível de consistência predefinido](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantias de associadas com níveis de consistência

Os SLAs abrangentes fornecidos pelo Azure Cosmos DB garante que 100% de pedidos de leitura irão cumprir a garantia de consistência para qualquer nível de consistência que escolher. Uma solicitação de leitura é considerada cumpriu a consistência de SLA, se todas as garantias de consistência associadas com o nível de consistência são cumpridas. As definições de precisas dos níveis de cinco consistência no Cosmos DB ao utilizar o [TLA + linguagem de especificação (TLA +)](http://lamport.azurewebsites.net/tla/tla.html) são fornecidos na [do azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) repositório do GitHub. A semântica dos níveis de cinco consistência é descrita abaixo:

- **Nível de consistência "strong" =**: oferece consistência forte um [transação atómica](https://aphyr.com/posts/313-strong-consistency-models) garantir, com as leituras garantidas para devolver a versão mais recente de compromisso de um item. Um cliente nunca pode ver uma gravação não consolidada ou parcial e é sempre garantido ao ler a escrita mais recente confirmada.
- **Nível de consistência = "estagnação limitada"**: as leituras são garantidas para honrar a garantia de prefixo consistente. As poderão desfasamento de leituras escritas por no máximo K versões ou prefixos (ou seja, atualizações) de um item ou T' "intervalo de tempo. Por conseguinte, quando escolher estagnação limitada, o "envelhecimento" pode ser configurado de duas formas: o número de versões (K) do item ou o intervalo de tempo (t) através do qual as podem desfasamento de leituras de gravações. Estagnação limitada ofertas total global do pedido, exceto dentro da "janela limitada". As garantias de leitura monotónica existem dentro de uma região dentro e fora da "limitada janela." Consistência forte tem a mesma semântica que do estagnação limitada, mas com igual a "janela de envelhecimento" a zero. Estagnação limitada é também referida como "atraso de tempo de transação atómica". Quando um cliente executa operações de leitura numa região aceitar as gravações, as garantias fornecidas pelo consistência de estagnação limitada são idênticas com a consistência forte.
- **Nível de consistência = "session"**: as leituras são garantidas para honrar o prefixo consistente, leituras monotónicas, escritas monotónica, garantias de leitura-your-escritas, escrita de acordo com leituras. Consistência da sessão é confinada a uma sessão de cliente.
- **Nível de consistência = "prefixo consistente"**: as atualizações devolvidas são alguns prefixos de todas as atualizações, sem intervalos. Prefixo consistente garante que leituras nunca veem escritas fora de ordem.
- **Nível de consistência = "eventual"**: não é garantido ordenação para leituras. Na ausência de qualquer escrita adicional, as réplicas no final de convergência.

## <a name="consistency-levels-explained-through-baseball"></a>Níveis de consistência explicadas através do basebol

Como o [replicados dados consistência através de Beisebol](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) paper ilustra, imagine uma sequência de escritas que representa a classificação de um jogo de beisebol com a classificação de linha de inning por inning. Este jogo de beisebol hipotético está atualmente no meio o sétimo inning (a síndrome sétima – inning stretch), e a equipe principal está ganhando 2 a 5.

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Execuções** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Visitantes** | 0 | 0 | 1 | 0 | 5 | 0 | 0 |  |  | 2 |
| **página inicial** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Um contentor do Cosmos DB mantém os visitantes e totais de execução da Equipe principal. Enquanto o jogo estiver em curso, o diferentes ler de garantias podem resultar em leitura pontuações diferentes de clientes. A tabela seguinte lista o conjunto completo de pontuações que podem ser devolvidos ao ler os visitantes e pontuações principais com cada um das garantias de consistência de cinco. Tenha em atenção que a pontuação dos visitantes é listada em primeiro lugar, e diferentes possíveis valores de retorno são separados por vírgulas.

| **Nível de consistência** | **Pontuações** |
| - | - |
| **Forte** | 2 a 5 |
| **Estagnação limitada** | as classificações que estão na maioria dos um inning desatualizados"2 e 3, 2 a 4, 2 a 5 |
| **Sessão** | <ul><li>para o gravador de"2 a 5</li><li> para qualquer pessoa que não seja o escritor: 0 de 0, 0-1, 0-2, 0 a 3, 4 de 0, 0-5, 1-0, 1-1, 1 a 2, 1 a 3, 1 a 4, 1 a 5, 2-0, 2-1, 2 de 2, 2 e 3, 2 a 4, 2 a 5</li><li>Depois de ler 1 a 3: 1 a 3, 1 a 4, 1 a 5, 2 e 3, 2 a 4, 2 a 5</li> |
| **Prefixo consistente** | 0 de 0, 0-1, 1-1, 1 a 2, 1 a 3, 2 e 3, 2 a 4, 2 a 5 |
| **Eventual** | 0 de 0, 0-1, 0-2, 0 a 3, 4 de 0, 0-5, 1-0, 1-1, 1 a 2, 1 a 3, 1 a 4, 1 a 5, 2-0, 2-1, 2 de 2, 2 e 3, 2 a 4, 2 a 5 |

## <a name="additional-reading"></a>Leitura adicional

Para saber mais sobre os conceitos de consistência, leia os artigos seguintes:

- [Alto nível TLA + especificações para os níveis de cinco consistência oferecidos pelo Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla)
- [Replicados explicada de consistência de dados através do basebol (vídeo) por Doug Tiago](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Replicados a consistência de dados explicada através do basebol (documento técnico), por Doug Tiago](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Garantias de sessão para os dados replicados consistentes tem rigidez](https://dl.acm.org/citation.cfm?id=383631)
- [Vantagens e desvantagens de consistência no moderno Design de sistemas de banco de dados distribuído: limite é apenas uma parte da história](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Probabilístico estagnação limitada (PBS) para práticos Quorums parciais](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Eventualmente consistente - Revisitada](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os níveis de consistência no Cosmos DB, leia os artigos seguintes:

* [Escolher o nível de consistência certo para a sua aplicação](consistency-levels-choosing.md)
* [Níveis de consistência entre as APIs do Cosmos DB](consistency-levels-across-apis.md)
* [Desvantagens de desempenho e disponibilidade para vários níveis de consistência](consistency-levels-tradeoffs.md)
* [Como configurar o nível de consistência predefinido](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Como substituir o nível de consistência predefinido](how-to-manage-consistency.md#override-the-default-consistency-level)

