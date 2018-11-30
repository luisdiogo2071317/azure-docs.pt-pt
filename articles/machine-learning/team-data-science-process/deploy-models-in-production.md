---
title: Implementar modelos na produção - Azure Machine Learning | Documentos da Microsoft
description: Como implementar modelos para produção, permitindo-lhes desempenhar um papel ativo na tomada de decisões de negócios.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 5b1614f92f7633e008f4f7176723002dc7730b15
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495986"
---
# <a name="deploy-models-in-production"></a>Implementar modelos na produção

Implantação de produção permite que um modelo para desempenhar um papel ativo numa empresa. Predições a partir de um modelo implementado podem ser utilizadas para a tomada de decisões comerciais.

## <a name="production-platforms"></a>Plataformas de produção

Existem várias abordagens e plataformas para colocar os modelos em produção. Aqui estão algumas opções:

- [Onde pretende implementar modelos com o serviço Azure Machine Learning](../service/how-to-deploy-and-where.md)
- [Implementação de um modelo no SQL server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Antes da implantação, é necessário assegurar que a latência de modelo de classificação é baixa o suficiente para utilizar na produção.
>

>[!NOTE]
>Para ver a implementação com o Azure Machine Learning Studio, consulte [implementar um serviço web do Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A testes a / B

Quando vários modelos na produção, pode ser útil realizar [A testes a / B](https://en.wikipedia.org/wiki/A/B_testing) para comparar o desempenho dos modelos. 
 
## <a name="next-steps"></a>Passos Seguintes

Orientações passo a passo que demonstram todas as etapas do processo para **cenários específicos** também são fornecidas. Se estão listadas e estão associados ao descrições em miniatura da [instruções passo a passo do exemplo](walkthroughs.md) artigo. Eles ilustram como combinar a cloud, ferramentas no local e serviços num fluxo de trabalho ou um pipeline para criar uma aplicação inteligente. 
