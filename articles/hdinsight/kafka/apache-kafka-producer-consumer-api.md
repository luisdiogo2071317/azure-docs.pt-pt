---
title: Utilizar o Apache Kafka produtor e APIs de consumidor - o Azure HDInsight | Microsoft Docs
description: "Saiba como utilizar o Apache Kafka produtor e APIs de consumidor com Kafka no HDInsight. Estas APIs permitem-lhe desenvolver aplicações que escreverem e ler a partir do Apache Kafka."
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
ms.date: 01/18/2018
ms.author: larryfr
ms.openlocfilehash: b57745d6bd993a993e923c964327d9071e745413
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="apache-kafka-producer-and-consumer-apis"></a>APIs de consumidor e Apache Kafka produtor

Saiba como criar uma aplicação que utiliza o produtor Kafka e APIs de consumidor com Kafka no HDInsight.

Para obter documentação APIs, consulte [produtor API](https://kafka.apache.org/documentation/#producerapi) e [API de consumidor](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>Configurar o ambiente de desenvolvimento

Tem de ter os seguintes componentes instalados no seu ambiente de desenvolvimento:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ou equivalente, como OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Um cliente SSH e o comando `scp`. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="set-up-your-deployment-environment"></a>Configurar o ambiente de implementação

Este exemplo requer Kafka no HDInsight 3.6. Para saber como criar um Kafka num cluster do HDInsight, consulte o [começar a utilizar Kafka no HDInsight](apache-kafka-get-started.md) documento.

## <a name="build-and-deploy-the-example"></a>Criar e implementar o exemplo

1. Transfira os exemplos de [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Altere os diretórios para a localização do `Producer-Consumer` diretório e utilize o seguinte comando:

    ```
    mvn clean package
    ```

    Este comando cria um diretório com o nome `target`, que contém um ficheiro com o nome `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Utilize os seguintes comandos para copiar o ficheiro `kafka-producer-consumer-1.0-SNAPSHOT.jar` para o cluster HDInsight:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Substitua **SSHUSER** pelo utilizador SSH do seu cluster, e substitua **CLUSTERNAME** pelo nome do seu cluster. Quando lhe for pedido, introduza a palavra-passe do utilizador SSH.

## <a id="run"></a>Executar o exemplo

1. Para abrir uma ligação SSH para o cluster, utilize o seguinte comando:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Substitua **SSHUSER** pelo utilizador SSH do seu cluster, e substitua **CLUSTERNAME** pelo nome do seu cluster. Se lhe for solicitado, introduza a palavra-passe da conta de utilizador SSH. Para obter mais informações sobre como utilizar `scp` com o HDInsight, consulte [utilizar o SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Para criar o Kafka tópicos que são utilizados neste exemplo, utilize os seguintes comandos:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Substitua __o nome do cluster__ com o nome do cluster do HDInsight. Quando lhe for pedido, introduza a palavra-passe para a conta de início de sessão do cluster de HDInsight.

    > [!NOTE]
    > Se o seu início de sessão do cluster é diferente do valor predefinido de `admin`, substitua o `admin` valor nos comandos anteriores com o nome de início de sessão do cluster.

3. Para executar o produtor e escrever dados para o tópico, utilize o seguinte comando:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

4. Depois de terminar o produtor, utilize o seguinte comando para ler a partir do tópico:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    A leitura dos registos, juntamente com uma contagem de registos, é apresentada.

5. Utilize __Ctrl + C__ para sair do consumidor.

### <a name="multiple-consumers"></a>Vários consumidores

Os consumidores de Kafka utilizam um grupo de consumidores quando leem os registos. Utilizar o mesmo grupo com vários consumidores resulta em leituras com balanceamento de carga de um tópico. Cada consumidor no grupo recebe uma parte dos registos.

A aplicação de consumidor aceita um parâmetro que é utilizado como o ID de grupo. Por exemplo, o seguinte comando inicia um consumidor utilizando um ID de grupo de `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
```

Para ver este processo em ação, utilize os passos seguintes

1. Repita os passos 1 e 2 no [executar o exemplo](#run) secção para abrir uma nova sessão SSH.

2. Em ambas as sessões SSH, introduza o seguinte comando:

    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```
    
    > [!IMPORTANT]
    > Introduza os dois comandos em simultâneo, para que estes estão ambos em execução ao mesmo tempo.

    Tenha em atenção que o número de mensagens de leitura não é igual a secção anterior, onde era necessário apenas um consumidor. Em vez disso, a leitura de mensagens é dividida entre as instâncias.

O consumo pelos clientes dentro do mesmo grupo é processado pelas partições do tópico. Para o tópico `test` criado anteriormente, tem oito partições. Se abrir oito sessões SSH e iniciar um consumidor em todas as sessões, cada consumidor lê registos de uma única partição do tópico.

> [!IMPORTANT]
> Não podem existir mais instâncias de consumidor num grupo de consumidores do que partições. Neste exemplo, um grupo de consumidores pode incluir até oito consumidores, pois esse é o número de partições no tópico. Também pode ter vários grupos de consumidores, em que cada grupo não tem mais do que oito consumidores.

Os registos armazenados no Kafka são armazenados pela ordem em que são recebidos dentro de uma partição. Para obter uma entrega por ordem dos registos *dentro de uma partição*, crie um grupo de consumidores em que o número de instâncias de consumidor corresponde ao número de partições. Para obter uma entrega por ordem dos registos *dentro do tópico*, crie um grupo de consumidores com apenas uma instância de consumidor.

## <a name="next-steps"></a>Passos Seguintes

Neste documento, aprendeu a utilizar o produtor Kafka e a API do consumidor com Kafka no HDInsight. Utilize o seguinte para obter mais informações sobre como trabalhar com o Kafka:

* [Analisar os registos do Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicar dados entre clusters do Kafka](apache-kafka-mirroring.md)
* [Fluxos Kafka API com o HDInsight](apache-kafka-streams-api.md)
* [Utilizar a transmissão em fluxo de Apache Spark (DStream) com o Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Utilizar a Transmissão em Fluxo Estruturada de Apache Spark com o Kafka no HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Utilizar o Apache Spark estruturados de transmissão em fluxo para mover dados de Kafka no HDInsight para Cosmos DB](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Ligar ao Kafka através de uma Rede Virtual do Azure](apache-kafka-connect-vpn-gateway.md)