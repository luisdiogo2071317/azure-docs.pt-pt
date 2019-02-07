---
title: 'Tutorial: Aceder a dados de Pré-visualização de Armazenamento do Azure Data Lake Ger2 com o Azure Databricks através do Spark | Microsoft Docs'
description: Este tutorial mostra como executar consultas de Spark num cluster do Azure Databricks para aceder aos dados numa conta de armazenamento do Azure Data Lake Storage Gen2.
services: storage
author: dineshmurthy
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/29/2019
ms.author: dineshm
ms.openlocfilehash: 533665ebfa3d35ed5f03326cf5614e37056b7713
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813607"
---
# <a name="tutorial-access-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Tutorial: Aceder a dados de pré-visualização de geração 2 de armazenamento do Data Lake com o Azure Databricks com o Spark

Este tutorial mostra-lhe como ligar o seu cluster do Azure Databricks para dados armazenados numa conta de armazenamento do Azure que tenha de geração 2 de armazenamento do Azure Data Lake (pré-visualização) ativada. Esta ligação permite-lhe executar nativamente consultas e análises do seu cluster nos seus dados.

Neste tutorial, irá:

> [!div class="checklist"]
> * Criar um cluster do Databricks
> * Ingerir dados não estruturados numa conta de armazenamento
> * Executar análise nos seus dados no armazenamento de BLOBs

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Crie uma conta de geração 2 de armazenamento do Azure Data Lake.

  Ver [criar uma conta de geração 2 de armazenamento do Azure Data Lake](data-lake-storage-quickstart-create-account.md).

* Certifique-se de que a conta de utilizador tem o [função de contribuinte de dados de Blob de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) atribuídos ao mesmo.

* Instale v10 de AzCopy. Consulte [transferir dados com AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

### <a name="download-the-flight-data"></a>Transferir os dados de voos

Este tutorial utiliza dados de voo do Bureau de estatísticas de transporte para demonstrar como realizar uma operação de ETL. Tem de transferir estes dados para concluir o tutorial.

1. Aceda a [pesquisa e administração de tecnologia inovadora, Bureau de estatísticas de transportes](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

2. Selecione o **Prezipped ficheiro** caixa de verificação para selecionar todos os campos de dados.

3. Selecione o **transferir** botão e guardar os resultados para o seu computador. 

4. Descompacte o conteúdo do arquivo zipado e tome nota do nome do ficheiro e caminho do ficheiro. Precisa estas informações num passo posterior.

## <a name="get-your-storage-account-name"></a>Obter o nome da sua conta de armazenamento

Terá o nome da conta de armazenamento. Para obtê-lo, inicie sessão para o [portal do Azure](https://portal.azure.com/), escolha **todos os serviços** e filtra o termo *armazenamento*. Em seguida, selecione **contas de armazenamento** e localize a sua conta de armazenamento.

Cole o nome de um arquivo de texto. Precisará dela em breve.

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Criar um principal de serviço

Crie um principal de serviço ao seguir as orientações neste tópico: [How to: Utilizar o portal para criar um Azure AD principal de aplicações e serviço que pode aceder a recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Há algumas coisas que terá que fazer à medida que efetua os passos nesse artigo.

:heavy_check_mark: Quando realizar os passos no [atribuir a aplicação a uma função](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) secção do artigo, lembre-se de que atribuir a sua aplicação para o **função de contribuinte do armazenamento de BLOBs**.

:heavy_check_mark: Ao realizar os passos a [obter os valores para iniciar sessão](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) secção do artigo, colar o ID de inquilino, ID da aplicação e valores de chave de autenticação para um ficheiro de texto. Precisará aqueles em breve.

## <a name="create-an-azure-databricks-service"></a>Criar um serviço do Azure Databricks

Nesta secção, vai criar um serviço do Azure Databricks com o portal do Azure.

1. No Portal do Azure, selecione **Criar um recurso** > **Análise** > **Azure Databricks**.

    ![Databricks no portal do Azure](./media/data-lake-storage-use-databricks-spark/azure-databricks-on-portal.png "Databricks no portal do Azure")

2. Sob **serviço Azure Databricks**, forneça os valores seguintes para criar um serviço do Databricks:

    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome da área de trabalho**     | Indique um nome para a sua área de trabalho do Databricks.  |
    |**Subscrição**     | Na lista pendente, selecione a sua subscrição do Azure.        |
    |**Grupo de recursos**     | Especifique se quer criar um novo grupo de recursos ou utilizar um existente. Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. Para obter mais informações, veja [Descrição geral do Grupo de Recursos do Azure](../../azure-resource-manager/resource-group-overview.md). |
    |**Localização**     | Selecione **E.U.A. Oeste 2**. Para outras regiões disponíveis, veja [Serviços do Azure disponíveis por região](https://azure.microsoft.com/regions/services/).       |
    |**Escalão de Preço**     |  Selecione **padrão**.     |

    ![Criar uma área de trabalho do Azure Databricks](./media/data-lake-storage-use-databricks-spark/create-databricks-workspace.png "criar um serviço do Azure Databricks")

3. Selecione **Afixar ao dashboard** e, em seguida, selecione **Criar**.

4. A criação da conta demora alguns minutos. Durante a criação de conta, o portal apresenta o **submeter a implementação para o Azure Databricks** mosaico à direita. Para monitorizar o estado da operação, veja a barra de progresso na parte superior.

    ![Mosaico de implementação do Databricks](./media/data-lake-storage-use-databricks-spark/databricks-deployment-tile.png "Mosaico de implementação do Databricks")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Criar um cluster do Spark no Azure Databricks

1. No portal do Azure, vá para o serviço do Databricks que criou e selecione **iniciar área de trabalho**.

2. Está redirecionado para o portal do Azure Databricks. No portal, selecione **Cluster**.

    ![Databricks no Azure](./media/data-lake-storage-use-databricks-spark/databricks-on-azure.png "Databricks no Azure")

3. Na página **Novo cluster**, indique os valores para criar um cluster.

    ![Criar um cluster Databricks Spark no Azure](./media/data-lake-storage-use-databricks-spark/create-databricks-spark-cluster.png "Criar um cluster Databricks Spark no Azure")

4. Preencha os valores para os campos seguintes e aceite os valores predefinidos para os outros campos:

    * Introduza um nome para o cluster.

    * Neste artigo, crie um cluster com o **5.1** tempo de execução.

    * Certifique-se de que seleciona os **terminar após \_ \_ minutos de inatividade** caixa de verificação. Se o cluster não está a ser utilizado, indique uma duração (em minutos) para terminar o cluster.

    * Selecione **Criar cluster**. Depois do cluster está em execução, pode anexar blocos de notas para o cluster e executar tarefas do Spark.

## <a name="create-a-file-system-and-mount-it"></a>Criar um sistema de ficheiros e montá-la

Nesta secção, irá criar um sistema de ficheiros e uma pasta na sua conta de armazenamento.

1. Na [portal do Azure](https://portal.azure.com), vá para o serviço do Azure Databricks que criou e selecione **iniciar área de trabalho**.

2. No lado esquerdo, selecione **área de trabalho**. No menu pendente **Área de Trabalho**, selecione **Criar** > **Bloco de Notas**.

    ![Criar um bloco de notas no Databricks](./media/data-lake-storage-use-databricks-spark/databricks-create-notebook.png "criar bloco de notas no Databricks")

3. Na caixa de diálogo **Criar Bloco de Notas**, introduza um nome para o bloco de notas. Selecione **Python** como o idioma e, em seguida, selecione o Spark cluster que criou anteriormente.

4. Selecione **Criar**.

5. Copie e cole o seguinte bloco de código na primeira célula, mas não executar esse código ainda.

    ```Python
    configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": "<application-id>",
           "fs.azure.account.oauth2.client.secret": "<authentication-id>",
           "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token",
           "fs.azure.createRemoteFileSystemDuringInitialization": "true"}

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/folder1",
    mount_point = "/mnt/flightdata",
    extra_configs = configs)
    ```
18. Este bloco de código, substitua a `storage-account-name`, `application-id`, `authentication-id`, e `tenant-id` valores de marcador de posição este bloco de código com os valores que recolheu quando concluído os passos a [reserve conta de armazenamento configuração](#config) e [criar um principal de serviço](#service-principal) seções deste artigo. Substitua o `file-system-name` marcador de posição com qualquer nome que pretende dar o seu sistema de ficheiros.

19. Prima a **SHIFT + ENTER** chaves para executar o código nesse bloco. 

    Mantenha este bloco de notas aberto, como irá adicionar comandos a ele mais tarde.

## <a name="ingest-data"></a>Ingerir dados

### <a name="copy-source-data-into-the-storage-account"></a>Copiar dados de origem para a conta de armazenamento

Utilizar o AzCopy para copiar dados de sua *. csv* ficheiro na sua conta de geração 2 de armazenamento do Data Lake.

1. Abra uma janela de linha de comandos e introduza o seguinte comando para iniciar sessão na sua conta de armazenamento.

   ```bash
   azcopy login
   ```

   Siga as instruções aparecem na janela de prompt de comando para autenticar a sua conta de utilizador.

2. Para copiar dados a partir da *. csv* de conta, introduza o seguinte comando.

   ```bash
   azcopy cp "<csv-folder-path>" https://<storage-account-name>.dfs.core.windows.net/<file-system-name>/folder1/On_Time
   ```
   * Substitua a `<csv-folder-path>` valor de espaço reservado com o caminho do diretório para o *. csv* ficheiro (excluindo o nome do ficheiro).

   * Substitua o `storage-account-name` valor do marcador de posição pelo nome da sua conta de armazenamento.

   * Substitua o `file-system-name` marcador de posição com qualquer nome que pretende dar o seu sistema de ficheiros.

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Utilize o Databricks Notebook para converter CSV em Parquet

No bloco de notas que criou anteriormente, adicione uma nova célula e cole o seguinte código na célula. Substitua o `storage-account-name` valor do marcador de posição nesse trecho de código com o nome da pasta que guardou o ficheiro csv para.

```python
# Use the previously established DBFS mount point to read the data.
# create a data frame to read data.

flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time/<your-folder-name>/*.csv")

# read the airline csv file and write the output to parquet format for easy query.
 flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
 print("Done")
 ```

## <a name="explore-data"></a>Explorar dados

Numa nova célula, cole o seguinte código para obter uma lista de ficheiros CSV carregados via AzCopy. Substitua o `<csv-folder-path>` valor do marcador de posição com o mesmo valor para esse marcador de posição que utilizou anteriormente.

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata/On_Time/<your-folder-name>"))
```

Para criar um novo ficheiro e listar os ficheiros na pasta *parquet/flights*, execute este script:

```python
dbutils.fs.put("/mnt/flightdata/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/parquet/flights")
```

Com estes exemplos de código, explorou a natureza hierárquica do HDFS através dos dados armazenados numa conta de armazenamento com o Data Lake Storage Gen2 ativado.

## <a name="query-the-data"></a>Consultar os dados

Em seguida, pode começar a consultar os dados que carregou para a sua conta de armazenamento. Introduza cada um dos seguintes blocos de código em **Cmd 1** e prima **Cmd + Enter** para executar o script de Python.

Para criar quadros de dados para as origens de dados, execute o seguinte script:

* Substitua a `<csv-folder-path>` valor de espaço reservado com o caminho do diretório para o *. csv* ficheiro (excluindo o nome do ficheiro).

* Substitua a `<your-csv-file-name` valor de marcador de posição pelo nome da sua *csv* ficheiro.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time/<your-folder-name>/<your-csv-file-name>.csv")
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

Introduza este script para executar algumas consultas de análise básicos relativamente aos dados.

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
out1 = spark.sql("SELECT distinct(Reporting_Airline) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não forem necessários, elimine o grupo de recursos e todos os recursos relacionados. Para tal, selecione o grupo de recursos para a conta de armazenamento e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

[!div class="nextstepaction"] 
> [Extrair, transformar e carregar dados com o Apache Hive no Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)