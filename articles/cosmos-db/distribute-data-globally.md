---
title: Distribuir dados globalmente com o Azure Cosmos DB | Documentos da Microsoft
description: Mais informações sobre recuperação de dados, ativação pós-falha, com vários mestres e georreplicação de escala planetária com bases de dados global do Azure Cosmos DB, um serviço de base de dados com múltiplos modelos distribuída globalmente.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 227c243d82665dc533e3bfa6a1fe3e9bb775a262
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408900"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribuição de dados global com o Azure Cosmos DB

O Azure é onipresente – ele tem uma presença global em 50 + regiões geográficas e está continuamente a expandir. Com sua presença global e o suporte de vários mestre, um dos recursos diferenciados que Azure oferece aos seus desenvolvedores é a capacidade de criar, implementar e gerir facilmente aplicações distribuídas globalmente.

O [Azure Cosmos DB](../cosmos-db/introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. O Azure Cosmos DB oferece distribuição global chave na mão, [dimensionamento elástico de débito e armazenamento](../cosmos-db/partition-data.md) leitura em todo o mundo, de um só dígito e latências de milissegundos de escrita no percentil 99, [consistência bem definidos modelos](consistency-levels.md)e de elevada disponibilidade garantida, tudo apoiada por [SLAs abrangentes líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/). O Azure Cosmos DB [indexa automaticamente todos os seus dados](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem ter de lidar com a gestão de índices ou esquemas.

## <a name="global-distribution-with-multi-master"></a>Distribuição global com múltiplos principais

Como um serviço em nuvem, o Azure Cosmos DB com cuidado foi desenvolvido para suportar vários inquilinos, distribuição global e com vários mestres para documentos, chave-valor, gráfico e modelos de dados de família de colunas.

![Contentor do Azure Cosmos DB particionado e distribuídos em três regiões](./media/distribute-data-globally/global-apps.png)

**Um único contentor do Azure Cosmos DB particionado e distribuídos em várias regiões do Azure**

À medida que aprendemos durante a criação do Azure Cosmos DB, a adição de distribuição global não pode ser uma reflexão tardia. Não pode ser "bolted em" sobre um sistema de banco de dados de "site único". Abrangem os recursos oferecidos por uma base de dados distribuída globalmente para além dos recursos oferecidos por uma recuperação de desastre geográfico tradicional (Geo-DR) oferecida pelas bases de dados de "site único". Bases de dados de site único oferecendo a capacidade Geo-DR são um subconjunto estrito das bases de dados distribuídos globalmente.

Com o Azure Cosmos DB distribuição global chave na mão, os desenvolvedores não precisam criar seus próprios estruturais de replicação, empregando o padrão de Lambda sobre o registo de base de dados ou ao executar "escritas duplas" em várias regiões. Fazemos *não* Recomendamos essas abordagens, uma vez que é impossível garantir a correção dessas abordagens e fornecer SLAs de som.

## <a id="Next Steps"></a>Passos seguintes

* [Como o Azure Cosmos DB permite a distribuição global chave na mão](distribute-data-globally-turnkey.md)

* [Benefícios de chaves do Azure Cosmos DB distribuição global](distribute-data-globally-benefits.md)

* [Como configurar a replicação de base de dados global do Azure Cosmos DB](tutorial-global-distribution-sql-api.md)

* [Como ativar múltiplos principais para contas do Azure Cosmos DB](enable-multi-master.md)

* [Como funciona a ativação pós-falha automática e manual no Azure Cosmos DB](regional-failover.md)

* [Noções básicas sobre a resolução de conflitos no Azure Cosmos DB](multi-master-conflict-resolution.md)

* [através de vários mestres com bases de dados do código-fonte aberto NoSQL](multi-master-oss-nosql.md)
