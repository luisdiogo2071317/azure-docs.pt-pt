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
ms.openlocfilehash: bb915c18965a55c2b1fc7fe3cf1fa923d7f43fd6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43052524"
---
Os enlaces do Cosmos DB do Azure só são suportados para utilização com a API de SQL. Para todas as outras APIs do Azure Cosmos DB, deve aceder ao banco de dados da sua função com o cliente estático para a sua API, incluindo [API do MongoDB](../articles/cosmos-db/mongodb-introduction.md), [API para Cassandra](../articles/cosmos-db/cassandra-introduction.md), [API de Gremlin](../articles/cosmos-db/graph-introduction.md), e [API de Tabela](../articles/cosmos-db/table-introduction.md).
