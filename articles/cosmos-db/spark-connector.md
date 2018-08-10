---
title: Ligar o Apache Spark para Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre o conector do Spark do Azure Cosmos DB permite-lhe ligar o Apache Spark para Azure Cosmos DB. Pode efetuar agregações distribuídas no sistema de banco de dados multi-inquilino e distribuída globalmente da Microsoft.
keywords: o Apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: 57ad621006a79148aaa6e9435d6ede446539c648
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714758"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Acelerar a análise de macrodados utilizando o Apache Spark para o conector do Azure Cosmos DB
 
O Apache Spark para o conector do Azure Cosmos DB permite que o Azure Cosmos DB para ser uma entrada ou saída para tarefas do Apache Spark. Ligar [Spark](http://spark.apache.org/) ao [do Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) acelerará a sua capacidade para resolver problemas de ciência de dados rápida evolução. Pode utilizar o Azure Cosmos DB para manter e consultar dados rapidamente. O conector utiliza forma eficiente os índices geridos do Azure Cosmos DB nativos. Os índices permitirá colunas atualizáveis ao realizar análises e filtragem contra dados distribuídos globalmente, mudança de predicado de push pendente. Esse tipo de dados pode variar de Internet das coisas (IoT) para cenários de análise e ciência de dados.

Saiba mais sobre o conector neste vídeo:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

## <a name="connector-components"></a>Componentes de conector

O Spark para o conector do Azure Cosmos DB tem os seguintes componentes:

* [O Azure Cosmos DB](http://documentdb.com) permite-lhe aprovisionar e dimensionar o débito e armazenamento, em qualquer número de regiões geográficas.  

* [Apache Spark](http://spark.apache.org/) é um motor de processamento de código-fonte aberto poderosas, criado em torno de velocidade, facilidade de utilização e análise sofisticadas.  

* [Cluster do Apache Spark no Azure Databricks](https://docs.azuredatabricks.net/getting-started/index.html) permite-lhe executar tarefas do Spark no cluster do Spark.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Ligar o Apache Spark para Azure Cosmos DB

Existem duas abordagens para ligar o Apache Spark e o Azure Cosmos DB:

- [Azure Cosmos DB SQL Python SDK](https://github.com/Azure/azure-documentdb-python), um conector baseados em Python, que também é referido como *pyDocumentDB*.  

- [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java), um conector baseadas em Java.


**Versões suportadas**

| Componente | Versão |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x, 2.3.x |
| Scala|2.11|
| Versão de runtime do Azure Databricks | > 3.4 |
| Azure Cosmos DB SQL Java SDK | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Ligar com o Python ou pyDocumentDB SDK

O diagrama seguinte mostra a arquitetura da implementação do SDK do pyDocumentDB:

![Diagrama do Spark para o fluxo de dados do Azure Cosmos DB através de pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Fluxo de dados

Fluxo de dados da implementação do pyDocumentDB é o seguinte:

* O nó mestre do Spark liga-se para o nó de gateway do Azure Cosmos DB através de pyDocumentDB. Especifique apenas as ligações do Spark e o Azure Cosmos DB. Ligações para os respetivos nós mestre e de gateway são transparentes.  

* O nó de gateway torna a consulta na Azure Cosmos DB, onde a consulta é executada, em seguida, em relação a partições da coleção em nós de dados. A resposta para essas consultas é enviada para o nó de gateway e esse conjunto de resultados é devolvido para o nó mestre do Spark.  

* Consultas subsequentes (por exemplo, em relação a um Spark data frame) são enviadas para os nós de trabalho do Spark para processamento.  

Comunicação entre o Spark e o Azure Cosmos DB é limitada para o nó mestre do Spark e os nós de gateway do Azure Cosmos DB. As consultas passam o mais rápido que permite a camada de transporte entre estes dois nós.

Execute os seguintes passos para ligar o Spark para Azure Cosmos DB utilizando o pyDocumentDB SDK:

1. Criar uma [área de trabalho do Azure Databricks](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) e uma [cluster do Spark](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks). O Azure Databricks runtime versão 4.0 inclui o Apache Spark 2.3.0 e Scala 2.11 nessa área de trabalho.  

2. Quando o cluster é criado e está em execução, aceda a **área de trabalho** > **Create** > **biblioteca**.  
3. Na caixa de diálogo nova biblioteca, escolha **carregar ovo de Python ou PyPi** como origem. Fornecer **pydocumentdb** como o nome e selecione **biblioteca instalar**. o pyDocumentdb SDK já foi publicado para os pacotes de pip, para que possa encontrar e instalá-lo. 

   ![Captura de ecrã da caixa de diálogo nova biblioteca](./media/spark-connector/create-library.png)

4. Depois da biblioteca é instalada, anexá-lo para o cluster que criou anteriormente.  

5. Aceda a **área de trabalho** > **criar** > **bloco de notas**.  

6. Na **criar bloco de notas** caixa de diálogo, introduza um nome amigável de utilizador e escolha **Python** como a linguagem. Na lista pendente, selecione o cluster que criou anteriormente e selecione **criar**.  

7. Executar algumas consultas de Spark através de voos amostra dos dados hospedados em "doctorwho" conta do Azure Cosmos DB. (Esta conta está acessível ao público). O [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub aloja a versão HTML do bloco de notas. Transferir os ficheiros de repositório e aceda a `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html`. Pode importar o bloco de notas à sua conta do Azure Databricks e executá-lo. A secção seguinte explica a funcionalidade dos blocos de código em detalhes.

O fragmento de código seguinte mostra como importar o pyDocumentDB SDK e executar uma consulta no contexto do Spark. Conforme observado no fragmento de código, o pyDocumentDB SDK contém os parâmetros de ligação necessários para ligar a conta do Azure Cosmos DB. Importa as bibliotecas necessárias e configura a chave mestra e o anfitrião, para criar o cliente do Azure Cosmos DB (pydocumentdb.document_client).


```python
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]

# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)

```

Em seguida, pode executar consultas. O fragmento de código seguinte liga à coleção airports.codes na conta doctorwho e executa uma consulta para extrair as cidades de aeroporto no Estado norte-americano de Washington. 

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

```

Depois de executar a consulta, o resultado é um "query_iterable. QueryIterable"que é convertido numa lista de Python. Esta lista, por sua vez, é convertida para um Spark data frame. 

```python
# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(list(query))

# Show data
df.show()
```

### <a name="considerations-when-using-pydocumentdb-sdk"></a>Considerações ao utilizar o pyDocumentDB SDK
Ligar Spark para Azure Cosmos DB com o pyDocumentDB que SDK recomenda-se nos seguintes cenários:

* Que pretende utilizar o Python.  

* Retornando um relativamente pequeno conjunto de resultados do Azure Cosmos DB para Spark. Tenha em atenção que o conjunto de dados subjacente no Azure Cosmos DB pode ser muito grande, e estiver a aplicar os filtros ou em execução filtros de predicados contra a origem do Azure Cosmos DB.

## <a name="connect-by-using-the-java-sdk"></a>Ligar com o SDK de Java

O diagrama seguinte mostra a arquitetura de implementação do SDK de Java do Azure Cosmos DB SQL e dados se movem entre os nós de trabalho do Spark:

![Diagrama de fluxo de dados no Spark para o conector do Azure Cosmos DB](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Fluxo de dados

O fluxo de dados da implementação do SDK do Java é o seguinte:

* O nó mestre do Spark liga-se para o nó de gateway do Azure Cosmos DB para obter o mapa de partições. Especifique apenas as ligações de Spark e o Azure Cosmos DB. Ligações para os respetivos nós mestre e de gateway são transparentes.  

* Estas informações são fornecidas novamente para o nó mestre do Spark. Neste momento, deve ser capaz de analisar a consulta para determinar as partições e suas localizações no Azure Cosmos DB que precisa acessar.  

* Estas informações são transmitidas para os nós de trabalho do Spark.  

* Os nós de trabalho do Spark ligar as partições do Azure Cosmos DB diretamente, extrair e retornar os dados para as partições de Spark em nós de trabalho.  

Comunicação entre o Spark e o Azure Cosmos DB é significativamente mais rápido, uma vez que o movimento de dados é entre os nós de trabalho do Spark e os nós de dados do Azure Cosmos DB (partições). Neste exemplo, enviar alguns dados do Twitter de exemplo para a conta do Azure Cosmos DB e executar consultas do Spark ao utilizar esses dados. Utilize os seguintes passos para escrever dados de exemplo do Twitter ao Azure Cosmos DB:

1. Criar uma [conta API SQL do Azure Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) e [adicionar uma coleção](create-sql-api-dotnet.md#add-a-collection) para a conta.  

2. Transfira o [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) exemplo do GitHub. Utilize este feed para escrever dados de exemplo do Twitter ao Azure Cosmos DB.  

3. Abra uma linha de comandos e instalar os módulos Tweepy e pyDocumentdb ao executar os comandos seguintes:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Extraia os conteúdos de exemplo no feed do Twitter e abra o ficheiro config.py. Atualize os valores de masterKey, host, databaseId, collectionId e preferredLocations.  

5. Aceda a `http://apps.twitter.com/`e registar o script como um novo aplicativo de feed do Twitter. Depois de escolher um nome e uma aplicação para a sua aplicação, será fornecido com um **chave de consumidor, o segredo de consumidor, o token de acesso e o segredo de token de acesso**. Copie estes valores e atualizá-los no ficheiro config.py para fornecer o aplicativo o acesso programático ao Twitter.   

6. Guarde o ficheiro config.py. Abra uma linha de comandos e execute a aplicação Python utilizando o seguinte comando:

   ```bash
   Python driver.py
   ```

7. Ir para a coleção do Azure Cosmos DB no portal e certifique-se de que os dados do Twitter foi escritos para a coleção.

### <a name="find-and-attach-the-java-sdk-to-the-spark-cluster"></a>Localizar e anexar o SDK de Java para o cluster do Spark

1. Criar uma [área de trabalho do Azure Databricks](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) e uma [cluster do Spark](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks). O Azure Databricks runtime versão 4.0 inclui o Apache Spark 2.3.0 e Scala 2.11 nessa área de trabalho.  

2. Quando o cluster é criado e está em execução, aceda a **área de trabalho** > **Create** > **biblioteca**.  

3. Do **nova biblioteca** caixa de diálogo caixa, escolha **coordenada do Maven** como origem. Indique o valor da coordenada **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0**e selecione **criar biblioteca**. As dependências do Maven são resolvidas e o pacote é adicionado à sua área de trabalho. O formato de coordenadas de Maven anterior, 2.3.0 representa a versão do Spark, 2.11 representa a versão de Scala e 1.2.0 representa a versão do conector do Azure Cosmos DB. 

4. Depois da biblioteca é instalada, anexá-lo para o cluster que criou anteriormente. 

Este artigo demonstra o uso do conector do Spark SDK de Java nos seguintes cenários:

* Ler dados do Twitter a partir do Azure Cosmos DB.  

* Dados de leitura do Twitter que é de transmissão em fluxo para o Azure Cosmos DB.  

* Escreva os dados do Twitter ao Azure Cosmos DB. 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Ler dados do Twitter a partir do Azure Cosmos DB
 
Nesta secção, irá executar Spark consultas para ler um lote de dados do Twitter a partir do Azure Cosmos DB. O [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub aloja a versão HTML do bloco de notas. Transferir os ficheiros de repositório e aceda a `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html`. Pode importar o bloco de notas à sua conta do Azure Databricks e atualizar a conta URI, chave mestra, base de dados e nomes de coleções. Pode executar o bloco de notas ou criá-lo da seguinte forma:

1. Aceda à sua conta do Azure Databricks e selecione **área de trabalho** > **Create** > **bloco de notas**. 

2. Na **criar bloco de notas** caixa de diálogo, introduza um nome amigável de utilizador e escolha **Python** como a linguagem. Na lista pendente, selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, chave mestra, base de dados e valores de coleção para ligar à conta. Ler tweets com o comando spark.read.format().

   ```python
   # Configuration Map
   tweetsConfig = {
   "Endpoint" : "<Your Azure Cosmos DB endpoint>",
   "Masterkey" : "<Primary key of your Azure Cosmos DB account>",
   "Database" : "<Your Azure Cosmos DB database name>",
   "Collection" : "<Your Azure Cosmos DB collection name>", 
   "preferredRegions" : "East US",
   "SamplingRatio" : "1.0",
   "schema_samplesize" : "200000",
   "query_custom" : "SELECT c.id, c.created_at, c.user.screen_name, c.user.lang, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   }
   # Read Tweets
   tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()
   tweets.createOrReplaceTempView("tweets")
   #tweets.cache()

   ```

4. Execute a consulta para obter a contagem de tweets por hashtags diferentes dos dados em cache. 

   ```python
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

SDK de Java suporta os seguintes valores para o mapeamento de configuração: 

|Definição  |Descrição  |
|---------|---------|
|query_maxdegreeofparallelism  | Define o número de operações simultâneas de execução no lado do cliente durante a execução paralela da consulta. Se estiver definido para um valor maior que 0, isso limita o número de operações simultâneas para o valor atribuído. Se estiver definido como inferior a 0, o sistema decide automaticamente o número de operações simultâneas para executar. Como o conector mapeia cada partição de coleção com um executor, este valor não terá qualquer efeito sobre a operação de leitura.        |
|query_maxbuffereditemcount     |    Define o número máximo de itens que podem ser colocados em memória intermédia no lado do cliente durante a execução paralela da consulta. Se estiver definido para um valor maior que 0, limita o número de itens em buffer para o valor atribuído. Se estiver definido como inferior a 0, o sistema decide automaticamente o número de itens a memória intermédia.     |
|query_enablescan    |   Define a opção para ativar a análise nas consultas que não foi possível ser fornecidas, uma vez que a indexação foi excluído em caminhos de pedido.       |
|query_disableruperminuteusage  |  Desativa o pedido (RUs) de unidades / minuto capacidade para servir a consulta, se tiver aprovisionado regular RUs/segundo se esgota.       |
|query_emitverbosetraces   |   Define a opção para permitir consultas emitir os rastreios verbosos para investigação.      |
|query_pagesize  |   Define o tamanho da página de resultados de consulta para cada solicitação de consulta. Para otimizar o débito, utilize um tamanho de página grandes para reduzir o número de ida e volta ao obter os resultados.      |
|query_custom  |  Define a consulta do Azure Cosmos DB para substituir a consulta predefinida ao obter dados do Azure Cosmos DB. Tenha em atenção que, quando fornecer este valor, ele é utilizado em vez de uma consulta com enviadas predicados também.     |

Dependendo do cenário, deve usar valores de configuração diferentes para otimizar o desempenho e a taxa de transferência. Tenha em atenção que a chave de configuração está atualmente a maiúsculas e minúsculas e o valor de configuração é sempre uma cadeia de caracteres.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Dados de leitura do Twitter que é de transmissão em fluxo para o Azure Cosmos DB

Nesta seção, executa consultas para ler um feed de alterações de dados do Twitter de transmissão em fluxo do Spark. Enquanto executa as consultas nesta secção, certifique-se de que o feed de aplicação do Twitter está em execução e bombeando os dados para o Azure Cosmos DB. O [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub aloja a versão HTML do bloco de notas. Transferir os ficheiros de repositório e aceda a `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html`. Pode importar o bloco de notas à sua conta do Azure Databricks e atualizar a conta URI, chave mestra, base de dados e nomes de coleções. Pode executar o bloco de notas ou criá-lo da seguinte forma:

1. Aceda à sua conta do Azure Databricks e selecione **área de trabalho** > **Create** > **bloco de notas**.  

2. Na **criar bloco de notas** caixa de diálogo, introduza um nome amigável de utilizador e escolha **Scala** como a linguagem. Na lista pendente, selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, chave mestra, base de dados e valores de coleção para ligar à conta.

   ```scala
   // This script does the following:
   // - creates a structured stream from a Twitter feed CosmosDB collection (on top of change feed)
   // - get the count of tweets
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import org.codehaus.jackson.map.ObjectMapper
   import com.microsoft.azure.cosmosdb.spark.streaming._
   import java.time._

   val sourceConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB collection name>", 
   "ConnectionMode" -> "Gateway",
   "ChangeFeedCheckpointLocation" -> "/tmp",
   "changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Internal Count")
   ```
4. Iniciar a alteração de leitura do feed como um fluxo com o comando spark.readStream.format():

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. Inicie a consulta para a consola de transmissão em fluxo:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

SDK de Java suporta os seguintes valores para o mapeamento de configuração:

|Definição  |Descrição  |
|---------|---------|
|readchangefeed   |  Indica que o conteúdo da coleção é obtido a partir de Feed de alterações do cosmos DB. O valor predefinido é false.       |
|changefeedqueryname |   Uma cadeia de caracteres personalizada para identificar a consulta. O conector mantém um registo dos tokens de continuação de coleção para diferente alterar feed consultas em separado. Se readchangefeed for verdadeira, esta é uma configuração necessária que não é possível colocar o valor vazio.      |
|changefeedcheckpointlocation  |   Um caminho para o armazenamento de ficheiros local para manter os tokens de continuação em caso de falhas de nó.      |
|changefeedstartfromthebeginning  |  Define se o feed de alterações deve começar do início (true) ou do ponto atual (false). Por predefinição, começa com a atual (false).       |
|rollingchangefeed  |   Um valor Booleano indicando se a alteração feed deve ser da última consulta. O valor predefinido é false, o que significa que as alterações são contabilizadas a partir da primeiro leitura da coleção.      |
|changefeedusenexttoken  |   Um valor booleano para oferecer suporte a cenários de falha de processamento. Ele indica que o lote de feed de alterações atual foi manipuladas com elegância. Flexíveis distribuídas conjunto de dados deve utilizar os tokens de continuação seguintes para obter o lote subsequente de alterações.      |
| InferStreamSchema | Um valor booleano que indica se o esquema dos dados de transmissão em fluxo de amostragem no início da transmissão em fluxo. Por predefinição, este valor é definido como true. Se este parâmetro estiver definido como true e alterações de esquema dos dados de transmissão em fluxo depois dos dados são objeto de amostragem, propriedades recentemente adicionadas serão removidas na estrutura de dados de transmissão em fluxo. <br/><br/> Se pretender que o quadro de dados de transmissão em fluxo para ser independente de esquema, defina este parâmetro como false. Neste modo, o corpo dos documentos ler a partir do feed de alterações do Azure Cosmos DB são compactados numa propriedade de corpo. Esta propriedade é no resultante transmissão em fluxo data frame, além de valores de propriedade do sistema.
 |

### <a name="connection-settings"></a>Definições de ligação

SDK de Java suporta as seguintes definições de ligação:

|Definição  |Descrição  |
|---------|---------|
|connectionmode   |  Define o modo de ligação que o DocumentClient interno deve utilizar para comunicar com o Azure Cosmos DB. Valores permitidos são **DirectHttps** (valor predefinido) e **Gateway**. O modo de ligação DirectHttps encaminha os pedidos diretamente para as partições do CosmosDB e fornece uma vantagem de latência.       |
|connectionmaxpoolsize   |  Define o valor do tamanho do conjunto de ligação utilizada pelo DocumentClient interno. O valor predefinido é 100.       |
|connectionidletimeout  |  Define o valor de tempo limite, em segundos, das conexões ociosas. O valor predefinido é de 60.       |
|query_maxretryattemptsonthrottledrequests    |  Define o número máximo de tentativas. Utilize este valor em caso de falha do pedido devido a taxa de limitação no cliente. Se não for especificado, o valor predefinido é de 1000 tentativas de repetição.       |
|query_maxretrywaittimeinseconds   |  Conjuntos do tempo de repetição máximo em segundos. Por predefinição, é 1000 segundos.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Escrever os dados do Twitter ao Azure Cosmos DB 

Nesta secção, irá executar Spark consultas para escrever um lote de dados do Twitter para uma nova coleção na mesma base de dados. O [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub aloja a versão HTML do bloco de notas. Transferir os ficheiros de repositório e aceda a `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html`. Pode importar o bloco de notas à sua conta do Azure Databricks e atualizar a conta URI, chave mestra, base de dados e nomes de coleções. Pode executar o bloco de notas ou criá-lo da seguinte forma:

1. Aceda à sua conta do Azure Databricks e selecione **área de trabalho** > **Create** > **bloco de notas**.  

2. Na **criar bloco de notas** caixa de diálogo, introduza um nome amigável de utilizador e escolha **Scala** como a linguagem. Na lista pendente, selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, a chave mestra, a base de dados e valores de coleção para ligar à coleção de base de dados para ler e escrever dados do Twitter.

   ```scala
   %scala
   // Import Necessary Libraries
   import org.joda.time._
   import org.joda.time.format._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.config.Config

   // Maps
   val readConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000",
   "query_custom" -> "SELECT c.id, c.created_at, c.user.screen_name, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   )
   val writeConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000"
   ) 

   // Configs
   // get read
   val readConfig = Config(readConfigMap)
   val tweets = spark.read.cosmosDB(readConfig)
   tweets.createOrReplaceTempView("tweets")
   tweets.cache()

   // get write
   val writeConfig = Config(writeConfigMap)
   ```
4. Execute a consulta para obter a contagem de tweets por hashtags diferentes dos dados em cache. 

   ```scala
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

5. Crie um novo quadro de dados de etiquetas de tweets e guarde os dados para a nova coleção. Depois de executar o código a seguir, pode, regresse ao portal e certifique-se de que os dados são escritos na coleção. 

   ```scala
   %scala
   // Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
   import org.apache.spark.sql.{Row, SaveMode, SparkSession}

   // Create new DataFrame of tweets tags
   val tweets_bytags = spark.sql("select '2018-06-12' as currdt, hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc limit 10")

   // Save to Cosmos DB (using Append in this case)
   // Ensure the baseConfig contains a Read-Write Key
   // The key provided in our examples is a Read-Only Key

   tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
   ```

SDK de Java suporta os seguintes valores para o mapeamento de configuração:

|Definição  |Descrição  |
|---------|---------|
| BulkImport | Um valor booleano que indica se os dados devem ser importados com a biblioteca do BulkExecutor. Por predefinição, este valor é definido como true. |
|WritingBatchSize  |   Indica o tamanho do lote para utilizar quando estiver a escrever dados para a coleção do Azure Cosmos DB. <br/><br/> Se o parâmetro de BulkImport estiver definido como true, WritingBatchSize parâmetro indica o tamanho do lote fornecido como entrada para a API de importAll da biblioteca do BulkExecutor de documentos. Por predefinição, este valor é definido para 100 mil. <br/><br/> Se BulkImport parâmetro estiver definido como false, WritingBatchSize parâmetro indica o tamanho do lote para utilizar quando estiver a escrever para a coleção do Azure Cosmos DB. O conector envia pedidos de createDocument e upsertDocument de forma assíncrona no batch. Quanto maior for o tamanho de lote, o débito mais é possível obter, desde que os recursos de cluster estão disponíveis. Por outro lado, especifique um tamanho mais pequeno número de batch para limitar a taxa de e o consumo de RU. Por predefinição, a escrita de tamanho de lote é definido como 500.  |
|Upsert   |  Uma cadeia de valor booleano que indica se deve utilizar upsertDocument em vez de CreateDocument quando estiver a escrever para a coleção do Azure Cosmos DB.   |
| WriteThroughputBudget |  Uma cadeia de caracteres de número inteiro que representa o número de RU\s que pretende alocar para a tarefa de Spark de ingestão em massa, fora do débito total alocado à coleção. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Escrever os dados do Twitter que é a transmissão em fluxo para o Azure Cosmos DB 

Nesta seção, executa consultas para escrever um feed de alterações de dados do Twitter para uma nova coleção na mesma base de dados de transmissão em fluxo do Spark. O [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub aloja a versão HTML do bloco de notas. Transferir os ficheiros de repositório e aceda a `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html`. Pode importar o bloco de notas à sua conta do Azure Databricks e atualizar a conta URI, chave mestra, base de dados e nomes de coleções. Pode executar o bloco de notas ou criá-lo da seguinte forma:

1. Aceda à sua conta do Azure Databricks e selecione **área de trabalho** > **Create** > **bloco de notas**.  

2. Na **criar bloco de notas** caixa de diálogo, introduza um nome amigável de utilizador e escolha **Scala** como a linguagem. Na lista pendente, selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, a chave mestra, a base de dados e valores de coleção para ligar à coleção de base de dados para ler e escrever dados do Twitter.

   ```scala
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import com.microsoft.azure.cosmosdb.spark.streaming._

   // Configure connection to Azure Cosmos DB Change Feed (Trades)
   val ConfigMap = Map(
   // Account settings
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   // Change feed settings
   "ReadChangeFeed" -> "true",
   "ChangeFeedStartFromTheBeginning" -> "true",
   "ChangeFeedCheckpointLocation" -> "dbfs:/cosmos-feed",
   "ChangeFeedQueryName" -> "Structured Stream Read",
   "InferStreamSchema" -> "true"
   )
   ```
4. Iniciar a alteração de leitura do feed como um fluxo com o comando spark.readStream.format():
 
   ```scala
   // Start reading change feed of trades as a stream
   var streamdata = spark
     .readStream
     .format(classOf[CosmosDBSourceProvider].getName)
     .options(ConfigMap)
     .load()
   ```

5. Definir a configuração de coleção de destino e iniciar a tarefa de transmissão em fluxo através do método writeStream.format():

   ```scala
   val sinkConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "checkpointLocation" -> "streamingcheckpointlocation6",
   "WritingBatchSize" -> "100",
   "Upsert" -> "true")

   // Start the stream writer
   val streamingQueryWriter = streamdata
    .writeStream
    .format(classOf[CosmosDBSinkProvider].getName)
    .outputMode("append")
    .options(sinkConfigMap)
    .start()
    ```

SDK de Java suporta os seguintes valores para o mapeamento de configuração:

|Definição  |Descrição  |
|---------|---------|
|Upsert   |  Uma cadeia de valor booleano que indica se deve utilizar upsertDocument em vez de CreateDocument quando estiver a escrever para a coleção do Azure Cosmos DB.   |
|checkpointlocation  |   Um caminho para o armazenamento de ficheiros local para manter os tokens de continuação em caso de falhas de nó.   |
|WritingBatchSize  |  Indica o tamanho do lote para utilizar ao escrever dados para a coleção do Azure Cosmos DB. O conector envia pedidos de createDocument e upsertDocument de forma assíncrona no batch. Quanto maior for o tamanho de lote, o débito mais é possível obter, desde que os recursos de cluster estão disponíveis. Por outro lado, especifique um tamanho mais pequeno número de batch para limitar a taxa de e o consumo de RU. Por predefinição, a escrita de tamanho de lote é definido como 500.  |


### <a name="considerations-when-using-java-sdk"></a>Considerações ao utilizar o SDK de Java

Ligar Spark para Azure Cosmos DB com o SDK de Java recomenda-se nos seguintes cenários:

* Que pretende utilizar o Python ou Scala.  

* Tem uma grande quantidade de dados a transferir entre Apache Spark e o Azure Cosmos DB. O SDK do Java executa melhor do que o pyDocumentDB. Para obter mais informações sobre a diferença de desempenho de consulta, consulte a [wiki de execuções de teste de consulta](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Passos Seguintes

Se ainda não o fez, transfira o Spark para o conector do Azure Cosmos DB a partir da [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark) repositório do GitHub. Explore os seguintes recursos adicionais no repositório:

* [Exemplos de agregações](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Scripts de exemplo e blocos de notas](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Pode também querer rever os [guia de conjuntos de dados, Apache Spark SQL e DataFrames](http://spark.apache.org/docs/latest/sql-programming-guide.html)e o [Apache Spark no Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artigo.
