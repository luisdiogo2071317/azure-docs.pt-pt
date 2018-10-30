---
title: Migrar clusters do Apache Hadoop no local para o Azure HDInsight - motivação e benefícios
description: Aprenda a motivação e os benefícios para migrar clusters do Hadoop no local para Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: efaedccde854d8cd7fa777aa9457db7203ce833a
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221946"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---motivation-and-benefits"></a>Migrar clusters do Apache Hadoop no local para o Azure HDInsight - motivação e benefícios

Este artigo é a primeira de uma série sobre melhores práticas para a migração de implementações no local Apache Hadoop ao meio ambiente do sistema para o Azure HDInsight. Esta série de artigos destina-se a pessoas que são responsáveis pelo design, implementação e migração de soluções do Apache Hadoop no HDInsight do Azure. As funções que podem tirar partido destes artigos incluem arquitetos de cloud, os administradores do Hadoop e engenheiros de DevOps. Os desenvolvedores de software, os engenheiros de dados e os cientistas de dados devem também se beneficiar a explicação de diferentes tipos de clusters de trabalho na cloud.

## <a name="why-to-migrate-to-azure-hdinsight"></a>Por que migrar para o Azure HDInsight

O Azure HDInsight é uma distribuição de nuvem dos componentes do Hadoop do [Hortonworks Data Platform(HDP)](https://hortonworks.com/products/data-center/hdp/). O Azure HDInsight torna mais fácil, rápido e rentável processar quantidades enormes de dados. HDInsight inclui as arquiteturas de código-fonte aberto mais populares, tais como:

- Apache Hadoop
- Apache Spark
- Apache Hive com o LLAP
- Apache Kafka
- Apache Storm
- Apache HBase
- R

## <a name="advantages-that-azure-hdinsight-offers-over-on-premises-hadoop"></a>Vantagens do Azure HDInsight oferece ao longo do Hadoop no local

- **Baixo custo** -os custos podem ser reduzidos através de [criar clusters a pedido](../hdinsight-hadoop-create-linux-clusters-adf.md) e pagando apenas aquilo que utiliza. Computação e armazenamento desacoplados proporciona flexibilidade ao manter o volume de dados independente do tamanho do cluster.
- **Automatizada de criação do cluster** - automatizada criação do cluster requer configuração mínima e a configuração. Automatização pode ser utilizada para clusters a pedido.
- **Geridos hardware e configuração** -não é necessário se preocupar sobre o hardware físico ou a infraestrutura com um cluster do HDInsight. Especifique apenas a configuração do cluster, e o Azure o configura.
- **Facilmente escalável** -HDInsight permite-lhe [dimensionamento](../hdinsight-administer-use-portal-linux.md) cargas de trabalho ou reduzir verticalmente. Azure se encarrega de redistribuição de dados e a carga de trabalho reequilibrar sem interromper as tarefas de processamento de dados.
- **Disponibilidade global** -HDInsight está disponível em mais [regiões](https://azure.microsoft.com/regions/services/) que qualquer outra oferta de análise de macrodados. O Azure HDInsight também está disponível no Azure Government, na China e na Alemanha, o que lhe permite satisfazer as suas necessidades empresariais nas principais áreas soberanas.
- **Segura e em conformidade** -HDInsight permite-lhe proteger seus ativos de dados empresariais com [rede Virtual do Azure](../hdinsight-extend-hadoop-virtual-network.md), [encriptação](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md)e a integração com [Active Directory do Azure Diretório](../domain-joined/apache-domain-joined-introduction.md). HDInsight também cumpre os mais populares da indústria e governamentais [normas de conformidade](https://azure.microsoft.com/overview/trusted-cloud).
- **Gerenciamento de versão simplificado** -Azure HDInsight gere a versão dos componentes do Hadoop ao meio ambiente do sistema e as mantém atualizadas. Atualizações de software são, normalmente, um processo complexo para implementações no local.
- **Clusters menores otimizadas para cargas de trabalho específicas com menos dependências entre os componentes** -uma configuração de Hadoop típico no local utiliza um cluster único que tem muitas finalidades. Com o Azure HDInsight, é possível criar clusters de carga de trabalho específica. Criação de clusters para cargas de trabalho específicas, remove a complexidade de manter um único cluster com a crescente complexidade.
- **Produtividade** -pode utilizar várias ferramentas para o Hadoop e Spark no seu ambiente de desenvolvimento preferido.
- **Extensibilidade com ferramentas personalizadas ou aplicações de terceiros** -clusters do HDInsight podem ser estendidas com componentes instalados e também pode ser integrados com as outras soluções de macrodados utilizando [num único clique](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/)  implementações a partir do mercado do Azure.
- **Facilitar a gestão, administração e monitoramento** -Azure HDInsight integra-se [Log Analytics do Azure](../hdinsight-hadoop-oms-log-analytics-tutorial.md) para fornecer uma única interface com a qual pode monitorizar todos os clusters.
- **Integração com outros serviços do Azure** -HDInsight pode ser facilmente integrado com outros serviços do Azure populares, como o seguinte:

    - Azure Data Factory (ADF)
    - Armazenamento de Blobs do Azure
    - Armazenamento do Azure Data Lake Ger2
    - Azure Cosmos DB
    - Base de Dados SQL do Azure
    - Azure Analysis Services

- **Autorrecuperação processos e componentes** -HDInsight verifique constantemente os componentes de infraestrutura e o código-fonte aberto com a própria infraestrutura de monitorização. Ele também automaticamente recupera falhas críticas, como indisponibilidade de componentes de código aberto e nós. Alertas são acionados no Ambari se qualquer componente de sistemas operacionais falhou.

Para obter mais informações, consulte o artigo [o que é o Azure HDInsight e a pilha de tecnologia do Hadoop](../hadoop/apache-hadoop-introduction.md).

## <a name="migration-planning-process"></a>Processo de planejamento de migração

Os seguintes passos são recomendados para planejar uma migração de clusters do Hadoop no local para Azure HDInsight:

1. Compreenda a implementação de no local atual e topologias.
2. Compreenda o escopo do projeto atual, as linhas do tempo e conhecimentos de equipe.
3. Compreenda os requisitos do Azure.
4. Crie um plano detalhado, com base nas práticas recomendadas.

## <a name="next-steps"></a>Passos Seguintes

Leia o artigo seguinte nesta série:

- [Práticas recomendadas de arquitetura para no local para migração do Azure HDInsight Hadoop](apache-hadoop-on-premises-migration-best-practices-architecture.md)