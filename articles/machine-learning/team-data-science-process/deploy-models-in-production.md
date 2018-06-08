---
title: Implementar modelos em produção - Azure Machine Learning | Microsoft Docs
description: Como implementar modelos para produção, permitindo-lhes desempenha um papel Active Directory nas decisões de negócio.
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
ms.date: 11/30/2017
ms.author: deguhath
ms.openlocfilehash: 0505d58261ec32015f8847b710791249f87f049b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838297"
---
# <a name="deploy-models-in-production"></a>Implementar modelos na produção

Implementação de produção permite que um modelo de reproduzir uma função ativa numa empresa. Predições de um modelo implementada podem ser utilizadas para as decisões de negócio.

## <a name="production-platforms"></a>Plataformas de produção
Existem várias abordagens e plataformas para colocar os modelos em produção. Seguem-se algumas opções:


- [Modelo de implementação no Azure Machine Learning](../desktop-workbench/model-management-overview.md)
- [Implementação de um modelo no SQL server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


>[!NOTE]
>Antes da implementação, um tem de assegurar que a latência da classificação do modelo é baixa o suficiente para utilizar na produção.
>


>[!NOTE]
>Para implementação utilizando o Azure Machine Learning Studio, consulte [implementar um serviço web do Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>Um teste a / B
Quando vários modelos na produção, poderá ser útil efetuar [um teste a / B](https://en.wikipedia.org/wiki/A/B_testing) para comparar o desempenho dos modelos. 

 
## <a name="next-steps"></a>Passos Seguintes

Instruções que demonstram todos os passos no processo de **cenários específicos** também são fornecidos. São listados e ligados com descrições de miniaturas no [instruções de exemplo](walkthroughs.md) artigo. Estes mostram como combinar em nuvem, ferramentas no local e serviços para um fluxo de trabalho ou pipeline para criar uma aplicação inteligente. 
 


