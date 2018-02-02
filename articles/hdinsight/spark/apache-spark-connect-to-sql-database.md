---
title: "Utilizar o Apache Spark para ler e escrever dados à base de dados SQL do Azure | Microsoft Docs"
description: "Saiba como configurar uma ligação entre o cluster do HDInsight Spark e uma base de dados SQL do Azure para ler os dados, escrever dados e fluxo de dados numa base de dados SQL"
services: hdinsight
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/29/2018
ms.author: nitinme
ms.openlocfilehash: 28ed6b9774bb85c7ec806c7775c34f8bc3d66bde
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>Utilize o cluster do HDInsight Spark para ler e escrever dados à base de dados SQL do Azure

Saiba como ligar um cluster do Apache Spark no Azure HDInsight com uma base de dados SQL do Azure e, em seguida, ler, escrever e transmitir dados para a base de dados do SQL Server. As instruções neste artigo utilizam um bloco de notas do Jupyter para executar os fragmentos de código Scala. No entanto, pode criar uma aplicação autónoma no Scala ou Python e executar as mesmas tarefas. 

## <a name="prerequisites"></a>Pré-requisitos

* **Cluster do HDInsight Spark do Azure**.  Siga as instruções apresentadas em [criar um cluster do Apache Spark no HDInsight](apache-spark-jupyter-spark-sql.md).

* **Base de dados SQL do Azure**. Siga as instruções apresentadas em [criar uma base de dados SQL do Azure](../../sql-database/sql-database-get-started-portal.md). Certifique-se de que cria uma base de dados com o exemplo **AdventureWorksLT** esquema e de dados. Além disso, certifique-se que criar uma regra de firewall ao nível do servidor para permitir que o endereço IP do cliente aceder à base de dados do SQL Server no servidor. As instruções para adicionar a regra de firewall está disponível no mesmo artigo. Uma vez que criou a base de dados SQL do Azure, certifique-se de que mantém os seguintes valores à mão. É necessário para ligar à base de dados a partir de um cluster do Spark.

    * Nome do servidor que aloja a base de dados SQL do Azure
    * Nome de base de dados SQL do Azure
    * Nome do utilizador de administrador de base de dados do Azure SQL / palavra-passe

* **SQL Server Management Studio**. Siga as instruções apresentadas em [SSMS de utilização para ligar e consultar dados](../../sql-database/sql-database-connect-query-ssms.md).

## <a name="create-a-jupyter-notebook"></a>Criar um bloco de notas do Jupyter

Comece por criar um bloco de notas do Jupyter associado ao cluster do Spark. Utilize este bloco de notas para executar os fragmentos de código utilizados neste artigo. 

1. Do [portal do Azure](https://portal.azure.com/), abra o seu cluster. 

2. Do **ligações rápidas** secção, clique em **Cluster dashboards** para abrir o **Cluster dashboards** vista.  Se não vir **ligações rápidas**, clique em **descrição geral** no menu no painel esquerdo.

    ![Dashboard de cluster no Spark](./media/apache-spark-connect-to-sql-database/hdinsight-cluster-dashboard-on-spark.png "Cluster dashboard Spark") 

3. Clique em **bloco de notas do Jupyter**. Se lhe for solicitado, introduza as credenciais de administrador do cluster.

    ![Bloco de notas do Jupyter no Spark](./media/apache-spark-connect-to-sql-database/hdinsight-jupyter-notebook-on-spark.png "bloco de notas do Jupyter no Spark")
   
   > [!NOTE]
   > Também pode aceder o bloco de notas do Jupyter no cluster do Spark, abrindo o seguinte URL no browser. Substitua **CLUSTERNAME** pelo nome do cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

4. No bloco de notas do Jupyter, no canto superior direito, clique em **novo**e, em seguida, clique em **Spark** para criar um bloco de notas Scala. Blocos de notas do Jupyter no cluster do HDInsight Spark fornecem também a **PySpark** kernel para aplicações de Python2 e o **PySpark3** kernel para Python3 aplicações. Para este artigo, criamos um bloco de notas Scala.
   
    ![Kernels para bloco de notas do Jupyter no Spark](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "Kernels para bloco de notas do Jupyter no Spark")

    Para obter mais informações sobre os kernels, veja [Utilizar kernels de blocos de notas do Jupyter com clusters do Apache Spark no HDInsight](apache-spark-jupyter-notebook-kernels.md).

   > [!NOTE]
   > Neste artigo, vamos utilizar um kernel do Spark (Scala) porque os dados de transmissão em fluxo do Spark na base de dados do SQL Server só são suportados no Scala e Java atualmente. Apesar de leitura a partir do e respetiva escrita no SQL Server podem ser realizadas com o Python, por razões de consistência neste artigo, utilizamos Scala para todas as operações de três.
   >

5. Esta ação abre um novo bloco de notas com um nome predefinido, **Untitled**. Clique no nome do bloco de notas e introduza um nome à sua escolha.

    ![Forneça um nome para o bloco de notas](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Forneça um nome para o bloco de notas")

Agora pode começar a criar a sua aplicação.
    
## <a name="read-data-from-azure-sql-database"></a>Ler dados a partir da base de dados SQL do Azure

Nesta secção, é possível ler dados de uma tabela (por exemplo, **SalesLT.Address**) que existe na base de dados AdventureWorks.

1. Uma novo de notas do Jupyter, numa célula código, cole o seguinte fragmento e substitua os valores de marcador de posição com os valores para a base de dados SQL do Azure.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Prima **SHIFT + ENTER** para executar a célula do código.  

2. O fragmento seguinte cria um URL de JDBC que pode passar para o dataframe Spark APIs cria um `Properties` objeto para reter os parâmetros. Cole o fragmento de código célula e prima **SHIFT + ENTER** para ser executada.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

3. O fragmento seguinte cria um dataframe com os dados de uma tabela na base de dados SQL do Azure. Este fragmento, utilizamos uma **SalesLT.Address** tabela que está disponível como parte do **AdventureWorksLT** base de dados. Cole o fragmento de código célula e prima **SHIFT + ENTER** para ser executada.

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

4. Agora pode efetuar operações em dataframe, tais como obter o esquema de dados:

       sqlTableDF.printSchema
   
    Ver um resultado semelhante ao seguinte:

    ![Forneça um nome para o bloco de notas](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "Forneça um nome para o bloco de notas")

5. Também pode efetuar operações como obter as primeiras 10 linhas.

       sqlTableDF.show(10)

6. Em alternativa, obter colunas específicas do conjunto de dados.

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>Escrever dados na base de dados SQL do Azure

Nesta secção, utilizamos um ficheiro CSV de exemplo disponível no cluster para criar uma tabela na base de dados SQL do Azure e preenchê-lo com dados. O ficheiro CSV de exemplo (**HVAC.csv**) está disponível em todos os clusters do HDInsight em `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`.

1. Uma novo de notas do Jupyter, numa célula código, cole o seguinte fragmento e substitua os valores de marcador de posição com os valores para a base de dados SQL do Azure.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Prima **SHIFT + ENTER** para executar a célula do código.  

2. O fragmento seguinte cria um URL de JDBC que pode passar para o dataframe Spark APIs cria um `Properties` objeto para reter os parâmetros. Cole o fragmento de código célula e prima **SHIFT + ENTER** para ser executada.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

3. O fragmento seguinte extrai o esquema dos dados em HVAC.csv e utiliza o esquema ao carregar os dados de CSV num dataframe `readDf`. Cole o fragmento de código célula e prima **SHIFT + ENTER** para ser executada.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

4. Utilize o `readDf` dataframe para criar uma tabela temporária, `temphvactable`. Em seguida, utilize a tabela temporária para criar uma tabela do hive, `hvactable_hive`.

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

5. Por fim, utilize a tabela de hive para criar uma tabela na base de dados SQL do Azure. O fragmento seguinte cria `hvactable` na base de dados SQL do Azure.

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

6. Ligar à base de dados SQL do Azure com o SSMS e certifique-se de que vê um `dbo.hvactable` não existe.

    a. Iniciar o SSMS e ligar à base de dados SQL do Azure, fornecendo detalhes de ligação, conforme mostrado na captura de ecrã abaixo.

    ![Ligar à base de dados SQL com o SSMS](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "ligar à base de dados SQL com o SSMS")

    b. A partir de Object Explorer, expanda a base de dados SQL do Azure e o nó de tabela para ver o **dbo.hvactable** criado.

    ![Ligar à base de dados SQL com o SSMS](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "ligar à base de dados SQL com o SSMS")

## <a name="stream-data-into-azure-sql-database"></a>Dados de sequência na base de dados SQL do Azure

Nesta secção, iremos transmitir dados para o **hvactable** que já criados na base de dados SQL do Azure na secção anterior.

1. Como primeiro passo, certifique-se de que existem não existem registos no **hvactable**. Com o SSMS, execute a consulta seguinte na tabela.

       DELETE FROM [dbo].[hvactable]

2. Crie um novo bloco de notas do Jupyter no cluster do HDInsight Spark. Numa célula código, cole o fragmento seguinte e, em seguida, prima **SHIFT + ENTER**:

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

3. Podemos transmitir dados a partir de **HVAC.csv** para o hvactable. Ficheiro HVAC.csv está disponível no cluster na */HdiSamples/HdiSamples/SensorSampleData/AVAC/*. No seguinte fragmento, vamos obter primeiro o esquema dos dados para transmissão em fluxo. Em seguida, criamos um dataframe de transmissão em fluxo utilizando o esquema dessa. Cole o fragmento de código célula e prima **SHIFT + ENTER** para ser executada.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema1).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

4. O resultado mostra o esquema de **HVAC.csv**. O **hvactable** tem o mesmo esquema. A saída apresenta uma lista de colunas na tabela.

    ![Esquema de tabela](./media/apache-spark-connect-to-sql-database/schema-of-table.png "esquema de tabela")

5. Por fim, utilize o seguinte fragmento para ler dados a partir de HVAC.csv e transmitir em fluxo para o **hvactable** na base de dados SQL do Azure. Cole o fragmento numa célula de código, substitua os valores de marcador de posição com os valores para a base de dados SQL do Azure e, em seguida, prima **SHIFT + ENTER** para ser executada.

       val WriteToSQLQuery  = readStreamDf.writeStream.foreach(new ForeachWriter[Row] {
          var connection:java.sql.Connection = _
          var statement:java.sql.Statement = _
          
          val jdbcUsername = "<SQL DB ADMIN USER>"
          val jdbcPassword = "<SQL DB ADMIN PWD>"
          val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
          val jdbcPort = 1433
          val jdbcDatabase ="<AZURE SQL DB NAME>"
          val driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
          val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
  
         def open(partitionId: Long, version: Long):Boolean = {
           Class.forName(driver)
           connection = DriverManager.getConnection(jdbc_url, jdbcUsername, jdbcPassword)
           statement = connection.createStatement
           true
         }
  
         def process(value: Row): Unit = {
           val Date  = value(0)
           val Time = value(1)
           val TargetTemp = value(2)
           val ActualTemp = value(3)
           val System = value(4)
           val SystemAge = value(5)
           val BuildingID = value(6)  
    
           val valueStr = "'" + Date + "'," + "'" + Time + "'," + "'" + TargetTemp + "'," + "'" + ActualTemp + "'," + "'" + System + "'," + "'" + SystemAge + "'," + "'" + BuildingID + "'"
           statement.execute("INSERT INTO " + "dbo.hvactable" + " VALUES (" + valueStr + ")")   
           }

         def close(errorOrNull: Throwable): Unit = {
            connection.close
          }
         })
        
         var streamingQuery = WriteToSQLQuery.start()

6. Certifique-se de que os dados estão a ser transmissão em fluxo para o **hvactable** executando a seguinte consulta. Everytime executar a consulta, mostra o número de linhas no aumento de tabela.

        SELECT COUNT(*) FROM hvactable

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar um cluster do Spark do HDInsight para analisar dados no Data Lake Store](apache-spark-use-with-data-lake-store.md)
* [Processar eventos de transmissão em fluxo estruturados utilizando EventHub](apache-spark-eventhub-structured-streaming.md)
* [Utilizar o Spark estruturado de transmissão em fluxo com Kafka no HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
