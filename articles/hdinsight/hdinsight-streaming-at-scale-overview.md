---
title: "Transmissão em fluxo em escala no Azure HDInsight | Microsoft Docs"
description: "Como utilizar os dados de transmissão em fluxo com dimensionáveis clusters do HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: raghavmohan
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: ramoha
ms.openlocfilehash: 46b5723805ab5d8bc1cf5b5183d9501cd3e4e3a2
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="streaming-at-scale-in-hdinsight"></a>Transmissão em fluxo em escala no HDInsight

Soluções de macrodados em tempo real agirem sobre dados em movimento. Normalmente, estes dados são de maior valor na respetiva hora de chegada. Se o fluxo de dados de entrada for maior do que pode ser processada nesse momento, poderá ter de limitação para baixo de recursos. Em alternativa, um cluster do HDInsight pode aumentar verticalmente para satisfazer a sua solução de transmissão em fluxo através da adição de nós pedido.

Numa aplicação de transmissão em fluxo, um ou mais origens de dados são geração de eventos (por vezes milhões por segundo) que têm de ser ingeridos rapidamente sem remover quaisquer informações úteis. Os eventos de entrada são processados com *fluxo colocação em memória intermédia*, também chamado *eventos colocação*, por um serviço, tal como [Kafka](kafka/apache-kafka-introduction.md) ou [Event Hubs](https://azure.microsoft.com/services/event-hubs/). Depois de recolher os eventos, em seguida, pode analisar os dados a utilizar um sistema de análise em tempo real dentro de *processamento de fluxo* camada, tais como [Storm](storm/apache-storm-overview.md) ou [transmissão em fluxo do Spark](spark/apache-spark-streaming-overview.md). Os dados processados podem ser armazenados em sistemas de armazenamento de longa duração, como [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)e apresentar em tempo real num business intelligence dashboard, tais como [Power BI](https://powerbi.microsoft.com), Tableau ou uma página web personalizado .

![Padrões de transmissão em fluxo do HDInsight](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka oferece um débito elevado, o serviço de colocação de mensagens de latência baixa e faz agora parte do suite Apache do Software de origem abrir (OSS). Kafka utiliza um publicar e subscrever os fluxos de modelo e arquivos de mensagens de dados particionados em segurança num cluster distribuído, replicado. Kafka é dimensionado linearmente à medida que aumenta o débito.

Para obter mais informações, consulte [introduzindo o Apache Kafka no HDInsight](kafka/apache-kafka-introduction.md).

## <a name="apache-storm"></a>Apache Storm

O Apache Storm é um sistema de cálculo distribuído, com tolerância a falhas, open source que está otimizado para processamento de fluxos de dados em tempo real com o Hadoop. A unidade de núcleos de dados para um evento de entrada é uma cadeia de identificação, que é um conjunto de pares chave/valor imutável. Uma sequência de formulários estas cadeias de identificação unbounded provém de uma transmissão em fluxo, que é de um Spout. O Spout encapsula num wrapper uma origem de dados de transmissão em fluxo (tal como Kafka) e emite cadeias de identificação. Um topologia do storm é uma sequência de transformações nestes fluxos.

Para obter mais informações, consulte [o que é Apache Storm no Azure HDInsight?](storm/apache-storm-overview.md).

## <a name="spark-streaming"></a>Transmissão em fluxo do Spark

Transmissão em fluxo do Spark é uma extensão do spark, o que permite-lhe reutilizar o mesmo código que utiliza para processamento de batches. Pode combinar batch e consultas interativas na mesma aplicação. Ao contrário do Storm, transmissão em fluxo do Spark fornece monitorização de estado exatamente-depois de o processamento de semântica. Quando utilizado em combinação com o [API direta Kafka](http://spark.apache.org/docs/latest/streaming-kafka-integration.html), que garante que todos os dados de Kafka são recebidos por transmissão em fluxo do Spark exatamente uma vez, é possível alcançar o ponto-a-ponto exatamente-garante uma vez. Uma das força da codificação da Spark de transmissão em fluxo é respetivas capacidades de tolerância a falhas, recuperar falhou nós rapidamente quando estão a ser utilizados vários nós do cluster.

Para obter mais informações, consulte [o que é a transmissão em fluxo do Spark?](hdinsight-spark-streaming-overview.md).

## <a name="scaling-a-cluster"></a>Dimensionamento de um cluster

Embora possa especificar o número de nós do cluster durante a criação, poderá querer aumentar ou diminuir o cluster para corresponder a carga de trabalho. Todos os clusters do HDInsight permitem-lhe [alterar o número de nós no cluster](hdinsight-administer-use-management-portal.md#scale-clusters). É possível remover clusters do Spark sem perda de dados, como todos os dados são armazenados no Storage do Azure ou de Data Lake Store.

Existem vantagens em desacoplamento tecnologias. Por exemplo, Kafka é um evento de memória intermédia de tecnologia de, pelo que é e/s muito intensiva e não precisa de muito poder de processamento. Em comparação, processadores de fluxo, tais como a transmissão em fluxo do Spark são intensivas de computação, que requerem mais poderosas VMs. Fazendo com que estas tecnologias desacopladas em clusters diferentes, pode dimensioná-los separadamente ao melhor utilização de VMs.

### <a name="scale-the-stream-buffering-layer"></a>Dimensionar o fluxo de memória intermédia de camada

O fluxo de memória intermédia de tecnologias Kafka e Hubs de eventos utilizam ambos partições e consumidores lida essas partições. Aumentar o débito de entrada requer como aumentar verticalmente o número de partições e adicionar partições fornece o paralelismo de aumento. No Event Hubs, não é possível alterar a contagem da partição após a implementação, pelo que é importante começar a utilizar a escala de destino em mente. Com Kafka, é possível [adicionar partições](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion), mesmo quando Kafka está a processar dados. Kafka fornece uma ferramenta para reatribuir partições, `kafka-reassign-partitions.sh`. O HDInsight fornece um [réplicas de partição reequilíbrio ferramenta](https://github.com/hdinsight/hdinsight-kafka-tools), `rebalance_rackaware.py`. Esta ferramenta rebalancing chama o `kafka-reassign-partitions.sh` ferramenta de forma a que cada réplica está a ser um domínio de falhas separada e o domínio de atualização, tornando a tolerância a falhas de cientes e de aumento Kafka bastidor.

### <a name="scale-the-stream-processing-layer"></a>Dimensionar a camada de processamento de fluxo

Apache Storm e Spark de transmissão em fluxo suportam adicionar nós de trabalho para os clusters, mesmo quando os dados que está a ser processados.

Para tirar partido dos novos nós adicionados através do dimensionamento Storm, terá de reequilibrar as topologias do Storm iniciadas antes do tamanho do cluster foi aumentado. Este reequilíbrio pode ser feito utilizando o Storm web da IU ou o CLI. Para obter mais informações, consulte o [documentação do Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

Apache Spark utiliza três parâmetros de chaves para configurar o seu ambiente, consoante os requisitos da aplicação: `spark.executor.instances`, `spark.executor.cores`, e `spark.executor.memory`. Um *executor* é um processo que é iniciado para uma aplicação de Spark. Um executor é executado no nó de trabalho e é responsável por as devidas tarefas da aplicação. O número predefinido de executor e os tamanhos de executor de cada cluster é calculado com base no número de nós de trabalho e o tamanho de nó de trabalho. Estes números são armazenados no `spark-defaults.conf`ficheiros em cada nó principal do cluster.

Estes três parâmetros podem ser configurados ao nível do cluster, para todas as aplicações que são executados no cluster e também pode ser especificado para cada aplicação individuais. Para obter mais informações, consulte [gerir os recursos para clusters do Apache Spark](spark/apache-spark-resource-manager.md).

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Apache Storm no HDInsight](storm/apache-storm-tutorial-get-started-linux.md)
* [Topologias de exemplo para Apache Storm no HDInsight](storm/apache-storm-example-topology.md)
* [Introdução ao Spark no HDInsight](spark/apache-spark-overview.md)
* [Começar a utilizar o Apache Kafka no HDInsight](kafka/apache-kafka-get-started.md)