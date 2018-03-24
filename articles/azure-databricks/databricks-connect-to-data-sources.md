---
title: Ligar a origens de dados diferentes a partir do Azure Databricks | Microsoft Docs
description: Saiba como ligar a origens de dados diferentes a partir do Azure Databricks.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: nitinme
ms.openlocfilehash: 865313a7c6eabd847529b88ff5fff0b7db438fa5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Ligar a origens de dados a partir do Azure Databricks

Este artigo fornece ligações para todas as origens de dados diferentes no Azure que pode ser ligado ao Azure Databricks. Siga estas ligações para extrair dados das origens de dados do Azure (por exemplo, Blob Storage do Azure, Event Hubs do Azure, etc.) para um cluster do Azure Databricks os exemplos e executar tarefas analíticas nos mesmos. 

## <a name="prerequisites"></a>Pré-requisitos

* Tem de ter uma área de trabalho do Azure Databricks e um cluster do Spark. Siga as instruções apresentadas em [introdução ao Azure Databricks](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Origens de dados do Azure Databricks

A lista seguinte apresenta as origens de dados no Azure que pode utilizar com Databricks do Azure. Para obter uma lista completa das origens de dados que podem ser utilizadas com Databricks do Azure, consulte [origens de dados do Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Base de dados SQL do Azure](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Esta ligação fornece a API de DataFrame para ligar às bases de dados do SQL Server através de JDBC e como controlar o paralelismo de leituras através da interface JDBC. Este tópico fornece exemplos de detalhado utilizando a API de Scala, com o Python abreviado e exemplos de Spark SQL no final.
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html)

    Esta ligação fornece exemplos sobre como utilizar o principal de serviço do Azure Active Directory para autenticar com o Data Lake Store. Fornece também instruções sobre como aceder aos dados no Data Lake Store do Azure Databricks.

- [Armazenamento de Blobs do Azure](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    Esta ligação fornece exemplos sobre como aceder diretamente ao armazenamento de Blobs do Azure do Azure Databricks utilizando a chave de acesso ou a SAS para um contentor fornecido. A ligação também fornece informações sobre como aceder ao armazenamento de Blobs do Azure do Azure Databricks utilizando a API de RDD.

- [BD do Cosmos para o Azure](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    Esta ligação fornece instruções sobre como utilizar o [conector Azure Cosmos DB Spark](https://github.com/Azure/azure-cosmosdb-spark) de Databricks do Azure para aceder a dados na base de dados do Azure Cosmos.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    Esta ligação fornece instruções sobre como utilizar o [conector Azure Event Hubs Spark](https://github.com/Azure/azure-event-hubs-spark) de Databricks do Azure para aceder a dados em Event Hubs do Azure.

- [Azure SQL Data Warehouse](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    Esta ligação fornece instruções sobre como utilizar o conector de armazém de dados SQL do Azure para ligar a partir do Azure Databricks.
    

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as origens de onde pode importar dados para Databricks do Azure, consulte [origens de dados do Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


