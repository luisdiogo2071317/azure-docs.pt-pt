---
title: 'Tutorial: Realizar operações de ETL com o Azure Databricks'
description: Saiba como extrair dados de geração 2 de armazenamento do Data Lake para o Azure Databricks, transformá-los e, em seguida, carregue os dados para o Azure SQL Data Warehouse.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 01/24/2019
ms.openlocfilehash: e68bcf4ba6a8ca7bfcd5b61d42a253f7673e4654
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745322"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Tutorial: Extrair, transformar e carregar dados com o Azure Databricks

Neste tutorial, vai realizar um ETL (extração, transformação e carregamento de dados) operação ao utilizar o Azure Databricks. Extrair dados de geração 2 de armazenamento do Azure Data Lake para o Azure Databricks, executar transformações nos dados no Azure Databricks e, em seguida, carregá-los para o Azure SQL Data Warehouse.

Os passos neste tutorial utilizam o conector SQL Data Warehouse para o Azure Databricks para transferir dados para este último serviço. Por sua vez, este conector utiliza o Armazenamento de Blobs do Azure como armazenamento temporário para os dados que estão a ser transferidos entre um cluster do Azure Databricks e o Azure SQL Data Warehouse.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Crie um serviço do Azure Databricks.
> * Crie um cluster do Spark no Azure Databricks.
> * Criar um sistema de ficheiros e carregar dados para a geração 2 de armazenamento do Azure Data Lake.
> * Crie um principal de serviço.
> * Extrair dados de Store de Lake dados.
> * Transforme dados no Azure Databricks.
> * Carregar dados para o Azure SQL Data Warehouse.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Conclua estas tarefas antes de começar este tutorial:

* Criar um armazém de dados SQL do Azure, criar uma regra de firewall ao nível do servidor e ligar ao servidor como um administrador do servidor. Consulte [início rápido: Criar um armazém de dados SQL do Azure](../sql-data-warehouse/create-data-warehouse-portal.md).

* Crie uma chave mestra de base de dados para o armazém de dados SQL do Azure. Ver [crie uma chave mestra do banco de dados](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).

* Crie uma conta de geração 2 de armazenamento do Azure Data Lake. Ver [criar uma conta de geração 2 de armazenamento do Azure Data Lake](../storage/blobs/data-lake-storage-quickstart-create-account.md).

* Crie uma conta de armazenamento de Blobs do Azure e um contentor no mesmo. Obtenha também a chave de acesso para aceder à conta de armazenamento. Consulte [início rápido: Criar uma conta de armazenamento de Blobs do Azure](../storage/blobs/storage-quickstart-blobs-portal.md).

* Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-an-azure-databricks-service"></a>Criar um serviço do Azure Databricks

Nesta secção, vai criar um serviço do Azure Databricks com o portal do Azure.

1. No Portal do Azure, selecione **Criar um recurso** > **Análise** > **Azure Databricks**.

    ![Databricks no portal do Azure](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks no portal do Azure")

2. Sob **serviço Azure Databricks**, forneça os valores seguintes para criar um serviço do Databricks:

    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome da área de trabalho**     | Indique um nome para a sua área de trabalho do Databricks.        |
    |**Subscrição**     | Na lista pendente, selecione a sua subscrição do Azure.        |
    |**Grupo de recursos**     | Especifique se quer criar um novo grupo de recursos ou utilizar um existente. Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. Para obter mais informações, veja [Descrição geral do Grupo de Recursos do Azure](../azure-resource-manager/resource-group-overview.md). |
    |**Localização**     | Selecione **E.U.A. Oeste 2**.  Para outras regiões disponíveis, veja [Serviços do Azure disponíveis por região](https://azure.microsoft.com/regions/services/).      |
    |**Escalão de Preço**     |  Selecione **padrão**.     |

3. Selecione **Afixar ao dashboard** e, em seguida, selecione **Criar**.

4. A criação da conta demora alguns minutos. Durante a criação de conta, o portal apresenta o **submeter a implementação para o Azure Databricks** mosaico à direita. Para monitorizar o estado da operação, veja a barra de progresso na parte superior.

    ![Mosaico de implementação do Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Mosaico de implementação do Databricks")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Criar um cluster do Spark no Azure Databricks

1. No portal do Azure, vá para o serviço do Databricks que criou e selecione **iniciar área de trabalho**.

2. Está redirecionado para o portal do Azure Databricks. No portal, selecione **Cluster**.

    ![Databricks no Azure](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks no Azure")

3. Na página **Novo cluster**, indique os valores para criar um cluster.

    ![Criar um cluster Databricks Spark no Azure](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Criar um cluster Databricks Spark no Azure")

4. Preencha os valores para os campos seguintes e aceite os valores predefinidos para os outros campos:

    * Introduza um nome para o cluster.

    * Neste artigo, crie um cluster com o **5.1** tempo de execução.

    * Certifique-se de que seleciona os **terminar após \_ \_ minutos de inatividade** caixa de verificação. Se o cluster não está a ser utilizado, indique uma duração (em minutos) para terminar o cluster.

    * Selecione **Criar cluster**. Depois do cluster está em execução, pode anexar blocos de notas para o cluster e executar tarefas do Spark.

## <a name="create-a-file-system-and-upload-sample-data"></a>Criar um sistema de ficheiros e carregar dados de exemplo

Primeiro, crie um sistema de ficheiros na sua conta de geração 2 de armazenamento do Data Lake. Em seguida, pode carregar um ficheiro de dados de exemplo para o Data Lake Store. Este ficheiro vai ser utilizado mais tarde no Azure Databricks para executar algumas transformações.

1. Transfira o [small_radio_json](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) ficheiro de dados de exemplo para o sistema de ficheiros local.

2. Partir do [portal do Azure](https://portal.azure.com/), navegue para a conta de geração 2 de armazenamento do Data Lake que criou como um pré-requisito para este tutorial.

3. Do **descrição geral** página da conta de armazenamento, selecione **abrir no Explorador de**.

   ![Abra o Explorador de armazenamento](./media/databricks-extract-load-sql-data-warehouse/data-lake-storage-open-storage-explorer.png "abra o Explorador de armazenamento")

4. Selecione **abrir o Explorador de armazenamento do Azure** para abrir o Explorador de armazenamento.

   ![Abra segundo aviso de Explorador de armazenamento](./media/databricks-extract-load-sql-data-warehouse/data-lake-storage-open-storage-explorer-2.png "segundo aviso de abrir o Explorador de armazenamento")

   Explorador de armazenamento é aberto. Pode criar um sistema de ficheiros e carregar os dados de exemplo com as orientações neste tópico: [Quickstart: Utilize o Explorador de armazenamento do Azure para gerir dados numa conta de geração 2 de armazenamento do Azure Data Lake](../storage/blobs/data-lake-storage-explorer.md).

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Criar um principal de serviço

Crie um principal de serviço ao seguir as orientações neste tópico: [How to: Utilizar o portal para criar um Azure AD principal de aplicações e serviço que pode aceder a recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Há algumas coisas que terá que fazer à medida que efetua os passos nesse artigo.

:heavy_check_mark: Quando realizar os passos no [atribuir a aplicação a uma função](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) secção do artigo, lembre-se de que atribuir a sua aplicação para o **função de contribuinte do armazenamento de BLOBs**.

:heavy_check_mark: Ao realizar os passos a [obter os valores para iniciar sessão](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) secção do artigo, colar o ID de inquilino, ID da aplicação e valores de chave de autenticação para um ficheiro de texto. Precisará aqueles em breve.
Em primeiro lugar, crie um bloco de notas na sua área de trabalho do Azure Databricks e, em seguida, executar fragmentos de código para criar o sistema de ficheiros na sua conta de armazenamento.

## <a name="extract-data-from-the-data-lake-store"></a>Extrair dados do Store de Lake dados

Nesta secção, crie um bloco de notas na área de trabalho do Azure Databricks e, em seguida, executar fragmentos de código para extrair dados do Store de Lake dados para o Azure Databricks.

1. Na [portal do Azure](https://portal.azure.com), vá para o serviço do Azure Databricks que criou e selecione **iniciar área de trabalho**.

2. No lado esquerdo, selecione **área de trabalho**. No menu pendente **Área de Trabalho**, selecione **Criar** > **Bloco de Notas**.

    ![Criar um bloco de notas no Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "criar bloco de notas no Databricks")

3. Na caixa de diálogo **Criar Bloco de Notas**, introduza um nome para o bloco de notas. Selecione **Scala** como a linguagem e selecione o cluster do Spark que criou anteriormente.

    ![Forneça os detalhes para um bloco de notas no Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "fornecem detalhes para um bloco de notas no Databricks")

4. Selecione **Criar**.

5. Copie e cole o seguinte bloco de código na primeira célula.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<authentication-key>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   ```

6. Este bloco de código, substitua a `application-id`, `authentication-id`, e `tenant-id` valores de marcador de posição este bloco de código com os valores que recolheu quando concluído os passos a [reserve configuração de conta de armazenamento](#config). Substitua o `storage-account-name` valor do marcador de posição pelo nome da sua conta de armazenamento.

7. Prima a **SHIFT + ENTER** chaves para executar o código nesse bloco.

8. Agora, pode carregar o ficheiro de json de exemplo como um quadro de dados no Azure Databricks. Cole o seguinte código numa célula de novo. Substitua os marcadores de posição mostrados entre parênteses Retos pelos seus valores.

   ```scala
   val df = spark.read.json("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json")
   ```

   * Substitua o `file-system-name` valor do marcador de posição pelo nome que deu a seu sistema de ficheiros no Explorador de armazenamento.

   * Substitua o `storage-account-name` marcador de posição pelo nome da sua conta de armazenamento.

9. Prima a **SHIFT + ENTER** chaves para executar o código nesse bloco.

10. Execute o seguinte código para ver o conteúdo do quadro de dados:

    ```scala
    df.show()
    ```
   Verá um resultado semelhante ao seguinte fragmento:

   ```bash
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
   | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
   | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
   ...
   ...
   ```

   Os dados foram agora extraídos do Armazenamento do Azure Data Lake Ger2 para o Azure Databricks.

## <a name="transform-data-in-azure-databricks"></a>Transformar dados no Azure Databricks

Os dados de exemplo em bruto **small_radio_json** ficheiro captura o público-alvo para uma estação de rádio e tem uma variedade de colunas. Nesta secção, vai transformar os dados para obter apenas colunas específicas do conjunto de dados.

1. Em primeiro lugar, obter apenas as colunas **firstName**, **lastName**, **sexo**, **localização**, e **nível**do dataframe que criou.

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
   ```

   Receber um resultado conforme mostrado no seguinte fragmento:

   ```bash
   +---------+----------+------+--------------------+-----+
   |firstname|  lastname|gender|            location|level|
   +---------+----------+------+--------------------+-----+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   +---------+----------+------+--------------------+-----+
   ```

2. Pode transformar mais os dados para mudar o nome da coluna **level** para **subscription_type**.

   ```scala
   val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
   renamedColumnsDF.show()
   ```

   Receber um resultado conforme mostrado no seguinte fragmento.

   ```bash
   +---------+----------+------+--------------------+-----------------+
   |firstname|  lastname|gender|            location|subscription_type|
   +---------+----------+------+--------------------+-----------------+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   +---------+----------+------+--------------------+-----------------+
   ```

## <a name="load-data-into-azure-sql-data-warehouse"></a>Carregar dados para o Azure SQL Data Warehouse

Nesta secção, vai carregar os dados transformados para o Azure SQL Data Warehouse. Utilize o conector Azure SQL Data Warehouse para o Azure Databricks para carregar diretamente um dataframe como uma tabela num SQL data warehouse.

Conforme mencionado anteriormente, o conector SQL Data Warehouse utiliza o armazenamento de Blobs do Azure como armazenamento temporário para carregar dados entre o Azure Databricks e o Azure SQL Data Warehouse. Assim, comece por indicar a configuração para ligar à conta de armazenamento. Deve já ter já criado a conta como parte dos pré-requisitos deste artigo.

1. Indique a configuração para aceder à conta de Armazenamento do Azure a partir do Azure Databricks.

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. Especifique uma pasta temporária para utilizar ao mover dados entre o Azure Databricks e o Azure SQL Data Warehouse.

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. Execute o seguinte fragmento para armazenar as chaves de acesso do armazenamento de Blobs do Azure na configuração. Esta ação garante que não precisa manter a chave de acesso no bloco de notas em texto simples.

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. Indique os valores para ligar à instância do Azure SQL Data Warehouse. Tem de ter criado um armazém de dados SQL como um pré-requisito.

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. Execute o fragmento seguinte para carregar o dataframe transformado, **renamedColumnsDF**, como uma tabela num SQL data warehouse. Este fragmento cria uma tabela chamada **SampleTable** na base de dados SQL.

   ```scala
   spark.conf.set(
       "spark.sql.parquet.writeLegacyFormat",
       "true")

   renamedColumnsDF.write
       .format("com.databricks.spark.sqldw")
       .option("url", sqlDwUrlSmall) 
       .option("dbtable", "SampleTable")
       .option( "forward_spark_azure_storage_credentials","True")
       .option("tempdir", tempDir)
       .mode("overwrite")
       .save()
   ```

6. Ligar à base de dados SQL e certifique-se de que consegue ver uma base de dados com o nome **SampleTable**.

   ![Certifique-se a tabela de exemplo](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "verificar sampletable")

7. Execute uma consulta select para verificar os conteúdos da tabela. A tabela deve ter os mesmos dados que o **renamedColumnsDF** dataframe.

    ![Verificar o conteúdo de sampletable](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "verificar o conteúdo de sampletable")

## <a name="clean-up-resources"></a>Limpar recursos

Depois de concluir o tutorial, pode terminar o cluster. Na área de trabalho do Azure Databricks, selecione **Clusters** à esquerda. Para o cluster terminar, em **ações**, aponte para o botão de reticências (...) e selecione o **Terminate** ícone.

![Parar um cluster do Databricks](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Parar um cluster do Databricks")

Se não terminar manualmente o cluster, para automaticamente, desde que selecionou o **terminar após \_ \_ minutos de inatividade** caixa de verificação quando criou o cluster. Nesse caso, o cluster para automaticamente se tiver estado inativo durante o período de tempo especificado.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um serviço do Azure Databricks
> * Criar um cluster do Spark no Azure Databricks
> * Criar um bloco de notas no Azure Databricks
> * Extrair dados de uma conta de geração 2 de armazenamento do Data Lake
> * Transformar dados no Azure Databricks
> * Carregar dados para o Azure SQL Data Warehouse

Avance para o próximo tutorial para saber como transmitir dados em tempo real em fluxo para o Azure Databricks mediante a utilização dos Hubs de Eventos do Azure.

> [!div class="nextstepaction"]
>[Transmitir dados para o Azure Databricks utilizando os Hubs de Eventos](databricks-stream-from-eventhubs.md)