---
title: 'Azure Cosmos DB: API de .NET de Executor em massa, SDK e recursos | Documentos da Microsoft'
description: Saiba tudo sobre a API de .NET de Executor em massa e o SDK, incluindo as datas de lançamento, datas de extinção e as alterações feitas entre cada versão do SDK de .NET de Executor de em massa Azure Cosmos DB.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/14/2018
ms.author: ramkris
ms.openlocfilehash: ffd8f438429cd8769ac0dbff7f489327166e0000
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294463"
---
# <a name="net-bulk-executor-library-download-information"></a>Biblioteca de .NET em massa Executor: transferir informações 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Feed de alterações de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Fornecedor de Recursos REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Em massa Executor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Em massa Executor - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Descrição**</td><td>A biblioteca de Executor em massa permite que os aplicativos de cliente realizar operações em massa em contas do Azure Cosmos DB. Biblioteca de Executor em massa fornece BulkImport, BulkUpdate e BulkDelete espaços de nomes. O módulo pode em massa de BulkImport ingerir documentos de forma otimizada, de modo a que o débito aprovisionado para uma coleção é consumido para o respetivo limite máximo permitido. BulkUpdate módulo pode em massa atualizar os dados existentes nos contentores do Azure Cosmos DB como patches. O módulo de BulkDelete pode em massa documentos de eliminação de forma otimizada, de modo a que o débito aprovisionado para uma coleção é consumido para o respetivo limite máximo permitido.</td></tr>

<tr><td>**Transferência de SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**Biblioteca do BulkExecutor no GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**Documentação da API**</td><td>[Documentação de referência de .net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Introdução**</td><td>[Introdução ao .NET SDK da biblioteca do Executor em massa](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Estrutura de suporte atual**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)(versão > = 2.0.0)</li><li>
[Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/)(versão > = 9.0.1)
</li></ul></td></tr>
</table></br>

## <a name="release-notes"></a>Notas de versão

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Foi adicionado suporte para a operação de BulkDelete para contas de API de SQL do Azure Cosmos DB.
* Foi adicionado suporte para a operação de BulkImport para contas de API do MongoDB do Azure Cosmos DB.
* Diminuí a cópia de segurança a dependência de SDK .NET do DocumentDB para a versão 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Foi adicionado suporte para a operação de BulkImport para contas de API do Azure Cosmos DB Gremlin.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Correção de erros menor para a operação de BulkImport para contas de API de SQL do Azure Cosmos DB.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Foi adicionado suporte BulkImport BulkUpdate para operações de e para contas de API de SQL do Azure Cosmos DB.
