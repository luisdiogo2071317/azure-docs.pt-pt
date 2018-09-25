---
title: Utilizar o Azure Cosmos DB com vários mestres com bases de dados de NoSQL de código-fonte aberto
description: ''
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ea3a16cf6b5aa4e46ad401e59aacb4d936a7429a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963905"
---
# <a name="using-azure-cosmos-db-multi-master-with-open-source-nosql-databases"></a>Utilizar o Azure Cosmos DB com vários mestres com bases de dados de NoSQL de código-fonte aberto

Suporte de vários mestre do Azure Cosmos DB é uma implementação nativa, do lado do servidor que está disponível para todas as ofertas de NoSQL de código-fonte aberto suportadas pelo Cosmos DB. Também é acessível a todos do Cosmos DB suportada do SDK.

O Azure Cosmos DB é o primeiro serviço do mundo para oferecer suporte de vários mestre para esses bancos de dados do NoSQL de código-fonte aberto.

|Modelo  |Suporte  |
|---------|---------|
|MongoDB  | Ativa-ativa  |
|Graph  | Ativa-ativa |
|Cassandra  | Ativa-ativa   |

## <a name="use-mongodb-clients-with-multi-master"></a>Utilizar clientes do MongoDB com múltiplos principais

A funcionalidade de vários mestre está ativada para contas de API do MongoDB da mesma forma que ele está ativado para outras APIs do Azure Cosmos DB. Depois de ativar múltiplos principais para contas de API do MongoDB, cada instância de uma aplicação cliente pode selecionar a região preferencial para leituras e gravações. Da perspectiva de driver do MongoDB, a região preferencial é apresentada ao cliente como o conjunto principal de réplica. Desta forma, todas as regiões da base de dados distribuída podem agir como réplica definir primária. Azure Cosmos DB com vários mestres permite-lhe reduzir significativamente as latências de escrita para as suas aplicações do MongoDB e distribuídas globalmente. 

### <a name="set-the-primary-region"></a>Defina a região primária

Cada instância de um cliente de MongoDB pode selecionar a região primária, acrescentando `@<preferred_primary_region_name>` para o campo "nome da aplicação" Aceito pelo controlador de cliente MongoDB. A maioria dos drivers aceitam isso na cadeia de ligação, tais como:

`mongodb://fabrikam:KEY@fabrikam.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

Depois de ligar, pode ocorrer um dos seguintes casos:

* Se o nome da região preferencial está disponível como uma região de escrita, em seguida, do Azure Cosmos DB seleciona-lo como a região primária.

* Se não for fornecido um nome de região preferencial, o Azure Cosmos DB seleciona uma região predefinida como a região primária.

* Se não for fornecido um nome de região preferencial, mas se não está disponível como uma região de escrita para a conta de base de dados, o Azure Cosmos DB irá selecionar a região de escrita mais próxima que está disponível como a região primária.

Determinados drivers, como o controlador NodeJS sempre será primeiro problema escreve para o anfitrião especificado a cadeia de ligação inicial. Para esses drivers, para garantir que gravações são direcionadas para a região preferencial, além do nome de aplicação, deve modificar o nome DNS dentro da cadeia de ligação para incluir o nome da região. Certifique-se de que especificar o nome da região sem quaisquer espaços. Por exemplo:

`mongodb://fabrikam:KEY@fabrikam-westus.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

### <a name="conflict-resolution-mode"></a>Modo de resolução de conflito

O modo de resolução de conflito de contas de API do MongoDB do Azure Cosmos DB está sempre last-escritor-wins, utilizando o carimbo de hora do servidor regionais que aceite a gravação.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu sobre o suporte de vários mestre para contas de API do MongoDB do Azure Cosmos DB. Em seguida, observe os seguintes recursos:

* [Como ativar múltiplos principais para contas do Azure Cosmos DB](enable-multi-master.md)

* [Noções básicas sobre a resolução de conflitos no Azure Cosmos DB](multi-master-conflict-resolution.md)
