---
title: Topologias do Apache Storm de exemplo no HDInsight | Microsoft Docs
description: "Uma lista das topologias do Storm de exemplo criado e testado com Apache Storm no HDInsight, incluindo as topologias c# e Java básicas e trabalhar com os Event Hubs."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: c6c1a1483191e654c245eb3f05ee9e8406510b08
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Topologias do Storm de exemplo e componentes para o Apache Storm no HDInsight

Segue-se uma lista de exemplos criado e mantido pela Microsoft para utilização com Apache Storm no HDInsight. Estes exemplos abrangem uma variedade de tópicos, criar básico c# e topologias de Java para trabalhar com os serviços do Azure como a base de dados SQL do Cosmos DB do Event Hubs, HBase no HDInsight e Storage do Azure. Alguns exemplos demonstram também como trabalhar com tecnologias não do Azure, ou ainda não sejam da Microsoft, tais como SignalR e Socket.IO.

| Descrição | Demonstra | Arquitetura de linguagem / |
|:--- |:--- |:--- |
| [Escrever para o Azure Data Lake Store do Apache Storm](apache-storm-write-data-lake-store.md) |Escrever para o Azure Data Lake Store |Java |
| [Origem de evento Hub Spout e Bolt](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Origem do Event Hub Spout e Bolt |Java |
| [Desenvolver topologias baseadas em Java para o Apache Storm no HDInsight][5797064f] |Maven |Java |
| [Desenvolver topologias c# para Apache Storm no HDInsight com o Visual Studio][16fce2d1] |Ferramentas do HDInsight para o Visual Studio |C#, Java |
| [Processar eventos provenientes dos Hubs de eventos do Azure com o Storm no HDInsight (c#)][844d1d81] |Event Hubs |C# e Java |
| [Process events from Azure Event Hubs with Storm on HDInsight (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/) (Processar eventos dos Hubs de Eventos do Azure com o Storm no HDInsight [Java]) |Event Hubs |Java |
| [Processar os dados de sensor vehicle provenientes dos Hubs de eventos a utilizar o Storm no HDInsight][246ee964] |Event Hubs, Cosmos DB, armazenamento do Azure Blob (WASB) |C#, Java |
| [Extração, transformação e carregamento (ETL) de Event Hubs do Azure para HBase, utilizar o Storm no HDInsight][b4b68194] |Event Hubs, HBase |C# |
| [Projeto de topologia de c# Storm de modelo para trabalhar com os serviços do Azure do Storm no HDInsight][ce0c02a2] |Event Hubs, Cosmos DB, SQL Server da base de dados, HBase, SignalR |C#, Java |
| [Testes de desempenho de escalabilidade para ler de utilizar o Storm no HDInsight de Event Hubs do Azure][d6c540e3] |Débito de mensagem, os Event Hubs, base de dados SQL |C#, Java |
| [Utilizar o Python com o Storm no HDInsight](apache-storm-develop-python-topology.md) |Componentes de Python com uma topologia de Flux |Python |
| [Utilizar Kafka Storm no HDInsight](../hdinsight-apache-storm-with-kafka.md) | Apache Storm leitura e escrita Apache Kafka | Java |

### <a name="next-steps"></a>Próximos Passos

* [Introdução ao Apache Storm no HDInsight][2b8c3488]
* [Saiba como implementar e gerir topologias do Storm com o Storm no HDInsight][6eb0d3b8]

[2b8c3488]:apache-storm-tutorial-get-started-linux.md "Saiba como criar um Storm no cluster do HDInsight e utilizar o Storm Dashboard para implementar as topologias de exemplo."
[6eb0d3b8]:apache-storm-deploy-monitor-topology.md "Saiba como implementar e gerir topologias utilizando o Dashboard do Storm baseado na web e a IU do Storm ou as ferramentas do HDInsight para Visual Studio."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Saiba como criar topologias de C#, utilizando as ferramentas do HDInsight para Visual Studio."
[5797064f]:apache-storm-develop-java-topology.md "Saiba como criar topologias Storm em Java, com o Maven, criando uma topologia do wordcount básico."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "Saiba como ler e escrever dados a partir de Hubs de eventos do Azure com o Storm no HDInsight."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Saiba como utilizar uma topologia do Storm para ler mensagens a partir de Event Hubs do Azure, leia os documentos da base de dados do Azure Cosmos para referenciar a dados e guardar dados no armazenamento do Azure."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Várias topologias para demonstrar o débito quando ao ler a partir de Event Hubs do Azure e armazenar a base de dados do SQL Server utiliza o Apache Storm no HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Saiba como ler dados a partir do Event Hubs do Azure, agregação & transformar os dados, em seguida, guarde-o para o HBase no HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Este projeto contém modelos para spouts, bolts e topologias para interagir com vários serviços do Azure como base de dados SQL, base de dados do Cosmos e Hubs de eventos."

