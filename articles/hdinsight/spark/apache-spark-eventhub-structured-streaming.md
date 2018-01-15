---
title: "Utilizar o Apache Spark estruturados de transmissão em fluxo com os Event Hubs no Azure HDInsight | Microsoft Docs"
description: "Crie um exemplo sobre como enviar um fluxo de dados para o Hub de eventos do Azure e, em seguida, receber esses eventos num cluster do Spark do HDInsight utilizando uma aplicação scala de transmissão em fluxo do Apache Spark."
keywords: "Apache spark de transmissão em fluxo, transmissão em fluxo do spark, amostra do spark, o apache spark transmissão em fluxo de exemplo, o exemplo do event hub do azure, o exemplo do spark"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: e0486d2c5f78da1d1e4a12703f120eccef43c305
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-structured-streaming-on-hdinsight-to-process-events-from-event-hubs"></a>Apache Spark estruturados de transmissão em fluxo no HDInsight para processar eventos a partir do Event Hubs

Neste artigo, que aprende a processar a telemetria em tempo real com o Spark estruturados de transmissão em fluxo. Para tal, execute os seguintes passos de alto nível:

1. Compilar e executar na sua estação de trabalho local um exemplo de aplicação de produtor de evento que gera eventos para enviar para os Event Hubs.
2. Utilize o [Spark Shell](apache-spark-shell.md) para definir e executar uma aplicação simples de Spark estruturados de transmissão em fluxo.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte [clusters do Apache Spark criar no Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* Um espaço de nomes de Event Hubs do Azure. Consulte [criar um espaço de nomes de Event Hubs do Azure](apache-spark-eventhub-streaming.md#create-an-azure-event-hub) para obter mais informações.

* Kit de desenvolvimento Java Oracle. Pode instalar a [aqui](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

* Apache Maven. Poderá transferi-lo de [aqui](https://maven.apache.org/download.cgi). Estão disponíveis instruções para instalar o Maven [aqui](https://maven.apache.org/install.html).

## <a name="build-configure-and-run-the-event-producer"></a>Criar, configurar e executar o produtor de eventos
Nesta tarefa, clonar um exemplo de aplicação que cria eventos aleatórios e envia-as para um Hub de eventos configurado. Esta aplicação de exemplo está disponível no GitHub em [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).

1. Certifique-se de que tem de instalar as ferramentas do git. Pode transferir da [GIT transfere](http://www.git-scm.com/downloads). 
2. Abra uma linha de comandos ou a shell de terminal e execute o seguinte comando a partir do diretório da sua escolha para clonar o projeto.
        
        git clone https://github.com/hdinsight/eventhubs-sample-event-producer.git eventhubs-client-sample

3. Esta ação cria uma nova pasta designada eventhubs-cliente-sample. Dentro da shell ou a linha de comandos, navegue para esta pasta.
4. Execute o Maven para criar a aplicação utilizando o seguinte comando:

          mvn package

5. Dentro da shell ou a linha de comandos, navegue para o diretório de destino que é criado e contém o ficheiro ``com-microsoft-azure-eventhubs-client-example-0.2.0.jar``.
6. Em seguida, terá de criar cópias de segurança da linha de comandos para executar o produtor de evento no seu Hub de eventos. Fazer isto, substituindo os valores no comando da seguinte forma:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "<namespace>" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "<policyKey>" --message-length 32 --thread-count 1 --message-count -1

7. Por exemplo, um comando completo seria ter um aspeto semelhante ao seguinte:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "sparkstreaming" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU=" --message-length 32 --thread-count 1 --message-count -1

8. O comando é iniciado e se a configuração estiver correta dentro de alguns momentos Consulte saída relacionados com os eventos que está a enviar ao seu Hub de eventos, semelhante ao seguinte:

        Map('policyKey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, 'eventhubsName -> hub1, 'policyName -> RootManageSharedAccessKey, 'eventhubsNamespace -> sparkstreaming, 'messageCount -> -1, 'messageLength -> 32, 'threadCount -> 1)
        Events per thread: -1 (-1 for unlimited)
        10 > Sun Jun 18 11:32:58 PDT 2017 > 1024 > 1024 > Sending event: ZZ93958saG5BUKbvUI9wHVmpuA2TrebS
        10 > Sun Jun 18 11:33:46 PDT 2017 > 2048 > 2048 > Sending event: RQorGRbTPp6U2wYzRSnZUlWEltRvTZ7R
        10 > Sun Jun 18 11:34:33 PDT 2017 > 3072 > 3072 > Sending event: 36Eoy2r8ptqibdlfCYSGgXe6ct4AyOX3
        10 > Sun Jun 18 11:35:19 PDT 2017 > 4096 > 4096 > Sending event: bPZma9V0CqOn6Hj9bhrrJT0bX2rbPSn3
        10 > Sun Jun 18 11:36:06 PDT 2017 > 5120 > 5120 > Sending event: H2TVD77HNTVyGsVcj76g0daVnYxN4Sqs

9. Deixe o produtor de eventos em execução enquanto impedem os passos.

## <a name="run-spark-shell-on-your-hdinsight-cluster"></a>Execute a Shell do Spark no cluster do HDInsight
Nesta tarefa, irá SSH no nó principal do cluster do HDInsight, inicie a Shell do Spark e executar uma aplicação de transmissão em fluxo do Spark que obtém e processa os eventos provenientes dos Hubs de eventos. 

Por este ponto de cluster do HDInsight deve estar pronto. Caso contrário, terá de aguardar até terminar o aprovisionamento. Assim que estiver pronto, pode continue com os seguintes passos:

1. Ligar ao cluster do HDInsight através de SSH. Para obter instruções, consulte [ligar ao HDInsight utilizando SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

5. A aplicação que criar requer o pacote de Hubs de eventos de transmissão em fluxo do Spark. Para executar a Shell do Spark, de modo a que obtém automaticamente esta dependência de [Maven Central](https://search.maven.org), certifique-se ser alimentação os pacotes de comutador com as coordenadas do Maven da seguinte forma:

        spark-shell --packages "com.microsoft.azure:spark-streaming-eventhubs_2.11:2.1.5"

6. Depois de concluída a Shell do Spark carregar, deverá ver:

        Welcome to
            ____              __
            / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
        /___/ .__/\_,_/_/ /_/\_\   version 2.1.1.2.6.2.3-1
            /_/
                
        Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_151)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> 

7. Copie o seguinte fragmento de código para um editor de texto e modificá-lo para que não tem a chave de política e definir o espaço de nomes conforme apropriado para o seu Hub de eventos.

        val eventhubParameters = Map[String, String] (
            "eventhubs.policyname" -> "RootManageSharedAccessKey",
            "eventhubs.policykey" -> "<policyKey>",
            "eventhubs.namespace" -> "<namespace>",
            "eventhubs.name" -> "hub1",
            "eventhubs.partition.count" -> "2",
            "eventhubs.consumergroup" -> "$Default",
            "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            "eventhubs.sql.containsProperties" -> "true"
            )
            
8. Se observar o ponto final de EventHub compatível com o seguinte formato, a parte que lê `iothub-xxxxxxxxxx` é o nome de espaço de nomes de EventHub compatível e pode ser utilizado para `eventhubs.namespace`. O campo `SharedAccessKeyName` pode ser utilizado para `eventhubs.policyname`, e `SharedAccessKey` para `eventhubs.policykey`: 

        Endpoint=sb://iothub-xxxxxxxxxx.servicebus.windows.net/;SharedAccessKeyName=xxxxx;SharedAccessKey=xxxxxxxxxx 

9. Cole o fragmento modificado o scala espera > linha de comandos e prima retorno. Deverá ver uma saída semelhante a:

        scala> val eventhubParameters = Map[String, String] (
            |       "eventhubs.policyname" -> "RootManageSharedAccessKey",
            |       "eventhubs.policykey" -> "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU",
            |       "eventhubs.namespace" -> "sparkstreaming",
            |       "eventhubs.name" -> "hub1",
            |       "eventhubs.partition.count" -> "2",
            |       "eventhubs.consumergroup" -> "$Default",
            |       "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            |       "eventhubs.sql.containsProperties" -> "true"
            |     )
        eventhubParameters: scala.collection.immutable.Map[String,String] = Map(eventhubs.sql.containsProperties -> true, eventhubs.name -> hub1, eventhubs.consumergroup -> $Default, eventhubs.partition.count -> 2, eventhubs.progressTrackingDir -> /eventhubs/progress, eventhubs.policykey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, eventhubs.namespace -> hdiz-docs-eventhubs, eventhubs.policyname -> RootManageSharedAccessKey)

10. Em seguida, começar a criar um Spark estruturados de transmissão em fluxo consulta possível especificar a origem. Colar o seguinte na Shell de Spark e prima retorno.

        val inputStream = spark.readStream.
        format("eventhubs").
        options(eventhubParameters).
        load()

11. Deverá ver uma saída semelhante a:

        inputStream: org.apache.spark.sql.DataFrame = [body: binary, offset: bigint ... 5 more fields]

12. Em seguida, crie a consulta para que escreve o resultado para a consola. Fazê-lo, colar o seguinte na Shell de Spark e premindo retorno.

        val streamingQuery1 = inputStream.writeStream.
        outputMode("append").
        format("console").start().awaitTermination()

13. Deverá ver alguns lotes começar com o resultado semelhante ao seguinte

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        [Stage 0:>                                                          (0 + 2) / 2]

14. Isto é seguido os resultados de saída do processamento da cada microbatch de eventos. 

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        17/06/18 18:57:39 WARN TaskSetManager: Stage 1 contains a task of very large size (419 KB). The maximum recommended task size is 100 KB.
        +--------------------+------+---------+------------+---------+------------+----------+
        |                body|offset|seqNumber|enqueuedTime|publisher|partitionKey|properties|
        +--------------------+------+---------+------------+---------+------------+----------+
        |[7B 22 74 65 6D 7...|     0|        0|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   112|        1|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   224|        2|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   336|        3|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   448|        4|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   560|        5|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   672|        6|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   784|        7|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   896|        8|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1008|        9|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1120|       10|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1232|       11|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1344|       12|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1456|       13|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1568|       14|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1680|       15|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1792|       16|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1904|       17|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2016|       18|  1497734889|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2128|       19|  1497734889|     null|        null|     Map()|
        +--------------------+------+---------+------------+---------+------------+----------+
        only showing top 20 rows

15. Chegarem novos eventos do produtor de evento, são processados por esta consulta estruturada de transmissão em fluxo.
16. Lembre-se de que elimina o cluster de HDInsight quando terminar de executar este exemplo.



## <a name="see-also"></a>Consulte também

* [Descrição geral da transmissão em fluxo do Spark](apache-spark-streaming-overview.md)













