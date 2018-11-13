---
title: Saiba como realizar operações de extração, carregamento e transferência com o Azure Databricks
description: Aprenda a extrair dados da Pré-visualização do Armazenamento do Azure Data Lake Ger2 para o Azure Databricks, transformá-los e carregá-los para o Azure SQL Data Warehouse.
services: azure-databricks
ms.service: azure-databricks
author: jamesbak
ms.author: jamesbak
ms.reviewer: jasonwhowell
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/27/2018
ms.openlocfilehash: 5c1684f7367aec2d283cd6ad310657def49dd3cb
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282565"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Tutorial: Extrair, transformar e carregar dados com o Azure Databricks

Neste tutorial, vai realizar uma operação de ETL (extração, transformação e carregamento de dados) para mover dados de uma conta do Armazenamento do Azure Data Lake Storage Gen2 ativado para o Azure SQL Data Warehouse com o Azure Databricks.

A ilustração seguinte mostra o fluxo da aplicação:

![Azure Databricks com Armazenamento do Data Lake Ger2 e SQL Data Warehouse](./media/handle-data-using-databricks/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks com Armazenamento do Data Lake Ger2 e SQL Data Warehouse")

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar uma área de trabalho do Azure Databricks
> * Criar um cluster do Spark no Azure Databricks
> * Criar uma conta compatível com o Armazenamento do Azure Data Lake Ger2
> * Carregar dados para o Armazenamento do Azure Data Lake Ger2
> * Criar um bloco de notas no Azure Databricks
> * Extrair dados do Armazenamento do Data Lake Ger2
> * Transformar dados no Azure Databricks
> * Carregar dados para o Azure SQL Data Warehouse

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* Cria um armazém do Azure SQL Data Warehouse e uma regra de firewall ao nível do servidor e liga ao servidor como administrador do servidor. Siga as instruções em [Início Rápido: Criar um armazém do Azure SQL Data Warehouse](../../sql-data-warehouse/create-data-warehouse-portal.md)
* Crie uma chave mestra de base de dados para o Azure SQL Data Warehouse. Siga as instruções em [Create a Database Master Key](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) (Criar uma Chave Mestra de Base de Dados).
* [Criar uma conta de Armazenamento do Azure Data Lake Ger2](quickstart-create-account.md)

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-an-azure-databricks-workspace"></a>Criar uma área de trabalho do Azure Databricks

Nesta secção, vai criar uma área de trabalho do Azure Databricks com o portal do Azure.

1. No Portal do Azure, selecione **Criar um recurso** > **Análise** > **Azure Databricks**.

    ![Databricks no portal do Azure](./media/handle-data-using-databricks/azure-databricks-on-portal.png "Databricks no portal do Azure")

2. Em **Serviço Azure Databricks**, forneça os valores para criar uma área de trabalho do Databricks.

    ![Criar uma área de trabalho do Azure Databricks](./media/handle-data-using-databricks/create-databricks-workspace.png "Criar uma área de trabalho do Azure Databricks")

    Forneça os seguintes valores:

    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome da área de trabalho**     | Indique um nome para a sua área de trabalho do Databricks.        |
    |**Subscrição**     | Na lista pendente, selecione a sua subscrição do Azure.        |
    |**Grupo de recursos**     | Especifique se quer criar um novo grupo de recursos ou utilizar um existente. Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. Para obter mais informações, veja [Descrição geral do Grupo de Recursos do Azure](../../azure-resource-manager/resource-group-overview.md). |
    |**Localização**     | Selecione **E.U.A. Oeste 2**. Para outras regiões disponíveis, veja [Serviços do Azure disponíveis por região](https://azure.microsoft.com/regions/services/).        |
    |**Escalão de Preço**     |  Escolha entre **Standard** ou **Premium**. Para obter mais informações sobre estes escalões, veja [Página de preços do Databricks](https://azure.microsoft.com/pricing/details/databricks/).       |

    Selecione **Afixar ao dashboard** e, em seguida, selecione **Criar**.

3. A criação da conta demora alguns minutos. Durante a criação da conta, o portal apresenta o mosaico **Submeter a implementação para o Azure Databricks** no lado direito. Pode ter de se deslocar para a direita no dashboard para ver o mosaico. É também apresentada uma barra de progresso junto à parte superior do ecrã. Pode ver qualquer área de progresso.

    ![Mosaico de implementação do Databricks](./media/handle-data-using-databricks/databricks-deployment-tile.png "Mosaico de implementação do Databricks")

## <a name="create-a-spark-cluster-in-databricks"></a>Criar um cluster do Spark no Databricks

1. No portal do Azure, aceda à área de trabalho do Databricks que criou e, em seguida, selecione **Iniciar Área de Trabalho**.

2. Será redirecionado para o portal do Azure Databricks. No portal, selecione **Cluster**.

    ![Databricks no Azure](./media/handle-data-using-databricks/databricks-on-azure.png "Databricks no Azure")

3. Na página **Novo cluster**, indique os valores para criar um cluster.

    ![Criar um cluster Databricks Spark no Azure](./media/handle-data-using-databricks/create-databricks-spark-cluster.png "Criar um cluster Databricks Spark no Azure")

    Preencha os valores para os campos seguintes e aceite os valores predefinidos para os outros campos:

    * Introduza um nome para o cluster.
    * Para este artigo, crie um cluster com o runtime **4.2**.
    * Certifique-se de que seleciona a caixa de verificação **Terminar após \_\_ minutos de atividade**. Indique uma duração (em minutos) para terminar o cluster, caso não esteja a ser utilizado.

    Selecione **Criar cluster**. Depois de o cluster estar em execução, pode anexar blocos de notas ao cluster e executar tarefas do Spark.

## <a name="create-storage-account-file-system"></a>Criar sistema de ficheiro da conta de armazenamento

Nesta secção, vai criar um bloco de notas na área de trabalho do Azure Databricks e, em seguida, executar fragmentos de código para configurar a conta de armazenamento.

1. No [portal do Azure](https://portal.azure.com), aceda à área de trabalho do Azure Databricks que criou e selecione **Iniciar Área de Trabalho**.

2. No painel esquerdo, selecione **Área de Trabalho**. No menu pendente **Área de Trabalho**, selecione **Criar** > **Bloco de Notas**.

    ![Criar um bloco de notas no Databricks](./media/handle-data-using-databricks/databricks-create-notebook.png "Criar um bloco de notas no Databricks")

3. Na caixa de diálogo **Criar Bloco de Notas**, introduza um nome para o bloco de notas. Selecione **Scala** como a linguagem e selecione o cluster do Spark que criou anteriormente.

    ![Criar um bloco de notas no Databricks](./media/handle-data-using-databricks/databricks-notebook-details.png "Criar um bloco de notas no Databricks")

    Selecione **Criar**.

4. Introduza o seguinte código na primeira célula e execute o código:

    ```scala
    spark.conf.set("fs.azure.account.key.<ACCOUNT_NAME>.dfs.core.windows.net", "<ACCOUNT_KEY>") 
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
    dbutils.fs.ls("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/")
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false") 
    ```

    Prima **SHIFT + ENTER** para executar a célula do código.

    Agora, o sistema de ficheiros é criado para a conta de armazenamento.

## <a name="upload-data-to-the-storage-account"></a>Carregar dados para a conta de armazenamento

O próximo passo é carregar um ficheiro de dados de exemplo para a conta de armazenamento para transformá-lo mais tarde no Azure Databricks. 

> [!NOTE]
> Se ainda não tiver uma conta compatível com o Armazenamento do Azure Data Lake Ger2, siga o [início rápido para criar uma](./quickstart-create-account.md).

1. Transfira (**small_radio_json**) do repositório [Exemplos de U-SQL e Deteção de Problemas](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) e anote o caminho onde guarda o ficheiro.

2. Em seguida, carregue os dados de exemplo para a conta de armazenamento. O método utilizado para carregar os dados para a sua conta de armazenamento difere consoante tenha, ou não, o espaço de nomes hierárquico ativado.

    Se o espaço de nomes hierárquico está ativado na sua conta de Armazenamento do Azure, pode utilizar o Azure Data Factory, distp ou AzCopy (versão 10) para processar o carregamento. A versão 10 do AzCopy só está disponível para pré-visualização neste momento. Para utilizar o AzCopy, cole o código seguinte numa janela de comando:

    ```bash
    set ACCOUNT_NAME=<ACCOUNT_NAME>
    set ACCOUNT_KEY=<ACCOUNT_KEY>
    azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
    ```
    
## <a name="extract-data-from-azure-storage"></a>Extrair dados do Armazenamento do Azure

Regresse ao seu Bloco de Notas do DataBricks e introduza o seguinte código numa nova célula:

1. Adicione o fragmento seguinte a uma célula de código vazia e substitua os valores dos marcadores de posição por aqueles que guardou anteriormente na conta de armazenamento~.

    ```scala
    dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
    dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
    ```

    Prima **SHIFT + ENTER** para executar a célula do código.

2. Agora, pode carregar o ficheiro json de exemplo como um dataframe no Azure Databricks. Cole o código seguinte numa nova célula e, em seguida, prima **SHIFT + ENTER** (garantindo que substitui os valores do marcador de posição):

    ```scala
    val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
    ```

3. Execute o código abaixo para ver os conteúdos do dataframe.

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

Os dados de exemplo em bruto **small_radio_json.json** capturam o público-alvo de uma estação de rádio e têm várias colunas. Nesta secção, vai transformar os dados para obter apenas colunas específicas do conjunto de dados.

1. Comece por obter apenas as colunas *firstName*, *lastName*, *gender*, *location* e *level* do dataframe que já criou.

    ```scala
    val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
    ```

    Receberá um resultado conforme mostrado no seguinte fragmento:

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

2.  Pode transformar mais os dados para mudar o nome da coluna **level** para **subscription_type**.

    ```scala
    val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
    renamedColumnsDF.show()
    ```

    Receberá um resultado conforme mostrado no seguinte fragmento.

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

Nesta secção, vai carregar os dados transformados para o Azure SQL Data Warehouse. Ao utilizar o conector Azure SQL Data Warehouse para o Azure Databricks, pode carregar diretamente um dataframe como uma tabela no armazém do SQL Data Warehouse.

Conforme mencionado antes, o conector SQL Data Warehouse utiliza o Armazenamento de Blobs do Azure como armazenamento temporário para carregar dados entre o Azure Databricks e o Azure SQL Data Warehouse. Assim, comece por indicar a configuração para ligar à conta de armazenamento. Já deverá ter criado a conta como parte dos pré-requisitos deste artigo.

1. Indique a configuração para aceder à conta de Armazenamento do Azure a partir do Azure Databricks.

    ```scala
    val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
    val fileSystemName = "<FILE_SYSTEM_NAME>"
    val accessKey =  "<ACCESS_KEY>"
    ```

2. Especifique uma pasta temporária que será utilizada ao mover dados entre o Azure Databricks e o Azure SQL Data Warehouse.

    ```scala
    val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
    ```

3. Execute o seguinte fragmento para armazenar as chaves de acesso do armazenamento de Blobs do Azure na configuração. Desta forma, não é necessário manter a chave de acesso em texto simples no bloco de notas.

    ```scala
    val acntInfo = "fs.azure.account.key."+ storageURI
    sc.hadoopConfiguration.set(acntInfo, accessKey)
    ```

4. Indique os valores para ligar à instância do Azure SQL Data Warehouse. Como parte dos pré-requisitos, teve de criar um armazém de dados do SQL.

    ```scala
    //SQL Data Warehouse related settings
    val dwDatabase = "<DATABASE NAME>"
    val dwServer = "<DATABASE SERVER NAME>" 
    val dwUser = "<USER NAME>"
    val dwPass = "<PASSWORD>"
    val dwJdbcPort =  "1433"
    val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
    val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
    val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
    ```

5. Execute o fragmento seguinte para carregar o dataframe transformado, **renamedColumnsDF**, como uma tabela no SQL Data Warehouse. Este fragmento cria uma tabela chamada **SampleTable** na base de dados SQL.

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

6. Ligue à base de dados SQL e confirme que vê **SampleTable**.

    ![Verificar sampletable](./media/handle-data-using-databricks/verify-sample-table.png "Verificar sampletable")

7. Execute uma consulta select para verificar os conteúdos da tabela. Deverá ter os mesmos dados que o dataframe **renamedColumnsDF**.

    ![Verificar conteúdo de sampletable](./media/handle-data-using-databricks/verify-sample-table-content.png "Verificar conteúdo de sampletable")

## <a name="clean-up-resources"></a>Limpar recursos

Depois de executar o tutorial, pode terminar o cluster. Para tal, na área de trabalho do Azure Databricks, no painel esquerdo, selecione **Clusters**. Para o cluster que quer terminar, mova o cursor sobre o botão de reticências na coluna **Ações** e selecione o ícone **Terminar**.

![Parar um cluster do Databricks](./media/handle-data-using-databricks/terminate-databricks-cluster.png "Parar um cluster do Databricks")

Se não terminar manualmente o cluster, este irá parar automaticamente, desde que tenha selecionado a caixa de verificação **Terminar após __ minutos de inatividade** ao criar o cluster. Nesse caso, o cluster para automaticamente se tiver estado inativo durante o período de tempo especificado.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma área de trabalho do Azure Databricks
> * Criar um cluster do Spark no Azure Databricks
> * Criar uma conta compatível com o Armazenamento do Azure Data Lake Ger2
> * Carregar dados para o Armazenamento do Azure Data Lake Ger2
> * Criar um bloco de notas no Azure Databricks
> * Extrair dados do Armazenamento do Data Lake Ger2
> * Transformar dados no Azure Databricks
> * Carregar dados para o Azure SQL Data Warehouse

Avance para o próximo tutorial para saber como transmitir dados em tempo real em fluxo para o Azure Databricks mediante a utilização dos Hubs de Eventos do Azure.

> [!div class="nextstepaction"]
>[Transmitir dados para o Azure Databricks utilizando os Hubs de Eventos](../../azure-databricks/databricks-stream-from-eventhubs.md)
