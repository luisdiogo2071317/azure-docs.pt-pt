---
title: O que é o HBase no Azure HDInsight?
description: Uma introdução ao Apache HBase no HDInsight, uma base de dados NoSQL baseada no Hadoop. Saiba mais sobre casos de utilização e compare o HBase a outros clusters do Hadoop.
keywords: bigtable, nosql, o que é hbase, apache hbase, hbase, descrição geral de habase,
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: 8bcdccc0a8cdb50d2c587513b887f6dfdb95c13f
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51287292"
---
# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>O que é o HBase no HDInsight: uma base de dados NoSQL que fornece capacidades semelhantes a BigTable para o Hadoop
O Apache HBase é uma base de dados NoSQL open source baseada no Hadoop e modelada segundo o Google BigTable. O HBase fornece acesso aleatório e consistência forte para grandes quantidades de dados não estruturados e semiestruturados numa base de dados sem esquema, organizada por famílias de colunas.

Da perspectiva do usuário, o HBase é semelhante a uma base de dados. Os dados são armazenados em linhas e colunas de uma tabela e dados de uma linha são agrupados por família de colunas. O HBase é uma base de dados sem esquema uma vez que não é preciso definir as colunas nem os tipos de dados nelas armazenados antes de os utilizar. O código open source é dimensionado linearmente para processar petabytes de dados em milhares de nós. Pode depender da redundância de dados, do processamento em lotes e de outras funcionalidades fornecidas por aplicações distribuídas do ecossistema do Hadoop.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Como é implementado o HBase no Azure HDInsight?

O HBase do HDInsight é fornecido como um cluster gerido que está integrado no ambiente do Azure. Os clusters são configurados para armazenar dados diretamente no [armazenamento do Azure](./../hdinsight-hadoop-use-blob-storage.md) que proporciona baixa latência e uma maior elasticidade em termos de opções de desempenho e custo. Isto permite aos clientes criar sites interativos que funcionam com grandes conjuntos de dados, criar serviços que armazenam dados telemétricos e de sensores de milhões de pontos finais e analisar esses dados com tarefas do Hadoop. O HBase e o Hadoop são bons pontos de partida para projetos de macrodados no Azure; em particular, podem permitir que as aplicações em tempo real trabalhem com grandes conjuntos de dados.

A implementação do HDInsight tira partido da arquitetura de escalamento horizontal do HBase para fornecer fragmentação automática de tabelas, consistência forte para operações de leitura e escrita e ativação pós-falha automática. O desempenho é melhorado graças à colocação em cache dentro da memória para as operações de leitura e à transmissão em fluxo de alto débito para as operações de escrita. O cluster HBase pode ser criado no interior da rede virtual. Para obter mais detalhes, veja [Create HDInsight clusters on Azure Virtual Network](./apache-hbase-provision-vnet.md) (Criar clusters do HDInsight na Rede Virtual do Azure).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Como são geridos os dados no HBase do HDInsight?
É possível gerir os dados no HBase ao utilizar os comandos `create`, `get`, `put` e `scan` da shell do HBase. Os dados são escritos na base de dados através do comando `put` e lidos através do comando `get`. O comando `scan` é utilizado para obter dados de várias linhas numa tabela. É possível ainda gerir os dados através da API C# do HBase, que fornece uma biblioteca de cliente sobre a API REST do HBase. Além disso, é possível consultar uma base de dados do HBase através do Hive. Para uma introdução a estes modelos de programação, veja [Get started using HBase with Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md) (Introdução à utilização do HBase com o Hadoop no HDInsight). Estão também disponíveis coprocessadores, que permitem o processamento de dados nos nós que alojam a base de dados.

> [!NOTE]
> O thrift não é suportado pelo HBase no HDInsight.
>

## <a name="scenarios-use-cases-for-hbase"></a>Cenários: casos de utilização do HBase
O caso de utilização canónico para o qual o BigTable (e, por extensão, o HBase) foi criado a partir de pesquisa na web. Os motores de busca criam índices que mapeiam termos para as páginas Web que os contêm. No entanto, o HBase é adequado para muitos outros casos de utilização, sendo que vários deles estão descritos nesta secção.

* Arquivo de chave-valor
  
    O HBase pode ser utilizado como um arquivo de chave-valor e é adequado para gerir sistemas de mensagens. O Facebook utiliza o HBase no seu sistema de mensagens e este é ideal para armazenar e gerir comunicações na Internet. O WebTable utiliza o HBase para procurar e gerir tabelas extraídas de páginas Web.
* Dados de sensores
  
    O HBase é útil para capturar os dados que são recolhidos de forma incremental a partir de várias origens. Isto inclui a análise de dados sociais, as séries de tempo, a atualização de dashboards interativos com tendências e contadores e a gestão de sistemas de registo de auditoria. Os exemplos incluem o terminal comercial Bloomberg e o OpenTSDB (Open Time Series Database), que armazena e fornece acesso a métricas recolhidas sobre o estado de funcionamento dos sistemas de servidor.
* Consulta em tempo real
  
    O [Phoenix](http://phoenix.apache.org/) é um motor de consulta SQL para o Apache HBase. Está acessível como um controlador JDBC e permite a consulta e gestão de tabelas do HBase através de SQL.
* HBase como uma plataforma
  
    As aplicações podem ser executadas sobre o HBase ao utilizá-lo como um arquivo de dados. Os exemplos incluem Phoenix, OpenTSDB, Kiji e Titan. As aplicações também podem ser integradas no HBase. Os exemplos incluem Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia e Drill.

## <a name="next-steps"></a>Passos seguintes
* [Get started using HBase with Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md) (Introdução à utilização do HBase com o Hadoop no HDInsight)
* [Create HDInsight clusters on Azure Virtual Network](./apache-hbase-provision-vnet.md) (Criar clusters do HDInsight na Rede Virtual do Azure)
* [Configurar a replicação do HBase no HDInsight](apache-hbase-replication.md)
* [Use Maven to build Java applications that use HBase with HDInsight (Hadoop)](./apache-hbase-build-java-maven-linux.md) (Utilizar o Maven para criar aplicações Java que utilizam o HBase com o HDInsight [Hadoop])

## <a name="see-also"></a>Ver também
* [Apache HBase](https://hbase.apache.org/)
* [Guia de referência do HBase do Apache](https://hbase.apache.org/book.html)
* [Bigtable: um Sistema de Armazenamento Distribuído para Dados Estruturados](http://research.google.com/archive/bigtable.html)
* [Apache HBase/Phoenix – dicas, truques e práticas recomendadas no Azure HDInsight](https://blogs.msdn.microsoft.com/ashish/2016/08/28/hdinsight-hbase-faq/)




