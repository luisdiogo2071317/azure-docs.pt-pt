---
title: 'Azure Cosmos DB: executor em massa API de .NET, SDK & recursos | Microsoft Docs'
description: Saiba tudo sobre o executor em massa, incluindo as datas de versão, as datas de extinção e as alterações efetuadas entre cada versão do executor em massa de BD do Cosmos Azure .NET SDK do SDK e .NET API.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 5f2bc50a59219a38c2fcf4d501de4d8a11f6fbf1
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300759"
---
# <a name="net-bulk-executor-library-download-information"></a>Biblioteca de executor do .NET em massa: transferir as informações 

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
> * [executor em massa - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [executor em massa - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Descrição**</td><td>A biblioteca de executor em massa permite que aplicações de cliente efetuar operações em massa na base de dados do Azure Cosmos contas. biblioteca de executor em massa fornece BulkImport e BulkUpdate espaços de nomes. BulkImport módulo pode efetuar em massa de inserção documentos de uma forma otimizada, de modo a que o débito aprovisionado para uma coleção é consumido para o respetivo limite máximo permitido. BulkUpdate módulo pode efetuar em massa atualizar os dados existentes na base de dados do Azure Cosmos contentores como patches.</td></tr>

<tr><td>**Transferência do SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**Biblioteca de BulkExecutor no GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**Documentação da API**</td><td>[Documentação de referência de .net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Introdução**</td><td>[Introdução à biblioteca de executor em massa .NET SDK](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Arquitetura suportada atual**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)(versão > = 1.21.1)</li><li>
[Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/)(versão > = 10.0.2)
</li></ul></td></tr>
</table></br>

