---
title: Outros produtos de aprendizagem automática da Microsoft - Azure Machine Learning | Microsoft Docs
description: Para além do Azure Machine Learning, existem diversas opções na Microsoft para criar, implementar e gerir modelos de aprendizagem automática.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: garyericson, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: overview
ms.date: 04/11/2018
ms.openlocfilehash: 20fd51f8906861e725b80202235e1684c31fd562
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268404"
---
# <a name="other-machine-learning-products-and-services-from-microsoft"></a>Outros produtos de aprendizagem automática e serviços da Microsoft

Para além do [Azure Machine Learning](overview-what-is-azure-ml.md), existem diversas opções na Microsoft para criar, implementar e gerir modelos de aprendizagem automática. 
* Serviços de Machine Learning do SQL Server
* Microsoft Machine Learning Server
* Máquina Virtual de Ciência de Dados do Azure
* Spark MLLib no HDInsight
* Serviço Batch AI Training
* Microsoft Cognitive Toolkit (CNTK)
* Serviços Cognitivos do Azure


## <a name="sql-server-machine-learning-services"></a>Serviços de Machine Learning do SQL Server
O [Microsoft Machine Learning Services do SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) permite-lhe executar, preparar e implementar modelos de aprendizagem automática com R ou Python. Pode utilizar os dados localizados no local e em bases de dados do SQL Server. 

Utilize o Microsoft Machine Learning Services quando tiver de preparar ou implementar modelos no local ou no Microsoft SQL Server. Os modelos criados com o Machine Learning Services podem ser implementados com a Gestão de Modelos do Azure Machine Learning. 

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server 
O [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) é um servidor empresarial para alojar e gerir cargas de trabalho paralelas e distribuídas de processos de R e Python. O Microsoft Machine Learning Server funciona em Linux, Windows, Hadoop e Apache Spark. Também está disponível no [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Oferece um motor de execução para as soluções com [pacotes do Microsoft Machine Learning](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) e expande o R e o Python de código aberto com suporte para os cenários seguintes:

- Análises de elevado desempenho
- Análises estatísticas
- Aprendizagem automática
- Conjuntos de dados extremamente grandes

Os pacotes proprietários, que se instalam com o servidor, constituem uma funcionalidade adicional. Para desenvolvimento, pode utilizar IDEs como o [R Tools for Visual Studio](https://www.visualstudio.com/vs/rtvs/) e o [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python/).

Utilize o Microsoft Machine Learning Server quando precisar de:

- Criar e implementar modelos criados com R e Python num servidor
- Distribuir preparações em R e Python em escala num cluster do Hadoop ou do Spark

## <a name="azure-data-science-virtual-machine"></a>Máquina Virtual de Ciência de Dados do Azure
A [Máquina Virtual de Ciência de Dados](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) é uma máquina virtual personalizada na cloud do Microsoft Azure, concebida especificamente para fazer ciência de dados. Tem muitas ferramentas populares de ciência de dados e outras pré-instaladas e pré-configuradas para permitir a rápida criação de aplicações inteligentes para análises avançadas. A VM de Ciência de Dados está disponível nas versões do Windows Server 2016 e 2012, e numa VM do Linux no Ubuntu 16.04 LTS e em distribuições baseadas em 7.4 OpenLogic CentOS. 

Utilize a VM de Ciência de Dados quando precisar de executar ou alojar as suas tarefas num único nó. Ou se tiver de aumentar verticalmente de forma remota o processamento numa máquina individual. A Máquina Virtual de Ciência de Dados é suportada como destino para a Experimentação do Azure Machine Learning e a Gestão de Modelos do Azure Machine Learning. 

## <a name="spark-mllib-in-hdinsight"></a>Spark MLLib no HDInsight
O [Spark MLLib no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) permite-lhe criar modelos como parte de trabalhos do Spark que estão a ser executados em macrodados. Com o Spark, pode facilmente transformar e preparar dados e, depois, aumentar horizontalmente a criação de modelos num único trabalho. Os modelos criados através do Spark MLLib podem ser implementados, geridos e monitorizados com a Gestão de Modelos do Azure Machine Learning. As execuções de preparações podem ser enviadas e geridas com a Experimentação do Azure Machine Learning. O Spark também pode ser utilizado para aumentar horizontalmente trabalhos de preparação de dados criados no Machine Learning Workbench. 

Utilize o Spark quando precisar de aumentar horizontalmente o processamento de dados e criar modelos como parte de um pipeline de dados. Pode criar trabalhos do Spark em Scala, Java, Python ou R. 

## <a name="batch-ai-training"></a>Batch AI Training 
O [Azure Batch AI Training](https://aka.ms/batchaitraining) ajuda-o a fazer experimentações em paralelo com os seus modelos de AI através de qualquer arquitetura e, depois, prepara-os em escala em GPUs em clusters.  Descreva os requisitos do seu trabalho e a configuração a executar e nós tratamos do resto. 

O Batch AI Training permite-lhe aumentar horizontalmente trabalhos de aprendizagem profunda em GPUs em clusters mediante a utilização de arquiteturas como:

- Cognitive Toolkit
- Caffe
- Chainer
- TensorFlow

A Gestão de Modelos do Azure Machine Learning pode ser utilizada para pegar em modelos do Batch AI Training para implementá-los, geri-los e monitorizá-los.  O Batch AI Training vai ser integrado na Experimentação do Azure Machine Learning no futuro. 

## <a name="microsoft-cognitive-toolkit-cntk"></a>Microsoft Cognitive Toolkit (CNTK)
O [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) é um conjunto de ferramentas unificadas de aprendizagem profunda que descreve as redes neurais como fases computacionais num gráfico direcionado. Neste gráfico direcionado, os nós de folha representam os valores de entrada ou os parâmetros de rede, ao passo que os outros nós representam operações de matriz após as respetivas entradas. O Cognitive Toolkit permite-lhe ver e combinar facilmente tipos de modelos populares, como DNNs (Deep Neural Networks, Redes Neurais Profundas) feed-forward, redes convolucionais (CNNs) e redes recorrentes (RNNs/LSTMs). Implementa a aprendizagem de gradiente descendente estocástico (SGD [stochastic gradient descent], propagação de erros retroativa) com diferenciação e paralelização automática em várias GPUs e servidores.

Utilize o Cognitive Toolkit quando quiser utilizar a aprendizagem profunda para criar um modelo.  O Cognitive Toolkit pode ser utilizado em qualquer um dos serviços anteriores.

## <a name="azure-cognitive-services"></a>Serviços Cognitivos do Azure
Os [Serviços Cognitivos do Azure](https://docs.microsoft.com/azure/#pivot=products&panel=ai) são um conjunto de cerca de 30 APIs com as quais pode criar aplicações que utilizam métodos neurais de comunicação. Estas APIs permitem que as suas aplicações vejam, escutem, falem, compreendam e interpretem as necessidades do utilizador com apenas algumas linhas de código. Adicione facilmente funcionalidades inteligentes às suas aplicações, tais como: 

- Deteção de emoções e sentimentos
- Reconhecimento de voz e visão
- Language understanding (LUIS)
- Conhecimentos e investigação

Os Serviços Cognitivos podem servir para desenvolver aplicações em vários dispositivos e plataformas. As APIs são melhoradas constantemente e são fáceis de configurar. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Passos seguintes

Leia a descrição geral do [Azure Machine Learning](overview-what-is-azure-ml.md).
