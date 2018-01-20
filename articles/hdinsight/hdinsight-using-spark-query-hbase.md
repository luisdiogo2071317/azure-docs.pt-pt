---
title: Utilizar o Spark para ler e escrever dados de HBase - Azure HDInsight | Microsoft Docs
description: Utilize o conector de HBase do Spark para ler e escrever dados a partir de um cluster do Spark para um cluster HBase.
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: ccbcd1d9cb45da7076d73f71a2ed692e71816650
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="use-spark-to-read-and-write-hbase-data"></a>Utilizar o Spark para ler e escrever dados de HBase

Apache HBase, normalmente, é consultado com a API de nível baixo (análises, obtém e PUT) ou com uma sintaxe de SQL Server utilizando o Phoenix. Apache também fornece o conector HBase, Spark, que é uma prática e performant alternativa para consultar e modificar os dados armazenados pelo HBase.

## <a name="prerequisites"></a>Pré-requisitos

* Dois separam os clusters do HDInsight, HBase um e um Spark Spark 2.1 (HDInsight 3.6) instalado.
* O cluster do Spark tem de comunicar diretamente com o cluster HBase com latência mínima, pelo que a configuração recomendada é implementar ambos os clusters na mesma rede virtual. Para obter mais informações, consulte [baseado em Linux criar clusters HDInsight no portal do Azure](hdinsight-hadoop-create-linux-clusters-portal.md).
* Acesso SSH para cada cluster.
* Acesso ao armazenamento de predefinido de cada cluster.

## <a name="overall-process"></a>Processo geral

O processo de alto nível para ativar o cluster do Spark consultar o seu cluster do HDInsight é o seguinte:

1. Prepare alguns dados de exemplo no HBase.
2. Adquirir o ficheiro de hbase site.xml da sua pasta de configuração de cluster HBase (/ hbase/etc/conf).
3. Coloque uma cópia do hbase site.xml na sua pasta de configuração do Spark 2 (/ spark2/etc/conf).
4. Executar `spark-shell` referenciar o conector do HBase Spark pelo respetivo Maven coordena no `packages` opção.
5. Defina um catálogo que mapeia o esquema do Spark para HBase.
6. Interagir com os dados de HBase utilizando RDD ou DataFrame APIs.

## <a name="prepare-sample-data-in-hbase"></a>Preparar os dados de exemplo no HBase

Neste passo, pode criar e preencher uma tabela simple no HBase que, em seguida, pode consultar ao utilizar o Spark.

1. Ligar ao nó principal do cluster do HBase através de SSH. Para obter mais informações, consulte [ligar ao HDInsight utilizando SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
2. Execute a shell de HBase:

        hbase shell

3. Criar um `Contacts` tabela com as famílias de coluna `Personal` e `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Carregar algumas linhas de amostra de dados:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

## <a name="acquire-hbase-sitexml-from-your-hbase-cluster"></a>Adquirir hbase-site.xml do seu cluster HBase

1. Ligar ao nó principal do cluster do HBase através de SSH.
2. Copie o hbase-site.xml do armazenamento local para a raiz de armazenamento de predefinido do seu cluster HBase:

        hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml /

3. Navegue para o cluster de HBase utilizando o [portal do Azure](https://portal.azure.com).
4. Selecione as contas de armazenamento. 

    ![Contas de armazenamento](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

5. Selecione a conta de armazenamento na lista que tem uma marca de verificação sob a coluna predefinida.

    ![Conta do storage predefinida](./media/hdinsight-using-spark-query-hbase/default-storage.png)

6. No painel de conta de armazenamento, selecione o mosaico de Blobs.

    ![Mosaico de BLOBs](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

7. Na lista de contentores, selecione o contentor que é utilizado pelo HBase cluster.
8. Na lista de ficheiros, selecione `hbase-site.xml`.

    ![HBase-site.xml](./media/hdinsight-using-spark-query-hbase/hbase-site-xml.png)

9. No painel de propriedades Blob, selecione transfira e guarde `hbase-site.xml` para uma localização no seu computador local.

    ![Transferência](./media/hdinsight-using-spark-query-hbase/download.png)

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Colocar hbase site.xml no seu cluster do Spark

1. Navegue para o cluster do Spark utilizando o [portal do Azure](https://portal.azure.com).
2. Selecione as contas de armazenamento.

    ![Contas de armazenamento](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

3. Selecione a conta de armazenamento na lista que tem uma marca de verificação sob a coluna predefinida.

    ![Conta do storage predefinida](./media/hdinsight-using-spark-query-hbase/default-storage.png)

4. No painel de conta de armazenamento, selecione o mosaico de Blobs.

    ![Mosaico de BLOBs](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

5. Na lista de contentores, selecione o contentor que é utilizado pelo cluster do Spark.
6. Selecione o carregamento.

    ![Carregar](./media/hdinsight-using-spark-query-hbase/upload.png)

7. Selecione o `hbase-site.xml` ficheiros anteriormente transferidos para o computador local.

    ![Carregar hbase site.xml](./media/hdinsight-using-spark-query-hbase/upload-selection.png)

8. Selecione o carregamento.
9. Ligar ao nó principal do cluster do Spark através de SSH.
10. Cópia `hbase-site.xml` do armazenamento de predefinido do cluster do Spark para a pasta de configuração de 2 do Spark no armazenamento local do cluster:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Execute a Shell de Spark referenciar o conector de HBase do Spark

1. Ligar ao nó principal do cluster do Spark através de SSH.
2. Inicie a shell do spark, especificando o pacote do conector do Spark HBase:

        spark-shell --packages com.hortonworks:shc-core:1.1.0-2.1-s_2.11

3. Mantenha esta instância de Spark Shell aberta e avance para o passo seguinte.

## <a name="define-a-catalog-and-query"></a>Definir uma consulta e de catálogo

Neste passo, é possível definir um objeto de catálogo que mapeia o esquema do Spark para HBase. 

1. Na sua Shell Spark aberta, execute o seguinte `import` instruções:

        import org.apache.spark.sql.{SQLContext, _}
        import org.apache.spark.sql.execution.datasources.hbase._
        import org.apache.spark.{SparkConf, SparkContext}
        import spark.sqlContext.implicits._

2. Defina um catálogo para a tabela de contactos que criou no HBase:
    1. Definir um esquema de catálogo para a tabela HBase `Contacts`.
    2. Identificar rowkey como `key`e mapear os nomes de coluna utilizados no Spark para a família de colunas, o nome da coluna e o tipo de coluna como utilizado no HBase.
    3. O rowkey também tem de ser definida em detalhe como uma coluna com nome (`rowkey`), que tem uma família de coluna específico `cf` de `rowkey`.

            def catalog = s"""{
                |"table":{"namespace":"default", "name":"Contacts"},
                |"rowkey":"key",
                |"columns":{
                |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
                |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
                |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
                |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
                |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
                |}
            |}""".stripMargin

3. Definir um método que fornece um DataFrame em torno do seu `Contacts` tabela HBase:

            def withCatalog(cat: String): DataFrame = {
                spark.sqlContext
                .read
                .options(Map(HBaseTableCatalog.tableCatalog->cat))
                .format("org.apache.spark.sql.execution.datasources.hbase")
                .load()
            }

4. Crie uma instância do DataFrame:

        val df = withCatalog(catalog)

5. Consulta o DataFrame:

        df.show()

6. Deve ver duas linhas de dados:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Registe uma tabela temporária que possa consultar a tabela HBase com o Spark SQL:

        df.registerTempTable("contacts")

8. Emita uma consulta de SQL relativamente a `contacts` tabela:

        val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
        query.show()

9. Deverá ver resultados nestes:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Inserir novos dados

1. A inserir um novo registo de contacto, definir uma `ContactRecord` classe:

        case class ContactRecord(
            rowkey: String,
            officeAddress: String,
            officePhone: String,
            personalName: String,
            personalPhone: String
            )

2. Criar uma instância do `ContactRecord` e colocá-la numa matriz:

        val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

        var newData = new Array[ContactRecord](1)
        newData(0) = newContact

3. Guarde a matriz de novos dados no HBase:

        sc.parallelize(newData).toDF.write
        .options(Map(HBaseTableCatalog.tableCatalog -> catalog))
        .format("org.apache.spark.sql.execution.datasources.hbase").save()

4. Examine os resultados:
    
        df.show()

5. Deverá ver um resultado como este:

        +------+--------------------+--------------+------------+--------------+
        |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
        +------+--------------------+--------------+------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
        | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
        |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+------------+--------------+

## <a name="next-steps"></a>Passos Seguintes

* [Conector do Spark HBase](https://github.com/hortonworks-spark/shc)
