---
title: Utilizar o Apache Hive como uma ferramenta ETL - o Azure HDInsight | Microsoft Docs
description: Utilizar o Apache Hive para extrair, transformar e carregar (ETL) de dados no Azure HDInsight.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 1ccbfe23e9c887a98a0dbfa8031078a15c6e41b6
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Utilizar o Apache Hive como uma ferramenta de extracção, transformação e carregamento (ETL)

Normalmente, tem de apagar e transforme dados de entrada antes de carregá-lo para um destino adequado para análise. Operações de extração, transformação e carregar (ETL) são utilizadas para preparar os dados e carregue-a para um destino de dados.  Hive no HDInsight pode ler dados não estruturados, processar os dados conforme necessário e, em seguida, carregar os dados para um armazém de dados relacionais para sistemas de suporte da decisão. Esta abordagem, o dados são extraídos da origem de e armazenados no armazenamento escalável, tais como o blobs Storage do Azure ou do Azure Data Lake Store. Os dados, em seguida, são transformados utilizando uma sequência de consultas do Hive e, finalmente, são testados no ramo de registo em preparação para em massa ao carregar para o arquivo de dados de destino.

## <a name="use-case-and-model-overview"></a>Utilize a descrição geral do cenário e modelo

A figura seguinte mostra uma descrição geral de caso de utilização e o modelo para a automatização de ETL. Dados de entrada são transformados para gerar a saída adequada.  Durante esse transformação, os dados podem alterar forma, o tipo de dados e o mesmo idioma.  Processos de ETL podem converter Imperial métrica, altere os fusos horários e melhorar a precisão corretamente alinhar com os dados existentes no destino.  Processos de ETL também podem combinar novos dados com dados existentes para manter reporting atualizado ou para fornecer mais informações sobre os dados existentes.  Aplicações como o relatório de ferramentas e serviços, em seguida, podem consumir estes dados no formato pretendido.

![Apache Hive como ETL](./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png)

Hadoop é normalmente utilizado em processos de ETL importar um grande número de ficheiros de texto (por exemplo, CSVs) ou uma mais pequeno, mas frequentemente alterar o número de ficheiros de texto, ou ambos.  Ramo de registo é uma ótima ferramenta para utilizar para preparar os dados antes de carregá-lo para o destino de dados.  Ramo de registo permite-lhe criar um esquema ao longo de CSV e utilizar uma linguagem semelhante a SQL para gerar os programas de MapReduce que interagem com os dados. 

Os passos típicos para utilizar o Hive para efetuar ETL são os seguintes:

1. Carregar dados para o Azure Data Lake Store ou do Blob Storage do Azure.
2. Crie uma base de dados do arquivo de metadados (através da SQL Database do Azure) para utilizar o Hive no armazenar as esquemas.
3. Criar um cluster do HDInsight e ligar o arquivo de dados.
4. Defina o esquema para aplicar no momento da leitura através de dados no arquivo de dados:

    ```
    DROP TABLE IF EXISTS hvac;

    --create the hvac table on comma-separated sensor data stored in Azure Storage blobs
    
    CREATE EXTERNAL TABLE hvac(`date` STRING, time STRING, targettemp BIGINT,
        actualtemp BIGINT, 
        system BIGINT, 
        systemage BIGINT, 
        buildingid BIGINT)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    STORED AS TEXTFILE LOCATION 'wasb://{container}@{storageaccount}.blob.core.windows.net/HdiSamples/SensorSampleData/hvac/';
    ```

5. Transformar os dados e carregue-o para o destino.  Existem várias formas de utilizar o Hive durante a transformação e carregamento:

    * Consulta e preparar dados utilizando o Hive e guardá-lo como um CSV no Azure Data Lake Store ou armazenamento de Blobs do Azure.  Em seguida, utilize uma ferramenta como o SQL Server Integration Services (SSIS) para adquirir essas CSVs e carregar os dados para uma base de dados relacional do destino como o SQL Server.
    * Consulta os dados diretamente a partir do Excel ou c# utilizando o controlador ODBC do Hive.
    * Utilize [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md) para ler os ficheiros CSV simples preparados e carregá-los para a base de dados relacional do destino.

## <a name="data-sources"></a>Origens de dados

Origens de dados são normalmente externos dados que podem ser correspondidos aos dados existentes no arquivo de dados, por exemplo:

* Dados de redes sociais, ficheiros de registo, sensores e aplicações que geram ficheiros de dados.
* Conjuntos de dados obtidos de fornecedores de dados, por exemplo, estatísticas de meteorologia ou o fornecedor de números de vendas.
* Dados de transmissão em fluxo capturado, filtrados e processados através de uma ferramenta adequada ou framework.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Destinos de saída

Pode utilizar o Hive para dados de saída a uma variedade de destinos, incluindo:

* Base de dados relacional, tais como o SQL Server ou SQL Database do Azure.
* Um armazém de dados, tais como o Azure SQL Data Warehouse.
* Excel.
* Armazenamento de BLOBs e de tabela do Azure.
* Aplicações ou serviços que necessitam de dados a serem processados em formatos específicos ou, como os ficheiros que contêm tipos específicos de estrutura de informações.
* Um arquivo de documentos JSON como <a href="https://azure.microsoft.com/services/cosmos-db/">CosmosDB</a>.

## <a name="considerations"></a>Considerações

O modelo ETL é normalmente utilizado quando pretender:

* Carregar dados de fluxo ou grandes volumes de dados não estruturados ou semiestruturados partir de origens externas para uma base de dados existente ou o sistema de informações.
* Limpar, transformar e validar os dados antes de carregá-lo, talvez através da utilização de mais do que uma transformação passar o cluster.
* Gere relatórios e visualizações que são atualizadas regularmente.  Por exemplo, se o relatório demora demasiado tempo a gerar durante o dia, pode agendar o relatório seja executado noite.  Pode utilizar o agendador do Azure e o PowerShell para executar automaticamente uma consulta do Hive.

Se o destino para os dados não é uma base de dados, pode gerar um ficheiro no formato adequado na consulta, por exemplo um CSV. Este ficheiro, em seguida, pode ser importado para o Excel ou do Power BI.

Se precisar de executar várias operações nos dados como parte do processo de ETL, considere como geri-los. Se as operações são controladas por um programa externo, vez como um fluxo de trabalho da solução, terá de decidir se algumas operações podem ser executadas em paralelo e para detetar quando cada tarefa é concluída. Pode ser mais fácil a tentar orquestrar uma sequência de operações, utilizando scripts externos ou de programas personalizados utilizando um mecanismo de fluxo de trabalho, tais como Oozie dentro do Hadoop. Para obter mais informações sobre Oozie, consulte [orquestração de fluxo de trabalho e tarefa](https://msdn.microsoft.com/library/dn749829.aspx).

<!-- ## Next steps -->
<!-- * [ETL at scale](../hdinsight-etl-at-scale.md): Learn more about performing ETL at scale. -->
<!-- * [Operationalize Data Pipelines with Oozie](hdinsight-operationalize-data-pipeline.md): Learn how to build a data pipeline that uses Hive to summarize CSV flight delay data, stage the prepared data in Azure Storage blobs, and then use Sqoop to load the summarized data into Azure SQL Database. -->
<!-- * [ETL Deep Dive](../hdinsight-etl-deep-dive.md): Walk through an end-to-end ETL pipeline.  -->
