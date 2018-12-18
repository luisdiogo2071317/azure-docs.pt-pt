---
title: 'Tutorial: Aceder a dados de Pré-visualização de Armazenamento do Azure Data Lake Ger2 com o Azure Databricks através do Spark | Microsoft Docs'
description: Este tutorial mostra como executar consultas de Spark num cluster do Azure Databricks para aceder aos dados numa conta de armazenamento do Azure Data Lake Storage Gen2.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: dineshm
ms.openlocfilehash: b0382d31f9d16228ca3447ace9c7d4f171b206f6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548991"
---
# <a name="tutorial-access-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Aceder a dados de pré-visualização de geração 2 de armazenamento do Data Lake com o Azure Databricks com o Spark

Este tutorial mostra-lhe como ligar o seu cluster do Azure Databricks para dados armazenados numa conta de armazenamento do Azure que tenha de geração 2 de armazenamento do Azure Data Lake (pré-visualização) ativada. Esta ligação permite-lhe executar nativamente consultas e análises do seu cluster nos seus dados.

Neste tutorial, irá:

> [!div class="checklist"]
> * Criar um cluster do Databricks
> * Ingerir dados não estruturados numa conta de armazenamento
> * Executar a análise nos seus dados no armazenamento de Blobs

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial demonstra como consumir e consultar dados de voos, disponíveis a partir do [Departamento de Transportes dos Estados Unidos](https://transtats.bts.gov/DL_SelectFields.asp). 

1. Selecione o **Prezipped ficheiro** caixa de verificação para selecionar todos os campos de dados.
2. Selecione **transferir** e guardar os resultados para o seu computador.
3. Tornar uma nota do nome do ficheiro e o caminho do download; precisa estas informações num passo posterior.

Para concluir este tutorial, precisa de uma conta de armazenamento com as capacidades de análise. É recomendável concluir nossa [guia de introdução](data-lake-storage-quickstart-create-account.md) sobre o assunto para criar uma. Depois de criá-la, navegue para a conta de armazenamento para obter as definições de configuração.

1. Sob **configurações**, selecione **chaves de acesso**.
2. Selecione o **cópia** junto a **chave1** para copiar o valor da chave.

O nome e a chave da conta são necessários para passos posteriores neste tutorial. Abra um editor de texto e utilize o nome e a chave da conta para referência futura.

## <a name="create-a-databricks-cluster"></a>Criar um cluster do Databricks

A próxima etapa é criar um cluster do Databricks para criar uma área de trabalho de dados.

1. Partir do [portal do Azure](https://portal.azure.com), selecione **criar um recurso**.
2. Introduza **Azure Databricks** no campo de pesquisa.
3. Selecione **criar** no painel do Azure Databricks.
4. Nomeie o seu serviço de Databricks **myFlightDataService** (Verifique a *afixar ao dashboard* caixa de verificação à medida que criar o serviço).
5. Selecione **iniciar área de trabalho** para abrir a área de trabalho numa nova janela do browser.
6. Selecione **Clusters** na barra de navegação esquerdo.
7. Selecione **criar Cluster**.
8. Introduza **myFlightDataCluster** no campo **Nome do cluster**.
9. Selecione **Standard_D8s_v3** no campo **Tipo de Trabalho**.
10. Altere o valor **Mínimo de Trabalhos** para **4**.
11. Selecione **criar clusters** na parte superior da página. (Este processo pode demorar até 5 minutos a concluir.)
12. Quando o processo estar concluído, selecione **Azure Databricks** na parte superior esquerda da barra de navegação.
13. Selecione **Bloco de Notas** na secção **Novo** na parte inferior da página.
14. Introduza um nome à sua escolha no **Name** campo e selecione **Python** como a linguagem.
15. Todos os outros campos podem ser deixados como valores predefinidos.
16. Selecione **Criar**.
17. Cole o código seguinte para o **Cmd 1** célula. Substitua os marcadores de posição mostrados entre parênteses Retos, exemplo pelos seus próprios valores:

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
18. Prima **SHIFT + ENTER** para executar a célula do código.

## <a name="ingest-data"></a>Ingerir dados

### <a name="copy-source-data-into-the-storage-account"></a>Copiar dados de origem para a conta de armazenamento

A tarefa seguinte consiste em utilizar o AzCopy para copiar dados do ficheiro *.csv* para o armazenamento do Azure. Abra uma janela da linha de comandos e introduza os comandos seguintes. Certifique-se de que substitua os marcadores de posição `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>`, e `<ACCOUNT_KEY>` com os valores correspondentes que reserve num passo anterior.

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
6. Cole o código seguinte para o **Cmd 1** célula. (Esse código automático-poupa no editor.)

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
    print("Done")
    ```

## <a name="explore-data"></a>Explorar dados

Regresse à área de trabalho do Databricks e selecione o **recente** ícone na barra de navegação à esquerda.

1. Selecione o **Flight Data Analytics** bloco de notas.
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

### <a name="run-simple-queries"></a>Executar consultas simples

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

### <a name="run-complex-queries"></a>Executar consultas complexas

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

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não forem necessários, elimine o grupo de recursos e todos os recursos relacionados. Para tal, selecione o grupo de recursos para a conta de armazenamento e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

[!div class="nextstepaction"] 
> [Extrair, transformar e carregar dados com o Apache Hive no Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)

