---
title: Utilizar a API de fluxos do Apache Kafka - o Azure HDInsight | Microsoft Docs
description: "Saiba como utilizar a API do Apache Kafka fluxos com Kafka no HDInsight. Esta API permite-lhe executar o fluxo de processamento entre tópicos Kafka."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: 1ea20eceb28fead003c7279632b1e75ae1fd3553
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="apache-kafka-streams-api"></a>Fluxos do Apache Kafka API

Saiba como criar uma aplicação que utiliza a API de fluxos Kafka e executá-lo com Kafka no HDInsight.

Ao trabalhar com Apache Kafka, processamento de fluxo, muitas vezes, é feito através do Apache Spark ou Storm. Kafka versão 0.10.0 (no HDInsight 3.5 e 3.6) introduziu a API de fluxos Kafka. Esta API permite-lhe transformar fluxos de dados entre tópicos de entrada e de saída, com uma aplicação que é executado no Kafka. Em alguns casos, isto poderá ser uma alternativa à criação de um Spark ou Storm solução de transmissão em fluxo. Para obter mais informações sobre fluxos Kafka, consulte o [introdução à fluxos](https://kafka.apache.org/10/documentation/streams/) documentação em Apache.org.

## <a name="set-up-your-development-environment"></a>Configurar o ambiente de desenvolvimento

Tem de ter os seguintes componentes instalados no seu ambiente de desenvolvimento:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ou equivalente, como OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Um cliente SSH e o comando `scp`. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="set-up-your-deployment-environment"></a>Configurar o ambiente de implementação

Este exemplo requer Kafka no HDInsight 3.6. Para saber como criar um Kafka num cluster do HDInsight, consulte o [começar a utilizar Kafka no HDInsight](apache-kafka-get-started.md) documento.

## <a name="build-and-deploy-the-example"></a>Criar e implementar o exemplo

Utilize os seguintes passos para criar e implementar o projeto no seu Kafka num cluster do HDInsight.

1. Transfira os exemplos de [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Altere os diretórios para o `Streaming` diretório e, em seguida, utilize o seguinte comando para criar um pacote de jar:

    ```bash
    mvn clean package
    ```

    Este comando cria um diretório com o nome `target`, que contém um ficheiro com o nome `kafka-streaming-1.0-SNAPSHOT.jar`.

3. Utilize o seguinte comando para copiar o `kafka-streaming-1.0-SNAPSHOT.jar` ficheiro ao cluster do HDInsight:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Substitua **SSHUSER** pelo utilizador SSH do seu cluster, e substitua **CLUSTERNAME** pelo nome do seu cluster. Se lhe for solicitado, introduza a palavra-passe da conta de utilizador SSH. Para obter mais informações sobre como utilizar `scp` com o HDInsight, consulte [utilizar o SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-the-example"></a>Executar o exemplo

1. Para abrir uma ligação SSH para o cluster, utilize o seguinte comando:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Substitua **SSHUSER** pelo utilizador SSH do seu cluster, e substitua **CLUSTERNAME** pelo nome do seu cluster. Se lhe for solicitado, introduza a palavra-passe da conta de utilizador SSH. Para obter mais informações sobre como utilizar `scp` com o HDInsight, consulte [utilizar o SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. Para criar o Kafka tópicos que são utilizados neste exemplo, utilize os seguintes comandos:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

    Substitua __o nome do cluster__ com o nome do cluster do HDInsight. Quando lhe for pedido, introduza a palavra-passe para a conta de início de sessão do cluster de HDInsight.

    > [!NOTE]
    > Se o seu início de sessão do cluster é diferente do valor predefinido de `admin`, substitua o `admin` valor nos comandos anteriores com o nome de início de sessão do cluster.

5. Para executar este exemplo, terá de efetuar três ações:

    * Iniciar a solução de fluxos contida no `kafka-streaming.jar`.
    * Iniciar um produtor que escreve o `test` tópico.
    * Iniciar um consumidor para que possa ver o resultado escrito o `wordcounts` tópico

    Pode realizar estas operações abrindo a três sessões SSH. Mas, em seguida, tem de definir `$KAFKABROKERS` e `$KAFKAZKHOSTS` para cada ao executar o passo 4 desta secção em cada sessão SSH. Uma solução mais fácil consiste em utilizar o `tmux` utilitário, que pode dividir a apresentação SSH atual em várias secções. Para iniciar o fluxo, o produtor e o consumidor utilizando `tmux`, utilize o seguinte comando:

    ```bash
    tmux new-session '/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer' \; split-window -h 'java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS' \; split-window -v '/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test' \; attach
    ```

    Este comando divide a apresentação SSH nas três secções:

    * Secção esquerda executa um consumidor de consola, que lê mensagens do `wordcounts` tópico:`/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer`

        > [!NOTE]
        > O `--property` parâmetros dizer o consumidor da consola para a chave (word) juntamente com a contagem (valor) de impressão. Este parâmetro também configura o desserializador a utilizar ao ler estes valores do Kafka.

    * A secção superior direita é executado a solução de fluxos de API:`java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS`

    * A secção inferior direita é executado o produtor de consola e aguarda no introduzir mensagens a enviar para o `test` tópico:`/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test`
 
6. Depois do `tmux` comando divide a apresentação, o cursor está na secção inferior direita. Inicie a introdução de frases. Após cada frase, o painel esquerdo é atualizado para apresentar uma contagem das palavras exclusivas. O resultado é semelhante ao seguinte texto:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]
    > A contagem aumenta sempre que uma palavra é encontrada.

7. Utilize o __Ctrl + C__ para o produtor de sair. Continuar a utilizar __Ctrl + C__ para sair da aplicação e o consumidor.

## <a name="next-steps"></a>Passos Seguintes

Neste documento, aprendeu a utilizar a API de fluxos de Kafka com Kafka no HDInsight. Utilize o seguinte para obter mais informações sobre como trabalhar com o Kafka:

* [Analisar os registos do Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicar dados entre clusters do Kafka](apache-kafka-mirroring.md)
* [API Kafka produtor e de consumidor com o HDInsight](apache-kafka-producer-consumer-api.md)
* [Utilizar a transmissão em fluxo de Apache Spark (DStream) com o Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Utilizar a Transmissão em Fluxo Estruturada de Apache Spark com o Kafka no HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Utilizar o Apache Spark estruturados de transmissão em fluxo para mover dados de Kafka no HDInsight para Cosmos DB](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Ligar ao Kafka através de uma Rede Virtual do Azure](apache-kafka-connect-vpn-gateway.md)