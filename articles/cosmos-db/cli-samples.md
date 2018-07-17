---
title: Exemplos de CLI do Azure para o Azure Cosmos DB | Microsoft Docs
description: Exemplos de CLI do Azure – Criar e gerir contas, bases de dados, contentores, regiões e firewalls do Azure Cosmos DB.
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.date: 11/29/2017
ms.author: sngun
ms.openlocfilehash: 42cfe71210b95732b4b69f7ca21a8b647e187a38
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858865"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Exemplos da CLI do Azure para o Azure Cosmos DB

A tabela seguinte inclui ligações para scripts da CLI do Azure de exemplo para o Azure Cosmos DB. As páginas de referência para todos os comandos da CLI do Azure Cosmos DB encontram-se disponíveis em [Referência do Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Criar a conta, a base de dados e os contentores do Azure Cosmos DB**||
|[Criar uma conta da API de SQL](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Cria a conta, a base de dados e o contentor de uma API de Azure Cosmos DB única para utilização com a API de SQL. |
| [Criar uma conta da API de MongoDB](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cria a conta, a base de dados e a coleção de uma API de MongoDB do Azure Cosmos DB única. |
| [Criar uma conta da API Gremlin](scripts/create-graph-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cria a conta, a base de dados e o contentor de uma API Gremlin do Azure Cosmos DB única. |
|**Dimensionar o Azure Cosmos DB**||
| [Dimensionar o débito do contentor](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Altera o débito aprovisionado num contentor.|
|[Replicar a conta de base de dados do Azure Cosmos DB em várias regiões e configurar prioridades de ativação pós-falha](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Replica globalmente os dados da conta em várias regiões com uma prioridade de ativação pós-falha especificada.|
|**Proteger o Azure Cosmos DB**||
| [Obter chaves de contas](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtém as chaves de escrita mestra primária e secundária, e as chaves só de leitura primária e secundária da conta.|
| [Obter a cadeia de ligação do MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtém a cadeia de ligação para ligar a aplicação MongoDB à sua conta do Azure Cosmos DB.|
|[Voltar a gerar chaves da conta](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Volta a gerar a chave mestra ou só de leitura da conta.|
|[Criar uma firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma política de controlo de acesso IP de entrada para limitar o acesso à conta a partir de um conjunto aprovado de computadores e/ou serviços cloud.|
|**Elevada disponibilidade, recuperação após desastre, cópia de segurança e restauro**||
|[Configurar política de ativação pós-falha](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Define a prioridade de ativação pós-falha de cada região em que a conta é replicada.|
|**Ligar o Azure Cosmos DB a recursos**||
|[Ligar uma aplicação Web ao Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Crie e ligue uma base de dados do Azure Cosmos DB e uma aplicação Web do Azure.|
|||
