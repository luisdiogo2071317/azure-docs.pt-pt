---
title: Escolher o nível de consistência certo para a sua aplicação que utiliza o Azure Cosmos DB
description: Escolher o nível de consistência certo para a sua aplicação no Azure Cosmos DB.
keywords: consistência, desempenho, do azure cosmos db, azure, do Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/24/2018
ms.author: mjbrown
ms.openlocfilehash: a1c7d750bcd0c3f37d2269aee299e0ccd8c4ef4a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849297"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Escolha o nível de consistência certo para a sua aplicação

Bases de dados distribuídas depender de replicação para elevada disponibilidade, latência baixa ou ambos, tornam o compromisso fundamental entre a consistência de leitura versus disponibilidade, latência e débito. A maioria dos comercialmente disponíveis bases de dados distribuídas perguntar aos desenvolvedores escolher entre os dois modelos de consistência extreme: consistência forte e consistência eventual. O Azure Cosmos DB permite aos desenvolvedores escolher entre os cinco modelos de consistência bem definidos: forte, estagnação limitada, sessão, prefixo consistente e eventual. Cada um desses modelos de consistência é bem definidos, intuitiva e pode ser utilizada para cenários específicos do mundo real. Cada um dos modelos de cinco consistência fornecer [vantagens e desvantagens de desempenho e disponibilidade](consistency-levels-tradeoffs.md) e são apoiados por SLAs abrangentes. As seguintes considerações simples lhe ajudarão a tornar a opção certa em muitos cenários comuns.

## <a name="sql-api-and-table-api"></a>SQL API e a API de tabela

Considere os seguintes pontos se seu aplicativo for criado com o Cosmos DB SQL API ou a API de tabela

- Para muitos cenários do mundo real, consistência da sessão é o ideal e é a opção recomendada. Para obter mais informações, consulte, [procedimentos gerir o token de sessão para a sua aplicação](how-to-manage-consistency.md#utilize-session-tokens).

- Se seu aplicativo exigir consistência forte, recomenda-se que utilize o nível de consistência de estagnação limitada.

- Se precisar de mais estritas garante a consistência do que aqueles fornecidos com consistência de sessão e a latência de milissegundos de dígito único para escritas, recomenda-se que utilize limitado a nível de consistência vinculada.  

- Se seu aplicativo exigir consistência eventual, recomenda-se que utilize o nível de consistência de prefixo consistente.

- Se precisar de menos garantias de consistência rigorosa dos que foram fornecidas pelo consistência da sessão, é recomendado que utilize o nível de consistência de prefixo consistente.

- Se precisar da maior disponibilidade e a latência mais baixa, em seguida, utilize o nível de consistência eventual.

## <a name="cassandra-mongodb-and-gremlin-api"></a>API de Cassandra, MongoDB e Gremlin

- Para obter detalhes sobre o mapeamento entre "o nível de consistência de leitura" oferecidos em níveis de consistência do Apache Cassandra e o Cosmos DB, consulte [níveis de consistência e APIs do Cosmos DB](consistency-levels-across-apis.md#cassandra-mapping).

- Para obter detalhes sobre o mapeamento entre a "Preocupação de leitura" dos níveis de consistência do MongoDB e o Azure Cosmos DB, consulte [níveis de consistência e APIs do Cosmos DB](consistency-levels-across-apis.md#mongo-mapping).

## <a name="consistency-guarantees-in-practice"></a>Garantias de consistência na prática

Poderá receber mais garantias de consistência na prática. Garantias de consistência para uma operação de leitura correspondem para a atualização e ordenação do Estado da base de dados que pedir. Consistência de leitura é associada às encomendas e a propagação das operações de escrita/atualização.  

* Quando o nível de consistência é definido como **estagnação limitada**, Cosmos DB garante que os clientes sempre ler o valor de uma gravação anterior, com um atraso vinculado de acordo com a janela de limitada.

* Quando o nível de consistência é definido como **forte**, a janela de envelhecimento é equivalente a zero e os clientes são garantidos para ler o valor mais recente consolidado da operação de escrita.

* Para obter os níveis de três consistência restantes, a janela de envelhecimento é amplamente dependente de sua carga de trabalho. Por exemplo, se existirem não existem operações de escrita na base de dados, uma operação de leitura com **eventual**, **sessão**, ou **prefixo consistente** níveis de consistência é provável que produzem os mesmos resultados como uma operação de leitura com o nível de consistência forte.

Se a sua conta do Cosmos DB está configurada com um nível de consistência que não seja a consistência forte, pode descobrir a probabilidade de que os clientes podem obter fortes e leituras consistentes das cargas de trabalho ao procurar no probabilístico estagnação limitada (PBS) métrica. Esta métrica está exposta no portal do Azure, para obter mais informações, consulte [métrica de Monitor Roleta estagnação limitada (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

Probabilístico estagnação limitada mostra como eventual é a consistência eventual. Esta métrica fornece uma visão geral de quantas vezes pode obter uma consistência mais forte que o nível de consistência que tenha configurado atualmente na sua conta do Cosmos DB. Em outras palavras, pode ver a probabilidade (medida em milissegundos) de leituras fortemente consistentes para uma combinação de escrita de introdução e regiões de leitura.

## <a name="next-steps"></a>Passos Seguintes

Leia mais sobre os níveis de consistência nos seguintes artigos:

* [Mapeamento de nível de consistência entre as APIs do Cosmos DB](consistency-levels-across-apis.md)
* [Desvantagens de desempenho e disponibilidade para vários níveis de consistência](consistency-levels-tradeoffs.md)
* [Como gerir o token de sessão para a sua aplicação](how-to-manage-consistency.md#utilize-session-tokens)
* [Monitorizar a métrica de Roleta estagnação limitada (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
