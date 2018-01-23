---
title: Arquitetura de Hadoop - Azure HDInsight | Microsoft Docs
description: Descreve o processamento e armazenamento de Hadoop em clusters do HDInsight.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 85383cc32e67db1f7e6964dc0b55bf3977311d40
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="hadoop-architecture-in-hdinsight"></a>Arquitetura de Hadoop no HDInsight

Hadoop inclui dois componentes nucleares, o sistema de ficheiros de densidade elevada (HDFS), que fornece armazenamento e ainda outro recurso Negotiator (YARN) que fornece processamento. Com as capacidades de armazenamento e processamento de um cluster torna-se com capacidade para executar programas de MapReduce para efetuar o processamento de dados pretendido.

> [!NOTE]
> Normalmente, não está implementada uma HDFS dentro do cluster do HDInsight para fornecer armazenamento. Em vez disso, uma camada de interface compatível com HDFS é utilizada pelos componentes do Hadoop. A capacidade de armazenamento real é fornecida pelo armazenamento do Azure ou do Azure Data Lake Store. Para o Hadoop, as tarefas do MapReduce em execução no cluster do HDInsight executam como se um HDFS estavam presentes e, por isso, requerem sem alterações para suportar as suas necessidades de armazenamento. Hadoop no HDInsight, externo do armazenamento, mas o processamento de YARN permanece um componente fundamental. 

<!--   As described in [HDInsight architecture](hdinsight-architecture.md)  -->

Este artigo apresenta YARN e como coordena a execução de aplicações do HDInsight.

## <a name="yarn-basics"></a>Noções básicas YARN 

YARN é regida pelas e orquestra processamento de dados no Hadoop. YARN tem dois núcleos os serviços executados como processos em nós do cluster: 

* ResourceManager 
* NodeManager

O ResourceManager concede aos recursos de computação de cluster para aplicações como as tarefas do MapReduce. O ResourceManager concede estes recursos, como contentores, onde cada contentor é composta por uma alocação de núcleos de CPU e memória RAM. Se combinado de todos os recursos disponíveis num cluster e, em seguida, distribuída-los em blocos de um determinado número de núcleos e memória, cada bloco de recursos é um contentor. Cada nó do cluster tem uma capacidade para um determinado número de contentores, e, por conseguinte, o cluster tem um limite fixo no número de contentores disponíveis. A alocação de recursos num contentor é configurável. 

Quando uma aplicação de MapReduce é executada num cluster, o ResourceManager fornece a aplicação os contentores na qual pretende executar. O ResourceManager controla o estado das aplicações, capacidade de cluster disponível, em execução e controla aplicações à medida que concluir e os respetivos recursos da versão. 

O ResourceManager também executa um processo de servidor web que fornece uma interface de utilizador web, que pode utilizar para monitorizar o estado das aplicações. 

Quando um utilizador submete uma aplicação de MapReduce para executar no cluster, a aplicação foi submetida para o ResourceManager. Por sua vez, o ResourceManager aloca um contentor em nós de NodeManager disponíveis. Os nós de NodeManager são onde executa efetivamente a aplicação. O contentor primeiro alocado executa uma aplicação especial, denominada a ApplicationMaster. Este ApplicationMaster é responsável por adquirir recursos, sob a forma de contentores subsequentes, necessário para executar a aplicação foi submetida. O ApplicationMaster examina as fases da aplicação (o mapa de testar e reduzir a fase) e fatores na quantidade de dados tem de ser processada. Em seguida, pede a ApplicationMaster (*negoceia*) dos recursos do ResourceManager em nome da aplicação. O ResourceManager por sua vez concede aos recursos da NodeManagers no cluster para Applicationmastê-lo a utilizar ao executar a aplicação. 

NodeManagers executadas as tarefas que compõem a aplicação, em seguida, informar os respetivos progresso e o estado a ApplicationMaster. O ApplicationMaster relatórios, por sua vez, o estado da aplicação para o ResourceManager. O ResourceManager devolve resultados para o cliente.

## <a name="yarn-on-hdinsight"></a>YARN no HDInsight

Todos os tipos de cluster do HDInsight implementar YARN. O ResourceManager é implementado para elevada disponibilidade com uma instância primária e secundária, o que executar no primeiro e segundo nós principais no cluster, respetivamente. Apenas a uma instância do ResourceManager está ativa cada vez. As instâncias de NodeManager sejam executadas em nós de trabalho disponíveis no cluster.

![YARN no HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="see-also"></a>Consulte também

* [Utilizar o MapReduce no Hadoop no HDInsight](hadoop/hdinsight-use-mapreduce.md)

<!--  * [HDInsight Architecture](hdinsight-architecture.md)  -->
