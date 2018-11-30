---
title: Configurar ambientes de ciência de dados no Azure | Documentos da Microsoft
description: Configure dados de ambientes de ciência no Azure para utilização no processo de ciência de dados de equipa.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/29/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: c8154400621fae719c7097e36c8e14d3f946d7c2
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445912"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Configurar ambientes de ciência de dados para utilizar no Team Data Science Process
O processo de ciência de dados de equipa utiliza vários ambientes de ciência de dados para o armazenamento, processamento e análise de dados. Eles incluem o armazenamento de Blobs do Azure, vários tipos de máquinas virtuais do Azure, clusters de HDInsight (Hadoop) e áreas de trabalho do Azure Machine Learning. A decisão sobre o ambiente no qual pretende utilizar depende do tipo e a quantidade de dados a ser modelada e o destino de destino para que os dados na cloud. 

* Para obter orientações sobre questões a considerar ao tomar esta decisão, consulte [planear o Azure Machine Learning dados ciência ambiente](plan-your-environment.md). 
* Para um catálogo de alguns dos cenários que poderá encontrar durante a realização de análises avançadas, consulte [cenários para o processo de ciência de dados de equipa](plan-sample-scenarios.md)

Os seguintes artigos descrevem como configurar os vários ambientes de ciência de dados utilizados pelo processo de ciência de dados de equipa.

* [Conta de armazenamento do Azure](../../storage/common/storage-quickstart-create-account.md)
* [Cluster do HDInsight (Hadoop)](customize-hadoop-cluster.md)
* [Área de trabalho de Machine Learning Studio do Azure](../studio/create-workspace.md)

O **Máquina Virtual de ciência de dados (DSVM) do Microsoft** também está disponível como uma imagem de máquina virtual do Azure (VM). Esta VM está previamente instalado e configurado com várias ferramentas populares que são frequentemente utilizadas para análise de dados e machine learning. A DSVM está disponível no Windows e Linux. Para obter mais informações, consulte [introdução para o baseado na nuvem dados de Máquina Virtual de ciência para Linux e Windows](../data-science-virtual-machine/overview.md).

Saiba como criar:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [DSVM do Ubuntu](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [VM de aprendizagem profunda](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
