---
title: "Integração do Apache Spark com o Event Hubs do Azure | Microsoft Docs"
description: "Integrar com o Apache Spark para ativar a transmissão em fluxo estruturada com os Event Hubs"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: shvija;sethm
ms.openlocfilehash: 3b44c7e7387eceb2d9ea0b2c214b13a82869110f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Integração do Apache Spark com o Event Hubs do Azure

Os Hubs de eventos do Azure integra-se na perfeição com [Apache Spark](https://spark.apache.org/) tornar a criação de ponto a ponto distribuído a transmissão em fluxo de aplicações fácil. Esta integração suporta [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [transmissão em fluxo do Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html), [a transmissão em fluxo estruturada](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). O conector de Hubs de eventos para o Apache Spark não está disponível na [GitHub](https://github.com/Azure/azure-event-hubs-spark). Esta biblioteca também está disponível para utilização em projetos Maven a partir de [repositório Central Maven](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C).

Este artigo mostra como efetuar uma aplicação contínua [Azure Databrick](https://azure.microsoft.com/services/databricks/). Embora este artigo utiliza [Azure Databricks](https://azure.microsoft.com/services/databricks/), também estão disponíveis com Clusters do Spark [HDInsight](../hdinsight/spark/apache-spark-overview.md).

O exemplo seguinte utiliza dois blocos de notas Scala: um para transmissão em fluxo de eventos a partir de um hub de eventos e outra para o envio de eventos de regressar ao mesmo.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma, [criar uma conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Uma instância de Event Hubs. Se não tiver uma, [criar um](event-hubs-create.md)
* Um [Azure Databricks](https://azure.microsoft.com/services/databricks/) instância. Se não tiver uma, [criar um](../azure-databricks/quickstart-create-databricks-workspace-portal.md)
* [Criar uma biblioteca através da coordenada maven](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.0`

Utilize o seguinte código no seu bloco de notas para eventos de fluxo do hub de eventos.

```scala
import org.apache.spark.eventhubs._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()

// Select the body column and cast it to a string.
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
O código de exemplo seguintes envia eventos para o hub de eventos com as APIs do lote de Spark. Também pode escrever uma consulta de transmissão em fluxo para enviar eventos para o hub de eventos.

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build

val eventHubsConf = EventHubsConf(connectionString)

// Create a column representing the partitionKey.
val partitionKeyColumn = (col("id") % 5).cast("string").as("partitionKey")
// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(partitionKeyColumn, bodyColumn)
df.write
  .format("eventhubs")
  .options(eventHubsConf.toMap)
  .save() 

```

## <a name="next-steps"></a>Passos Seguintes

Este artigo mostrou como funciona o conector do Event Hubs para edifício em tempo real, falhas tolerância a transmissão em fluxo soluções. Saiba mais sobre a transmissão em fluxo estruturada e os Event Hubs conector integrada ao seguir estas ligações:

* [A transmissão em fluxo estruturada + guia de integração de Hubs de eventos do Azure](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Transmissão em fluxo do Spark + guia de integração de Hubs de eventos](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
