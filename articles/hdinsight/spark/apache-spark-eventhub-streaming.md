---
title: 'Tutorial: Processar dados a partir de Hubs de eventos do Azure com o Apache Spark no Azure HDInsight | Microsoft Docs'
description: Ligar o Apache Spark no Azure HDInsight ao Event Hubs do Azure e processar os dados de transmissão em fluxo.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: 9b59f5d58234aaf8f8385f722d6659548e066933
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787256"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-spark-in-hdinsight"></a>Tutorial: Processo tweets com o Event Hubs do Azure e o Spark no HDInsight

Neste tutorial, irá aprender a criar uma aplicação de transmissão em fluxo do Apache Spark para enviar os tweets para um hub de eventos do Azure e criar outra aplicação de ler os tweets do hub de eventos. Para obter uma explicação detalhada de transmissão em fluxo do Spark, consulte [Apache Spark descrição geral de transmissão em fluxo](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight oferece as mesmas funcionalidades de transmissão em fluxo para um cluster do Spark no Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Enviar mensagens para o Hub de eventos do Azure
> * Ler mensagens do Hub de eventos do Azure

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* **Concluir o artigo [Tutorial: carregar os dados e executar consultas num cluster do Apache Spark no Azure HDInsight](./apache-spark-load-data-run-query.md)**.

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter

Para receber um fluxo de tweets, crie uma aplicação no Twitter. Siga as instruções criar uma aplicação do Twitter e anote os valores que precisa para concluir este tutorial.

1. Navegue até à [gestão de aplicações do Twitter](https://apps.twitter.com/).
2. Selecione **criar nova aplicação**.
3. Forneça os seguintes valores:

    - Nome: forneça o nome da aplicação. O valor utilizado para este tutorial é **HDISparkStreamApp0423**. Este nome tem de ser um nome exclusivo.
    - Descrição: forneça uma breve descrição da aplicação. O valor utilizado para este tutorial é **um simples HDInsight Spark aplicações de transmissão em fluxo**.
    - Web site: forneça site da aplicação. Não tem de ser um site válido.  O valor utilizado para este tutorial é **http://www.contoso.com**.
    - URL de chamada de retorno: pode deixar em branco.

4. Selecione **Sim, leu e aceita o contrato de programador Twitter**e, em seguida, selecione **criar a sua aplicação Twitter**.
5. Selecione o **chaves e os Tokens de acesso** separador.
6. Selecione **crie minha token de acesso** no fim da página.
7. Anote os seguintes valores da página.  Necessitar destes valores mais tarde no tutorial:

    - **Chave de consumidor (chave de API)**    
    - **Segredo de consumidor (segredo de API)**  
    - **Token de acesso**
    - **Segredo de Token de acesso**   

## <a name="create-an-azure-event-hub"></a>Criar um Hub de Eventos do Azure

Utilize este hub de eventos para armazenar tweets.

1. Inicie sessão no [Portal do Azure](https://ms.portal.azure.com).
2. Selecione **crie um recurso** na parte superior esquerda do ecrã.
3. Selecione **Internet das coisas**, em seguida, selecione **Event Hubs**.

    ![Criar o hub de eventos de exemplo de transmissão em fluxo do Spark](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "criar o hub de eventos de exemplo de transmissão em fluxo do Spark")
4. Introduza os valores seguintes para o novo espaço de nomes de hub de eventos:

    - **Nome**: introduza um nome para o hub de eventos.  O valor utilizado para este tutorial é **myeventhubns20180403**.
    - **Escalão de preço**: selecione **padrão**.
    - **Grupo de recursos**: tem a opção para criar um novo ou selecione o grupo de recursos para o cluster do Spark. 
    - **Localização**: selecione o mesmo **localização** do seu cluster do Apache Spark no HDInsight para reduzir os custos e latência.

    ![Forneça um nome de hub de eventos para o exemplo de transmissão em fluxo do Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "fornecer um nome de hub de eventos para o exemplo de transmissão em fluxo do Spark")
5. Selecione **criar** para criar o espaço de nomes.

6. Abra o espaço de nomes event hub utilizando as instruções seguintes:

    1. No portal, selecione **todos os serviços**.
    2. Na caixa do filtro, introduza **dos event hubs**.
    3. Faça duplo clique o espaço de nomes que criou.
    4. Selecione **+ Hub de eventos**.

6. Na lista de espaço de nomes de Event Hubs, selecione o espaço de nomes criado recentemente.      
5. Selecione **Event Hubs**e, em seguida, selecione **+ Hub de eventos** para criar um novo Hub de eventos.
  

6. Introduza os seguintes valores:

    - Nome: Atribua um nome para o seu Hub de eventos.
    - Contagem de partição: 10
    - Retenção de mensagem: 1. 
   
    ![Forneça detalhes do hub de eventos para o exemplo de transmissão em fluxo do Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "fornecem detalhes de hub de eventos de exemplo de transmissão em fluxo do Spark")

7. Selecione **Criar**.
8. Selecione **políticas de acesso partilhado** para o espaço de nomes (nota não é as políticas de acesso do event hub partilhado) e, em seguida, selecione **RootManageSharedAccessKey**.
    
     ![Definir políticas de Hub de eventos para o Spark exemplo de transmissão em fluxo](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "políticas de definir o Hub de eventos para o Spark exemplo de transmissão em fluxo")

9. Guardar os valores de **chave primária** e **chave primária de cadeia de ligação** para utilizar mais tarde no tutorial.

     ![Ver as chaves de política de Hub de eventos para o Spark exemplo de transmissão em fluxo](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "política de Hub de eventos de vista de chaves para o exemplo de transmissão em fluxo do Spark")


## <a name="send-tweets-to-the-event-hub"></a>Enviar tweets para o hub de eventos

Tem de criar um bloco de notas do Jupyter e dê-lhe nome **SendTweetsToEventHub**. 

1. Execute o seguinte código para adicionar as bibliotecas de Maven externas:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Execute o seguinte código para enviar os tweets para o hub de eventos:

    ```
    import java.util._
    import scala.collection.JavaConverters._
    import java.util.concurrent._
    
    import org.apache.spark._
    import org.apache.spark.streaming._
    import org.apache.spark.eventhubs.ConnectionStringBuilder

    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    import com.microsoft.azure.eventhubs._
    val pool = Executors.newFixedThreadPool(1)
    val eventHubClient = EventHubClient.create(connStr.toString(), pool)
    
    def sendEvent(message: String) = {
          val messageData = EventData.create(message.getBytes("UTF-8"))
          eventHubClient.get().send(messageData)
          println("Sent event: " + message + "\n")
    }
    
    import twitter4j._
    import twitter4j.TwitterFactory
    import twitter4j.Twitter
    import twitter4j.conf.ConfigurationBuilder

    // Twitter application configurations
    // Replace values below with yours   
    val twitterConsumerKey = "<CONSUMER KEY>"
    val twitterConsumerSecret = "<CONSUMER SECRET>"
    val twitterOauthAccessToken = "<ACCESS TOKEN>"
    val twitterOauthTokenSecret = "<TOKEN SECRET>"
    
    val cb = new ConfigurationBuilder()
    cb.setDebugEnabled(true).setOAuthConsumerKey(twitterConsumerKey).setOAuthConsumerSecret(twitterConsumerSecret).setOAuthAccessToken(twitterOauthAccessToken).setOAuthAccessTokenSecret(twitterOauthTokenSecret)
    
    val twitterFactory = new TwitterFactory(cb.build())
    val twitter = twitterFactory.getInstance()

    // Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!
    
    val query = new Query(" #Azure ")
    query.setCount(100)
    query.lang("en")
    var finished = false
    while (!finished) {
      val result = twitter.search(query)
      val statuses = result.getTweets()
      var lowestStatusId = Long.MaxValue
      for (status <- statuses.asScala) {
        if(!status.isRetweet()){
          sendEvent(status.getText())
        }
        lowestStatusId = Math.min(status.getId(), lowestStatusId)
        Thread.sleep(2000)
      }
      query.setMaxId(lowestStatusId - 1)
    }
    
    // Closing connection to the Event Hub
    eventHubClient.get().close()
    ```

3. Abra o hub de eventos no portal do Azure.  No **descrição geral**, deverá ver alguns gráficos que mostra as mensagens enviadas para o hub de eventos.

## <a name="read-tweets-from-the-event-hub"></a>Leitura tweets do hub de eventos

Tem de criar outro bloco de notas do Jupyter e dê-lhe nome **ReadTweetsFromEventHub**. 

1. Execute o seguinte código para adicionar uma biblioteca externa do Maven:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```
2. Execute o seguinte código para ler tweets a partir do seu hub de eventos:

    ```
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    val customEventhubParameters = EventHubsConf(connectionString).setMaxEventsPerTrigger(5)
    val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()
    //incomingStream.printSchema    
    
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Event Hub message format is JSON and contains "body" field
    // Body is binary, so you cast it to string to see the actual content of the message
    val messages = incomingStream.withColumn("Offset", $"offset".cast(LongType)).withColumn("Time (readable)", $"enqueuedTime".cast(TimestampType)).withColumn("Timestamp", $"enqueuedTime".cast(LongType)).withColumn("Body", $"body".cast(StringType)).select("Offset", "Time (readable)", "Timestamp", "Body")
    
    messages.printSchema
    
    messages.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
    ```

## <a name="clean-up-resources"></a>Limpar recursos

Com o HDInsight, os dados são armazenados no Storage do Azure ou do Azure Data Lake Store, pelo que pode eliminar em segurança um cluster quando não está em utilização. Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Uma vez que os custos do cluster são muito superiores aos custos do armazenamento, faz sentido do ponto de vista económico eliminar os clusters quando não estiverem a ser utilizados. Se planeia funcionam no próximo tutorial imediatamente, poderá pretender manter o cluster.

Abra o cluster no portal do Azure e selecione **eliminar**.

![Eliminar cluster do HDInsight](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "cluster do HDInsight eliminar")

Pode também selecionar o nome do grupo de recursos para abrir a página de grupo de recursos e, em seguida, selecione **eliminar grupo de recursos**. Ao eliminar o grupo de recursos, tem de eliminar o cluster do HDInsight Spark tanto a conta do storage predefinida.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

* Ler a mensagem de um hub de eventos.
Avançar para o seguinte artigo para ver o que pode criar uma aplicação de aprendizagem. 

> [!div class="nextstepaction"]
> [Criar uma aplicação de aprendizagem](./apache-spark-ipython-notebook-machine-learning.md)


