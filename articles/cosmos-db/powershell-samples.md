---
title: Exemplos do Azure PowerShell para o Azure Cosmos DB | Microsoft Docs
description: Exemplos do Azure PowerShell - Scripts para ajudá-lo a criar e gerir contas do Azure Cosmos DB.
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: 066826661381f40af2943359eb75abae8306bb7b
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855588"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Exemplos do Azure PowerShell para o Azure Cosmos DB

A tabela seguinte inclui ligações para scripts de exemplo do Azure PowerShell para o Azure Cosmos DB. Atualmente, só pode gerir a conta do Azure Cosmos DB a partir do PowerShell; outros recursos, como bases de dados e contentores, não podem ser geridos através do PowerShell.

| |  |
|---|---|
|**Criar uma conta do Azure Cosmos DB**||
|[Criar uma conta da API de SQL](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma única conta do Azure Cosmos DB para utilização com a API SQL. |
|[Criar uma conta da API de MongoDB](scripts/create-mongodb-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma única conta do Azure Cosmos DB para utilização com a API MongoDB. |
|[Criar uma conta da API Gremlin](scripts/create-graph-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma única conta do Azure Cosmos DB para utilização com a API Gremlin. |
|**Dimensionar o Azure Cosmos DB**||
|[Replicar a conta do Azure Cosmos DB em várias regiões e configurar prioridades de ativação pós-falha](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Replica globalmente os dados da conta em várias regiões com uma prioridade de ativação pós-falha especificada.|
|**Proteger o Azure Cosmos DB**||
| [Obter chaves de contas](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Obtém as chaves de escrita mestra primária e secundária, e as chaves só de leitura primária e secundária da conta.|
| [Obter a cadeia de ligação do MongoDB](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Obtém a cadeia de ligação para ligar a aplicação MongoDB à sua conta do Azure Cosmos DB.|
|[Voltar a gerar chaves da conta](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Volta a gerar a chave mestra ou só de leitura da conta.|
|[Criar uma firewall](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma política de controlo de acesso IP de entrada para limitar o acesso à conta a partir de um conjunto aprovado de computadores e/ou serviços cloud.|
|**Elevada disponibilidade, recuperação após desastre, cópia de segurança e restauro**||
|[Configurar política de ativação pós-falha](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Define a prioridade de ativação pós-falha de cada região em que a conta é replicada.|
|||
