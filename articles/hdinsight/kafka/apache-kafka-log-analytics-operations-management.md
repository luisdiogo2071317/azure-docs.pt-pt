---
title: O log Analytics para o Apache Kafka - Azure HDInsight
description: Saiba como utilizar o Log Analytics para analisar os registos do cluster do Apache Kafka no HDInsight do Azure.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 69eaa0028f1115cafbd1ed28b66940d7faaed062
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608550"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Analisar registos para o Apache Kafka no HDInsight

Saiba como utilizar o Log Analytics para analisar os registos gerados pelo Apache Kafka no HDInsight.

## <a name="enable-log-analytics-for-apache-kafka"></a>Ativar o Log Analytics para o Apache Kafka

Os passos para ativar o Log Analytics para o HDInsight são os mesmos para todos os clusters do HDInsight. Utilize as ligações seguintes para compreender como criar e configurar os serviços necessários:

1. Crie uma área de trabalho do Log Analytics. Para obter mais informações, consulte a [começar com uma área de trabalho do Log Analytics](https://docs.microsoft.com/azure/log-analytics) documento.

2. Crie um Kafka num cluster do HDInsight. Para obter mais informações, consulte a [introdução ao Apache Kafka no HDInsight](apache-kafka-get-started.md) documento.

3. Configure o cluster de Kafka para utilizar o Log Analytics. Para obter mais informações, consulte a [utilize o Log Analytics para monitorizar o HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md) documento.

    > [!NOTE]  
    > Também pode configurar o cluster para utilizar o Log Analytics, utilizando o `Enable-AzureRmHDInsightOperationsManagementSuite` cmdlet. Este cmdlet requer as seguintes informações:
    >
    > * O nome do cluster do HDInsight.
    > * O ID de área de trabalho do Log Analytics. Pode encontrar o ID de área de trabalho na sua área de trabalho do Log Analytics.
    > * A chave primária para a ligação do Log Analytics. Para localizar o principal aberto de chave, a área de trabalho no portal do Azure, selecione __definições avançadas__ no menu à esquerda. A partir de definições avançadas, selecione __origens ligadas__>__servidores Linux__.


> [!IMPORTANT]  
> Pode demorar cerca de 20 minutos antes dos dados estão disponíveis para o Log Analytics.

## <a name="query-logs"></a>Registos de consulta

1. Partir do [portal do Azure](https://portal.azure.com), selecione a sua área de trabalho do Log Analytics.

2. Selecione __pesquisa de registos__. A partir daqui, pode pesquisar os dados recolhidos do Kafka. Seguem-se alguns exemplos de procura:

    * Utilização do disco: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Utilização da CPU: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Mensagens de entrada por segundo: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Bytes recebidos por segundo: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Bytes de saída por segundo: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]  
    > Substitua os valores de consulta com as suas informações específicas do cluster. Por exemplo, `ClusterName_s` tem de ser definido para o nome do cluster. `HostName_s` tem de ser definido para o nome de domínio de um nó de trabalho no cluster.

    Também pode introduzir `*` para pesquisar todos os tipos com sessão iniciados. Atualmente os seguintes registos estão disponíveis para consultas:

    | Tipo de registo | Descrição |
    | ---- | ---- |
    | registo\_kafkaserver\_CL | Server.log de Mediador Kafka |
    | registo\_kafkacontroller\_CL | Controller.log de Mediador Kafka |
    | métricas\_kafka\_CL | Métricas de JMX do Kafka |

    ![Imagem da pesquisa de utilização da CPU](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações no Log Analytics, consulte a [começar com uma área de trabalho do Log Analytics](../../log-analytics/log-analytics-get-started.md) documento.

Para obter mais informações sobre como trabalhar com o Apache Kafka, consulte os seguintes documentos:

 * [Espelho Apache Kafka, entre clusters do HDInsight](apache-kafka-mirroring.md)
 * [Aumentar a escalabilidade do Apache Kafka no HDInsight](apache-kafka-scalability.md)
 * [Utilizar a transmissão em fluxo (DStreams) com o Apache Kafka do Apache Spark](../hdinsight-apache-spark-with-kafka.md)
 * [Utilizar o Apache Spark estruturado de transmissão em fluxo com o Apache Kafka](../hdinsight-apache-kafka-spark-structured-streaming.md)
