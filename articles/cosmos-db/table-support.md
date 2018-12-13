---
title: Suporte de armazenamento de tabelas do Azure no Azure Cosmos DB
description: Saiba como a API de Tabela do Azure Cosmos DB e as Tabelas de Armazenamento do Azure funcionam em conjunto.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.topic: overview
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 6c6b0650ae8f4ed780b72e8fcd414e3f26e4f5f3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878742"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Programar com a API de Tabela do Azure Cosmos DB e o armazenamento de Tabelas do Azure

A API de Tabela do Azure Cosmos DB e o armazenamento de Tabelas do Azure partilham o mesmo modelo de dados de tabela e expõem as mesmas operações para criar, eliminar, atualizar e consultar através dos SDKs. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Programar com a API de Tabela do Azure Cosmos DB

Neste momento, a [API de Tabela do Azure Cosmos DB](table-introduction.md) tem quatro SDKs disponíveis para programação: 
- SDK .NET [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget). Esta biblioteca tem as mesmas classes e assinaturas de método que o [SDK do Armazenamento do Windows Azure](https://www.nuget.org/packages/WindowsAzure.Storage) público, mas também tem a capacidade de ligar a contas do Azure Cosmos DB através da API de Tabela. Tenha em atenção que a biblioteca `Microsoft.Azure.CosmosDB.Table` está atualmente disponível apenas para o .NET Standard, ainda não está disponível para .NET Core.
- [SDK Python](table-sdk-python.md). O novo SDK Python Azure Cosmos DB é o único SDK que suporta o armazenamento de Tabelas no Python. Este SDK liga ao armazenamento de Tabelas do Azure e à API de Tabela do Azure Cosmos DB.
- [SDK Java](table-sdk-java.md). Este SDK do Armazenamento do Azure tem capacidade para ligar a contas do Azure Cosmos BD através da API de Tabela.
- [SDK Node.js](table-sdk-nodejs.md). Este SDK do Armazenamento do Azure tem capacidade para ligar a contas do Azure Cosmos BD através da API de Tabela.

Estão disponíveis informações adicionais sobre como trabalhar com a API de Tabela no artigo [FAQ: Programar com a API de Tabela](faq.md#table).

## <a name="developing-with-azure-table-storage"></a>Programar com o armazenamento de Tabelas do Azure

O armazenamento de Tabelas do Azure tem estes SDKs disponíveis para programação:

- [SDK .NET WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/). Esta biblioteca permite-lhe funcionar com o serviço de Tabelas de armazenamento.
- [SDK Python](table-sdk-python.md). O SDK de Tabelas do Azure Cosmos DB para Python também suporta o serviço de Tabelas de armazenamento.
- [SDK do Armazenamento do Azure para Java](https://github.com/azure/azure-storage-java). Este SDK do Armazenamento do Azure fornece uma biblioteca de cliente no Java para consumir o armazenamento de Tabelas do Azure.
- [SDK Node.js](table-sdk-nodejs.md). Este SDK fornece um pacote Node.js e uma biblioteca de cliente JavaScript compatível com o browser para consumir o serviço de Tabelas de armazenamento.
- [Módulo PowerShell do AzureRmStorageTable](https://www.powershellgallery.com/packages/AzureRmStorageTable/1.0.0.7). Este módulo PowerShell tem cmdlets para trabalhar com Tabelas de armazenamento.
- [Biblioteca de Cliente do Armazenamento do Azure para C++](https://github.com/Azure/azure-storage-cpp/). Esta biblioteca permite-lhe criar aplicações com o Armazenamento do Azure.
- [Biblioteca de Cliente das Tabelas de Armazenamento do Azure para Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Este projeto fornece um pacote Ruby que torna mais fácil aceder aos serviços de Tabela de armazenamento do Azure.
- [Biblioteca de Cliente PHP das Tabelas de Armazenamento do Azure](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Este projeto fornece uma biblioteca de cliente PHP que torna mais fácil aceder aos serviços de Tabela de armazenamento do Azure.


   





