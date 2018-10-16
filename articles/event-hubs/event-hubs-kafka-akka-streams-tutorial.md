---
title: Utilizar fluxos Akka com os Hubs de eventos do Azure para o Apache Kafka | Documentos da Microsoft
description: A ligar fluxos Akka um Kafka ativado para o Hub de eventos
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
ms.date: 08/06/2018
ms.author: bahariri
ms.openlocfilehash: a7eb335a2e4764287ab9e0b67dc75c2443ba2856
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322010"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Utilizar fluxos Akka com os Hubs de eventos para o Apache Kafka
Este tutorial mostra-lhe como ligar Akka fluxos para os hubs de eventos de Kafka-ativado sem alterar os seus clientes de protocolo ou os seus próprios clusters em execução. Os Hubs de eventos do Azure para o Kafka suporta [Apache Kafka versão 1.0.](https://kafka.apache.org/10/documentation.html)

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Criar um espaço de nomes dos Hubs de Eventos
> * Clonar o projeto de exemplo
> * Execute o produtor Akka fluxos 
> * Execute o consumidor Akka fluxos

> [!NOTE]
> Este exemplo está disponível no [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certifique-se de que tem os seguintes pré-requisitos:

* Leia o artigo [Hubs de Eventos para o Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md). 
* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
* [Kit de desenvolvimento Java (JDK 1.8 +)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * No Ubuntu, execute `apt-get install default-jdk` para instalar o JDK.
    * Certifique-se de que define a variável de ambiente JAVA_HOME para apontar para a pasta onde está instalado o JDK.
* [Baixe](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) num arquivo binário Maven
    * No Ubuntu, pode executar `apt-get install maven` para instalar o Maven.
* [Git](https://www.git-scm.com/downloads)
    * No Ubuntu, pode executar `sudo apt-get install git` para instalar o Git.

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes dos Hubs de Eventos

Um espaço de nomes de Hubs de eventos é necessário para enviar ou receber a partir de qualquer serviço de Hubs de eventos. Ver [criar Kafka ativado os Hubs de eventos](event-hubs-create-kafka-enabled.md) para obter informações sobre como obter um ponto de extremidade do Kafka de Hubs de eventos. Certifique-se copiar a cadeia de ligação de Hubs de eventos para utilização posterior.

## <a name="clone-the-example-project"></a>Clonar o projeto de exemplo

Agora que tem uma cadeia de ligação de Hubs de eventos habilitados no Kafka, clonar os Hubs de eventos do Azure para o repositório do Kafka e navegue para o `akka` subpasta:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/akka/java
```

## <a name="run-akka-streams-producer"></a>Execute o produtor Akka fluxos

Usando o exemplo de produtor Akka fluxos fornecido, envie mensagens para o serviço de Hubs de eventos.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Fornecer um ponto de extremidade do Kafka de Hubs de eventos

#### <a name="producer-applicationconf"></a>Produtor application.conf

Atualização do `bootstrap.servers` e `sasl.jaas.config` valores em `producer/src/main/resources/application.conf` para direcionar o produtor para o ponto de final do Kafka de Hubs de eventos com a autenticação correta.

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

### <a name="run-producer-from-the-command-line"></a>Execute o produtor a partir da linha de comandos

Para executar o produtor da linha de comando, gerar JAR e, em seguida, execute a partir de Maven (ou gerar JAR com o Maven, em seguida, executar em Java, adicionando o JAR(s) de Kafka necessário para o caminho da classe):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

O produtor começa a enviar eventos para o hub de eventos de Kafka-ativado no tópico `test`e imprime os eventos para stdout.

## <a name="run-akka-streams-consumer"></a>Execute o consumidor Akka fluxos

Usando o exemplo de consumidor fornecido, receba mensagens de hubs de eventos de Kafka-ativado.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Fornecer um ponto de extremidade do Kafka de Hubs de eventos

#### <a name="consumer-applicationconf"></a>Application.conf de consumidor

Atualização do `bootstrap.servers` e `sasl.jaas.config` valores em `consumer/src/main/resources/application.conf` para direcionar o consumidor para o ponto de final do Kafka de Hubs de eventos com a autenticação correta.

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

### <a name="run-consumer-from-the-command-line"></a>Execute o consumidor da linha de comando

Para executar o consumidor da linha de comando, gerar JAR e, em seguida, execute a partir de Maven (ou gerar JAR com o Maven, em seguida, executar em Java, adicionando o JAR(s) de Kafka necessário para o caminho da classe):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Se o hub de eventos habilitados no Kafka tem eventos (por exemplo, se o produtor também está em execução), em seguida, o consumidor começa a receber eventos de tópico `test`. 

Veja a [Akka fluxos Kafka guia](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) para obter mais informações sobre Akka fluxos.

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a ligar Akka fluxos para os hubs de eventos de Kafka-ativado sem alterar os clientes de protocolo ou os seus próprios clusters em execução. Suporta os Hubs de eventos do Azure para o Kafka [Apache Kafka versão 1.0.](https://kafka.apache.org/10/documentation.html). Fez as seguintes ações como parte deste Tutorial: 

> [!div class="checklist"]
> * Criar um espaço de nomes dos Hubs de Eventos
> * Clonar o projeto de exemplo
> * Execute o produtor Akka fluxos 
> * Execute o consumidor Akka fluxos

Para saber mais sobre os Hubs de eventos e os Hubs de eventos para o Kafka, consulte o tópico seguinte:  

* [Saiba mais sobre Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Saiba mais sobre os Hubs de eventos para o Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Explore mais exemplos sobre os Hubs de eventos do GitHub do Kafka](https://github.com/Azure/azure-event-hubs-for-kafka)
* Utilize o [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [transmitir eventos do Kafka no local para Hubs de Eventos ativados pelo Kafka na cloud.](event-hubs-kafka-mirror-maker-tutorial.md)
* Aprenda a transmitir para o Kafka através de Hubs de eventos ativados [aplicativos nativos do Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md) ou [Apache Flink](event-hubs-kafka-flink-tutorial.md)
