---
title: Configurar ambientes de ciência de dados no Azure | Documentos da Microsoft
description: Configure dados de ambientes de ciência no Azure para utilização no processo de ciência de dados de equipa.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 481cfa6a-7ea3-46ac-b0f9-2e3982c37153
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 6c28a64830afeb19c6a9264888b296c3b99990d1
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39262570"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Configurar ambientes de ciência de dados para utilizar no Team Data Science Process
O processo de ciência de dados de equipa utiliza vários ambientes de ciência de dados para o armazenamento, processamento e análise de dados. Eles incluem o armazenamento de Blobs do Azure, vários tipos de máquinas virtuais do Azure, clusters de HDInsight (Hadoop) e áreas de trabalho do Azure Machine Learning. A decisão sobre o ambiente no qual pretende utilizar depende do tipo e a quantidade de dados a ser modelada e o destino de destino para que os dados na cloud. 

* Para obter orientações sobre questões a considerar ao tomar esta decisão, consulte [planear o Azure Machine Learning dados ciência ambiente](plan-your-environment.md). 
* Para um catálogo de alguns dos cenários que poderá encontrar durante a realização de análises avançadas, consulte [cenários para o processo de ciência de dados de equipa](plan-sample-scenarios.md)

Ligações neste menu para tópicos que descrevem como configurar os vários ambientes de ciência de dados utilizados pelo processo de ciência de dados de equipa.

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

O **Máquina Virtual de ciência de dados (DSVM) do Microsoft** também está disponível como uma imagem de máquina virtual do Azure (VM). Esta VM está previamente instalado e configurado com várias ferramentas populares que são frequentemente utilizadas para análise de dados e machine learning. A DSVM está disponível no Windows e Linux. Para obter mais informações, consulte [introdução para o baseado na nuvem dados de Máquina Virtual de ciência para Linux e Windows](../data-science-virtual-machine/overview.md).

Saiba como criar:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [DSVM do Ubuntu](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [VM de aprendizagem profunda](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
