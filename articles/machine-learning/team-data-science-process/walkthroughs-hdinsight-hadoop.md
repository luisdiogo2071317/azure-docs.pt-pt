---
title: Instruções de ciência de dados de Hadoop do HDInsight com o Hive no Azure | Documentos da Microsoft
description: Exemplos do processo de ciência de dados de equipa que o orientam por meio do uso do Hive no Azure HDInsight Hadoop para realizar a Análise Preditiva.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 2aa64d6dc144e85e1a0f03b8548fa47dba91a2eb
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393164"
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a>Instruções de ciência de dados de Hadoop do HDInsight com o Hive no Azure 

Esta instruções passo a passo utiliza o Hive com um cluster de Hadoop do HDInsight para realizar a Análise Preditiva. Eles seguem as etapas descritas no processo de ciência de dados de equipa. Para uma descrição geral do processo de ciência de dados de equipa, consulte [processo de ciência de dados](overview.md). Para uma introdução ao Azure HDInsight, consulte [introdução ao Azure HDInsight, a pilha de tecnologia do Hadoop e os clusters do Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md).

Instruções de ciência de dados adicionais que executar o processo de ciência de dados de equipa estão agrupadas pela **plataforma** que utilizam. Ver [instruções passo a passo, executar o processo de ciência de dados de equipa](walkthroughs.md) para também uma itemização destes exemplos.


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a>Prever gorjetas para utilizar o Hive com o HDInsight Hadoop

O [clusters do Hadoop de HDInsight utilização](hive-walkthrough.md) estas instruções utilizam dados a partir de Nova Iorque capotados para prever: 

- Se uma dica é pago 
- A distribuição de quantidades de sugestão

O cenário é implementado utilizando o Hive com uma [cluster do Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/). Irá aprender a armazenar, explorar e dados de engenheiro de funcionalidade de um NYC disponível publicamente táxis conjunto de dados de viagens e Europeia. Também utilizar o Azure Machine Learning para criar e implementar os modelos.

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a>Prever cliques de anúncio, utilizando o Hive com o HDInsight Hadoop

O [Clusters do Hadoop de HDInsight do uso do Azure num conjunto de dados de 1 TB](hive-criteo-walkthrough.md) estas instruções utilizam um publicamente disponíveis [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) clique em conjunto de dados para prever se uma dica é pago e o intervalo de quantidades esperado. O cenário é implementado utilizando o Hive com uma [cluster do Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) para armazenar, explorar, engenheiro de recursos e reduzir os dados de exemplo. Ele usa o Azure Machine Learning para criar, preparar e classificar um modelo de classificação binária prever se um usuário clica num anúncio. Conclusão do passo a passo que mostra como publicar um desses modelos como um serviço Web.


## <a name="next-steps"></a>Passos Seguintes

Para uma discussão sobre os principais componentes que compõem o processo de ciência de dados de equipa, consulte [descrição geral do processo de ciência de dados de equipa](overview.md).

Para uma discussão sobre o ciclo de vida do processo de ciência de dados de equipa que pode utilizar para estruturar os seus projetos de ciência de dados, consulte [ciclo de vida do processo de ciência de dados de equipa](lifecycle.md). O ciclo de vida descreve os passos, do início ao fim, que projetos seguem, normalmente, quando forem executados. 

