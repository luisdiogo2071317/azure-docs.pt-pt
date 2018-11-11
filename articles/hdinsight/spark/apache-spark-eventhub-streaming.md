---
title: 'Tutorial: Processar os dados dos Hubs de eventos do Azure com o Apache Spark no Azure HDInsight '
description: Ligue o Apache Spark no HDInsight do Azure para os Hubs de eventos do Azure e processar os dados de transmissão em fluxo.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,mvc
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: eb2145d2e7b787bafa0b546449282454f7059999
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283415"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-spark-in-hdinsight"></a>Tutorial: Processo publicar tweets com os Hubs de eventos do Azure e do Spark no HDInsight

Neste tutorial, saiba como criar uma aplicação de transmissão em fluxo do Apache Spark para enviar tweets para um hub de eventos do Azure e criar outra aplicação para ler os tweets do hub de eventos. Para obter uma explicação detalhada do Spark streaming, consulte [descrição geral de transmissão em fluxo do Apache Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight oferece as mesmas funcionalidades de transmissão em fluxo para um cluster do Spark no Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Enviar mensagens para o Hub de eventos do Azure
> * Ler as mensagens do Hub de eventos do Azure

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* **Concluir o artigo [Tutorial: Load data and run queries on an Apache Spark cluster in Azure HDInsight](./apache-spark-load-data-run-query.md)** (Tutorial: Carregar dados e executar consultas num cluster do Apache Spark no Azure HDInsight).

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter

Para receber um fluxo de tweets, crie uma aplicação no Twitter. Siga as instruções criar um aplicativo do Twitter e anote os valores que precisa para concluir este tutorial.

1. Navegue até [gestão de aplicações do Twitter](https://apps.twitter.com/).
2. Selecione **criar nova aplicação**.
3. Forneça os seguintes valores:

    - Nome: forneça o nome da aplicação. O valor utilizado para este tutorial é **HDISparkStreamApp0423**. Este nome tem de ser um nome exclusivo.
    - Descrição: forneça uma breve descrição da aplicação. O valor utilizado para este tutorial é **uma transmissão em fluxo aplicativo simples de Spark do HDInsight**.
    - Web site: forneça o site da aplicação. Não tem de ser um site válido.  O valor utilizado para este tutorial é **http://www.contoso.com**.
    - URL de chamada de retorno: pode deixá-lo em branco.

4. Selecione **Sim, eu li e aceito o contrato de desenvolvedor do Twitter**e, em seguida, selecione **criar a sua aplicação do Twitter**.
5. Selecione o **chaves e Tokens de acesso** separador.
6. Selecione **criar meu token de acesso** no final da página.
7. Anote os seguintes valores da página.  Vai precisar destes valores mais tarde no tutorial:

    - **Chave de consumidor (chave de API)**    
    - **Segredo de consumidor (segredo de API)**  
    - **Token de acesso**
    - **Segredo de Token de acesso**   

## <a name="create-an-azure-event-hub"></a>Criar um Hub de Eventos do Azure

Utilize este hub de eventos para armazenar os tweets.

1. Inicie sessão no [Portal do Azure](https://ms.portal.azure.com).
2. Selecione **criar um recurso** na parte superior esquerda do ecrã.
3. Selecione **Internet das coisas**, em seguida, selecione **dos Hubs de eventos**.

    ![Criar hub de eventos para o exemplo de transmissão em fluxo do Spark](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "criar hub de eventos para o exemplo de transmissão em fluxo do Spark")
4. Introduza os seguintes valores para o novo espaço de nomes de hub de eventos:

    - **Nome**: introduza um nome para o hub de eventos.  O valor utilizado para este tutorial é **myeventhubns20180403**.
    - **Escalão de preço**: selecione **padrão**.
    - **Grupo de recursos**: tem a opção para criar um novo ou selecione o grupo de recursos para o cluster do Spark. 
    - **Localização**: selecione o mesmo **localização** como o seu cluster do Apache Spark no HDInsight para reduzir os custos e latência.

    ![Forneça um nome de hub de eventos para o exemplo de transmissão em fluxo do Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "fornecer um nome de hub de eventos para o exemplo de transmissão em fluxo do Spark")
5. Selecione **criar** para criar o espaço de nomes.

7. Abra o namespace do hub de eventos com as instruções seguintes:

    1. No portal, selecione **todos os serviços**.
    2. Na caixa de filtro, introduza **os hubs de eventos**.
    3. Selecione o espaço de nomes criado recentemente.
    4. Selecione **+ Hub de eventos**.

8. Introduza os seguintes valores:

    - Nome: Dê um nome para o seu Hub de eventos.
    - Contagem de partição: 10
    - Retenção de mensagens: 1. 
   
    ![Forneça os detalhes do hub de eventos para o exemplo de transmissão em fluxo do Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "fornecer os detalhes do hub de eventos para o exemplo de transmissão em fluxo do Spark")

9. Selecione **Criar**.
10. Selecione **políticas de acesso partilhado** para o espaço de nomes (Observe que não é as políticas de acesso de partilhado do hub de eventos) e, em seguida, selecione **RootManageSharedAccessKey**.
    
     ![Definir políticas do Hub de eventos para o exemplo de transmissão em fluxo de Spark](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "políticas de definir o Hub de eventos para o exemplo de transmissão em fluxo de Spark")

11. Guarde os valores da **chave primária** e **ligação chave primária da cadeia de caracteres** para utilizar mais tarde no tutorial.

     ![Ver chaves de política do Hub de eventos para o exemplo de transmissão em fluxo de Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "chaves de política do Hub de eventos do modo de exibição para o exemplo de transmissão em fluxo do Spark")


## <a name="send-tweets-to-the-event-hub"></a>Enviar tweets para o hub de eventos

Terá de criar um bloco de notas do Jupyter e designe- **SendTweetsToEventHub**. 

1. Execute o seguinte código para adicionar as bibliotecas externas do Maven:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Execute o seguinte código para enviar tweets para o hub de eventos:

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

## <a name="read-tweets-from-the-event-hub"></a>Ler tweets do hub de eventos

Terá de criar outro bloco de notas do Jupyter e designe- **ReadTweetsFromEventHub**. 

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
    
    val customEventhubParameters = EventHubsConf(connStr).setMaxEventsPerTrigger(5)
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

Com o HDInsight, os seus dados são armazenados no Armazenamento do Azure ou no Azure Data Lake Store, pelo que pode eliminar um cluster em segurança quando este não estiver a ser utilizado. Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Se planeja a trabalhar imediatamente no próximo tutorial, pode querer manter o cluster, caso contrário, vamos continuar e eliminar o cluster.

Abra o cluster no portal do Azure e, em seguida, selecione **Eliminar**.

![Eliminar o cluster do HDInsight](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "Eliminar o cluster do HDInsight")

Também pode selecionar o nome do grupo de recursos para abrir a página do grupo de recursos e, em seguida, selecionar **Eliminar grupo de recursos**. Ao eliminar o grupo de recursos, está a eliminar o cluster do Spark no HDInsight e a conta de armazenamento predefinida.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

* Leia a mensagem de um hub de eventos.
Avance para o artigo seguinte para ver o que pode criar uma aplicação de machine learning. 

> [!div class="nextstepaction"]
> [Criar uma aplicação de machine learning](./apache-spark-ipython-notebook-machine-learning.md)


