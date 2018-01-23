---
title: Arquitetura de lambda com a base de dados do Azure Cosmos e o HDInsight (Apache Spark) | Microsoft Docs
description: "Este artigo descreve como implementar uma arquitetura de lambda através do Azure Cosmos DB, HDInsight e Spark"
keywords: arquitetura de lambda
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: 273aeae9-e31c-4a43-b216-5751c46f212e
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 01/19/2018
ms.author: denlee
ms.openlocfilehash: f88f3fb05495b0f3330d5a4cde7718fe89b2f694
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="azure-cosmos-db-implement-a-lambda-architecture-on-the-azure-platform"></a>Azure Cosmos DB: Implementar uma arquitetura de lambda na plataforma do Azure 

Arquiteturas de lambda ativar o processamento de dados eficiente em grande escala de conjuntos de dados. Arquiteturas de lambda utilizam processamento em lote, processamento de fluxo e uma camada para minimizar a latência envolvidos na consulta de macrodados. 

Para implementar uma arquitetura de lambda no Azure, pode combinar as seguintes tecnologias para acelerar a análise de macrodados em tempo real:
* [BD do Azure do Cosmos](https://azure.microsoft.com/services/cosmos-db/), primeiro serviço de base de dados globalmente distribuída e múltiplos modelo da indústria. 
* [Apache Spark para o Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/), uma arquitetura de processamento que executa aplicações de análise de dados em grande escala
* BD do Azure do Cosmos [alterar feed](change-feed.md), que as sequências novos dados para a camada de batch para o HDInsight processar
* O [Spark para o conector Azure Cosmos DB](spark-connector.md)

Este artigo descreve as noções básicas sobre uma arquitetura de lambda com base na estrutura de camada multi original e as vantagens de uma arquitetura de lambda "rearchitected" simplifica operações.  

Para uma descrição geral da arquitetura de lambda e os recursos disponíveis no exemplo de arquitetura lambda, veja o vídeo seguinte:

> [!VIDEO https:///channel9.msdn.com/Events/Connect/2017/T135/player]
>

## <a name="what-is-a-lambda-architecture"></a>O que é uma arquitetura de lambda?
Uma arquitetura de lambda é um genérico, dimensionável, e dados com tolerância a falhas de arquitetura para o endereço de processamento de lote e velocidade cenários de latência, conforme descrito pelo [Nathan Marz](https://twitter.com/nathanmarz).

![Diagrama que mostra uma arquitetura de lambda](./media/lambda-architecture/lambda-architecture-intro.png)

Origem: http://lambda-architecture.net/

Estes princípios básicos de uma arquitetura de lambda são descritos no diagrama anterior, como por [https://lambda-architecture.net](http://lambda-architecture.net/).

 1. Todos os **dados** é feita para *ambos* o *camada batch* e *camada velocidade*.
 2. O **camada batch** tem um conjunto de dados principal (imutável acrescentar só de conjunto de dados não processados) e previamente calcula as vistas de batch.
 3. O **servir camada** possui as vistas de batch para consultas rápidas. 
 4. O **camada velocidade** compensates por período de tempo de processamento (para a camada de servir) e lida com apenas dados recentes.
 5. Todas as consultas podem ser respondidas por intercalar os resultados de vistas do batch e vistas em tempo real ou efetuando o ping-los individualmente.

Após ler mais, iremos será possível implementar esta arquitetura utilizando apenas o seguinte:

* Collection(s) do Cosmos BD do Azure
* Cluster do HDInsight (Apache Spark 2.1)
* Spark conector [1.0](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark_2.1.0_2.11-1.0.0)

## <a name="speed-layer"></a>Camada de velocidade

De uma perspetiva de operações, a manter dois fluxos de dados garantindo o estado correto dos dados pode ser um endeavor complicada. Para simplificar as operações, utilizar o [alteração da base de dados do Azure Cosmos feed suporte](change-feed.md) para manter o estado do *camada batch* ao revelando o registo de alterações de base de dados do Azure Cosmos através o *alterar Feed API* para sua *camada velocidade*.  
![Diagrama de realce a novos dados, a camada de velocidade e parte do conjunto de dados principal da arquitetura de lambda](./media/lambda-architecture/lambda-architecture-change-feed.png)

O que é importante destas camadas:

 1. Todos os **dados** é feito o Push *apenas* na base de dados do Azure Cosmos, deste modo, pode evitar problemas de conversão de vários.
 2. O **camada batch** tem um conjunto de dados principal (imutável acrescentar só de conjunto de dados não processados) e previamente calcula as vistas de batch.
 3. O **servir camada** mencionado na secção seguinte.
 4. O **camada velocidade** utiliza o HDInsight (Apache Spark) para ler o feed de alteração de BD do Cosmos do Azure. Isto permite-lhe manter os dados, bem como a consulta e processá-la em simultâneo.
 5. Todas as consultas podem ser respondidas por intercalar os resultados de vistas do batch e vistas em tempo real ou efetuando o ping-los individualmente.
 
### <a name="code-example-spark-structured-streaming-to-an-azure-cosmos-db-change-feed"></a>Exemplo de código: Spark estruturados de transmissão em fluxo para uma alteração de base de dados do Azure Cosmos feed
Para executar um protótipo rápido da alteração feed como parte da base de dados do Azure Cosmos o **camada de velocidade**, pode testá-lo utilizando o Twitter dados como parte do [alterações de processamento de fluxo através do Azure Cosmos DB alterar do Feed e o Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)exemplo. Para iniciar a saída do Twitter, consulte o exemplo de código no [fluxo feed do Twitter da BD do Cosmos](https://github.com/tknandu/TwitterCosmosDBFeed). Com o exemplo anterior, estiver a carregar dados do Twitter na base de dados do Azure Cosmos e, em seguida, pode configurar o cluster do HDInsight (Apache Spark) para estabelecer ligação com a alteração do feed. Para obter mais informações sobre como configurar esta configuração, consulte [Apache Spark para o programa de configuração do conector do Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup).  

O fragmento de código seguinte mostra como configurar `spark-shell` para executar uma estruturados tarefa transmissão em fluxo para ligar a uma alteração de base de dados do Azure Cosmos feed, que analisa o fluxo em tempo real de dados do Twitter, para efetuar uma contagem contínua de intervalo.

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceConfigMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[MASTER KEY]",
"Database" -> "[DATABASE]",
"Collection" -> "[COLLECTION]",
"ConnectionMode" -> "Gateway",
"ChangeFeedCheckpointLocation" -> "checkpointlocation",
"changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Interval Count")

// Start reading change feed as a stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()

// Start streaming query to console sink
val query = streamData.withColumn("countcol", streamData.col("id").substr(0, 0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
```

Para exemplos de código completo, consulte [azure-cosmosdb-spark/lambda/samples](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda), incluindo:
* [Consulta de transmissão em fluxo do Cosmos DB alteração Feed.scala](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala)
* [Consulta de etiquetas de transmissão em fluxo do Cosmos DB alteração Feed.scala](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala)

O resultado é um `spark-shell` consola, que é continuamente executada uma tarefa de transmissão em fluxo estruturada que efetua uma contagem de intervalo contra os dados do Twitter da alteração da base de dados do Azure Cosmos feed. A imagem seguinte mostra a saída da tarefa de fluxo e o intervalo de contagens.

![Transmissão em fluxo de saída que mostra a contagem de intervalo contra os dados do Twitter do feed de alteração de base de dados do Azure Cosmos](./media/lambda-architecture/lambda-architecture-speed-layer-twitter-count.png) 

Para obter mais informações sobre a base de dados do Azure Cosmos alterar feed, consulte:

* [Trabalhar com a alteração de feed de suporte do BD Azure Cosmos](change-feed.md)
* [Introdução ao Azure alteração CosmosDB Feed biblioteca de processador](https://azure.microsoft.com/blog/introducing-the-azure-cosmosdb-change-feed-processor-library/)
* [Alterações de processamento de fluxo: Azure CosmosDB alterar feed + Apache Spark](https://azure.microsoft.com/blog/stream-processing-changes-azure-cosmosdb-change-feed-apache-spark/)

## <a name="batch-and-serving-layers"></a>Batch e que serve camadas
Uma vez que os dados da nova são carregados para o Azure Cosmos DB (onde o feed de alterações está a ser utilizado para a camada de velocidade), isto é onde o **conjunto de dados mestra** (um imutável acrescentar só de conjunto de dados não processados) reside. A partir deste ponto em diante, utilizar o HDInsight (Apache Spark) para executar as funções de computação de pré-lançamento do **camada batch** para **servir camada**, conforme mostrado na imagem seguinte:

![Diagrama de realce a camada de lote e que serve uma camada da arquitetura de lambda](./media/lambda-architecture/lambda-architecture-batch-serve.png)

O que é importante destas camadas:

 1. Todos os **dados** é instalada apenas para a base de dados do Azure Cosmos (para evitar problemas de multi-difusão).
 2. O **camada batch** tem um principal conjunto de dados (imutável acrescentar só de conjunto de dados não processados) armazenado na base de dados do Azure Cosmos. Utilizar o Spark HDI, pode pré-calcular as agregações para ser armazenada nas vistas de batch calculada.
 3. O **servir camada** é uma base de dados de base de dados do Azure Cosmos com coleções para o conjunto de dados mestra e calculada a vista de batch.
 4. O **camada velocidade** abordado posteriormente neste artigo.
 5. Todas as consultas podem ser respondidas por intercalar os resultados das vistas de lote e a vistas em tempo real ou efetuando o ping-los individualmente.

### <a name="code-example-pre-computing-batch-views"></a>Exemplo de código: previamente vistas de batch de computação
Para demonstramos como realizar previamente calculadas vistas na sua **conjunto de dados mestra** do Apache Spark BD do Cosmos do Azure, utilize os seguintes fragmentos de código dos blocos de notas [Lambda arquitetura Rearchitected - camada do Batch ](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) e [Lambda arquitetura Rearchitected - Batch para que serve camada](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb). Neste cenário, utilize os Twitter os dados armazenados na base de dados do Azure Cosmos.

Vamos começar por criar a ligação de configuração para os dados do Twitter dentro da BD do Cosmos Azure utilizando o código de PySpark abaixo.

```
# Configuration to connect to Azure Cosmos DB
tweetsConfig = {
  "Endpoint" : "[Endpoint URL]",
  "Masterkey" : "[Master Key]",
  "Database" : "[Database]",
  "Collection" : "[Collection]", 
  "preferredRegions" : "[Preferred Regions]",
  "SamplingRatio" : "1.0",
  "schema_samplesize" : "200000",
  "query_custom" : "[Cosmos DB SQL Query]"
}

# Create DataFrame
tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()

# Create Temp View (to run Spark SQL statements)
tweets.createOrReplaceTempView("tweets")
```

Em seguida, vamos executar a seguinte instrução de Spark SQL para determinar o hashtags 10 principais do conjunto de tweets. Para esta consulta de Spark SQL, iremos estiver a executar este um bloco de notas do Jupyter sem o gráfico de barras de saída diretamente seguir este fragmento de código.

```
%%sql
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

![Gráfico que mostra o número de tweets por hashtag](./media/lambda-architecture/lambda-architecture-batch-hashtags-bar-chart.png)

Agora que configurou a sua consulta, guardá-lo novamente para uma coleção vamos utilizando o conector do Spark para guardar os dados de saída para uma coleção diferente.  Neste exemplo, utilize Scala para demonstram a ligação. É semelhante ao anterior exemplo, criar a ligação de configuração para guardar o DataFrame de Spark do Apache para uma coleção de BD do Cosmos Azure diferente.

```
val writeConfigMap = Map(
    "Endpoint" -> "[Endpoint URL]",
    "Masterkey" -> "[Master Key]",
    "Database" -> "[Database]",
    "Collection" -> "[New Collection]", 
    "preferredRegions" -> "[Preferred Regions]",
    "SamplingRatio" -> "1.0",
    "schema_samplesize" -> "200000"
)

// Configuration to write
val writeConfig = Config(writeConfigMap)

```

Depois de especificar o `SaveMode` (que indica se pretende `Overwrite` ou `Append` documentos), crie um `tweets_bytags` DataFrame semelhante para a consulta de Spark SQL no exemplo anterior.  Com o `tweets_bytags` DataFrame criado, pode guardá-lo utilizando o `write` método utilizando especificado anteriormente `writeConfig`.

```
// Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
import org.apache.spark.sql.{Row, SaveMode, SparkSession}

// Create new DataFrame of tweets tags
val tweets_bytags = spark.sql("select hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc")

// Save to Cosmos DB (using Append in this case)
tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Esta última instrução agora tem guardado o DataFrame Spark para uma nova coleção de base de dados do Azure Cosmos; numa perspetiva de arquitetura do lambda, este é o **batch vista** dentro o **servir camada**.
 
#### <a name="resources"></a>Recursos

Para exemplos de código completo, consulte [azure-cosmosdb-spark/lambda/samples](vhttps://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda) , incluindo:
* Lambda arquitetura Rearchitected - Batch camada [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb)
* Lambda arquitetura Rearchitected - Batch a camada que serve [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb)

## <a name="speed-layer"></a>Camada de velocidade
Apontou anteriormente, utilizando a biblioteca de Feed de alteração do Azure Cosmos DB permite-lhe simplificam as operações entre as camadas de lote e velocidade. Nesta arquitetura, utilizar o Apache Spark (através do HDInsight) para efetuar o *estruturados de transmissão em fluxo* consultas em relação as dados. Também pode pretender manter temporariamente os resultados das suas consultas de transmissão em fluxo estruturados para que outros sistemas podem aceder a estes dados.

![Diagrama de realce a camada de velocidade da arquitetura de lambda](./media/lambda-architecture/lambda-architecture-speed.png)

Para fazer isto, crie uma coleção de base de dados do Azure Cosmos separada para guardar os resultados das suas consultas de transmissão em fluxo estruturados.  Isto permite-lhe ter acesso de outros sistemas estas informações não apenas do Apache Spark. Bem com a funcionalidade Time-to-Live (TTL) do Cosmos DB, pode configurar os documentos automaticamente será eliminado após um período de tempo definido.  Para obter mais informações sobre a funcionalidade de TTL de BD do Cosmos do Azure, consulte [expirar os dados em coleções de base de dados do Azure Cosmos automaticamente com TTL](time-to-live.md)

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceCollectionName = "[SOURCE COLLECTION NAME]"
val sinkCollectionName = "[SINK COLLECTION NAME]"

val configMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[COSMOSDB MASTER KEY]",
"Database" -> "[DATABASE NAME]",
"Collection" -> sourceCollectionName,
"ChangeFeedCheckpointLocation" -> "changefeedcheckpointlocation")

val sourceConfigMap = configMap.+(("changefeedqueryname", "Structured Stream replication streaming test"))

// Start to read the stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
val sinkConfigMap = configMap.-("collection").+(("collection", sinkCollectionName))

// Start the stream writer to new collection
val streamingQueryWriter = streamData.writeStream.format(classOf[CosmosDBSinkProvider].getName).outputMode("append").options(sinkConfigMap).option("checkpointLocation", "streamingcheckpointlocation")
var streamingQuery = streamingQueryWriter.start()

```

## <a name="lambda-architecture-rearchitected"></a>Arquitetura de lambda: Rearchitected
Conforme indicado nas secções anteriores, pode simplificar a arquitetura de lambda original utilizando os seguintes componentes:
* Azure Cosmos DB
* A biblioteca do Azure Cosmos DB alteração Feed para evitar a necessidade de multicast os dados entre as camadas de lote e velocidade
* Apache Spark no HDInsight
* O conector do Spark para o Azure Cosmos DB

![Diagrama que mostra o rearchitecture da arquitetura do lambda utilizando Azure Cosmos DB, Spark e a API de Feed de alteração do Azure Cosmos DB](./media/lambda-architecture/lambda-architecture-re-architected.png)

Com esta conceção, só precisa de dois serviços geridos, base de dados do Azure Cosmos e o HDInsight. Em conjunto, endereços de batch, servir e camadas de velocidade da arquitetura do lambda. Isto simplifica não apenas as operações, mas também o fluxo de dados. 
 1. Todos os dados são forçados na base de dados do Azure Cosmos para processamento
 2. A camada do batch tem um conjunto de dados principal (imutável acrescentar só de conjunto de dados não processados) e previamente calcula as vistas de batch
 3. A camada de possui as vistas de lote de dados para consultas rápidas.
 4. A camada de velocidade compensates por período de tempo de processamento (para a camada de servir) e lida com apenas dados recentes.
 5. Todas as consultas podem ser respondidas por intercalação resultados de vistas de lote e a vistas em tempo real.

### <a name="resources"></a>Recursos

 * **Novos dados**: O [fluxo feed do Twitter CosmosDB](https://github.com/tknandu/TwitterCosmosDBFeed), que é o mecanismo de push novos dados na base de dados do Azure Cosmos.
 * **Camada de lote:** a camada de batch é composta pelo *conjunto de dados mestra* (um imutável acrescentar só de conjunto de dados não processados) e a capacidade de computação previamente vistas de lote de dados que são enviadas por push para o **servir camada** .
    * O **Lambda arquitetura Rearchitected - Batch camada** bloco de notas [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) consultas a *conjunto de dados mestra* conjunto de vistas do batch.
 * **Camada de servir:** o **servir camada** é composto de dados previamente calculados, resultando em vistas de lote (por exemplo, agregações, segmentações de dados específicas, etc.) para consultas rápidas.
    * O **Lambda arquitetura Rearchitected - Batch a camada que serve** bloco de notas [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) pushes de dados de batch para a camada de servir; ou seja, Spark consulta uma coleção de batch de tweets, processa-os e armazena-a outra coleção (um lote calculado).
* **Camada de velocidade:** o **camada velocidade** é composto do Spark que utiliza a alteração da base de dados do Azure Cosmos feed para ler e atuar sobre imediatamente. Os dados também podem ser guardados no *calculada RT* para que outros sistemas podem consultar os dados em tempo real processados, por oposição a executar um em tempo real consultar próprios.
    * O [consulta de transmissão em fluxo do Cosmos DB alterar Feed](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala) scala script executa uma consulta de transmissão em fluxo a mudança de base de dados do Azure Cosmos feed calcular um número de intervalo a partir da shell de spark.
    * O [consulta de etiquetas de transmissão em fluxo do Cosmos DB alterar Feed](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala) scala script executa uma consulta de transmissão em fluxo a mudança de base de dados do Azure Cosmos feed calcular um número de intervalo das etiquetas da shell do spark.
  
## <a name="next-steps"></a>Passos Seguintes
Se ainda não o fez, transfira o Spark para o conector Azure Cosmos BD a partir do [cosmosdb-azure-spark](https://github.com/Azure/azure-cosmosdb-spark) repositório do GitHub e explorar os recursos adicionais no repositório:
* [Arquitetura de lambda](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)
* [Exemplos de agregações distribuída](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)
* [Scripts de exemplo e blocos de notas](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)
* [Demonstrações de transmissão em fluxo estruturadas](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)
* [Alteração feed demonstrações](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Fluxo de processamento de alterações com o Azure Cosmos DB alteração Feed e o Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)

Pode também querer rever o [Apache Spark SQL, DataFrames e conjuntos de dados guia](http://spark.apache.org/docs/latest/sql-programming-guide.html) e [Apache Spark no Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artigo.
