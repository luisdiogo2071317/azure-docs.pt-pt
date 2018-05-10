---
title: 'Do Azure Cosmos DB: API do SQL Server Async Java, SDK & recursos | Microsoft Docs'
description: Saiba tudo sobre a API do SQL Server Async Java e o SDK, incluindo as datas de versão, as datas de extinção e as alterações efetuadas entre cada versão do SDK de Java do assíncrona de BD SQL do Azure Cosmos.
services: cosmos-db
documentationcenter: java
author: SnehaGunda
manager: kfile
ms.assetid: a452ffa2-c15d-4b0a-a8c1-ec9b750ce52b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 03/20/2018
ms.author: sngun
ms.openlocfilehash: 25a84c42430c76d296e12d3f83040fa18febdcb1
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Async SDK Java para API do SQL Server: notas de versão e recursos
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Feed de alteração de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Fornecedor de Recursos REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

O SDK Java do SQL Server API Async difere de acordo com o SDK de Java de API do SQL Server, fornecendo as operações assíncronas com suporte do [biblioteca Netty](http://netty.io/). O pré-existente [SQL API Java SDK](sql-api-sdk-java.md) não suporta operações assíncronas. 

<table>

<tr><td>**Transferência do SDK**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**Documentação da API**</td><td>[Documentação de referência da API de Java](https://azure.github.io/azure-cosmosdb-java/)</td></tr>

<tr><td>**Contribuir para o SDK**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Introdução**</td><td>[Introdução ao SDK de Java assíncrona](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**exemplo de código**</td><td>[Github](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Sugestões de desempenho**</td><td>[Leia-me do Github](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**Tempo de execução mínimo suportado**</td><td>[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Notas de versão

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Adicionado suporte de back-pressão na consulta.
* Suporte adicionado para o id de intervalo de chaves de partição na consulta.
* Correção para permitir que o token de continuação maior no cabeçalho de pedido (bugfix github #24).
* dependência netty atualizada para 4.1.22.Final para garantir JVM encerrado após a conclusão da thread principal.
* Corrigir evitar a transmitir o token da sessão quando ao ler os recursos principais.
* Adicionar mais exemplos.
* Adicionar mais benchmarking cenários.
* Ficheiros de cabeçalho de Java fixos para a geração de documentação do java adequado.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* SDK GA com suporte de ponto a ponto para a utilização de e/s não bloquear o [biblioteca Netty](http://netty.io/) no modo de gateway. 

## <a name="release-and-retirement-dates"></a>Datas de lançamento e de extinção
A Microsoft vai fornecer pelo menos notificação **12 meses** previamente extinguir um SDK para smooth a transição para uma versão mais recente/suportado.

Novas funcionalidades e a funcionalidade e otimizações apenas são adicionadas ao SDK atual. Por isso, recomenda-se que sempre atualizar para a versão mais recente do SDK antecipadamente quanto possível.

Qualquer pedido de BD do Cosmos utilizando um SDK extinto será rejeitado pelo serviço.

<br/>

| Versão | Data da versão | Data de retirada |
| --- | --- | --- |
| [1.0.1](#1.0.1) |20 de Abril de 2018|--- |
| [1.0.0](#1.0.0) |27 de fevereiro de 2018|--- |

## <a name="faq"></a>FAQ
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Consulte também
Para saber mais sobre a base de dados do Cosmos, consulte [base de dados do Microsoft Azure Cosmos](https://azure.microsoft.com/services/cosmos-db/) página do serviço.

