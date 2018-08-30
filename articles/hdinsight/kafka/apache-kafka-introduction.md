---
title: Introdução ao Apache Kafka no HDInsight - Azure
description: 'Saiba mais sobre o Apache Kafka no HDInsight: o que é, o que faz e onde encontrar exemplos e obter informações sobre como começar.'
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 04/11/2018
ms.openlocfilehash: 4aa6fd4e469ec2185df82cdb994862f4f7b5d896
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049408"
---
# <a name="what-is-apache-kafka-on-hdinsight"></a>O que é o Apache Kafka no HDInsight?

O [Apache Kafka](https://kafka.apache.org) é uma plataforma de transmissão em fluxo distribuída de código aberto que pode ser utilizada para criar aplicações e pipelines de dados de transmissão em fluxo em tempo real. O Kafka também fornece a funcionalidade de mediador de mensagem semelhante a uma fila de mensagens, onde pode publicar e subscrever fluxos de dados nomeados. 

Seguem-se as características específicas do Kafka no HDInsight:

* É um serviço gerido que fornece um processo de configuração simplificado. O resultado é uma configuração que é testada e suportada pela Microsoft.

* A Microsoft fornece um Contrato de Nível de Serviço (SLA) de 99,9% de tempo de atividade do Kafka. Para obter mais informações, veja o documento [Informações do SLA para o HDInsight](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/).

* Utiliza os Managed Disks do Azure como arquivo de cópias de segurança do Kafka. Os Managed Disks podem fornecer até 16 TB de armazenamento por mediador Kafka. Para obter mais informações sobre como configurar os discos geridos com o Kafka no HDInsight, consulte [Increase scalability of Kafka on HDInsight](apache-kafka-scalability.md) (Aumentar a escalabilidade do Kafka no HDInsight).

    Para obter mais informações sobre os discos geridos, consulte [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

* O Kafka foi concebido com uma única vista dimensional de um bastidor. O Azure separa um bastidor em duas dimensões: Domínios de Atualização (UD) e Domínios de Falha (FD). A Microsoft oferece ferramentas que reequilibram as réplicas e partições do Kafka em UDs e FDs. 

    Para obter mais informações, consulte [Elevada disponibilidade com o Kafka no HDInsight](apache-kafka-high-availability.md).

* O HDInsight permite-lhe alterar o número de nós de trabalho (que alojam o mediador Kafka) após a criação do cluster. O escalonamento pode ser executado a partir do portal do Azure, do Azure PowerShell e de outras interfaces de gestão do Azure. Para o Kafka, deve reequilibrar as réplicas de partições após as operações de dimensionamento. Reequilibrar partições permite ao Kafka tirar partido do novo número de nós de trabalho.

    Para obter mais informações, consulte [Elevada disponibilidade com o Kafka no HDInsight](apache-kafka-high-availability.md).

* O Azure Log Analytics pode ser utilizado para monitorizar o Kafka no HDInsight. O Log Analytics apresenta informações ao nível de máquinas virtuais, como métricas de discos e NIC e métricas de JMX do Kafka.

    Para obter mais informações, consulte [Analisar registos para o Kafka no HDInsight](apache-kafka-log-analytics-operations-management.md).

### <a name="kafka-on-hdinsight-architecture"></a>Kafka na arquitetura do HDInsight

O diagrama seguinte mostra uma configuração do Kafka comum que utiliza grupos de consumidores, particionamento e replicação para oferecer leitura paralela de eventos com tolerância a falhas:

![Diagrama de configuração do cluster do Kafka](./media/apache-kafka-introduction/kafka-cluster.png)

O Apache ZooKeeper gere o estado do cluster do Kafka. O Zookeeper foi concebido para transações em simultâneo, resilientes e de baixa latência. 

O Kafka armazena os registos (dados) em **tópicos**. Os registos são produzidos por **produtores** e consumidos por **consumidores**. Os produtores enviam os registos para **mediadores** Kafka. Cada nó de trabalho no cluster HDInsight é um mediador Kafka. 

Registos de partição de tópicos em mediadores. Quando consumir registos, pode utilizar até um consumidor por partição para alcançar o processamento paralelo dos dados.

A replicação é utilizada para duplicar as partições nos nós, ao proteger contra interrupções do nó (mediador). A partição assinalada com *(L)* no diagrama é a partição líder dessa partição específica. O tráfico do produtor é encaminhado para o líder de cada nó mediante a utilização do estado gerido pelo ZooKeeper.

## <a name="why-use-kafka-on-hdinsight"></a>Porquê utilizar o Kaftka no HDInsight?

Seguem-se as tarefas comuns e os padrões que podem ser efetuados com o Kafka no HDInsight:

* **Replicação de dados do Kafka**: o Kafka proporciona o utilitário MirrorMaker, que replica os dados entre clusters do Kafka.

    Para obter mais informações sobre como utilizar o MirrorMaker, consulte [Replicar tópicos do Kafka com o Kafka no HDInsight](apache-kafka-mirroring.md).

* **Padrão de mensagem de publicação-subscrição**: o Kafka fornece uma API de Produtor para publicar registos num tópico do Kafka. A API de Consumidor é utilizada ao subscrever um tópico.

    Para obter mais informações, consulte [Introdução ao Kafka no HDInsight](apache-kafka-get-started.md).

* **Processamento de fluxo**: o Kafka é muitas vezes utilizado com o Apache Storm ou Spark para processamento de fluxo em tempo real. O Kafka 0.10.0.0 (versão 3.5 e 3.6 do HDInsight) introduziu uma API de transmissão em fluxo que lhe permite criar soluções de transmissão em fluxo, sem precisar do Storm ou do Spark.

    Para obter mais informações, consulte [Introdução ao Kafka no HDInsight](apache-kafka-get-started.md).

* **Escala horizontal**: transmissões em fluxo de partições do Kafka nos nós no cluster do HDInsight. Os processos de consumidor podem estar associados a partições individuais para fornecer balanceamento de carga ao consumir registos.

    Para obter mais informações, consulte [Introdução ao Kafka no HDInsight](apache-kafka-get-started.md).

* **Entrega por ordem**: em cada partição, os registos são armazenados na transmissão em fluxo pela ordem em que foram recebidos. Ao associar um processo de consumidor por partição, pode garantir que os registos são processados por ordem.

    Para obter mais informações, consulte [Introdução ao Kafka no HDInsight](apache-kafka-get-started.md).

## <a name="use-cases"></a>Casos de utilização

* **Mensagens**: uma vez que suporta o padrão de mensagem de publicação-subscrição, o Kafka é frequentemente utilizado como um mediador de mensagem.

* **Controlo de atividades**: uma vez que o Kafka fornece os registos por ordem, pode ser utilizado para controlar e voltar a criar atividades. Por exemplo, ações do utilizador num site ou numa aplicação.

* **Agregação**: com o processamento de transmissão em fluxo, pode agregar informações a partir de várias transmissões em fluxo para combinar e centralizar as informações nos dados operacionais.

* **Transformação**: com o processamento de transmissão em fluxo, pode combinar e enriquecer os dados a partir de vários tópicos de entrada em um ou mais tópicos de saída.

## <a name="next-steps"></a>Passos seguintes

Utilize as seguintes ligações para saber como utilizar o Apache Kafka no HDInsight:

* [Início Rápido: Criar o Kafka no HDInsight](apache-kafka-get-started.md)

* [Tutorial: Utilizar o Apache Spark com Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Tutorial: Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)
