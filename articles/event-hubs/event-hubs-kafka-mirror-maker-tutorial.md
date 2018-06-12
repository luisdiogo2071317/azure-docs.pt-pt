---
title: Utilizar Kafka MirrorMaker com o Event Hubs do Azure para Kafka ecossistema | Microsoft Docs
description: Utilize Kafka MirrorMaker para espelhar um cluster de Kafka Event Hubs.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: mirror-maker
ms.custom: mvc
ms.date: 05/07/2018
ms.author: bahariri
ms.openlocfilehash: 0693fc2fff5735fb2b3c0a9b8f1d3d256746f40d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298326"
---
# <a name="using-kafka-mirrormaker-with-event-hubs-for-kafka-ecosystems"></a>Utilizar Kafka MirrorMaker com os Event Hubs para ecossistemas Kafka

> [!NOTE]
> Este exemplo está disponível no [GitHub](https://github.com/Azure/azure-event-hubs)

Uma consideração principais para aplicações de escala na nuvem modernas é a capacidade para atualizar, melhorar e alterar a infraestrutura sem interromper o serviço. Este tutorial mostra como um hub de eventos ativado Kafka e Kafka MirrorMaker pode integrar um pipeline Kafka existente do Azure por "espelhamento" o fluxo de entrada Kafka no serviço de Event Hubs. 

Um ponto final Kafka de Hubs de eventos do Azure permite-lhe ligar ao Azure Event Hubs utilizando o protocolo de Kafka (ou seja, os clientes Kafka). Ao efetuar alterações mínimas a uma aplicação Kafka, pode ligar ao Event Hubs do Azure e desfrutar as vantagens do ecossistema do Azure. Kafka ativado os Hubs de eventos atualmente suporta versões Kafka 1.0 e posteriores.

Este exemplo mostra como espelhar um mediador Kafka num hub de eventos ativado Kafka Kafka MirrorMaker a utilizar.

   ![Kafka MirrorMaker com os Event Hubs](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certifique-se de que tem:

* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * No Ubuntu, execute `apt-get install default-jdk` para instalar o JDK.
    * Certifique-se de que define a variável de ambiente JAVA_HOME para apontar para a pasta onde está instalado o JDK.
* [Transferir](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) um arquivo binário Maven
    * No Ubuntu, pode executar `apt-get install maven` para instalar o Maven.
* [Git](https://www.git-scm.com/downloads)
    * No Ubuntu, pode executar `sudo apt-get install git` para instalar o Git.

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes de Event Hubs

É necessário um espaço de nomes de Event Hubs para enviar e receber a partir de qualquer serviço de Event Hubs. Consulte [criar um Kafka ativado para o Hub de eventos](event-hubs-create.md) para obter instruções sobre a obtenção de um ponto final Kafka de Hubs de eventos. Certifique-se de que copie a cadeia de ligação de Event Hubs para utilização posterior.

## <a name="clone-the-example-project"></a>Clonar o projeto de exemplo

Agora que tem um Kafka ativada a cadeia de ligação de Event Hubs, clone o repositório de Event Hubs do Azure e navegue para o `mirror-maker` subpasta:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Configurar um cluster de Kafka

Utilize o [guia de introdução ao Kafka](https://kafka.apache.org/quickstart) para configurar um cluster com as definições pretendidas (ou utilizar um cluster de Kafka existente).

## <a name="kafka-mirrormaker"></a>Kafka MirrorMaker

Kafka MirrorMaker permite que o "espelhamento" de um fluxo. Dada a origem e destino Kafka clusters, MirrorMaker assegura que as mensagens enviadas para o cluster de origem são recebidas pelo clusters de origem e de destino. Este exemplo mostra como espelhar uma origem de cluster Kafka com um hub de eventos ativado Kafka de destino. Este cenário pode ser utilizado para enviar dados a partir de um pipeline Kafka existente para os Event Hubs sem interromper o fluxo de dados. 

Para obter informações mais detalhadas sobre Kafka MirrorMaker, consulte o [Kafka espelhamento/MirrorMaker guia](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

### <a name="configuration"></a>Configuração

Para configurar Kafka MirrorMaker, atribua-lhe um cluster de Kafka como consumidor/origem e de um hub de eventos Kafka ativado como o produtor/destino.

#### <a name="consumer-configuration"></a>Configuração de consumidor

Atualizar o ficheiro de configuração de consumidor `source-kafka.config`, que indica MirrorMaker as propriedades da origem de Kafka cluster.

##### <a name="source-kafkaconfig"></a>kafka.config de origem

```xml
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Configuração de produtor

Atualizar o ficheiro de configuração produtor `mirror-eventhub.config`, que indica MirrorMaker para enviar os dados duplicados (ou "espelhados") para o serviço de Event Hubs. Especificamente, alterar `bootstrap.servers` e `sasl.jaas.config` para apontar para o ponto final Kafka de Hubs de eventos. O serviço de Event Hubs requer (SASL) uma comunicação segura, que é conseguida através da definição as últimas três propriedades na configuração do seguinte: 

##### <a name="mirror-eventhubconfig"></a>espelho eventhub.config

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-mirrormaker"></a>Executar MirrorMaker

Execute o script de Kafka MirrorMaker raiz diretório Kafka utilizando os ficheiros de configuração recentemente atualizados. Certifique-se copiar os ficheiros de configuração para a raiz do diretório de Kafka, ou atualizar os caminhos no comando seguinte.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Para confirmar que os eventos são atingir o hub de eventos ativado Kafka, consulte as estatísticas de entrada no [portal do Azure](https://azure.microsoft.com/features/azure-portal/), ou executar um consumidor contra o hub de eventos.

Com MirrorMaker em execução, quaisquer eventos enviados para a origem de Kafka cluster são recebidos pelo Kafka cluster e o Kafka espelhada ativado o serviço de hub de eventos. Ao utilizar MirrorMaker e um ponto de final do Event Hubs Kafka, pode migrar um pipeline Kafka existente para o serviço de Event Hubs do Azure gerida sem alterar do cluster existente ou interromper qualquer fluxo de dados em curso.

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Saiba mais sobre os Event Hubs do ecossistema de Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* Saiba mais sobre [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) para eventos de fluxo de Kafka no local para Kafka ativado os hubs de eventos na nuvem.
* Saiba como fluxo em Kafka ativado através de Event Hubs [aplicações nativas de Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md), [Apache Flink](event-hubs-kafka-flink-tutorial.md), ou [Akka fluxos](event-hubs-kafka-akka-streams-tutorial.md).
