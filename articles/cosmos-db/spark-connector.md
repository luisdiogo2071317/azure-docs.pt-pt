---
title: O Apache Spark a ligar ao Azure Cosmos DB | Documentos da Microsoft
description: Utilize este tutorial para saber mais sobre o conector do Spark do Azure Cosmos DB permite-lhe ligar Apache Spark para Azure Cosmos DB efetue globalmente distribuídas sciences agregações e esses dados no multi-inquilino distribuídas de sistema de banco de dados da Microsoft que concebido para a cloud.
keywords: o Apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: cae66a40882231f7762af29cdeaaf658dd2aa968
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113952"
---
# <a name="accelerate-real-time-big-data-analytics-by-using-the-spark-to-azure-cosmos-db-connector"></a>Acelerar a análise de macrodados em tempo real com o Spark para o conector do Azure Cosmos DB
 
O Spark para o conector do Azure Cosmos DB permite que o Azure Cosmos DB para agir como entrada ou saída para tarefas do Apache Spark. Ligar [Spark](http://spark.apache.org/) ao [do Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) acelerará a sua capacidade para resolver problemas de ciência de dados rápida evolução em que pode utilizar o Azure Cosmos DB para manter e consultar dados rapidamente. O Spark para o conector do Azure Cosmos DB com eficiência utiliza os índices geridos do Azure Cosmos DB nativos. Os índices permitirá colunas atualizáveis ao realizar análises e push filtragem de predicados contra mudança globalmente distribuído dados, que variam de Internet das coisas (IoT) para cenários de análise e ciência de dados.

Saiba mais sobre o Spark para o conector do Azure Cosmos DB neste vídeo:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

## <a name="connector-components"></a>Componentes de conector

O Spark para o conector do Azure Cosmos DB utiliza os seguintes componentes:

* [O Azure Cosmos DB](http://documentdb.com) permite aos clientes aprovisionar e dimensionar o débito e armazenamento em qualquer número de regiões geográficas.  

* [Apache Spark](http://spark.apache.org/) é um motor de processamento de código-fonte aberto poderosa que é criado em torno de velocidade, facilidade de utilização e análise sofisticadas.  

* [Cluster do Apache Spark no Azure Databricks](https://docs.azuredatabricks.net/getting-started/index.html) para executar tarefas do spark no cluster do spark.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Ligar o Apache Spark para Azure Cosmos DB

Existem duas abordagens para ligar o Apache Spark e o Azure Cosmos DB:

1. Usando [SDK de Python do Azure Cosmos DB SQL](https://github.com/Azure/azure-documentdb-python), spark baseados em Python para o conector do Cosmos DB, que também é referido como "pyDocumentDB".  

2. Ao utilizar o [SDK de Java do Azure Cosmos DB SQL](https://github.com/Azure/azure-documentdb-java) spark baseadas em Java para o conector do Cosmos DB.


**Versões suportadas**

| Componente | Versão |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x, 2.3.x |
| Scala|2.11|
| Versão de runtime do Databricks | > 3.4 |
| Azure Cosmos DB SQL Java SDK | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Ligar com o Python ou pyDocumentDB SDK

A imagem seguinte mostra a arquitetura de implementação do SDK pyDocumentDB:

![Fluxo de dados do Azure Cosmos DB através de pyDocumentDB DB do spark](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Fluxo de dados

Fluxo de dados da implementação do pyDocumentDB é o seguinte:

* O nó mestre do spark liga-se para o nó de gateway do Azure Cosmos DB através de pyDocumentDB. Um utilizador Especifica o spark e apenas as ligações do Azure Cosmos DB. Ligações para os respetivos nós mestre e de gateway são transparentes ao usuário.  

* O nó de gateway torna a consulta na Azure Cosmos DB em que a consulta é executada, em seguida, em relação a partições da coleção em nós de dados. A resposta para essas consultas é enviada para o nó de gateway e esse conjunto de resultados é devolvido para o nó mestre do spark.  

* Consultas subsequentes (por exemplo, em relação a um spark data frame) são enviadas para os nós de trabalho do Spark para processamento.  

Comunicação entre o spark e o Azure Cosmos DB é limitada para o nó mestre do spark e os nós de gateway do Azure Cosmos DB. As consultas passam o mais rápido que permite a camada de transporte entre estes dois nós.

Execute os seguintes passos para ligar o spark para Azure Cosmos DB utilizando o pyDocumentDB SDK:

1. Criar uma [área de trabalho do Azure Databricks](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) e uma [cluster do spark](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (Databricks runtime versão 4.0 (inclui Apache Spark 2.3.0, Scala 2.11) nessa área de trabalho.  

2. Quando o cluster é criado e está em execução, navegue até **área de trabalho** > **Create** > **biblioteca**.  
3. Na caixa de diálogo nova biblioteca, escolha **carregar ovo de Python ou PyPi** como a origem, fornecer **pydocumentdb** como o nome e selecione **instalar biblioteca**. SDK de PyDocumentdb já está publicado para os pacotes de pip para que possa encontrá-lo e instalar. 

   ![Criar e anexar biblioteca](./media/spark-connector/create-library.png)

4. Depois da biblioteca é instalada, anexá-lo para o cluster que criou anteriormente.  

5. Em seguida, navegue para o **área de trabalho** > **Create** > **bloco de notas**.  

6. Na **criar bloco de notas** caixa de diálogo caixa, introduza um nome amigável de utilizador, escolha **Python** como a linguagem. No menu pendente selecione o cluster que criou anteriormente e selecione **criar**.  

7. A simplicidade do transporte de comunicações torna a execução de uma consulta do spark para Azure Cosmos DB com o pyDocumentDB relativamente simples. Em seguida será executado poucas consultas do spark ao utilizar os dados de exemplo de voos hospedado em "doctorwho" conta do Cosmos DB, que é acessível publicamente. A versão HTML do bloco de notas é alojada na [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub. Deve transferir os ficheiros de repositório e navegue para `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html` pode importar o bloco de notas à sua conta do Azure Databricks e executá-lo. A secção seguinte explica a funcionalidade dos blocos de código no detalhadas.

O fragmento de código seguinte mostra como importar o pyDocumentDB SDK e executar uma consulta no contexto do spark. Conforme observado no fragmento de código, o pyDocumentDB SDK contém os parâmetros de ligação necessários para ligar a conta do Azure Cosmos DB. Ela importa as bibliotecas necessárias, configura a chave mestra e o anfitrião para criar o cliente do Azure Cosmos DB (pydocumentdb.document_client).


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

Em seguida, pode executar consultas, o fragmento de código seguinte liga à coleção airports.codes na conta DoctorWho e executa uma consulta para extrair as cidades de aeroporto no Estado norte-americano de Washington. 

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

Depois da consulta é executada, o resultado é um "query_iterable. QueryIterable"que é convertido numa lista de Python, que depois é convertida para um spark data frame. 

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

## <a name="considerations-when-using-pydocumentdb-sdk"></a>Considerações ao utilizar o pyDocumentDB SDK
Ligar spark para Azure Cosmos DB com o pyDocumentDB que SDK recomenda-se nos seguintes cenários:

* Que pretende utilizar o Python.  

* Retornando um relativamente pequeno conjunto de resultados do Azure Cosmos DB para spark. Tenha em atenção que o conjunto de dados subjacente no Azure Cosmos DB pode ser muito grande e estiver a aplicar os filtros ou em execução filtros de predicados contra a origem do Azure Cosmos DB.

## <a name="connect-by-using-the-java-sdk"></a>Ligar com o SDK de Java

A imagem seguinte mostra a arquitetura de implementação do SDK de Java do Azure Cosmos DB SQL e dados se movem entre os nós de trabalho do spark:

![Fluxo de dados no Spark para o conector do Azure Cosmos DB](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Fluxo de dados

O fluxo de dados da implementação do SDK do Java é o seguinte:

* O nó mestre do spark liga-se para o nó de gateway do Azure Cosmos DB para obter o mapa de partições. Um utilizador especifica apenas o spark e ligações de Azure Cosmos DB. Ligações para os respetivos nós mestre e de gateway são transparentes ao usuário.  

* Estas informações são fornecidas novamente para o nó mestre do spark. Neste momento, deve ser capaz de analisar a consulta para determinar as partições e suas localizações no Azure Cosmos DB que precisa acessar.  

* Estas informações são transmitidas para os nós de trabalho do spark.  

* Ligue os nós de trabalho do spark para as partições do Azure Cosmos DB diretamente para extrair os dados e retornar os dados para as partições de spark em nós de trabalho.  

A comunicação entre o spark e o Azure Cosmos DB é significativamente mais rápida devido aos dados de movimento é entre os nós de trabalho do spark e os nós de dados do Azure Cosmos DB (partições). Neste documento, irá enviar alguns dados do twitter de exemplo para a conta do Azure Cosmos DB e executar consultas do spark com esses dados. Utilize os seguintes passos para escrever dados de exemplo do Twitter ao Azure Cosmos DB:

1. Criar uma [conta API SQL do Azure Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) e [adicionar uma coleção](create-sql-api-dotnet.md#add-a-collection) para a conta.  

2. Transfira o [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) exemplo do GitHub, o que é utilizada para escrever dados de exemplo do Twitter ao Azure Cosmos DB.  

3. Abra uma linha de comandos e instale Tweepy e pyDocumentdb módulos ao executar os comandos seguintes:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Extraia o conteúdo do exemplo de feed do Twitter e abra o ficheiro config.py. Atualize os valores de masterKey, host, databaseId, collectionId e preferredLocations.  

5. Navegue para `http://apps.twitter.com/` e registar o script como um novo aplicativo de feed do Twitter. Depois de escolher um nome e uma aplicação para a sua aplicação, será fornecido com um **chave de consumidor, o segredo de consumidor, o token de acesso e o segredo de token de acesso**. Copie estes valores e atualizá-los no ficheiro config.py para fornecer o aplicativo o acesso programático ao Twitter.   

6. Guarde o ficheiro config.py. Linha de comandos aberta e executar a aplicação python utilizando o seguinte comando:

   ```bash
   Python driver.py
   ```

7. Navegue para a coleção do Azure Cosmos DB no portal e certifique-se de que os dados do twitter foi escritos para a coleção.

### <a name="find-and-attach-java-sdk-to-the-spark-cluster"></a>Localizar e anexar o SDK de Java para o cluster do spark

1. Criar uma [área de trabalho do Azure Databricks](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) e uma [cluster do spark](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (Databricks runtime versão 4.0 (inclui Apache Spark 2.3.0, Scala 2.11) nessa área de trabalho.  

2. Quando o cluster é criado e está em execução, navegue até **área de trabalho** > **Create** > **biblioteca**.  

3. Na caixa de diálogo nova biblioteca, escolha **coordenada do Maven** como a origem, indique o valor da coordenada **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0**e selecione  **Criar a biblioteca**. As dependências do Maven são resolvidas e o pacote é adicionado à sua área de trabalho. O formato de coordenadas de maven acima, 2.3.0 representa a versão do spark, 2.11 representa a versão de scala e 1.2.0 representa a versão do conector do Azure Cosmos DB. 

4. Depois da biblioteca é instalada, anexá-lo para o cluster que criou anteriormente. 

Este artigo demonstra o uso do conector do spark SDK de Java nos seguintes cenários:

* Ler dados do twitter a partir do Azure Cosmos DB  

* Ler dados do twitter que é a transmissão em fluxo para o Azure Cosmos DB  

* Escrever os dados do twitter ao Azure Cosmos DB 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Ler dados do twitter a partir do Azure Cosmos DB
 
Nesta secção, vai executar consultas do spark para ler um lote de dados do Twitter a partir do Azure Cosmos DB. A versão HTML do bloco de notas é alojada na [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub. Deve transferir os ficheiros de repositório e navegue para `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html` pode importar o bloco de notas à sua conta do Azure Databricks, atualizar a conta de URI, chave mestra, base de dados, nomes de coleções e executá-la ou pode criar o bloco de notas da seguinte forma:

1. Navegue até à sua conta do Azure Databricks e selecione o **área de trabalho** > **Create** > **bloco de notas**. 

2. Na **criar bloco de notas** caixa de diálogo caixa, introduza um nome amigável de utilizador, escolha **Python** como o idioma, no menu pendente selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, a chave mestra, a base de dados e coleção de valores para ligar à conta e ler os tweets com o comando spark.read.format().

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
|query_enablescan    |   Define a opção para ativar a análise nas consultas que não foi possível ser apresentadas porque a indexação foi excluído em caminhos de pedido.       |
|query_disableruperminuteusage  |  Desativa a unidades de pedido (RUs) / minuto a capacidade necessária para servir a consulta se regular aprovisionado RUs/segundo está esgotado.       |
|query_emitverbosetraces   |   Define a opção para permitir consultas emitir os rastreios verbosos para investigação.      |
|query_pagesize  |   Define o tamanho da página de resultados de consulta para cada solicitação de consulta. Para otimizar a taxa de transferência, utilize um tamanho de página grandes para reduzir o número de ida e volta ao obter os resultados de consultas.      |
|query_custom  |  Define a consulta do Azure Cosmos DB para substituir a consulta predefinida ao obter dados do Azure Cosmos DB. Tenha em atenção que quando este valor é fornecido, será utilizado em vez de uma consulta com enviadas predicados também.     |

Dependendo do cenário, os valores de configuração diferentes devem de ser utilizados para otimizar o desempenho e a taxa de transferência. Tenha em atenção que a chave de configuração está atualmente a maiúsculas e minúsculas e o valor de configuração é sempre uma cadeia de caracteres.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Ler dados do twitter que é a transmissão em fluxo para o Azure Cosmos DB

Nesta secção, vai executar consultas do spark para ler um feed de alterações de transmissão em fluxo de dados do twitter. Enquanto executa as consultas nesta secção, certifique-se de que o feed de aplicação do Twitter está em execução e bombeando os dados para o Azure Cosmos DB. A versão HTML do bloco de notas é alojada na [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub. Deve transferir os ficheiros de repositório e navegue para `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html` pode importar o bloco de notas à sua conta do Azure Databricks, atualizar a conta de URI, chave mestra, base de dados, nomes de coleções e executá-la ou pode criar o bloco de notas da seguinte forma:

1. Navegue até à sua conta do Azure Databricks e selecione o **área de trabalho** > **Create** > **bloco de notas**.  

2. Na **criar bloco de notas** caixa de diálogo caixa, introduza um nome amigável de utilizador, escolha **Scala** como o idioma, no menu pendente selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, a chave mestra, a base de dados e coleção de valores para ligar à conta do.

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
4. Inicie o feed como um fluxo com o comando spark.readStream.format() de alterações de leitura:

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
|changefeedqueryname |   Uma cadeia de caracteres personalizada para identificar a consulta. O conector mantém um registo dos tokens de continuação de coleção para diferente alterar feed consultas em separado. Se readchangefeedis true, esta é uma configuração necessária que não é possível colocar o valor vazio.      |
|changefeedcheckpointlocation  |   Um caminho para o armazenamento de ficheiros local para manter os tokens de continuação em caso de falhas de nó.      |
|changefeedstartfromthebeginning  |  Define se o feed de alterações deve começar do início (true) ou do ponto atual (false). Por predefinição, começa com a atual (false).       |
|rollingchangefeed  |   Um valor Booleano indicando se a alteração feed deve ser da última consulta. O valor predefinido é false, o que significa que as alterações serão contadas da primeira leitura da coleção.      |
|changefeedusenexttoken  |   Um valor booleano para oferecer suporte a cenários de falha de processamento. Ele é usado para indicar que o lote de feed de alterações atual tem sido manipuladas com elegância e o RDD deve utilizar os tokens de continuação seguintes para obter o lote subsequente de alterações.      |
| InferStreamSchema | Um valor booleano que indicados se o esquema dos dados de transmissão em fluxo de amostragem no início da transmissão em fluxo. Por predefinição, este valor é definido como true. Se este parâmetro estiver definido como true e alterações de esquema dos dados de transmissão em fluxo depois dos dados são objeto de amostragem, propriedades recentemente adicionadas serão removidas na estrutura de dados de transmissão em fluxo. <br/><br/> Se pretender que o quadro de dados de transmissão em fluxo para ser independente de esquema, defina este parâmetro como false. Neste modo, o corpo dos documentos ler a partir do feed de alterações do Azure Cosmos DB são compactados numa propriedade de "corpo" na estrutura de dados transmissão em fluxo resultante além de valores de propriedade do sistema.
 |

### <a name="connection-settings"></a>Definições de ligação

SDK de Java suporta as seguintes definições de ligação:

|Definição  |Descrição  |
|---------|---------|
|connectionmode   |  Define o modo de ligação que o DocumentClient interno deve utilizar para comunicar com o Azure Cosmos DB. Valores permitidos são **DirectHttps** (valor predefinido) e **Gateway**. O modo de ligação DirectHttps encaminha os pedidos diretamente para as partições do CosmosDB e fornece uma vantagem de latência.       |
|connectionmaxpoolsize   |  Define o valor do tamanho do conjunto de ligação utilizada pelo DocumentClient interno. O valor predefinido é 100.       |
|connectionidletimeout  |  Define o valor de tempo limite para conexões ociosas em segundos. O valor predefinido é de 60.       |
|query_maxretryattemptsonthrottledrequests    |  Define o número máximo de tentativas. Este valor é utilizado em caso de falha do pedido devido a taxa de limitação no cliente. Se não for especificado, o valor predefinido é de 1000 tentativas de repetição.       |
|query_maxretrywaittimeinseconds   |  Conjuntos do tempo de repetição máximo em segundos. Por predefinição, é 1000 segundos.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Escrever os dados do twitter ao Azure Cosmos DB 

Nesta secção, vai executar consultas do spark para escrever um lote de dados do twitter para uma nova coleção na mesma base de dados. A versão HTML do bloco de notas é alojada na [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub. Deve transferir os ficheiros de repositório e navegue para `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html` pode importar o bloco de notas à sua conta do Azure Databricks, atualizar a conta de URI, chave mestra, base de dados, nomes de coleções e executá-la ou pode criar o bloco de notas da seguinte forma:

1. Navegue até à sua conta do Azure Databricks e selecione o **área de trabalho** > **Create** > **bloco de notas**.  

2. Na **criar bloco de notas** caixa de diálogo caixa, introduza um nome amigável de utilizador, escolha **Scala** como o idioma, no menu pendente selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, a chave mestra, a base de dados e coleção de valores para ligar à coleção de base de dados para ler e escrever dados do twitter.

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

5. Criar novo quadro de dados de etiquetas de tweets e guardar os dados para a nova coleção. Depois de executar o código a seguir, pode, regresse ao portal e certifique-se de que os dados são escritos na coleção. 

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
|WritingBatchSize  |   Indica o tamanho do lote para utilizar ao escrever dados para a coleção do Azure Cosmos DB. <br/><br/> Se o parâmetro de BulkImport estiver definido como true, WritingBatchSize parâmetro indica o tamanho do lote fornecido como entrada para a API de importAll da biblioteca do BulkExecutor de documentos. Por predefinição, este valor é definido para 100 mil. <br/><br/> Se BulkImport parâmetro estiver definido como false, WritingBatchSize parâmetro indica o tamanho do lote ser usada para criar a coleção do Azure Cosmos DB. O conector envia pedidos de createDocument/upsertDocument de forma assíncrona no batch. Quanto maior for o lote dimensione o débito mais que podemos conseguir, desde que os recursos de cluster estão disponíveis. Por outro lado, especifique um tamanho mais pequeno número de batch para limitar a taxa de e o consumo de RU. Por predefinição, a escrita de tamanho de lote é definido como 500.  |
|Upsert   |  Uma cadeia de valor booleano que indica se upsertDocument deve ser utilizado em vez de CreateDocument ao escrever a coleção do cosmos DB.   |
| WriteThroughputBudget |  Uma cadeia de caracteres de número inteiro que representa o número de RU\s que pretende alocar para a tarefa do spark de ingestão em massa fora o débito total alocado à coleção. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Escrever os dados do twitter que é a transmissão em fluxo para o Azure Cosmos DB 

Nesta secção, vai executar consultas do spark para escrever o feed de alterações de dados do twitter para uma nova coleção na mesma base de dados de transmissão em fluxo. A versão HTML do bloco de notas é alojada na [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) repositório do GitHub. Deve transferir os ficheiros de repositório e navegue para `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html` pode importar o bloco de notas à sua conta do Azure Databricks, atualizar a conta de URI, chave mestra, base de dados, nomes de coleções e executá-la ou pode criar o bloco de notas da seguinte forma:

1. Navegue até à sua conta do Azure Databricks e selecione o **área de trabalho** > **Create** > **bloco de notas**.  

2. Na **criar bloco de notas** caixa de diálogo caixa, introduza um nome amigável de utilizador, escolha **Scala** como o idioma, no menu pendente selecione o cluster que criou anteriormente e selecione **criar**.  

3. Atualize o ponto final, a chave mestra, a base de dados e coleção de valores para ligar à coleção de base de dados para ler e escrever dados do twitter.

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
4. Inicie o feed como um fluxo com o comando spark.readStream.format() de alterações de leitura:
 
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
|Upsert   |  Uma cadeia de valor booleano que indica se upsertDocument deve ser utilizado em vez de CreateDocument ao escrever a coleção do cosmos DB.   |
|checkpointlocation  |   Um caminho para o armazenamento de ficheiros local para manter os tokens de continuação em caso de falhas de nó.   |
|WritingBatchSize  |  Indica o tamanho do lote para utilizar ao escrever dados para a coleção do Azure Cosmos DB. O conector envia pedidos de createDocument/upsertDocument de forma assíncrona no batch. Quanto maior for o lote dimensione o débito mais que podemos conseguir, desde que os recursos de cluster estão disponíveis. Por outro lado, especifique um tamanho mais pequeno número de batch para limitar a taxa de e o consumo de RU. Por predefinição, a escrita de tamanho de lote é definido como 500.  |


## <a name="considerations-when-using-java-sdk"></a>Considerações ao utilizar o SDK de Java

Spark ao ligar ao Azure Cosmos DB com Java SDK recomenda-se nos seguintes cenários:

* Que pretende utilizar o Python e/ou Scala.  

* Se tiver uma grande quantidade de dados a transferir entre Apache Spark e o Azure Cosmos DB, o SDK de Java tem um desempenho superior em comparação com o pyDocumentDB. Para dar uma idéia sobre a diferença de desempenho de consulta, consulte a [wiki de execuções de teste de consulta](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Passos Seguintes

Se ainda não o fez, transfira o Spark para o conector do Azure Cosmos DB a partir da [azure-cosmos DB-spark](https://github.com/Azure/azure-cosmosdb-spark) repositório do GitHub e explorar os recursos adicionais no repositório:

* [Exemplos de agregações distribuída](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Scripts de exemplo e blocos de notas](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Pode também querer rever os [guia de conjuntos de dados, Apache Spark SQL e DataFrames](http://spark.apache.org/docs/latest/sql-programming-guide.html) e o [Apache Spark no Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artigo.
