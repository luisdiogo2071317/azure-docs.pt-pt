---
title: "Extrair, transformação e carregamento (ETL) à escala - Azure HDInsight | Microsoft Docs"
description: "Saiba como ETL é utilizado no HDInsight com o Hadoop."
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
ms.openlocfilehash: 47c2d129cb296f6387142e03b14356bcd83ad698
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Extração, transformação e carregamento (ETL) à escala

Extração, transformação e carregamento (ETL) é o processo através do qual dados é adquirir a partir de várias origens, recolhidos numa localização padrão, limpa e processados e basicamente carregados para um arquivo de dados a partir da qual pode ser consultado. Os processos de ETL legados importar dados, limpe a mesma no local e, em seguida, guarde-o num motor de dados relacionais. Com o HDInsight, uma ampla variedade de componentes do ecossistema Hadoop suporta a realização de ETL à escala. 

A utilização do HDInsight no processo ETL pode ser resumida por este pipeline:

![Descrição geral do HDInsight ETL](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

As secções seguintes explorar cada um das fases ETL e os respetivos componentes associados.

## <a name="orchestration"></a>Orquestração

Orquestração abrange todas as fases do pipeline ETL. Tarefas ETL no HDInsight, muitas vezes, envolvem várias diferentes produtos trabalhar em conjunto com entre si.  Pode utilizar o Hive para limpar uma parte dos dados, enquanto o Pig cleans parte de outro.  Poderá utilizar o Azure Data Factory para carregar dados para o Azure SQL Database do Azure Data Lake Store.

Orquestração é necessário para executar a tarefa adequada no momento adequado.

### <a name="oozie"></a>Oozie

O Apache Oozie é um sistema de coordenação de fluxo de trabalho que gere as tarefas do Hadoop. Oozie é executado dentro de um cluster do HDInsight e está integrado com a pilha do Hadoop. Oozie suporta tarefas do Hadoop para Apache MapReduce, Apache Pig, Apache Hive e Apache Sqoop. Oozie também pode ser utilizado para agendar tarefas específicas para um sistema, tais como programas de Java ou scripts de shell.

Para obter mais informações, consulte [Oozie de utilização com o Hadoop para definir e executar um fluxo de trabalho no HDInsight](../hdinsight-use-oozie-linux-mac.md)

<!-- For a deep dive showing how to use Oozie to drive an end-to-end pipeline, see [Operationalize the Data Pipeline](hdinsight-operationalize-data-pipeline.md). -->

### <a name="azure-data-factory"></a>Azure Data Factory

O Azure Data Factory fornece capacidades de orquestração sob a forma de plataforma-como-um-serviço. É um serviço de integração de dados baseado na nuvem permite-lhe criar fluxos de trabalho condicionada por dados na nuvem para orquestrar e automatizar o movimento de dados e a transformação de dados. 

Utilizar o Azure Data Factory, pode:

1. Crie e agendar condicionada por dados fluxos de trabalho (pipelines denominados) que ingestão de dados de arquivos de dados diferentes.
2. Processo e os dados através de transformação de computação serviços como o Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics, do Azure Batch e do Azure Machine Learning.
3. Publicar os dados de saída em arquivos de dados como o Azure SQL Data Warehouse para consumo das aplicações de business intelligence (BI).

Para obter mais informações sobre o Azure Data Factory, consulte o [documentação](../../data-factory/introduction.md).

## <a name="ingest-file-storage-and-result-storage"></a>Armazenamento de ficheiros e armazenamento de resultado de inserção

Ficheiros de origem de dados, normalmente, são carregados para uma localização de armazenamento do Azure ou do Azure Data Lake Store. Os ficheiros podem estar em qualquer formato, mas normalmente são ficheiros simples, como CSVs. 

### <a name="azure-storage"></a>Storage do Azure 

[Armazenamento do Azure](https://azure.microsoft.com/services/storage/blobs/) tem [metas de escalabilidade específico](../../storage/common/storage-scalability-targets.md).  Nós mais análise, de armazenamento do Azure dimensiona melhor ao lidar com vários ficheiros mais pequenos.  Armazenamento do Azure garante que o mesmo desempenho, independentemente do quantos ficheiros ou a forma como os ficheiros grandes (desde que respeitam os limites).  Isto significa que pode armazenar terabytes de dados e ainda obter um desempenho consistente, se estiver a utilizar um subconjunto de dados de ou para todos os dados.

Armazenamento do Azure tem vários tipos diferentes de blobs.  Um *BLOBs de acréscimo* é uma excelente opção para armazenar registos web ou dados de sensor.  

Os blobs vários podem ser distribuídos em vários servidores para aumentar horizontalmente o acesso aos mesmos, mas um blob único só pode ser servido por um único servidor. Embora blobs podem ser logicamente agrupados em contentores de BLOBs, não há nenhum implicações de criação de partições deste agrupamento.

Armazenamento do Azure tem também uma camada de API de WebHDFS para o armazenamento de Blobs.  Todos os serviços no HDInsight podem aceder a ficheiros no Blob Storage do Azure para dados de limpeza e processamento de dados, da mesma forma para como esses serviços utilizaria distribuídas ficheiros de sistema Hadoop (HDFS).

Dados é normalmente ingeridos no armazenamento do Azure com o PowerShell, o SDK de armazenamento do Azure ou AZCopy.

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

O Azure Data Lake Store (ADLS) é um repositório de hiperescala geridos, para dados de análise que são compatíveis com HDFS.  ADLS utiliza uma paradigma de conceção que é semelhante ao HDFS e oferece ilimitada escalabilidade em termos de capacidade total e o tamanho dos ficheiros individuais. ADLS é muito bom ao trabalhar com ficheiros grandes, uma vez que um ficheiro grande pode ser armazenado em vários nós.  A criação de partições de dados no ADLS procedimento é efetuada em segundo plano.  Desta forma, fornece um débito maciço para executar tarefas de análise com milhares de executores simultâneos que leem e escrevem centenas de terabytes de dados de forma eficaz.

Dados é normalmente ingeridos em ADLS através do Azure Data Factory, ADLS SDKs, serviço AdlCopy, Apache DistCp ou Apache Sqoop.  Que estes serviços para utilizar em grande medida depende de onde os dados são.  Se os dados atualmente estão a ser um cluster de Hadoop existente, poderá utilizar o Apache DistCp, AdlCopy serviço ou do Azure Data Factory.  Caso esteja no Blob Storage do Azure, poderá utilizar o SDK .NET do Azure Data Lake Store, o Azure PowerShell ou o Azure Data Factory.

ADLS também está otimizado para ingestão de eventos utilizando o Hub de eventos do Azure ou o Apache Storm.

#### <a name="considerations-for-both-storage-options"></a>Considerações para ambas as opções de armazenamento

Para carregar conjuntos de dados no intervalo com vários terabytes, latência de rede pode ser um problema de principais, especialmente se os dados for proveniente de uma localização no local.  Nestes casos, pode utilizar as opções abaixo:

* O ExpressRoute do Azure: O Azure ExpressRoute permite-lhe criar ligações privadas entre os datacenters do Azure e da sua infraestrutura no local. Estas ligações fornecem uma opção fiável para transferência de grandes quantidades de dados. Para obter mais informações, consulte [documentação do Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

* Carregar "Offline" de dados. Pode utilizar [serviço importar/exportar do Azure](../../storage/common/storage-import-export-service.md) para enviar unidades de disco rígido com os seus dados para um centro de dados do Azure. Os dados pela primeira vez são carregados para o Blobs Storage do Azure. Em seguida, pode utilizar [do Azure Data Factory](../../data-factory/v1/data-factory-azure-datalake-connector.md) ou [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) ferramenta para copiar dados de blobs de armazenamento do Azure para o Data Lake Store.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

Armazém de dados SQL do Azure é uma escolha ideal para armazenar limpa e preparados resultados para análise futuras.  O Azure HDInsight pode ser utilizado para efetuar esses serviços para o armazém de dados do Azure SQL.

O Azure SQL Data Warehouse (armazém de dados do SQL Server) é um arquivo de base de dados relacional otimizado para cargas de trabalho de análise.  Armazém de dados SQL do Azure dimensiona com base em tabelas particionadas.  Podem ser particionadas tabelas em vários nós.  Nós de armazém de dados SQL do Azure estão selecionadas no momento da criação.  Dimensionamento após o facto de, mas que é um processo de Active Directory que poderão necessitar de movimento de dados. Consulte [SQL Data Warehouse - gerir computação](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md) para obter mais informações.

### <a name="hbase"></a>HBase

Apache HBase é um arquivo de chave-valor disponível no Azure HDInsight.  O Apache HBase é uma base de dados NoSQL open source baseada no Hadoop e modelada segundo o Google BigTable. O HBase fornece acesso aleatório performant e consistência forte para grandes quantidades de dados não estruturados e semiestruturados numa base de dados sem esquemas, organizada por famílias de coluna.

Os dados são armazenados nas linhas de uma tabela e os dados de uma linha são agrupados por família de colunas. O HBase é uma base de dados sem esquema uma vez que não é preciso definir as colunas nem os tipos de dados nelas armazenados antes de os utilizar. O código open source é dimensionado linearmente para processar petabytes de dados em milhares de nós. O HBase pode depender da redundância de dados, processamento em lote e outras funcionalidades que são fornecidas por aplicações distribuídas do ecossistema do Hadoop.   

O HBase é um excelente destino para dados de sensor e de registo para análise futura.

A escalabilidade de HBase é dependente do número de nós no cluster do HDInsight.

### <a name="azure-sql-database-and-azure-database"></a>Base de dados SQL do Azure e a base de dados do Azure

Como plataforma-como-um-serviço (PAAS), o Azure oferece três diferentes bases de dados relacionais.

* [Base de dados SQL do Azure](../../sql-database/sql-database-technical-overview.md) é uma implementação do Microsoft SQL Server. Para obter mais informações sobre o desempenho, consulte [otimização de desempenho na base de dados do Azure SQL](../../sql-database/sql-database-performance-guidance.md).
* [Base de dados do Azure para MySQL](../../mysql/overview.md) é uma implementação do Oracle MySQL.
* [Base de dados do Azure para PostgreSQL](../../postgresql/quickstart-create-server-database-portal.md) é uma implementação de PostgreSQL.

Estes produtos aumentar verticalmente, o que significa que são ampliadas adicionando mais CPU e memória.  Também pode optar por utilizar discos premium com os produtos para melhorar o desempenho de e/s.

## <a name="azure-analysis-services"></a>Azure Analysis Services 

Serviços de análise do Azure (AAS) é um motor de dados analíticos utilizado no suporte de decisão e análise de negócio, fornecendo os dados analíticos para os relatórios de empresas e aplicações de cliente, tais como o Power BI, Excel, relatórios do Reporting Services e outros dados ferramentas de visualização.

Podem Dimensionar cubos do Analysis Services alterando as camadas para cada cubo individuais.  Para obter mais informações, consulte [preços do Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Extrair e carregar

Depois dos dados existem no Azure, pode utilizar vários serviços para extrair e carregue-o para outros produtos.  HDInsight suporta Sqoop e Flume. 

### <a name="sqoop"></a>Sqoop

O Apache Sqoop é uma ferramenta concebida de forma eficiente transferir dados entre origens de dados estruturados, estruturados, semi-estruturados e não estruturados. 

Sqoop utiliza MapReduce para importar e exportar os dados, para fornecer operação paralela e tolerância a falhas.

### <a name="flume"></a>Flume

Apache Flume é um serviço distribuído, fiável e disponível para recolher, Agregar de forma eficiente e mover grandes quantidades de dados de registo. Flume tem uma arquitetura simple e flexível com base nos fluxos de dados de transmissão em fluxo. Flume é robustas e com tolerância a falhas com mecanismos de fiabilidade sincronizáveis e vários mecanismos de ativação pós-falha e recuperação. Flume utiliza um modelo de dados extensível simples que permite uma aplicação análise online.

Apache Flume não pode ser utilizado com o Azure HDInsight.  Uma instalação de Hadoop no local pode utilizar Flume para enviar dados para o Blobs Storage do Azure ou o Azure Data Lake Store.  Para obter mais informações, consulte [utilizando o Apache Flume com o HDInsight](https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Transformação

Depois de dados existem na localização escolhida, terá de limpá-lo, combiná-la ou prepará-la para um padrão de utilização específicos.  Hive, Pig e Spark SQL são todas as escolhas boas para esse tipo de trabalho.  São todos suportados no HDInsight. 

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)
<!-- * [Using Apache Hive as an ETL Tool](hdinsight-using-apache-hive-as-an-etl-tool.md) -->
