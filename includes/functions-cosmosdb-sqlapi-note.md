---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: c54aa861a47b11756f05e003e9b944df6c5b0e28
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360178"
---
Os enlaces do Cosmos DB do Azure só são suportados para utilização com a API de SQL. Para todas as outras APIs do Azure Cosmos DB, deve acessar o banco de dados da sua função com o cliente de estático para a sua API, incluindo [API do Azure Cosmos DB para o MongoDB](../articles/cosmos-db/mongodb-introduction.md), [Cassandra API](../articles/cosmos-db/cassandra-introduction.md), [ API do gremlin](../articles/cosmos-db/graph-introduction.md), e [API de tabela](../articles/cosmos-db/table-introduction.md).
