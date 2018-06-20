---
title: Transmitir para os Hubs de Eventos do Azure para o Ecossistema Kafka | Microsoft Docs
description: Transmita para os Hubs de Eventos, com o protocol Kafka e APIs.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/08/2018
ms.author: bahariri
ms.openlocfilehash: 8ef6240d19ce1ac1b891c95ce525a8bd211a2900
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297228"
---
# <a name="stream-into-event-hubs-for-the-kafka-ecosystem"></a>Transmitir para os Hubs de Eventos do Ecossistema Kafka

> [!NOTE]
> Este exemplo está disponível no [GitHub](https://github.com/Azure/azure-event-hubs)

Este início rápido mostra como transmitir para Hubs de Eventos ativados pelo Kafka, sem alterar os clientes de protocolo nem a execução dos seus próprios clusters. Vai aprender a utilizar os seus produtores e consumidores para comunicar com Hubs de Eventos ativados pelo Kafka, com apenas uma alteração de configuração nas aplicações. Os Hubs de Eventos do Azure do ecossistema Kafka suportam o [Apache Kafka versão 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, certifique-se de que tem os seguintes pré-requisitos:

* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Transferir](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) um arquivo binário Maven.
* [Git](https://www.git-scm.com/)
* [Um espaço de nomes de Hubs de Eventos ativado pelo Kafka](event-hubs-create.md)

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Enviar e receber mensagens com Kafka em Hubs de Eventos

1. Clone o [repositório de Hubs de Eventos do Azure](https://github.com/Azure/azure-event-hubs).

2. Navegue para `azure-event-hubs/samples/kafka/quickstart/producer`.

3. Atualize os detalhes de configuração do produtor em `src/main/resources/producer.config`, da seguinte forma:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Execute o código de produtor e transmita em fluxo para os Hubs de Eventos ativados pelo Kafka:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. Navegue para `azure-event-hubs/samples/kafka/quickstart/consumer`.

6. Atualize os detalhes de configuração para o consumidor em `src/main/resources/consumer.config`, da seguinte forma:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Execute o código e processo de consumidor, a partir de Hubs de Eventos ativados pelo Kafka, com os seus clientes do Kafka:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Se o cluster Kafka dos Hubs de Eventos tiver eventos, começará agora a recebê-los do consumidor.

## <a name="next-steps"></a>Passos seguintes

* [Saiba mais sobre Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Saiba mais sobre Hubs de Eventos para o Ecossistema Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Utilize o [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para [transmitir eventos do Kafka no local para Hubs de Eventos ativados pelo Kafka na cloud.](event-hubs-kafka-mirror-maker-tutorial.md)
* Saiba como transmitir para os Hubs de Eventos ativados pelo Kafka através do [Apache Flink](event-hubs-kafka-flink-tutorial.md) ou [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md).
