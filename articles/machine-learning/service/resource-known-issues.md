---
title: Problemas e resolução de problemas de conhecidos para o serviço Azure Machine Learning
description: Obter uma lista dos problemas conhecidos, soluções alternativas e resolução de problemas
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: d84040dc440c373ae9bae6dbac7a95109a387ba7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162751"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conhecidos e resolução de problemas serviço do Azure Machine Learning
 
Este artigo ajuda-o, encontre e corrija erros ou falhas encontrados ao utilizar o serviço Azure Machine Learning. 


## <a name="databricks"></a>Databricks

Problemas de Databricks e o Azure Machine Learning.

1. Recomendação de cluster do Databricks:
   
   Crie o cluster do Azure Databricks como v4.x com o Python 3. Recomendamos um cluster de elevada simultaneidade.
 
1. SDK de AML instalar falha no Databricks quando mais pacotes são instalados.

   Alguns pacotes e, por exemplo, `psutil upgrade libs`, pode causar conflitos. Para evitar erros de instalação, instale pacotes pela versão de lib congelar. Este problema é relacionados com o Databricks e não relacionadas com o SDK de AML. Exemplo:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="gather-diagnostics-information"></a>Recolher informações de diagnóstico
Por vezes, pode ser útil se pode fornecer informações de diagnóstico quando pedir ajuda. É aqui onde residem os ficheiros de registo:

## <a name="resource-quotas"></a>Quotas de recursos

Saiba mais sobre o [quotas de recursos](how-to-manage-quotas.md) que poderá encontrar ao trabalhar com o Azure Machine Learning.

## <a name="get-more-support"></a>Obter suporte mais

Pode submeter pedidos de suporte e obtenha ajuda de suporte técnico, fóruns e muito mais. [Saiba mais...](support-for-aml-services.md)
