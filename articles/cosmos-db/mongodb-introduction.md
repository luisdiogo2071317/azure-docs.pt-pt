---
title: Introdução ao Azure Cosmos DB API para MongoDB
description: Saiba como pode utilizar o Azure Cosmos DB para armazenar e consultar grandes volumes de documentos JSON com baixa latência através da API de MongoDB OSS populares.
keywords: API do Azure Cosmos DB para MongoDB
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: overview
ms.date: 12/19/2018
ms.author: sclyon
ms.openlocfilehash: d8b423bcbfc8f999ce04c7713a57cfa096fa2c55
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654041"
---
# <a name="azure-cosmos-db-for-mongodb-api-clients"></a>O Azure Cosmos DB para clientes de API do MongoDB

O [Azure Cosmos DB](introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. O Azure Cosmos DB proporciona [distribuição global chave na mão](distribute-data-globally.md), [dimensionamento elástico de débito e de armazenamento](partition-data.md) em todo o mundo, latências de milissegundos de um só dígito no percentil 99 e elevada disponibilidade garantida, tudo com o suporte de [SLA líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/). O Azure Cosmos DB [indexa automaticamente os dados](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem que tenha de lidar com a gestão de esquemas e índices. É multimodal e suporte modelos de dados em documentos, chaves-valores, gráficos e em colunas. Por predefinição, pode interagir com o Cosmos DB com a API de SQL. Além disso, o serviço Cosmos DB implementa os protocolos de transmissão para APIs de NoSQL comuns, incluindo, Cassandra, MongoDB, Gremlin e armazenamento de tabelas do Azure. Isto permite-lhe utilizar o seu controlador de cliente familiar do NoSQL e as ferramentas para interagir com a base de dados do Cosmos.

## <a name="wire-protocol-compatibility"></a>Compatibilidade de protocolo de transmissão

O cosmos DB implementa os protocolos de transmissão de bases de dados do NoSQL comuns incluindo, Cassandra, MongoDB, Gremlin e armazenamento de tabelas do Azure. Ao fornecer uma implementação nativa os protocolos de transmissão diretamente e eficiente dentro do Cosmos DB, ele permite que existente SDKs do cliente, drivers e ferramentas das bases de dados NoSQL interagir com o Cosmos DB de forma transparente. O cosmos DB não utiliza nenhum código-fonte das bases de dados para fornecer APIs compatíveis com o durante a transmissão para qualquer um dos bancos de dados NoSQL.

Por predefinição, o Azure Cosmos DB para a MongoDB API é compatível com a versão 3.2 do protocolo de transmissão. Recursos ou adicionados na versão 3.4 do protocolo de transmissão de operadores de consulta estão atualmente disponíveis como uma funcionalidade de pré-visualização. Qualquer controlador de cliente que suporte estas versões de protocolo para a API do MongoDB deve ser capaz de forma nativa ligar ao Azure Cosmos DB.

![API do Azure Cosmos DB para MongoDB](./media/mongodb-introduction/cosmosdb-mongodb.png) 

## <a name="key-benefits"></a>Principais vantagens

Os principais benefícios do Cosmos DB como uma base de dados totalmente gerido, globalmente distribuído, como um serviço são descritos [aqui](introduction.md). Além disso, ao implementar nativamente protocolos de transmissão de APIs populares de NoSQL, o Cosmos DB fornece as seguintes vantagens:

* Migre facilmente a sua aplicação para o Cosmos DB, preservando a partes significativas da lógica do aplicativo.
* Mantenha seu aplicativo portátil e continuará desconhecidas do fornecedor na cloud.
* Obtenha setor SLAs líderes do setor, com apoio financeiro para as APIs do NoSQL comuns com tecnologia do Cosmos DB.
* Dimensionar de forma elástica o débito aprovisionado e armazenamento para as bases de dados do Cosmos com base nas suas necessidades e pague apenas o débito e armazenamento de que precisa. Isso leva à redução significativa de custos.
* Distribuição de chave na mão, global com a replicação de vários mestre.

## <a name="cosmos-db-for-mongodb-api-clients"></a>Cosmos DB para clientes de API do MongoDB

Siga os inícios rápidos do MongoDB para criar uma conta do Azure Cosmos DB e para migrar a sua aplicação MongoDB existente para utilizar o Azure Cosmos DB ou criar uma nova:

* [Migrar uma aplicação Web Node.js do MongoDB](create-mongodb-nodejs.md).
* [Criar uma aplicação web com .NET e o portal do Azure com o Azure Cosmos DB para a MongoDB API](create-mongodb-dotnet.md)
* [Criar uma aplicação de consola com Java e o portal do Azure com o Azure Cosmos DB para a MongoDB API](create-mongodb-java.md)

## <a name="next-steps"></a>Passos Seguintes

Eis alguns sítios por onde começar:

* Siga o tutorial [Ligar-se a uma conta do MongoDB](connect-mongodb-account.md) para saber como obter as informações da cadeia de ligação da sua conta.
* Siga o tutorial [Utilizar Studio 3T (MongoChef) com o Azure Cosmos DB](mongodb-mongochef.md) para saber como criar uma ligação entre a sua base de dados Azure Cosmos DB e a aplicação MongoDB no Studio 3T.
* Siga os [migrar dados para o Azure Cosmos DB com o protocolo de suporte para a MongoDB API](mongodb-migrate.md) tutorial para importar os dados para uma base de dados do Cosmos.
* Ligar a uma conta do Cosmos com [Robomongo](mongodb-robomongo.md).
* Saiba como [configurar as preferências de leitura para as aplicações distribuídas globalmente](../cosmos-db/tutorial-global-distribution-mongodb.md).
