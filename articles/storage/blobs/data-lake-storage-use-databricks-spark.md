---
title: Aceder a dados de Pré-visualização de Armazenamento do Azure Data Lake Ger2 com o Azure Databricks através do Spark | Microsoft Docs
description: Saiba como executar consultas de Spark num cluster do Databricks para aceder aos dados numa conta de armazenamento do Azure Data Lake Ger2.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: dineshm
ms.openlocfilehash: 88a05eb8fa59740012ca6c7a8d8508d565854dc7
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975739"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Aceder aos dados de pré-visualização do Armazenamento do Azure Data Lake Ger2 com Azure Databricks através do Spark

Neste tutorial, vai aprender a executar consultas de Spark num cluster do Azure Databricks para consultar dados numa conta de armazenamento do Azure com a Pré-visualização do Azure Data Lake Storage Gen2 ativado.

> [!div class="checklist"]
> * Criar um cluster do Databricks
> * Ingerir dados não estruturados numa conta de armazenamento
> * Executar a análise nos seus dados no armazenamento de Blobs

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial demonstra como consumir e consultar dados de voos, disponíveis a partir do [Departamento de Transportes dos Estados Unidos](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time). Transfira, pelo menos, dois anos de dados de aviação (selecione todos os campos) e guarde o resultado no seu computador. Certifique-se de que anota o nome do ficheiro e o caminho da transferência; precisará destas informações num passo posterior.

> [!NOTE]
> Clique na caixa de verificação **Ficheiro Pré-zipado** para selecionar todos os campos de dados. A transferência terá muitos gigabytes, mas esta quantidade de dados é necessária para a análise.

## <a name="create-an-azure-storage-account-with-analytic-capabilities"></a>Criar uma conta de armazenamento do Azure com as capacidades de análise

Para começar, crie uma nova [conta de armazenamento com as capacidades de análise](data-lake-storage-quickstart-create-account.md) e dê-lhe um nome exclusivo. Em seguida, navegue para a conta de armazenamento para obter as definições de configuração.

1. Em **Definições**, clique em **Chaves de acesso**.
2. Clique no botão **Copiar** junto a **key1** para copiar o valor da chave.

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
14. Cole o código seguinte para o **Cmd 1** célula. Não se esqueça de substituir os marcadores de posição mostrados entre parênteses Retos, exemplo pelos seus próprios valores:

    ```scala
    %python%
    configs = {"fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "<service-client-id>",
        "fs.azure.account.oauth2.client.secret": "<service-credentials>",
        "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
        
    dbutils.fs.mount(
        source = "abfss://dbricks@<account-name>.dfs.core.windows.net/folder1",
        mount_point = "/mnt/flightdata",
        extra_configs = configs)
    ```
15. Prima **SHIFT + ENTER** para executar a célula do código.

## <a name="ingest-data"></a>Ingerir dados

### <a name="copy-source-data-into-the-storage-account"></a>Copiar dados de origem para a conta de armazenamento

A tarefa seguinte consiste em utilizar o AzCopy para copiar dados do ficheiro *.csv* para o armazenamento do Azure. Abra uma janela da linha de comandos e introduza os comandos seguintes. Certifique-se de que substitui os marcadores de posição `<DOWNLOAD_FILE_PATH>` e `<ACCOUNT_KEY>` pelos valores correspondentes utilizados num passo anterior.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Utilize o Databricks Notebook para converter CSV em Parquet

Volte a abrir o Databricks no seu browser e realize os passos seguintes:

1. Selecione **Azure Databricks** na parte superior esquerda da barra de navegação.
2. Selecione **Bloco de Notas** na secção **Novo** na parte inferior da página.
3. Introduza **CSV2Parquet** no campo **Nome**.
4. Todos os outros campos podem ser deixados como valores predefinidos.
5. Selecione **Criar**.
6. Cole o código seguinte na célula **Cmd 1** (este código é guardado automaticamente no editor).

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
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
display(dbutils.fs.ls("/mnt/flightdata/temp/"))
```

Para criar um novo ficheiro e listar os ficheiros na pasta *parquet/flights*, execute este script:

```python
dbutils.fs.put("/mnt/flightdata/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/temp/parquet/flights")
```

Com estes exemplos de código, explorou a natureza hierárquica do HDFS através dos dados armazenados numa conta de armazenamento com o Data Lake Storage Gen2 ativado.

## <a name="query-the-data"></a>Consultar os dados

Em seguida, pode começar a consultar os dados que carregou para a sua conta de armazenamento. Introduza cada um dos seguintes blocos de código em **Cmd 1** e prima **Cmd + Enter** para executar o script de Python.

### <a name="simple-queries"></a>Consultas simples

Para criar pacotes de dados para as origens de dados, execute o seguinte script:

> [!IMPORTANT]
> Certifique-se de que substitui o marcador de posição **<NOME_DO_FICHEIRO_CSV>** pelo nome do ficheiro que transferiu no início deste tutorial.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

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
#preferably run this in a separate cmd cell
display(flightDF)
```

Para executar consultas de análise relativamente aos dados, execute o seguinte script:

```python
#Run each of these queries, preferably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
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

## <a name="next-steps"></a>Passos Seguintes

* [Extrair, transformar e carregar dados com o Apache Hive no Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)
