---
title: 'Tutorial: Utilizar as APIs de Produtor e de Consumidor de Apache Kafka - Azure HDInsight | Microsoft Docs'
description: Saiba como utilizar as APIs de Produtor e de Consumidor de Apache Kafka com o Kafka no HDInsight. Neste tutorial, irá aprender a utilizar estas APIs com o Kafka no HDInsight a partir de uma aplicação Java.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2018
ms.author: larryfr
ms.openlocfilehash: b602f8bfe316e9c11dbff18273f37c99407c3da6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Tutorial: Utilizar as APIs de Produtor e de Consumidor de Apache Kafka

Saiba como utilizar as APIs de Produtor e de Consumidor de Kafka com o Kafka no HDInsight.

A API de Produtor de Kafka permite que as aplicações enviem fluxos de dados para o cluster de Kafka. A API de Consumidor de Kafka permite que as aplicações leiam fluxos de dados a partir do cluster.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar o ambiente de desenvolvimento
> * Configurar o ambiente de implementação
> * Compreender o código
> * Criar e implementar a aplicação
> * Executar a aplicação no cluster

Para obter mais informações sobre as APIs, veja a documentação do Apache dedicada à [API de Produtor](https://kafka.apache.org/documentation/#producerapi) e à [API de Consumidor](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>Configurar o ambiente de desenvolvimento

Tem de ter os seguintes componentes instalados no seu ambiente de desenvolvimento:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ou equivalente, como OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Um cliente SSH e o comando `scp`. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Num editor de texto ou IDE Java.

As variáveis de ambiente que se seguem podem ser definidas quando instala o Java e o JDK na sua estação de trabalho de desenvolvimento. No entanto, deve certificar-se de que existem e que contêm os valores corretos para o seu sistema.

* `JAVA_HOME` - deve apontar para o diretório onde o JDK está instalado.
* `PATH` - deve conter os seguintes caminhos:
  
    * `JAVA_HOME` (ou o caminho equivalente).
    * `JAVA_HOME\bin` (ou o caminho equivalente).
    * O diretório onde o Maven está instalado.

## <a name="set-up-your-deployment-environment"></a>Configurar o ambiente de implementação

Este tutorial requer o Kafka no HDInsight 3.6. Para saber como criar um Kafka num cluster do HDInsight, consulte o documento [Começar a utilizar o Kafka no HDInsight](apache-kafka-get-started.md).

## <a name="understand-the-code"></a>Compreender o código

A aplicação de exemplo está localizada em [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), no subdiretório `Producer-Consumer`. Essencialmente, a aplicação é composta por quatro ficheiros:

* `pom.xml`: este ficheiro define as dependências do projeto, a versão de Java e os métodos de empacotamento.
* `Producer.java`: este ficheiro envia 1 milhão (1.000.000) de frases aleatórias para o Kafka através da API de produtor.
* `Consumer.java`: este ficheiro utiliza a API de consumidor para ler os dados a partir de Kafka e emiti-los para STDOUT.
* `Run.java`: a interface de linha de comandos utilizada para executar o código do produtor e do consumidor.

### <a name="pomxml"></a>Pom.xml

Seguem-se os aspetos importantes a compreender em relação ao ficheiro `pom.xml`:

* Dependências: este projeto depende das APIs de produtor e de consumidor de Kafka, fornecidas pelo pacote `kafka-clients`. Esta dependência é definida pelo seguinte código XML:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]
    > A entrada `${kafka.version}` é declarada na secção `<properties>..</properties>` de `pom.xml` e está configurada para a versão de Kafka do cluster do HDInsight.

* Plug-ins: os plug-ins de Maven proporcionam diversas funcionalidades. Neste projeto, são utilizados os seguintes plug-ins:

    * `maven-compiler-plugin`: utilizado para definir a versão de Java utilizada pelo projeto para a versão 8. Esta é a versão de Java utilizada pelo HDInsight 3.6.
    * `maven-shade-plugin`: utilizado para gerar um JAR com dependências, que contém precisamente não só esta aplicação como todas as dependências. Também é utilizado para definir o ponto de entrada da aplicação, para que possa executar diretamente o ficheiro JAR sem ter de especificar a classe principal.

### <a name="producerjava"></a>Producer.Java

O produtor comunica com os anfitriões de mediador (nós de trabalho) de Kafka para armazenar os dados num tópico do Kafka. O fragmento de código que se segue foi retirado do ficheiro `Producer.java`:

```java
package com.microsoft.example;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import java.util.Properties;
import java.util.Random;
import java.io.IOException;

public class Producer
{
    public static void produce(String brokers) throws IOException
    {

        // Set properties used to configure the producer
        Properties properties = new Properties();
        // Set the brokers (bootstrap servers)
        properties.setProperty("bootstrap.servers", brokers);
        // Set how to serialize key/value pairs
        properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
        properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

        // So we can generate random sentences
        Random random = new Random();
        String[] sentences = new String[] {
                "the cow jumped over the moon",
                "an apple a day keeps the doctor away",
                "four score and seven years ago",
                "snow white and the seven dwarfs",
                "i am at two with nature"
        };

        String progressAnimation = "|/-\\";
        // Produce a bunch of records
        for(int i = 0; i < 1000000; i++) {
            // Pick a sentence at random
            String sentence = sentences[random.nextInt(sentences.length)];
            // Send the sentence to the test topic
            producer.send(new ProducerRecord<String, String>("test", sentence));
            String progressBar = "\r" + progressAnimation.charAt(i % progressAnimation.length()) + " " + i;
            System.out.write(progressBar.getBytes());
        }
    }
}
```

Este código liga-se aos anfitriões de mediador (nós de trabalho) de Kafka e, em seguida, envia 1.000.000 frases para o Kafka através da API de Produtor.

### <a name="consumerjava"></a>Consumer.Java

O consumidor comunica com os anfitriões de mediador (nós de trabalho) de Kafka e lê os registos de forma cíclica. O fragmento de código que se segue foi retirado do ficheiro `Consumer.java`:

```java
package com.microsoft.example;

import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import java.util.Properties;
import java.util.Arrays;

public class Consumer {
    public static void consume(String brokers, String groupId) {
        // Create a consumer
        KafkaConsumer<String, String> consumer;
        // Configure the consumer
        Properties properties = new Properties();
        // Point it to the brokers
        properties.setProperty("bootstrap.servers", brokers);
        // Set the consumer group (all consumers must belong to a group).
        properties.setProperty("group.id", groupId);
        // Set how to serialize key/value pairs
        properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        // When a group is first created, it has no offset stored to start reading from. This tells it to start
        // with the earliest record in the stream.
        properties.setProperty("auto.offset.reset","earliest");
        consumer = new KafkaConsumer<>(properties);

        // Subscribe to the 'test' topic
        consumer.subscribe(Arrays.asList("test"));

        // Loop until ctrl + c
        int count = 0;
        while(true) {
            // Poll for records
            ConsumerRecords<String, String> records = consumer.poll(200);
            // Did we get any?
            if (records.count() == 0) {
                // timeout/nothing to read
            } else {
                // Yes, loop over records
                for(ConsumerRecord<String, String> record: records) {
                    // Display record and count
                    count += 1;
                    System.out.println( count + ": " + record.value());
                }
            }
        }
    }
}
```

Neste código, o consumidor está configurado para ler a partir do início do tópico (`auto.offset.reset` está definido como `earliest`).

### <a name="runjava"></a>Run.Java

O ficheiro `Run.java` fornece uma interface de linha de comandos que executa o código do produtor ou do consumidor. Tem de fornecer as informações do anfitrião de mediador de Kafka como um parâmetro. Opcionalmente, pode incluir um valor de ID de grupo, o qual é utilizado pelo processo de consumidor. Se criar várias instâncias de consumidor com o mesmo ID de grupo, estes efetuarão o balanceamento de carga da leitura do tópico.

```java
package com.microsoft.example;

import java.io.IOException;
import java.util.UUID;

// Handle starting producer or consumer
public class Run {
    public static void main(String[] args) throws IOException {
        if(args.length < 2) {
            usage();
        }

        // Get the brokers
        String brokers = args[1];
        switch(args[0].toLowerCase()) {
            case "producer":
                Producer.produce(brokers);
                break;
            case "consumer":
                // Either a groupId was passed in, or we need a random one
                String groupId;
                if(args.length == 3) {
                    groupId = args[2];
                } else {
                    groupId = UUID.randomUUID().toString();
                }
                Consumer.consume(brokers, groupId);
                break;
            default:
                usage();
        }
        System.exit(0);
    }
    // Display usage
    public static void usage() {
        System.out.println("Usage:");
        System.out.println("kafka-example.jar <producer|consumer> brokerhosts [groupid]");
        System.exit(1);
    }
}
```

## <a name="build-and-deploy-the-example"></a>Criar e implementar o exemplo

1. Transfira os exemplos partir de [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Altere os diretórios para a localização do diretório `Producer-Consumer` e utilize o seguinte comando:

    ```
    mvn clean package
    ```

    Este comando cria um diretório com o nome `target`, que contém um ficheiro com o nome `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Utilize os seguintes comandos para copiar o ficheiro `kafka-producer-consumer-1.0-SNAPSHOT.jar` para o cluster HDInsight:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Substitua **SSHUSER** pelo utilizador SSH do seu cluster, e substitua **CLUSTERNAME** pelo nome do seu cluster. Quando lhe for pedido, introduza a palavra-passe do utilizador SSH.

## <a id="run"></a> Executar o exemplo

1. Para abrir uma ligação de SSH ao cluster, utilize o seguinte comando:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Substitua **SSHUSER** pelo utilizador SSH do seu cluster, e substitua **CLUSTERNAME** pelo nome do seu cluster. Se tal lhe for pedido, introduza a palavra-passe da conta de utilizador SSH. Para obter mais informações sobre como utilizar `scp` com o HDInsight, veja [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Para criar os tópicos de Kafka utilizados por este exemplo, utilize os seguintes passos:

    1. Para guardar o nome do cluster numa variável e instalar um utilitário de análise JSON (`jq`), utilize os comandos mais abaixo. Quando lhe for pedido, introduza o nome do cluster de Kafka:
    
        ```bash
        sudo apt -y install jq
        read -p 'Enter your Kafka cluster name:' CLUSTERNAME
        ```
    
    2. Para obter os anfitriões de mediador de Kafka e os anfitriões de Zookeeper, utilize os seguintes comandos. Quando lhe for pedido, introduza a palavra-passe para o início de sessão (administrador) do cluster.
    
        ```bash
        export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. Para criar o tópico `test`, utilize o seguinte comando:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
        ```

3. Para executar o produtor e escrever dados para o tópico, utilize o seguinte comando:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

4. Quando o produtor tiver terminado, utilize o seguinte comando para ler a partir do tópico:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    A leitura dos registos, juntamente com uma contagem de registos, é apresentada.

5. Utilize __Ctrl + C__ para sair do consumidor.

### <a name="multiple-consumers"></a>Vários consumidores

Os consumidores de Kafka utilizam um grupo de consumidores quando leem os registos. Utilizar o mesmo grupo com vários consumidores resulta em leituras com balanceamento de carga de um tópico. Cada consumidor no grupo recebe uma parte dos registos.

A aplicação de consumidor aceita um parâmetro que é utilizado como o ID de grupo. Por exemplo, o seguinte comando inicia um consumidor através de um ID de grupo de `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
```

Para ver este processo em ação, utilize o seguinte comando:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup' \; split-w
indow -h 'java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup' \; attach
```

Este comando utiliza `tmux` para dividir o terminal em duas colunas. É iniciado um consumidor em cada coluna, com o mesmo valor de ID de grupo. Após os consumidores concluírem a leitura, tenha em atenção que cada um deles lê apenas uma parte dos registos. Utilize __Ctrl + C __ duas vezes para sair do `tmux`.

O consumo pelos clientes dentro do mesmo grupo é processado pelas partições do tópico. Para o tópico `test` criado anteriormente, tem oito partições. Se iniciar oito consumidores, cada consumidor lê os registos de uma única partição do tópico.

> [!IMPORTANT]
> Não podem existir mais instâncias de consumidor num grupo de consumidores do que partições. Neste exemplo, um grupo de consumidores pode incluir até oito consumidores, pois esse é o número de partições no tópico. Também pode ter vários grupos de consumidores, em que cada grupo não tem mais do que oito consumidores.

Os registos armazenados no Kafka são armazenados pela ordem em que são recebidos dentro de uma partição. Para obter uma entrega por ordem dos registos *dentro de uma partição*, crie um grupo de consumidores em que o número de instâncias de consumidor corresponde ao número de partições. Para obter uma entrega por ordem dos registos *dentro do tópico*, crie um grupo de consumidores com apenas uma instância de consumidor.

## <a name="next-steps"></a>Passos seguintes

Neste documento, aprendeu a utilizar as APIs de Produtor e de Consumidor de Kafka com o Kafka no HDInsight. Utilize o seguinte para obter mais informações sobre como trabalhar com o Kafka:

* [Analisar os registos do Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicar dados entre clusters do Kafka](apache-kafka-mirroring.md)
* [API de Fluxos Kafka com o HDInsight](apache-kafka-streams-api.md)