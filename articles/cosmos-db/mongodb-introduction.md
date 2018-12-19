---
title: Introdução ao Azure Cosmos DB API para MongoDB
description: Saiba como pode utilizar o Azure Cosmos DB para armazenar e consultar grandes volumes de documentos JSON com baixa latência, utilizando o MongoDB de OSS populares.
keywords: o que é o MongoDB
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: overview
ms.date: 02/12/2018
ms.author: sclyon
ms.openlocfilehash: c4c33094f070e603057042eaea8187e29270fb77
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53606848"
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Introdução ao Azure Cosmos DB API para MongoDB

O [Azure Cosmos DB](../cosmos-db/introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. O Azure Cosmos DB proporciona [distribuição global chave na mão](distribute-data-globally.md), [dimensionamento elástico de débito e de armazenamento](partition-data.md) em todo o mundo, latências de milissegundos de um só dígito no percentil 99 e elevada disponibilidade garantida, tudo com o suporte de [SLA líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/). O Azure Cosmos DB [indexa automaticamente os dados](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem que tenha de lidar com a gestão de esquemas e índices. É multimodal e suporte modelos de dados em documentos, chaves-valores, gráficos e em colunas. 

![API do Azure Cosmos DB para MongoDB](./media/mongodb-introduction/cosmosdb-mongodb.png) 

As bases de dados do Azure Cosmos DB podem ser utilizadas como arquivo de dados para aplicações escritas para [MongoDB](https://docs.mongodb.com/manual/introduction/). Esta funcionalidade significa que ao utilizar os [controladores](https://docs.mongodb.org/ecosystem/drivers/) existentes, a aplicação escrita para MongoDB pode agora comunicar com o Azure Cosmos DB e utilizar as bases de dados do Azure Cosmos DB, em vez das bases de dados de MongoDB. Em muitos casos, pode mudar de MongoDB para Azure Cosmos DB, alterando simplesmente uma cadeia de ligação. Através desta funcionalidade, pode criar e executar facilmente aplicações de base de dados de MongoDB distribuídas globalmente no Azure Cloud com Azure Cosmos DB e respetivos [SLA abrangentes líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db), enquanto continua a utilizar competências e ferramentas familiares para MongoDB.

**Compatibilidade do MongoDB**: Pode utilizar o existente MongoDB conhecimentos, código do aplicativo e as ferramentas como o Azure Cosmos DB implementa o protocolo de transmissão do MongoDB. Pode desenvolver aplicações com o MongoDB e implementá-las na produção através do serviço totalmente gerido e distribuído a nível global do Azure Cosmos DB. Para obter mais informações sobre as versões suportadas, veja [Suporte do Protocolo do MongoDB](mongodb-feature-support.md#mongodb-protocol-support).

A API do Azure Cosmos DB para o MongoDB não é possível utilizar como um ponto de final direto para serviços como o Azure Stream Analytics, uma vez que a TI utiliza o mesmo [controladores cliente](https://docs.mongodb.org/ecosystem/drivers/) como o MongoDB nativo. Para integrar com o Azure Stream Analytics, considere usar [App Service do Azure](../app-service/overview.md) ou [serviço de funções do Azure](../azure-functions/functions-overview.md) como um serviço de middleware que possa escrever dados para a API do Azure Cosmos DB para o MongoDB.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Qual é a vantagem de utilizar o Azure Cosmos DB para aplicações MongoDB?

**Débito dimensionável de forma elástica e armazenamento:** Atender às suas necessidades de aplicativos ao facilmente aumentar ou reduzir verticalmente a sua base de dados do MongoDB. Os seus dados são armazenados em unidades de estado sólido (SSD) para baixas latências previsíveis. O Azure Cosmos DB suporta coleções de MongoDB que podem ser dimensionadas para tamanhos de armazenamento virtualmente ilimitados e débito aprovisionado. Pode dimensionar de forma totalmente integrada o Azure Cosmos DB, com um desempenho previsível, à medida que a sua aplicação aumenta. 

**Replicação de multirregião:** O Azure Cosmos DB replica de forma transparente os dados para todas as regiões que associou à conta do MongoDB, permitindo-lhe desenvolver aplicações que necessitam acesso global aos dados ao apresentar as responsabilidades entre consistência, disponibilidade e desempenho , tudo com garantias correspondentes. O Azure Cosmos DB oferece ativação pós-falha regional transparente com APIs multi-homing e a capacidade e dimensionar de forma elástica o débito e o armazenamento a nível global. Saiba mais em [Distribuir dados globalmente](distribute-data-globally.md).

**Sem gestão de servidor**: Não precisa de gerir e dimensionar seus bancos de dados do MongoDB. O Azure Cosmos DB é um serviço totalmente gerido, o que significa que não tem de gerir qualquer infraestrutura ou Máquinas Virtuais por si. O Azure Cosmos DB está disponível em mais de 30 [Regiões do Azure](https://azure.microsoft.com/regions/services/).

**Níveis de consistência sincronizáveis:** Como o Azure Cosmos DB suporta APIs com vários modelos as definições de consistência se aplicam ao nível da conta e impor a consistência é controlada por cada API. Até 3.6 do MongoDB, não havia nenhum conceito de uma consistência de sessão, portanto, se definir uma conta de API do Azure Cosmos DB para o MongoDB para utilizar a consistência da sessão, a consistência é desatualizada para o eventual, ao utilizar a API do Azure Cosmos DB para o MongoDB. Se precisar de uma garantia de leitura-your-own-escrita para uma API do Azure Cosmos DB para o MongoDB, o nível de consistência predefinido para a conta deve ser definido como forte ou estagnação limitada. Saiba mais em [Using consistency levels to maximize availability and performance](consistency-levels.md) (Utilizar níveis de consistência para maximizar a disponibilidade e desempenho).

| Nível de Consistência Predefinido do Azure Cosmos DB |   API do Mongo (3.4) |
|---|---|
|Eventual| Eventual |
|Prefixo Consistente| Eventual com uma ordem consistente |
|Sessão| Eventual com uma ordem consistente |
|Estagnação Limitada| Forte |
| Forte | Forte |

**Indexação automática**: Por predefinição, do Azure Cosmos DB automaticamente indexa todas as propriedades em documentos na base de dados MongoDB e não espera nem exige nenhum esquema ou criação de índices secundários. Além disso, a capacidade de índice exclusivo permite uma restrição de exclusividade em quaisquer campos de documentos que já estejam indexados automaticamente no Azure Cosmos DB.

**Nível empresarial**: Azure Cosmos DB suporta várias réplicas locais para fornecer proteção de dados e disponibilidade de 99,99% de falhas local e regional. O Azure Cosmos DB possui [certificações de conformidade](https://www.microsoft.com/trustcenter) de nível empresarial, bem como funcionalidades de segurança. 

## <a name="how-to-get-started"></a>Como começar

Siga os inícios rápidos do MongoDB para criar uma conta do Azure Cosmos DB e para migrar a sua aplicação MongoDB existente para utilizar o Azure Cosmos DB ou criar uma nova:

* [Migrar uma aplicação Web Node.js do MongoDB](create-mongodb-nodejs.md).
* [Criar uma aplicação web com .NET e o portal do Azure com a API do Azure Cosmos DB do MongoDB](create-mongodb-dotnet.md)
* [Criar uma aplicação de consola com Java e o portal do Azure com a API do Azure Cosmos DB do MongoDB](create-mongodb-java.md)

## <a name="next-steps"></a>Passos Seguintes

Informações sobre a API do Azure Cosmos DB para o MongoDB são integradas a documentação geral do Azure Cosmos DB, mas aqui estão alguns pontos para começar:

* Siga o tutorial [Ligar-se a uma conta do MongoDB](connect-mongodb-account.md) para saber como obter as informações da cadeia de ligação da sua conta.
* Siga o tutorial [Utilizar Studio 3T (MongoChef) com o Azure Cosmos DB](mongodb-mongochef.md) para saber como criar uma ligação entre a sua base de dados Azure Cosmos DB e a aplicação MongoDB no Studio 3T.
* Siga o tutorial [Migrar dados para o Azure Cosmos DB com o apoio do protocolo para o tutorial do MongoDB](mongodb-migrate.md) para importar os seus dados para uma API para a base de dados de MongoDB.
* Ligue-se a uma API para a conta do MongoDB com [Robomongo](mongodb-robomongo.md).
* Saiba como [configurar as preferências de leitura para as aplicações distribuídas globalmente](../cosmos-db/tutorial-global-distribution-mongodb.md).
