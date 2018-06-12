---
title: Utilizar Akka fluxos com o Event Hubs do Azure para Kafka ecossistema | Microsoft Docs
description: Ligar Akka fluxos para um Kafka ativado para o Hub de eventos
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: 9db27340a2210ea0be0564b15241952477e592ba
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304133"
---
# <a name="using-akka-streams-with-event-hubs-for-kafka-ecosystem"></a>Utilizar Akka fluxos com os Event Hubs para Kafka ecossistema

Uma das principais vantagens da utilização do Apache Kafka é a ecossistema de estruturas que pode ligar ao. Ativado por Kafka Event Hubs combina a flexibilidade de Kafka com suporte do ecossistema do Azure, da consistência e a escalabilidade.

Este tutorial mostra como ligar Akka fluxos para os hubs de eventos ativado Kafka sem alterar os clientes de protocolo ou os seus próprios clusters em execução. Os Event Hubs do Azure para Kafka ecossistema suporta [Apache Kafka versão 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certifique-se de que tem os seguintes pré-requisitos:

* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
* [Kit de desenvolvimento Java (JDK) 1.8 +](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * No Ubuntu, execute `apt-get install default-jdk` para instalar o JDK.
    * Certifique-se de que define a variável de ambiente JAVA_HOME para apontar para a pasta onde está instalado o JDK.
* [Transferir](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) um arquivo binário Maven
    * No Ubuntu, pode executar `apt-get install maven` para instalar o Maven.
* [Git](https://www.git-scm.com/downloads)
    * No Ubuntu, pode executar `sudo apt-get install git` para instalar o Git.

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes de Event Hubs

É necessário um espaço de nomes de Event Hubs para enviar ou receber a partir de qualquer serviço de Event Hubs. Consulte [criar Kafka ativada Event Hubs](event-hubs-create-kafka-enabled.md) para obter informações sobre como obter um ponto final Kafka de Hubs de eventos. Certifique-se de que copie a cadeia de ligação de Event Hubs para utilização posterior.

## <a name="clone-the-example-project"></a>Clonar o projeto de exemplo

Agora que tem uma cadeia de ligação ativada para Kafka Event Hubs, clone o repositório de Event Hubs do Azure e navegue para o `akka` subpasta:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/akka
```

## <a name="akka-streams-producer"></a>Produtor de fluxos de Akka

Utilizando o exemplo de produtor Akka fluxos fornecido, envie mensagens para o serviço de Event Hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Fornecer um ponto final Kafka de Hubs de eventos

#### <a name="producer-applicationconf"></a>Produtor application.conf

Atualização do `bootstrap.servers` e `sasl.jaas.config` valores na `producer/src/main/resources/application.conf` para direcionar o produtor ao ponto final do Event Hubs Kafka com a autenticação correta.

```xml
akka.kafka.producer {
    #Akka Kafka producer properties can be defined here


    # Properties defined by org.apache.kafka.clients.producer.ProducerConfig
    # can be defined in this configuration section.
    kafka-clients {
        bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
        sasl.mechanism=PLAIN
        security.protocol=SASL_SSL
        sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-producer-from-the-command-line"></a>Execute o produtor na linha de comandos

Para executar o produtor na linha de comandos, gerar o JAR e, em seguida, executar a partir de Maven (ou gerar JAR com o Maven, execute em Java ao adicionar o JAR(s) de Kafka necessário para o classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

O produtor começa a enviar eventos para o hub de eventos ativado Kafka no tópico `test`e imprime os eventos de stdout.

## <a name="akka-streams-consumer"></a>Consumidor de fluxos de Akka

Utilizando o exemplo fornecido consumidor, receba mensagens dos preparados Kafka event hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Fornecer um ponto final Kafka de Hubs de eventos

#### <a name="consumer-applicationconf"></a>Consumidor application.conf

Atualização do `bootstrap.servers` e `sasl.jaas.config` valores na `consumer/src/main/resources/application.conf` para direcionar o consumidor ao ponto final do Event Hubs Kafka com a autenticação correta.

```xml
akka.kafka.consumer {
    #Akka Kafka consumer properties defined here
    wakeup-timeout=60s

    # Properties defined by org.apache.kafka.clients.consumer.ConsumerConfig
    # defined in this configuration section.
    kafka-clients {
       request.timeout.ms=60000
       group.id=akka-example-consumer

       bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
       sasl.mechanism=PLAIN
       security.protocol=SASL_SSL
       sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-consumer-from-the-command-line"></a>Executar consumidor na linha de comandos

Para executar o consumidor na linha de comandos, gerar o JAR e, em seguida, executar a partir de Maven (ou gerar JAR com o Maven, execute em Java ao adicionar o JAR(s) de Kafka necessário para o classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Se o hub de eventos ativado Kafka tem eventos (por exemplo, se o produtor também está em execução), em seguida, o consumidor começa a receber eventos de tópico `test`. 

Veja o [Akka fluxos Kafka guia](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) para obter mais informações sobre Akka fluxos.

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Saiba mais sobre Hubs de Eventos para o Ecossistema Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Utilize [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [eventos de fluxo do Kafka local para Kafka ativado os Hubs de eventos na nuvem.](event-hubs-kafka-mirror-maker-tutorial.md)
* Saiba como fluxo em Kafka ativado através de Event Hubs [aplicações nativas de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md) ou [Apache Flink](event-hubs-kafka-flink-tutorial.md)
