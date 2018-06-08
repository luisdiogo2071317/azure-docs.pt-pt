---
title: Instruções de ciência de dados de Hadoop do HDInsight utilizando o Hive no Azure | Microsoft Docs
description: Exemplos do processo de ciência de dados de equipa percurso através da utilização de ramo de registo no Azure HDInsight Hadoop para Análise Preditiva.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: jhubbard
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
ms.openlocfilehash: 8b7eead12b6546ad86f6ff0aa48f4754c00a4734
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838760"
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a>Instruções de ciência de dados de Hadoop do HDInsight utilizando o Hive no Azure 

Estas instruções utilizam o Hive com um cluster do HDInsight Hadoop para Análise Preditiva. Siga os passos descritos no processo de ciência de dados de equipa. Para obter uma descrição geral do processo de ciência de dados de equipa, consulte [o processo de ciência de dados](overview.md). Para uma introdução ao Azure HDInsight, consulte [introdução ao Azure HDInsight, a pilha de tecnologia de Hadoop e clusters do Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md).

Instruções de ciência de dados adicionais que executar o processo de ciência de dados do agrupamento estão agrupadas pelo **plataforma** que utilizam. Consulte [instruções ao executar o processo de ciência de dados de equipa](walkthroughs.md) para um itemization destes exemplos.


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a>Prever sugestões taxi utilizando o Hive com o Hadoop do HDInsight

O [clusters do Hadoop do HDInsight de utilização](hive-walkthrough.md) instruções utilizam dados da Nova Iorque taxis para prever: 

- Indica se uma sugestão é paga 
- A distribuição de quantidades de sugestão

O cenário é implementado utilizando o Hive com uma [cluster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/). Saiba como armazenar, explore e dados da funcionalidade engenheiro de um NYC publicamente disponível taxi viagem e fare conjunto de dados. Também é utilizar o Azure Machine Learning para criar e implementar os modelos.

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a>Prever cliques de anúncios utilizando o Hive com o Hadoop do HDInsight

O [utilização do Azure HDInsight Hadoop Clusters um conjunto de dados de 1 TB](hive-criteo-walkthrough.md) instruções utilizam um publicamente disponíveis [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) clique em conjunto de dados para prever se uma sugestão é pagas e o intervalo de quantidades esperado. O cenário é implementado utilizando o Hive com uma [cluster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) para armazenar, explore, engenheiro de funcionalidade e pendente dados de exemplo. Utiliza o Azure Machine Learning para criar, dar formação e Pontuar um modelo de classificação binária prever se um utilizador clica num anúncio. Conclui as instruções que mostra como publicar um destes modelos como um serviço Web.


## <a name="next-steps"></a>Passos Seguintes

Para ver um debate os componentes chave que compõem o processo de ciência de dados de equipa, consulte [descrição geral do processo de ciência de dados de equipa](overview.md).

Para um debate sobre o ciclo de vida do processo de ciência de dados de equipa que pode utilizar para estruturar seus projetos de ciência de dados, consulte [ciclo de vida do processo de ciência de dados de equipa](lifecycle.md). O ciclo de vida descreve os passos, do início ao fim, que projetos siga, normalmente, quando estes são executados. 

