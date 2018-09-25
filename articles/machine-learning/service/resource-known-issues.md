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
ms.openlocfilehash: 27e73bc75c5f04190bad3dab49c1d46782982a18
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034146"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Problemas conhecidos e resolução de problemas serviço do Azure Machine Learning
 
Este artigo ajuda-o, encontre e corrija erros ou falhas encontrados ao utilizar o serviço Azure Machine Learning. 


## <a name="databricks-and-azure-machine-learning"></a>Databricks e do Azure Machine Learning

**Recomendação de cluster do Databricks:** 

Crie o cluster do Azure Databricks como v4.x com o Python 3. Recomendamos um cluster de elevada simultaneidade.
 
**SDK de AML instalar falha no Databricks, quando são instalados mais pacotes** alguns pacotes e, por exemplo, `psutil upgrade libs`, pode causar conflitos. Para evitar erros de instalação, instale pacotes pela versão de lib congelar. Este problema é relacionados com o Databricks e não relacionadas com o SDK de AML. Exemplo:
```
pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
```


## <a name="gather-diagnostics-information"></a>Recolher informações de diagnóstico
Por vezes, pode ser útil se pode fornecer informações de diagnóstico quando pedir ajuda. É aqui onde residem os ficheiros de registo:

## <a name="resource-quotas"></a>Quotas de recursos

Saiba mais sobre o [quotas de recursos](how-to-manage-quotas.md) que poderá encontrar ao trabalhar com o Azure Machine Learning.

## <a name="get-more-support"></a>Obter suporte mais

Pode submeter pedidos de suporte e obtenha ajuda de suporte técnico, fóruns e muito mais. [Saiba mais...](support-for-aml-services.md)
