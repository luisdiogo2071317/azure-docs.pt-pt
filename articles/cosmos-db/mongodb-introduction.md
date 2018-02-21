---
title: "Introdução ao Azure Cosmos DB: API para o MongoDB | Microsoft Docs"
description: "Saiba como pode utilizar a base de dados do Azure Cosmos para armazenar e grandes volumes de consulta de documentos JSON com baixa latência, utilizando as APIs do MongoDB OSS populares."
keywords: "o que é o MongoDB"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: anhoh
ms.openlocfilehash: ffca8f4518361e8c5447d7bb7ed6022eb0e96a4a
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Introdução ao Azure Cosmos DB: API para MongoDB

O [Azure Cosmos DB](../cosmos-db/introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. BD do Azure do Cosmos fornece [distribuição global ative chave](distribute-data-globally.md), [dimensionamento elástico de débito e armazenamento](partition-data.md) latências milissegundo em todo o mundo, dígito no percentil 99th e garantida alta disponibilidade, todas as cópia por [líder da indústria SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/). O Azure Cosmos DB [indexa automaticamente os dados](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem que tenha de lidar com a gestão de esquemas e índices. É multimodal e suporte modelos de dados em documentos, chaves-valores, gráficos e em colunas. 

![Azure Cosmos DB: MongoDB API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Bases de dados de base de dados do Cosmos do Azure podem ser utilizados como o arquivo de dados para aplicações escritas para [MongoDB](https://docs.mongodb.com/manual/introduction/). Esta funcionalidade significa que, ao utilizar existente [controladores](https://docs.mongodb.org/ecosystem/drivers/), a aplicação escritos para o MongoDB pode comunicar com a base de dados do Azure Cosmos e utilizar bases de dados de base de dados do Azure Cosmos em vez de bases de dados de MongoDB. Em muitos casos, pode mudar do MongoDB à base de dados do Azure Cosmos alterando simplesmente uma cadeia de ligação. Utilizar esta funcionalidade, pode facilmente criar e executar aplicações de base de dados de MongoDB do Azure na nuvem com distribuição global da BD do Azure Cosmos e [abrangentes líder da indústria SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db), enquanto continua a utilizar as competências familiares e as ferramentas para MongoDB.

**Compatibilidade do MongoDB**: pode utilizar o existente MongoDB conhecimentos, código de aplicação e ferramentas como base de dados do Azure Cosmos implementa o protocolo de transmissão do MongoDB 3.4 (versão 5) e suporta o [pipeline de agregação do MongoDB](mongodb-feature-support.md#aggregation-pipeline). Pode desenvolver aplicações com o MongoDB e implementá-las em produção completamente gerido a utilizar e o serviço de base de dados do Azure Cosmos globalmente distribuído.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>O que é a vantagem de utilizar a base de dados do Azure Cosmos para MongoDB aplicações?

**Débito e armazenamento dimensionável aprovisionadas:** cumprem as suas aplicações necessitam ao facilmente aumentar ou reduzir vertical a base de dados de MongoDB. Os seus dados são armazenados em unidades de estado sólido (SSD) para baixas latências previsíveis. BD do Azure do Cosmos suporta MongoDB coleções que podem ser dimensionados para tamanhos de armazenamento virtualmente ilimitados e débito aprovisionado. Pode dimensionar de forma totalmente integrada o Azure Cosmos DB, com um desempenho previsível, à medida que a sua aplicação aumenta. 

**Replicação de multirregião:** Azure Cosmos DB replica de forma transparente os dados para todas as regiões que associou à sua conta do MongoDB, permitindo-lhe desenvolver aplicações que necessitam acesso global aos dados ao fornecer as responsabilidades entre consistência, disponibilidade e desempenho, tudo com garantias correspondentes. O Azure Cosmos DB oferece ativação pós-falha regional transparente com APIs multi-homing e a capacidade e dimensionar de forma elástica o débito e o armazenamento a nível global. Saiba mais em [distribuir dados globalmente](distribute-data-globally.md).

**Nenhuma gestão de servidor**: não tem de gerir e dimensionar as bases de dados de MongoDB. BD do Cosmos do Azure é um serviço completamente gerido, o que significa que não tem de gerir qualquer infraestrutura ou de máquinas virtuais por si. BD do Cosmos do Azure está disponível no 30 + [regiões do Azure](https://azure.microsoft.com/regions/services/).

**Níveis de consistência sincronizáveis:** BD do Cosmos Azure implementa atualmente MongoDB versão 3.4, que tem duas definições de consistência, seguras e eventual. Como base de dados do Azure Cosmos é várias api, as definições de consistência são aplicáveis ao nível da conta e a imposição da consistência e é controlada por cada API. Até MongoDB 3.6, não havia nenhum conceito de consistência de sessão, para que o se definir uma conta de API do MongoDB para utilizar a consistência de sessão, a consistência é alterada uma versão anterior a eventual ao utilizar as APIs do MongoDB. Se precisar de uma garantia de leitura-your-proprietário-escrita de uma conta de API do MongoDB, o nível de consistência predefinida para a conta deve ser definido como strong ou tem um vínculo vinculada. Saiba mais em [Using consistency levels to maximize availability and performance](consistency-levels.md) (Utilizar níveis de consistência para maximizar a disponibilidade e desempenho).

| Azure Cosmos DB predefinido de nível de consistência |   API do mongo (3.4) |
|---|---|
|Eventual| Eventual |
|Prefixo Consistente| Eventual com uma ordem consistente |
|Sessão| Eventual com uma ordem consistente |
|Estagnação Limitada| Forte |
| Forte | Forte |

**A indexação automática**: por predefinição, base de dados do Azure Cosmos indexa automaticamente os todas as propriedades dentro de documentos no seu MongoDB da base de dados e não espera nem requer nenhum esquema ou criação de índices secundários. Além disso, a capacidade de índice exclusivo permite uma restrição de exclusividade em todos campos de documento que já são indexados automática do BD Azure Cosmos.

**Nível de Enterprise**: base de dados do Azure Cosmos suporta várias réplicas locais para fornecer proteção de dados de disponibilidade e de 99,99% face a falhas de locais e regionais. BD do Azure do Cosmos tem de nível empresarial [certificações de conformidade](https://www.microsoft.com/trustcenter) e funcionalidades de segurança. 

Saiba mais neste vídeo com o Azure Cosmos DB programa sénior, Aleksey Savateyev.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T136/player]
> 

## <a name="how-to-get-started"></a>Como começar

Siga os inícios rápidos MongoDB para criar uma conta de base de dados do Azure Cosmos e migrar a sua aplicação MongoDB existente ao utilizar base de dados do Azure Cosmos ou criar um novo:

* [Migrar uma aplicação de web de Node.js MongoDB existente](create-mongodb-nodejs.md).
* [Criar uma aplicação de web API do MongoDB com .NET e o portal do Azure](create-mongodb-dotnet.md)
* [Criar uma aplicação de consola do MongoDB API com Java e o portal do Azure](create-mongodb-java.md)

## <a name="next-steps"></a>Passos Seguintes

Informações sobre a API do Azure Cosmos base de dados MongoDB estão integradas a documentação de base de dados do Azure Cosmos geral, mas aqui estão alguns ponteiros para começar:

* Siga o [ligar a uma conta do MongoDB](connect-mongodb-account.md) tutorial para saber como obter as informações de cadeia de ligação da conta.
* Siga o [utilize Studio 3T (MongoChef) com base de dados do Azure Cosmos](mongodb-mongochef.md) tutorial para saber como criar uma ligação entre a base de dados de base de dados do Azure Cosmos e aplicação MongoDB no Studio t 3.
* Siga o [migrar dados a BD do Cosmos do Azure com o protocolo de suportem para o MongoDB](mongodb-migrate.md) tutorial para importar os dados a uma API para a base de dados de MongoDB.
* Ligar a uma API para utilizar a conta MongoDB [Robomongo](mongodb-robomongo.md).
* Saiba RUs quantos suas operações estiver a utilizar com o [GetLastRequestStatistics comandos e as métricas de portais do Azure](request-units.md#GetLastRequestStatistics).
* Saiba como [configurar as preferências de leitura para as aplicações distribuídas globalmente](../cosmos-db/tutorial-global-distribution-mongodb.md).
