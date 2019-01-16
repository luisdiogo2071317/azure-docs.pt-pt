---
title: 'Tutorial: Aprender a realizar a extração, carregar e transferir operações com o Azure Databricks'
description: Neste tutorial, saiba como extrair dados de pré-visualização do Azure Data Lake Storage geração 2 para o Azure Databricks, transformá-los e, em seguida, carregue os dados para o Azure SQL Data Warehouse.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 01/14/2019
ms.component: data-lake-storage-gen2
ms.openlocfilehash: e4e75c65178c4bbedcf781c2fbf2149a94a702cd
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321199"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Tutorial: Extrair, transformar e carregar dados com o Azure Databricks

Neste tutorial, vai utilizar do Azure Databricks para executar um ETL (extração, transformação e carregamento de dados) operação. Mover dados a partir de uma conta de armazenamento do Azure com o Azure Data Lake Storage Gen2 ativado para o Azure SQL Data Warehouse.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie uma área de trabalho do Azure Databricks.
> * Crie um cluster do Spark no Azure Databricks.
> * Crie uma conta com capacidade de geração 2 de armazenamento do Azure Data Lake.
> * Carregar dados para a geração 2 de armazenamento do Azure Data Lake.
> * Crie um bloco de notas no Azure Databricks.
> * Extrair dados de geração 2 de armazenamento do Data Lake.
> * Transforme dados no Azure Databricks.
> * Carregar dados para o Azure SQL Data Warehouse.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* Criar um armazém de dados SQL do Azure, criar uma regra de firewall ao nível do servidor e ligar ao servidor como um administrador do servidor. Siga as instruções no [início rápido: Criar um armazém de dados SQL do Azure](../../sql-data-warehouse/create-data-warehouse-portal.md) artigo.
* Crie uma chave mestra de base de dados para o armazém de dados SQL do Azure. Siga as instruções no [crie uma chave mestra do banco de dados](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) artigo.
* [Criar uma conta de geração 2 de armazenamento do Azure Data Lake](data-lake-storage-quickstart-create-account.md).
* Transfira (**small_radio_json**) do repositório [Exemplos de U-SQL e Deteção de Problemas](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) e anote o caminho onde guarda o ficheiro.
* Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="set-aside-storage-account-configuration"></a>Reservar a configuração da conta de armazenamento

Terá o nome da sua conta de armazenamento e um URI do ponto de extremidade sistema de ficheiros.

Para obter o nome da conta de armazenamento no portal do Azure, escolha **todos os serviços** e filtre o termo *armazenamento*. Em seguida, selecione **contas de armazenamento** e localize a sua conta de armazenamento.

Para obter o URI do ponto de extremidade sistema de ficheiros, escolha **propriedades**e no painel de propriedades, localize o valor da **primário ponto final do sistema de ficheiro ADLS** campo.

Cole esses valores num arquivo de texto. Precisará-los em breve.

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Criar um principal de serviço

Crie um principal de serviço ao seguir as orientações neste tópico: [How to: Utilizar o portal para criar um Azure AD principal de aplicações e serviço que pode aceder a recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Há alguns pontos específicos que terá que fazer à medida que efetua os passos nesse artigo.

:heavy_check_mark: Quando realizar os passos no [criar uma aplicação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application) secção do artigo, certifique-se de definir o **URL de início de sessão** campo o **criar** caixa de diálogo para o URI do ponto final que acabou recolhidos.

:heavy_check_mark: Quando realizar os passos no [atribuir a aplicação a uma função](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) secção do artigo, lembre-se de que atribuir a sua aplicação para o **função de contribuinte do armazenamento de BLOBs**.

:heavy_check_mark: Ao realizar os passos a [obter os valores para iniciar sessão](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) secção do artigo, colar o ID de inquilino, ID da aplicação e valores de chave de autenticação para um ficheiro de texto. Precisará aqueles em breve.

## <a name="create-the-workspace"></a>Criar a área de trabalho

Nesta secção, vai criar uma área de trabalho do Azure Databricks com o portal do Azure.

1. No Portal do Azure, selecione **Criar um recurso** > **Análise** > **Azure Databricks**.

    ![Databricks no portal do Azure](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks no portal do Azure")

1. Sob **serviço Azure Databricks**, forneça os valores seguintes para criar uma área de trabalho do Databricks:

    |Propriedade  |Descrição  |
    |---------|---------|
    |**Nome da área de trabalho**     | Indique um nome para a sua área de trabalho do Databricks.        |
    |**Subscrição**     | Na lista pendente, selecione a sua subscrição do Azure.        |
    |**Grupo de recursos**     | Especifique se quer criar um novo grupo de recursos ou utilizar um existente. Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. Para obter mais informações, veja [Descrição geral do Grupo de Recursos do Azure](../../azure-resource-manager/resource-group-overview.md). |
    |**Localização**     | Selecione **E.U.A. Oeste 2**.        |
    |**Escalão de Preço**     |  Selecione **padrão**.     |

    ![Criar uma área de trabalho do Azure Databricks](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Criar uma área de trabalho do Azure Databricks")

1. Selecione **Afixar ao dashboard** e, em seguida, selecione **Criar**.

1. A criação da conta demora alguns minutos. Durante a criação de conta, o portal apresenta o **submeter a implementação para o Azure Databricks** mosaico à direita. Para monitorizar o estado da operação, veja a barra de progresso na parte superior.

    ![Mosaico de implementação do Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Mosaico de implementação do Databricks")

## <a name="create-the-spark-cluster"></a>Criar o cluster do Spark

Para realizar as operações neste tutorial, precisa de um cluster do Spark. Utilize os seguintes passos para criar o cluster do Spark.

1. No portal do Azure, aceda à área de trabalho do Databricks que criou e selecione **iniciar área de trabalho**.

1. Está redirecionado para o portal do Azure Databricks. No portal, selecione **Cluster**.

    ![Databricks no Azure](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks no Azure")

1. Na página **Novo cluster**, indique os valores para criar um cluster.

    ![Criar um cluster Databricks Spark no Azure](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Criar um cluster Databricks Spark no Azure")

1. Preencha os valores para os campos seguintes e aceite os valores predefinidos para os outros campos:

    * Introduza um nome para o cluster.
    * Neste artigo, crie um cluster com o **5.1** tempo de execução.
    * Certifique-se de que seleciona os **terminar após \_ \_ minutos de inatividade** caixa de verificação. Se o cluster não está a ser utilizado, indique uma duração (em minutos) para terminar o cluster.

1. Selecione **Criar cluster**.

Depois do cluster está em execução, pode anexar blocos de notas para o cluster e executar tarefas do Spark.

## <a name="create-a-file-system"></a>Criar um sistema de ficheiros

Para armazenar dados na sua conta de armazenamento de geração 2 de armazenamento do Data Lake, terá de criar um sistema de ficheiros.

Em primeiro lugar, crie um bloco de notas na sua área de trabalho do Azure Databricks e, em seguida, executar fragmentos de código para criar o sistema de ficheiros na sua conta de armazenamento.

1. Na [portal do Azure](https://portal.azure.com), vá para a área de trabalho do Azure Databricks que criou e selecione **iniciar área de trabalho**.

2. No lado esquerdo, selecione **área de trabalho**. No menu pendente **Área de Trabalho**, selecione **Criar** > **Bloco de Notas**.

    ![Criar um bloco de notas no Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "criar bloco de notas no Databricks")

3. Na caixa de diálogo **Criar Bloco de Notas**, introduza um nome para o bloco de notas. Selecione **Scala** como a linguagem e selecione o cluster do Spark que criou anteriormente.

    ![Forneça os detalhes para um bloco de notas no Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "fornecem detalhes para um bloco de notas no Databricks")

    Selecione **Criar**.

4. Copie e cole o seguinte bloco de código na primeira célula, mas não executar esse código ainda.

    ```scala
    val configs = Map(
    "fs.azure.account.auth.type" -> "OAuth",
    "fs.azure.account.oauth.provider.type" -> "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
    "fs.azure.account.oauth2.client.id" -> "<application-id>",
    "fs.azure.account.oauth2.client.secret" -> "<authentication-key>"),
    "fs.azure.account.oauth2.client.endpoint" -> "https://login.microsoftonline.com/<tenant-id>/oauth2/token",
    "fs.azure.createRemoteFileSystemDuringInitialization"->"true")

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/<directory-name>",
    mountPoint = "/mnt/<mount-name>",
    extraConfigs = configs)
    ```

5. Este bloco de código, substitua a `storage-account-name`, `application-id`, `authentication-id`, e `tenant-id` valores de marcador de posição este bloco de código com os valores que recolheu quando concluído os passos a [reserve conta de armazenamento configuração](#config) e [criar um principal de serviço](#service-principal) seções deste artigo. Definir o `file-system-name`, `directory-name`, e `mount-name` valores de marcador de posição para apontam a quaisquer nomes que pretende dar o sistema de ficheiros, o diretório e a montagem.

6. Prima a **SHIFT + ENTER** chaves para executar o código nesse bloco.

## <a name="upload-the-sample-data"></a>Carregar os dados de exemplo

A próxima etapa é carregar um ficheiro de dados de exemplo para a conta de armazenamento para transformar mais tarde no Azure Databricks.

Carregar os dados de exemplo que transferiu para a sua conta de armazenamento. O método que utilizar para carregar os dados na sua conta de armazenamento difere consoante tenha ou o espaço de nomes hierárquico ativado.

Pode utilizar o Azure Data Factory, distp ou AzCopy (versão 10) para fazer o carregamento. AzCopy versão 10 está atualmente disponível apenas através de pré-visualização. Para utilizar o AzCopy, cole o código seguinte numa janela de comando:

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
```

## <a name="extract-the-data"></a>Extrair os dados

Para trabalhar com os dados de exemplo no Databricks, terá de extrair os dados da sua conta de armazenamento.

Regresse ao seu bloco de notas do Databricks e introduza o seguinte código numa célula de novo no seu bloco de notas.

Adicione o seguinte fragmento numa célula vazia de código. Substitua os marcadores de posição mostrados entre parênteses Retos pelos valores que guardou anteriormente da conta de armazenamento.

```scala
dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
```

Selecione as teclas Shift + Enter para executar o código.

Agora, pode carregar o ficheiro json de exemplo como um dataframe no Azure Databricks. Cole o seguinte código numa célula de novo. Substitua os marcadores de posição mostrados entre parênteses Retos pelos seus valores.

```scala
val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
```

Selecione as teclas Shift + Enter para executar o código.

Execute o seguinte código para ver o conteúdo do quadro de dados:

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

## <a name="transform-the-data"></a>Transformar os dados

Os dados de exemplo em bruto **small_radio_json** ficheiro captura o público-alvo para uma estação de rádio e tem uma variedade de colunas. Nesta secção, vai transformar os dados para obter apenas colunas específicas do conjunto de dados.

Em primeiro lugar, obter apenas as colunas **firstName**, **lastName**, **sexo**, **localização**, e **nível**do dataframe que criou.

```scala
val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
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

Pode transformar mais os dados para mudar o nome da coluna **level** para **subscription_type**.

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

## <a name="load-the-data"></a>Carregar os dados

Nesta secção, vai carregar os dados transformados para o Azure SQL Data Warehouse. Utilize o conector Azure SQL Data Warehouse para o Azure Databricks para carregar diretamente um dataframe como uma tabela num SQL data warehouse.

O conector SQL Data Warehouse utiliza o armazenamento de Blobs do Azure como armazenamento temporário para carregar dados entre o Azure Databricks e o Azure SQL Data Warehouse. Assim, comece por indicar a configuração para ligar à conta de armazenamento. Tem já tiver criado a conta como parte dos pré-requisitos deste artigo.

Indique a configuração para aceder à conta de Armazenamento do Azure a partir do Azure Databricks.

```scala
val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
val fileSystemName = "<FILE_SYSTEM_NAME>"
val accessKey =  "<ACCESS_KEY>"
```

Especifique uma pasta temporária para utilizar ao mover dados entre o Azure Databricks e o Azure SQL Data Warehouse.

```scala
val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
```

Execute o seguinte fragmento para armazenar as chaves de acesso do armazenamento de Blobs do Azure na configuração. Esta ação garante que não precisa manter a chave de acesso no bloco de notas em texto simples.

```scala
val acntInfo = "fs.azure.account.key."+ storageURI
sc.hadoopConfiguration.set(acntInfo, accessKey)
```

Indique os valores para ligar à instância do Azure SQL Data Warehouse. Tem de ter criado um armazém de dados SQL como um pré-requisito.

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

Execute o fragmento seguinte para carregar o dataframe transformado, **renamedColumnsDF**, como uma tabela num SQL data warehouse. Este fragmento cria uma tabela chamada **SampleTable** na base de dados SQL.

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

Ligar à base de dados SQL e certifique-se de que consegue ver uma base de dados com o nome **SampleTable**.

![Certifique-se a tabela de exemplo](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "verificar sampletable")

Execute uma consulta select para verificar os conteúdos da tabela. A tabela deve ter os mesmos dados que o **renamedColumnsDF** dataframe.

![Verificar o conteúdo de sampletable](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "verificar o conteúdo de sampletable")

## <a name="clean-up-resources"></a>Limpar recursos

Depois de concluir o tutorial, pode terminar o cluster. Na área de trabalho do Azure Databricks, selecione **Clusters** à esquerda. Para o cluster terminar, em **ações**, aponte para o botão de reticências (...) e selecione o **Terminate** ícone.

![Parar um cluster do Databricks](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Parar um cluster do Databricks")

Se não terminar manualmente o cluster, para automaticamente, desde que selecionou o **terminar após \_ \_ minutos de inatividade** caixa de verificação quando criou o cluster. Nesse caso, o cluster para automaticamente se tiver estado inativo durante o período de tempo especificado.

## <a name="next-steps"></a>Passos Seguintes

Avance para o próximo tutorial para saber como transmitir dados em tempo real para o Azure Databricks com o Event Hubs do Azure.

> [!div class="nextstepaction"]
>[Stream dados para o Azure Databricks com Hubs de eventos](../../azure-databricks/databricks-stream-from-eventhubs.md)
