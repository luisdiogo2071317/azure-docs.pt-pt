---
title: Integração do Azure Data Lake Storage Gen2 com outros serviços do Azure | Documentos da Microsoft
description: Compreender como a geração 2 de armazenamento do Azure Data Lake integra-se com outros serviços do Azure
documentationcenter: ''
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: nitinme
ms.openlocfilehash: 7bc4889403e1d52cfa3b18792a554f0faf605132
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885678"
---
# <a name="integrate-azure-data-lake-storage-gen2-with-other-azure-services"></a>Integrar a geração 2 de armazenamento do Azure Data Lake com outros serviços do Azure

Pode utilizar os serviços do Azure para ingerir, analisar e visualizar os dados na sua conta de armazenamento de geração 2 de armazenamento do Azure Data Lake. Escolha os serviços que melhor se adequam às que tarefas que está a tentar realizar.

## <a name="ingest-data-into-your-data-lake"></a>Ingerir dados no seu o data lake

Estes serviços ajudarão a preencher suas o data lake com dados.

### <a name="azure-data-factory"></a>Azure Data Factory

Pode usar [do Azure Data Factory](https://azure.microsoft.com/services/data-factory/) para ingerir dados a partir destas origens:

* Tabelas do Azure
* Bases de dados SQL do Azure
* Armazém de dados SQL do Azure
* Blobs de armazenamento do Azure
* Bases de dados no local

Ver [mover dados para e de geração 2 de armazenamento do Data Lake com o Data Factory](../../data-factory/connector-azure-data-lake-store.md).

## <a name="analyze-and-visualize-data-in-your-data-lake"></a>Analise e visualize os dados no seu data lake

Estes serviços podem utilizar a geração 2 de armazenamento do Data Lake como um ponto de final de armazenamento.

### <a name="azure-hdinsight"></a>Azure HDInsight

Pode aprovisionar um [do Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) cluster que utiliza a geração 2 de armazenamento do Data Lake como o armazenamento compatível com HDFS. Nesta versão, para clusters do Hadoop e o Storm no Windows e Linux, pode utilizar geração 2 de armazenamento do Data Lake apenas como um armazenamento adicional. Estes clusters ainda usam o armazenamento do Azure (WASB) como armazenamento predefinido. No entanto, para clusters do HBase no Windows e Linux, pode utilizar o geração 2 de armazenamento do Data Lake como armazenamento predefinido e como armazenamento adicional.

Consulte [clusters de utilização do Azure Data Lake Storage Gen2 com o Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Pode usar [do Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-overview.md) para trabalhar com macrodados à escala da cloud. Aprovisiona dinamicamente recursos e permite-lhe analisar exabytes de dados. O Data Lake Analytics está otimizado para utilizar a geração 2 de armazenamento do Data Lake como uma origem de dados. 

Ver [introdução ao Data Lake Analytics com a geração 2 de armazenamento do Data Lake](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="copy-data-to-other-repositories"></a>Copiar dados para outros repositórios

Utilize estes serviços para copiar dados de sua o data lake e colocá-los em outros repositórios.

### <a name="sql-data-warehouse"></a>SQL Data Warehouse

Pode utilizar o PolyBase para carregar dados de geração 2 de armazenamento do Data Lake para o SQL Data Warehouse. Para obter mais informações, consulte [Use Data Lake armazenamento Gen2 com o SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Consulte também

* [Descrição geral do Azure Data Lake Storage Gen2](data-lake-storage-introduction.md)
* [Utilizar a geração 2 de armazenamento do Azure Data Lake para requisitos de grandes volumes de dados](data-lake-storage-data-scenarios.md)
