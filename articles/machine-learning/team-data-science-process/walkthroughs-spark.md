---
title: Orientações passo a passo HDInsight Spark com PySpark e Scala no Azure | Documentos da Microsoft
description: Exemplos do processo de ciência de dados de equipa que orientam através da utilização de PySpark e Scala do Azure HDInsight Spark para realizar a Análise Preditiva.
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
ms.openlocfilehash: 2fd6d556e908c47baf3c58c46172c0684b66cdd9
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394273"
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>Instruções de ciência de dados HDInsight Spark com PySpark e Scala no Azure

Esta instruções passo a passo usar PySpark e Scala num cluster do Spark do Azure para fazer a Análise Preditiva. Eles seguem as etapas descritas no processo de ciência de dados de equipa. Para uma descrição geral do processo de ciência de dados de equipa, consulte [processo de ciência de dados](overview.md). Para uma descrição geral do Spark no HDInsight, consulte [introdução ao Spark no HDInsight](../../hdinsight/spark/apache-spark-overview.md).

Instruções de ciência de dados adicionais que executar o processo de ciência de dados de equipa estão agrupadas pela **plataforma** que utilizam. Ver [instruções passo a passo, executar o processo de ciência de dados de equipa](walkthroughs.md) para também uma itemização destes exemplos.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Prever gorjetas para táxis com PySpark no Spark do Azure

O [utilizar o Spark no Azure HDInsight](spark-overview.md) estas instruções utilizam dados a partir de Nova Iorque capotados para prever se uma dica é pago e o intervalo de quantidades esperado ser paga. Ele usa o processo de ciência de dados de equipa num cenário com um [cluster Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/) para armazenar, explorar e apresentam os dados de engenheiro da viagem de táxis NYC publicamente disponível e se comportarão de conjunto de dados. Este tópico de descrição geral define-se com um cluster do Spark do HDInsight e o Jupyter PySpark notebooks utilizados no resto do passo a passo. Estes blocos de notas mostram-lhe como explorar os seus dados e, em seguida, como criar e consumir modelos. O bloco de notas do exploração e modelação avançada de dados mostra como incluir a validação cruzada, hyper varrimento, parâmetro e modelar a avaliação.

### <a name="data-exploration-and-modeling-with-spark"></a>Exploração de dados e modelação com o Spark 
Explorar o conjunto de dados e criar, classificação e avaliar os modelos de machine learning ao trabalhar com o [criar modelos de classificação e regressão binários para dados com o Kit de ferramentas do Spark MLlib](spark-data-exploration-modeling.md) tópico.

### <a name="model-consumption"></a>Consumo de modelo
Para saber como classificar modelos de classificação e regressão criados neste tópico, consulte [pontuação e avaliar modelos de aprendizagem automática criados com o Spark](spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>A validação cruzada e varrimento de hiper-parâmetros
Ver [avançada de exploração de dados e modelagem com o Spark](spark-advanced-data-exploration-modeling.md) na forma como os modelos podem ser treinado varrimento de validação cruzada e de hyper-parâmetro a utilizar.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Prever gorjetas para táxis com o Scala Spark do Azure

O [utilizar Scala spark no Azure](scala-walkthrough.md) estas instruções utilizam dados a partir de Nova Iorque capotados para prever se uma dica é pago e o intervalo de quantidades esperado ser paga. Ele mostra como utilizar o Scala para supervisionadas do machine learning tarefas com o Spark do machine learning biblioteca (MLlib) e pacotes de SparkML num cluster do Azure HDInsight Spark. Ele explica as tarefas que constituem a [processo de ciência de dados](http://aka.ms/datascienceprocess): ingestão de dados e exploração, visualização, engenharia de funcionalidades, modelação e consumo do modelo. Modelos construídos incluem regressão logística e linear, florestas aleatórias e árvores de elevada gradação.


## <a name="next-steps"></a>Passos Seguintes

Para uma discussão sobre os principais componentes que compõem o processo de ciência de dados de equipa, consulte [descrição geral do processo de ciência de dados de equipa](overview.md).

Para uma discussão sobre o ciclo de vida do processo de ciência de dados de equipa que pode utilizar para estruturar os seus projetos de ciência de dados, consulte [ciclo de vida do processo de ciência de dados de equipa](lifecycle.md). O ciclo de vida descreve os passos, do início ao fim, que projetos seguem, normalmente, quando forem executados. 

