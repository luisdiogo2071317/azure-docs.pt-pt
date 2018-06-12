---
title: Utilizar o Apache Flink com o Event Hubs do Azure para a ecossistema de Kafka | Microsoft Docs
description: Ligação Apache Flink para um Kafka ativado para o hub de eventos
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: cb7ef0e9b6a612e3f4116cb626903770e4035368
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304140"
---
# <a name="apache-flink-with-event-hubs-for-the-kafka-ecosystem"></a>Apache Flink com os Hubs de eventos para a ecossistema de Kafka

Uma das principais vantagens da utilização do Apache Kafka é a ecossistema de estruturas que pode ligar ao. Kafka ativado combina os Event Hubs a flexibilidade de Kafka com suporte do ecossistema do Azure, da consistência e a escalabilidade.

Este tutorial mostra como ligar o Apache Flink para os hubs de eventos ativado Kafka sem alterar os clientes de protocolo ou os seus próprios clusters em execução. Os Event Hubs do Azure para Kafka ecossistema suporta [Apache Kafka versão 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certifique-se de que tem os seguintes pré-requisitos:

* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * No Ubuntu, execute `apt-get install default-jdk` para instalar o JDK.
    * Certifique-se de que define a variável de ambiente JAVA_HOME para apontar para a pasta onde está instalado o JDK.
* [Transferir](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) um arquivo binário Maven
    * No Ubuntu, pode executar `apt-get install maven` para instalar o Maven.
* [Git](https://www.git-scm.com/downloads)
    * No Ubuntu, pode executar `sudo apt-get install git` para instalar o Git.

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes de Event Hubs

É necessário um espaço de nomes de Event Hubs para enviar ou receber a partir de qualquer serviço de Event Hubs. Consulte [criar Kafka ativada Event Hubs](event-hubs-create-kafka-enabled.md) para obter informações sobre como obter um ponto final Kafka de Hubs de eventos. Certifique-se de que copie a cadeia de ligação de Event Hubs para utilização posterior.

## <a name="clone-the-example-project"></a>Clonar o projeto de exemplo

Agora que tem uma cadeia de ligação ativada para Kafka Event Hubs, clone o repositório de Event Hubs do Azure e navegue para o `flink` subpasta:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/flink
```

## <a name="flink-producer"></a>Flink produtor

Utilizando o exemplo de produtor Flink fornecido, envie mensagens para o serviço de Event Hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Fornecer um ponto final Kafka de Hubs de eventos

#### <a name="producerconfig"></a>Producer.config

Atualização do `bootstrap.servers` e `sasl.jaas.config` valores na `producer/src/main/resources/producer.config` para direcionar o produtor ao ponto final do Event Hubs Kafka com a autenticação correta.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Execute o produtor na linha de comandos

Para executar o produtor na linha de comandos, gerar o JAR e, em seguida, executar a partir de Maven (ou gerar JAR com o Maven, execute em Java ao adicionar o JAR(s) de Kafka necessário para o classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

O produtor começará agora a enviar eventos para o Kafka ativado para o Hub de eventos no tópico `test` e imprimir os eventos de stdout.

## <a name="flink-consumer"></a>Consumidor de Flink

Utilizando o exemplo fornecido consumidor, receber mensagens do Kafka ativada Event Hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Fornecer um ponto final Kafka de Hubs de eventos

#### <a name="consumerconfig"></a>Consumer.config

Atualização do `bootstrap.servers` e `sasl.jaas.config` valores na `consumer/src/main/resources/consumer.config` para direcionar o consumidor ao ponto final do Event Hubs Kafka com a autenticação correta.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Executar consumidor na linha de comandos

Para executar o consumidor na linha de comandos, gerar o JAR e, em seguida, executar a partir de Maven (ou gerar JAR com o Maven, execute em Java ao adicionar o JAR(s) de Kafka necessário para o classpath):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Se o hub de eventos ativado Kafka tem eventos (por exemplo, se o produtor também está em execução), em seguida, o consumidor agora começa receber eventos de tópico `test`.

Veja [Kafka conector Guia do Flink](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) para obter mais informações sobre a ligação Flink para Kafka.

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Saiba mais sobre Hubs de Eventos para o Ecossistema Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Utilize [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [eventos de fluxo do Kafka local para Kafka ativado os Hubs de eventos na nuvem.](event-hubs-kafka-mirror-maker-tutorial.md)
* Saiba como fluxo em Kafka ativado através de Event Hubs [aplicações nativas de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md) ou [Akka fluxos](event-hubs-kafka-akka-streams-tutorial.md).
