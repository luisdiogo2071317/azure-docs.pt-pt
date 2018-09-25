---
title: Principais benefícios da distribuição global do Azure Cosmos DB
description: Saiba mais sobre os benefícios de vários mestres, chaves do Azure Cosmos DB oferecidos pela georreplicação, com vários mestres e uso casos em que é útil.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 11d70a648bfc1882753688e5352835b04cee6b9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968355"
---
# <a name="distribute-data-globally-with-azure-cosmos-db"></a>Distribuir dados globalmente com o Azure Cosmos DB

Este artigo descreve os principais benefícios de distribuir dados globalmente e alguns cenários em tempo real, onde é necessária a distribuição de dados globais.

## <a name="key-benefits"></a>Principais vantagens

Seguem-se os principais benefícios disponíveis para contas de tirar partido das capacidades de distribuição de dados global do Azure Cosmos DB:

* **Latência de um só dígito** – Azure Cosmos DB oferece < 10 ms de leitura e escrita latência no percentil 99, garantida pelo [apoiado financeiramente SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **disponibilidade de 5 a 9** – Azure Cosmos DB oferece 99,999% de leitura e escrita de disponibilidade, garantida pelo [apoiado financeiramente SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **Resolução de conflitos flexível** – para os clientes a tirar partido das capacidades de vários mestres, Azure Cosmos DB fornece três modos para lidar com conflitos para garantir a integridade dos dados globais e consistência.

* **Consistência otimizável** – Azure Cosmos DB suporta 5 diferentes [níveis de consistência](consistency-levels.md) com a distribuição global, lembre-se com suporte para consistência forte tudo, mas para contas com capacidade de dominar o múltiplas.

* **Tolerância a falhas implícita** -com os dados replicados em várias regiões, os aplicativos podem usufruir tolerância de falhas elevada contra falhas regionais.

## <a name="use-cases"></a>Casos de utilização

Aqui é apenas uma pequena amostra dos cenários, que pode aproveitar o globalmente os recursos de distribuição do Azure Cosmos DB.

* **Aplicações de redes sociais** – aplicações de redes sociais requerem baixa latência, elevada disponibilidade e escalabilidade para fornecer uma excelente experiência para os usuários localizados em todo o mundo.

* **IoT** -implementações distribuídas geograficamente edge, muitas vezes, precisam de controlar os dados de séries de tempo a partir de várias localizações. Cada dispositivo pode ser hospedado para a região mais próxima. Quando os dispositivos são movidos para locais diferentes, esses dispositivos podem ser movidos escrever para a região disponível mais próxima.

* **E-Commerce** -E-Commerce requer muito baixa latência, bem como elevada disponibilidade. A distribuição geográfica de dados com leituras e gravações coloca os dados mais próxima dos utilizadores, aumentando a capacidade de resposta dos aplicativos. Disponibilidade de leitura e escrita em várias regiões fornece mais de disponibilidade.

* **Deteção de fraudes/anomalias** -, muitas vezes, os aplicativos que monitorizam a atividade do utilizador ou a atividade de conta são distribuídos globalmente e deve controlar vários eventos em simultâneo para atualizar as pontuações para manter o inline de métricas de risco.

* **Medição** - contagem e regular a utilização (tais como chamadas de API, utilizado de minutos de transações/segundo,) pode ser implementado globalmente com facilidade usando o Azure Cosmos DB com vários mestres. Resolução de conflito incorporado assegura a ambos os precisão das contagens e regulamento em tempo real.

## <a name="next-steps"></a>Passos Seguintes  

Neste artigo, aprendeu sobre os principais benefícios e casos de utilização para os recursos de distribuição de dados global do Azure Cosmos DB. Em seguida, observe os seguintes recursos:

* [Como o Azure Cosmos DB permite a distribuição global chave na mão](distribute-data-globally-turnkey.md)

* [Como configurar a replicação de base de dados global do Azure Cosmos DB](tutorial-global-distribution-sql-api.md)

* [Como ativar múltiplos principais para contas do Azure Cosmos DB](enable-multi-master.md)

* [Como funciona a ativação pós-falha automática e manual no Azure Cosmos DB](regional-failover.md)

* [Noções básicas sobre a resolução de conflitos no Azure Cosmos DB](multi-master-conflict-resolution.md)

* [através de vários mestres com bases de dados do código-fonte aberto NoSQL](multi-master-oss-nosql.md)
