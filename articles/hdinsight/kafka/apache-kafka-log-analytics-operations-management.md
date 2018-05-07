---
title: Análise de registo para Apache Kafka - o Azure HDInsight | Microsoft Docs
description: Saiba como utilizar a análise de registos para analisar os registos do cluster do Apache Kafka no Azure HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2018
ms.author: larryfr
ms.openlocfilehash: 9f366631ced4392831ad9ed97898a88b3290cd22
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Analisar os registos para Apache Kafka no HDInsight

Saiba como utilizar a análise de registos para analisar os registos gerados por Apache Kafka no HDInsight.

## <a name="enable-log-analytics-for-kafka"></a>Ativar análise de registos para Kafka

Os passos para ativar a análise de registos para o HDInsight são os mesmos para todos os clusters do HDInsight. Utilize as seguintes hiperligações para compreender como criar e configurar os serviços necessários:

1. Crie uma área de trabalho de análise de registos. Para obter mais informações, consulte o [começar com uma área de trabalho de análise de registos](https://docs.microsoft.com/azure/log-analytics) documento.

2. Crie um Kafka num cluster do HDInsight. Para obter mais informações, consulte o [começar a utilizar o Apache Kafka no HDInsight](apache-kafka-get-started.md) documento.

3. Configure o cluster de Kafka para utilizar a análise de registos. Para obter mais informações, consulte o [análise de registos de utilização para monitorizar HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md) documento.

    > [!NOTE]
    > Também pode configurar o cluster para utilizar a análise de registos, utilizando o `Enable-AzureRmHDInsightOperationsManagementSuite` cmdlet. Este cmdlet requer as seguintes informações:
    >
    > * O nome de cluster do HDInsight.
    > * O ID da área de trabalho de análise de registos. Pode encontrar o ID da área de trabalho na sua área de trabalho de análise de registos.
    > * A chave primária para a ligação de análise de registos. Para localizar a chave primária, selecione a instância de análise de registos e, em seguida, __Portal do OMS__. A partir do Portal do OMS, selecione __definições__, __origens ligadas__e, em seguida, __servidores Linux__.


> [!IMPORTANT]
> Pode demorar cerca de 20 minutos antes dos dados estão disponíveis para análise de registos.

## <a name="query-logs"></a>Registos de consulta

1. Do [portal do Azure](https://portal.azure.com), selecione a sua área de trabalho de análise de registos.

2. Selecione __Iniciar pesquisa__. Aqui, pode procurar os dados recolhidos a partir de Kafka. Seguem-se alguns exemplos de procura:

    * Utilização do disco: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Utilização da CPU: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Receber mensagens em fila por segundo: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Bytes recebidos por segundo: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Bytes enviados por segundo: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]
    > Substitua os valores de consulta com as informações específicas do cluster. Por exemplo, `ClusterName_s` tem de ser definida para o nome do cluster. `HostName_s` tem de ser definida para o nome de domínio de um nó de trabalho no cluster.

    Também pode introduzir `*` para procurar todos os tipos com sessão iniciados. Atualmente os seguintes registos estão disponíveis para consultas:

    | Tipo de registo | Descrição |
    | ---- | ---- |
    | registo\_kafkaserver\_CL | Kafka mediador server.log |
    | registo\_kafkacontroller\_CL | Kafka mediador controller.log |
    | métricas\_kafka\_CL | Métricas de Kafka JMX |

    ![Imagem da pesquisa de utilização da CPU](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a análise de registos, consulte o [começar com uma área de trabalho de análise de registos](../../log-analytics/log-analytics-get-started.md) documento.

Para obter mais informações sobre como trabalhar com Kafka, consulte os seguintes documentos:

 * [Espelho Kafka entre clusters do HDInsight](apache-kafka-mirroring.md)
 * [Aumentar a escalabilidade do Kafka no HDInsight](apache-kafka-scalability.md)
 * [Utilizar o Spark transmissão em fluxo (DStreams) com Kafka](../hdinsight-apache-spark-with-kafka.md)
 * [Utilizar o Spark estruturado de transmissão em fluxo com Kafka](../hdinsight-apache-kafka-spark-structured-streaming.md)
