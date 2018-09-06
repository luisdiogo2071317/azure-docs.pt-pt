---
title: Aceder a dados de Pré-visualização de Armazenamento do Azure Data Lake Ger2 com o Azure Databricks através do Spark | Microsoft Docs
description: Saiba como executar consultas de Spark num cluster do Databricks para aceder aos dados numa conta de armazenamento do Azure Data Lake Ger2.
services: hdinsight,storage
author: dineshm
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 6/27/2018
ms.author: dineshm
ms.openlocfilehash: 7d951a959da28187a5971ee218f2bd921d331727
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301803"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Aceder aos dados de pré-visualização do Armazenamento do Azure Data Lake Ger2 com Azure Databricks através do Spark

Neste tutorial, vai aprender a executar consultas de Spark num cluster do Azure Databricks para consultar dados numa conta com capacidade de pré-visualização do armazenamento do Azure Data Lake Ger2.

> [!div class="checklist"]
> * Criar um cluster do Databricks
> * Ingerir dados não estruturados numa conta de armazenamento
> * Acionar uma Função do Azure para processar dados
> * Executar a análise nos seus dados no armazenamento de Blobs

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial demonstra como consumir e consultar dados de voos, disponíveis a partir do [Departamento de Transportes dos Estados Unidos](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time). Transfira, pelo menos, dois anos de dados de aviação (selecione todos os campos) e guarde o resultado no seu computador. Certifique-se de que anota o nome do ficheiro e o caminho da transferência; precisará destas informações num passo posterior.

> [!NOTE]
> Clique na caixa de verificação **Ficheiro Pré-zipado** para selecionar todos os campos de dados. A transferência terá muitos gigabytes, mas esta quantidade de dados é necessária para a análise.

## <a name="create-an-azure-data-lake-storage-gen2-account"></a>Criar uma conta de Armazenamento do Azure Data Lake Ger2

Para começar, crie uma nova [conta de Armazenamento do Azure Data Lake Ger2](quickstart-create-account.md) e atribua-lhe um nome exclusivo. Em seguida, navegue para a conta de armazenamento para obter as definições de configuração.

> [!IMPORTANT]
> Durante a Pré-visualização, as Funções do Azure funcionam apenas com contas de Armazenamento do Azure Data Lake Ger2 criadas com um espaço de nomes simples.

1. Em **Definições**, clique em **Chaves de acesso**.
3. Clique no botão **Copiar** junto a **key1** para copiar o valor da chave.

O nome e a chave da conta são necessários para passos posteriores neste tutorial. Abra um editor de texto e utilize o nome e a chave da conta para referência futura.

## <a name="create-a-databricks-cluster"></a>Criar um cluster do Databricks

O passo seguinte consiste em criar um [Cluster do Databricks](https://docs.azuredatabricks.net/) para criar uma área de trabalho de dados.

1. Crie um [Serviço do Databricks](https://ms.portal.azure.com/#create/Microsoft.Databricks) e atribua-lhe o nome **myFlightDataService** (certifique-se de que seleciona a caixa de verificação *Afixar ao dashboard* quando criar o serviço).
2. Clique em **Iniciar Área de Trabalho** para abrir a área de trabalho numa nova janela do browser.
3. Clique em **Clusters** na barra de navegação esquerda.
4. Clique em **Criar Cluster**.
5. Introduza **myFlightDataCluster** no campo *Nome do cluster*.
6. Selecione **Standard_D8s_v3** no campo *Tipo de Trabalho*.
7. Altere o valor **Mínimo de Trabalhos** para *4*.
8. Clique em **Criar Cluster** na parte superior da página (este processo pode demorar até 5 minutos a concluir).
9. Quando o processo for concluído, selecione **Azure Databricks** na parte superior esquerda da barra de navegação.
10. Selecione **Bloco de Notas** na secção **Novo** na parte inferior da página.
11. Introduza um nome à sua escolha no campo **Nome** e selecione **Python** como linguagem.
12. Todos os outros campos podem ser deixados como valores predefinidos.
13. Selecione **Criar**.
14. Cole o código seguinte na célula **Cmd 1** e substitua os valores pelos valores preservados da sua conta de armazenamento.

    ```bash
    spark.conf.set("fs.azure.account.key.<account_name>.dfs.core.windows.net", "<account_key>") 
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
    dbutils.fs.ls("abfss://<file_system>@<account_name>.dfs.core.windows.net/")
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
    ```

## <a name="ingest-data"></a>Ingerir dados

### <a name="copy-source-data-into-the-storage-account"></a>Copiar dados de origem para a conta de armazenamento

A tarefa seguinte consiste em utilizar o AzCopy para copiar dados do ficheiro *.csv* para o armazenamento do Azure. Abra uma janela da linha de comandos e introduza os comandos seguintes. Certifique-se de que substitui os marcadores de posição `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>` e `<ACCOUNT_KEY>` pelos valores correspondentes utilizados num passo anterior.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Utilize o Databricks Notebook para converter CSV em Parquet

Volte a abrir o Databricks no seu browser e execute os passos seguintes:

1. Selecione **Azure Databricks** na parte superior esquerda da barra de navegação.
2. Selecione **Bloco de Notas** na secção **Novo** na parte inferior da página.
3. Introduza **CSV2Parquet** no campo **Nome**.
4. Todos os outros campos podem ser deixados como valores predefinidos.
5. Selecione **Criar**.
6. Cole o código seguinte na célula **Cmd 1** (este código é guardado automaticamente no editor).

    ```python
    #mount Azure Blob Storage as an HDFS file system to your databricks cluster
    #you need to specify a storage account and container to connect to. 
    #use a SAS token or an account key to connect to Blob Storage.  
    accountname = "<insert account name>"
    accountkey = " <insert account key>"
    fullname = "fs.azure.account.key." +accountname+ ".dfs.core.windows.net"
    accountsource = "abfs://dbricks@" +accountname+ ".dfs.core.windows.net/folder1"
    #create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/On_Time_On_Time*.csv")
    #read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet(accountsource + '/parquet/flights')

    #read the flight details parquet file 
    #flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")
    print("Done")
    ```

## <a name="explore-data-using-hadoop-distributed-file-system"></a>Explorar dados com o Sistema de Ficheiros Distribuído do Hadoop

Regresse à área de trabalho do Databricks e clique no ícone **Recente** na barra de navegação esquerda.

1. Clique no bloco de notas **Flight Data Analytics**.
2. Prima **Ctrl + Alt + N** para criar uma célula nova.

Introduza cada um dos seguintes blocos de código em **Cmd 1** e prima **Cmd + Enter** para executar o script de Python.

Para obter uma lista dos ficheiros CSV carregados através do AzCopy, execute o seguinte script:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"
dbutils.fs.ls(source + "/temp")
display(dbutils.fs.ls(source + "/temp/"))
```

Para criar um novo ficheiro e listar os ficheiros na pasta *parquet/flights*, execute este script:

```python
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"

dbutils.fs.help()

dbutils.fs.put(source + "/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls(source + "/temp/parquet/flights")
```
Com estes exemplos de código, explorou a natureza hierárquica do HDFS através dos dados armazenados numa conta com capacidade de Armazenamento do Azure Data Lake Ger2.

## <a name="query-the-data"></a>Consultar os dados

Em seguida, pode começar a consultar os dados que carregou para o Armazenamento do Azure Data Lake. Introduza cada um dos seguintes blocos de código em **Cmd 1** e prima **Cmd + Enter** para executar o script de Python.

### <a name="simple-queries"></a>Consultas simples

Para criar pacotes de dados para as origens de dados, execute o seguinte script:

> [!IMPORTANT]
> Certifique-se de que substitui o marcador de posição **<NOME_DO_FICHEIRO_CSV>** pelo nome do ficheiro que transferiu no início deste tutorial.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet(accountsource + '/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created via the Azure Function
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")

#print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

#print the flight database size
print("Number of flights in the database: ", flightDF.count())

#show the first 20 rows (20 is the default)
#to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

#Display to run visualizations
#preferrably run this in a separate cmd cell
display(flightDF)
```

Para executar consultas de análise relativamente aos dados, execute o seguinte script:

```python
#Run each of these queries, preferrably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet(accountsource + '/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet(accountsource + '/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

#using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights=out2.count()
print("Jan 2016: ", NumJan2016Flights," Feb 2016: ",NumFeb2016Flights)
Total= NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql("SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'") 
print('Airports in Texas: ', out.show(100))

#find all airlines that fly from Texas
out1 = spark.sql("SELECT distinct(Carrier) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```
### <a name="complex-queries"></a>Consultas complexas

Para executar as consultas mais complexas seguintes, execute cada segmento de cada vez no bloco de notas e inspecione os resultados.

```python
#find the airline with the most flights

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")

#show the top row without truncation
output.show(1, False)

#show the top 10 airlines
output.show(10, False)

#Determine which is the least on time airline

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE DepDelay>60 or ArrDelay>60 group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("select * from v")
#output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")
#show the top row without truncation
output.show(1, False)

#which airline improved its performance
#find the airline with the most improvement in delays
#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v1")
spark.sql("DROP VIEW IF EXISTS v2")
spark.sql("CREATE TEMPORARY VIEW v1 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2016 group by Carrier order by NumFlights desc LIMIT 10")
spark.sql("CREATE TEMPORARY VIEW v2 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2017 group by Carrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT distinct ac.AirlineName, v1.Carrier, v1.NumFlights, v2.NumFlights from v1 INNER JOIN v2 ON v1.Carrier = v2.Carrier INNER JOIN AirlineCodes ac ON v2.Carrier = ac.AirlineCode WHERE v1.NumFlights > v2.NumFlights")
#show the top row without truncation
output.show(10, False)

#display for visual analysis
display(output)
```

## <a name="next-steps"></a>Passos seguintes

* [Extrair, transformar e carregar dados com o Apache Hive no Azure HDInsight](tutorial-extract-transform-load-hive.md)
